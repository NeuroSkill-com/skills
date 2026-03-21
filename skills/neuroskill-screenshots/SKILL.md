---
name: neuroskill-screenshots
description: NeuroSkill screenshot search commands — `search-images` for OCR text and CLIP vision search, `screenshots-around` for temporal proximity, `screenshots-for-eeg` for finding what was on screen during EEG sessions, and `eeg-for-screenshots` for discovering brain state near matching screenshots. Cross-modal EEG↔screenshot bridging. Use when searching for past screen content, correlating screen activity with brain states, or exploring screenshot history.
---

# NeuroSkill Screenshot Search

NeuroSkill can periodically capture screenshots of the active window, embed them
with CLIP vision (ONNX), run OCR text extraction, and index both modalities in
separate HNSW indices for similarity search.

> **Note:** Screenshot capture must be enabled in Settings → Screenshots.
> Screenshots are served via `GET /screenshots/<day>/<filename>` on the API server
> and appear as indicators on the History day-view heatmap grid.

---

## LLM Tool Calls

When calling these commands via the LLM `skill` tool, use `command` + `args`:

```json
{"command": "search_screenshots", "args": {"query": "browser", "k": 10}}
{"command": "search_screenshots", "args": {"query": "TODO", "mode": "substring"}}
{"command": "screenshots_around", "args": {"timestamp": 1740412800, "window_secs": 60}}
{"command": "screenshots_for_eeg", "args": {"start_utc": 1740412800, "end_utc": 1740415500}}
{"command": "eeg_for_screenshots", "args": {"query": "compiler error", "k": 5}}
```

---

## Command Overview

| Command | Direction | Description |
|---|---|---|
| `search-images "query"` | Text → Screenshots | Search by OCR text (semantic/substring) |
| `search-images --by-image <path>` | Image → Screenshots | Search by visual similarity (CLIP) |
| `screenshots-around --at <utc>` | Timestamp → Screenshots | Find screenshots near a specific time |
| `screenshots-for-eeg` | **EEG → Screenshots** | Find screenshots captured during EEG epochs |
| `eeg-for-screenshots "query"` | **Screenshots → EEG** | Find brain data & labels near matched screenshots |

---

## `search-images` — Search Screenshots by OCR Text or Visual Similarity

Search all captured screenshots using OCR-extracted text or CLIP vision embeddings.

### OCR Text Search Modes

- `semantic` (default) — embeds the query text and searches the OCR HNSW index
- `substring` — direct SQL LIKE matching against raw OCR text

```bash
npx neuroskill search-images "compiler error"
npx neuroskill search-images "login page" --k 5
npx neuroskill search-images "TODO" --mode substring
npx neuroskill search-images "dashboard" --json | jq '.results[].filename'
npx neuroskill search-images "slack message" --mode semantic --k 10
npx neuroskill search-images "code review" --json | jq '.results[] | {file: .filename, app: .app_name, sim: .similarity}'
```

### Vision Search (CLIP)

Search by visual similarity using a reference image. The image is embedded via
the CLIP model and compared against the screenshots HNSW index.

```bash
npx neuroskill search-images --by-image reference.png
npx neuroskill search-images --by-image screenshot.jpg --k 5
npx neuroskill search-images --by-image layout-mock.webp --json | jq '.results[].filename'
```

### Options

| Flag | Default | Description |
|---|---|---|
| `--mode <m>` | `semantic` | Search mode: `semantic` (embedding similarity) or `substring` (literal text match) |
| `--by-image <path>` | — | Search by visual similarity instead of OCR text (CLIP vision) |
| `--k <n>` | `20` | Maximum number of results to return |
| `--json` | — | Output raw JSON (pipe-safe) |
| `--full` | — | Human-readable summary + colorized JSON |

### HTTP / WebSocket API

```bash
# Semantic OCR search (default):
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"search_screenshots","query":"compiler error","k":20,"mode":"semantic"}'

# Substring OCR search:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"search_screenshots","query":"TODO","k":10,"mode":"substring"}'

# Vision search (base64-encoded image):
IMAGE_B64=$(base64 -w0 reference.png)
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d "{\"command\":\"search_screenshots_by_image_b64\",\"image_b64\":\"$IMAGE_B64\",\"k\":20}"

# Vision search (raw embedding vector):
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"search_screenshots_vision","vector":[0.1, 0.2, ...],"k":20}'
```

### JSON Response

```jsonc
{
  "command": "search_screenshots",
  "ok": true,
  "query": "compiler error",
  "mode": "semantic",   // or "vision" for --by-image
  "k": 20,
  "count": 3,
  "results": [
    {
      "timestamp":    20260315143025,
      "unix_ts":      1741963825,
      "filename":     "20260315/20260315143025.webp",
      "app_name":     "VS Code",
      "window_title": "skill — cli.ts",
      "ocr_text":     "error[E0308]: mismatched types expected…",
      "similarity":   0.87
    }
  ]
}
```

---

## `screenshots-around` — Find Screenshots Near a Timestamp

Find all screenshots captured within a time window (±seconds) around a specific
Unix timestamp. Useful for seeing what was on screen during a specific EEG moment.

```bash
npx neuroskill screenshots-around --at 1740412800
npx neuroskill screenshots-around --at 1740412800 --seconds 120
npx neuroskill screenshots-around --at 1740412800 --json | jq '.results[].filename'
```

### Options

| Flag | Default | Description |
|---|---|---|
| `--at <utc>` | (required) | Target Unix timestamp in seconds |
| `--seconds <n>` | `60` | Window size: ±N seconds around the target |

### HTTP / WebSocket API

```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"screenshots_around","timestamp":1740412800,"window_secs":60}'
```

### JSON Response

```jsonc
{
  "command": "screenshots_around",
  "ok": true,
  "count": 4,
  "results": [
    {
      "timestamp":    20260224075945,
      "unix_ts":      1740412785,
      "filename":     "20260224/20260224075945.webp",
      "app_name":     "Firefox",
      "window_title": "GitHub - Pull Request",
      "ocr_text":     "Merge pull request #42..."
    }
  ]
}
```

---

## `screenshots-for-eeg` — EEG → Screenshots

Find screenshots captured near EEG recording timestamps.  Given an EEG time range
(auto: latest session, or `--start`/`--end`), finds all EEG embedding timestamps in
that range, then returns screenshots within `--window` seconds (default ±30s) of
each epoch.

This is the **"EEG → screenshots" cross-modal bridge**: start from brain data,
discover what was on screen at that moment.

```bash
npx neuroskill screenshots-for-eeg                             # auto: last session
npx neuroskill screenshots-for-eeg --start 1740412800 --end 1740415500
npx neuroskill screenshots-for-eeg --window 60                  # ±60s around each epoch
npx neuroskill screenshots-for-eeg --limit 20                   # max 20 results
npx neuroskill screenshots-for-eeg --json | jq '.results[].screenshot.filename'
npx neuroskill screenshots-for-eeg --json | jq '.results[] | {eeg: .eeg_timestamp_utc, file: .screenshot.filename}'
```

### Options

| Flag | Default | Description |
|---|---|---|
| `--start <utc>` | auto (latest session) | Start of EEG range (unix seconds) |
| `--end <utc>` | auto (latest session) | End of EEG range (unix seconds) |
| `--window <n>` | `30` | Temporal window: ±N seconds around each EEG epoch |
| `--limit <n>` | `50` | Maximum number of screenshot results |

### HTTP / WebSocket API

```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"screenshots_for_eeg","start_utc":1740412800,"end_utc":1740415500,"window_secs":30,"limit":50}'
```

### JSON Response

```jsonc
{
  "command": "screenshots_for_eeg",
  "ok": true,
  "start_utc":   1740412800,
  "end_utc":     1740415500,
  "window_secs": 30,
  "eeg_count":   541,
  "count":       12,
  "results": [
    {
      "eeg_timestamp_utc": 1740413130,
      "screenshot": {
        "timestamp":    20260224080530,
        "unix_ts":      1740413128,
        "filename":     "20260224/20260224080530.webp",
        "app_name":     "VS Code",
        "window_title": "main.rs",
        "ocr_text":     "fn dispatch(app: &AppHandle…",
        "similarity":   0.0
      }
    }
  ]
}
```

---

## `eeg-for-screenshots` — Screenshots → EEG

Find EEG data and labels near screenshots matching an OCR text query.
This is the **"screenshots → EEG" cross-modal bridge**: start from what
was on screen, discover what your brain was doing at that moment.

**Pipeline:**
1. Search screenshots by OCR text (semantic or substring mode).
2. For each matched screenshot, find EEG labels within `--window` seconds.
3. Report the associated EEG session and labels alongside each screenshot.

Use this to answer: *"When I was looking at [X], what was my brain doing?"*

```bash
npx neuroskill eeg-for-screenshots "compiler error"
npx neuroskill eeg-for-screenshots "TODO" --mode substring --k 5
npx neuroskill eeg-for-screenshots "dashboard" --window 120
npx neuroskill eeg-for-screenshots "login page" --json | jq '.results[].labels[].text'
npx neuroskill eeg-for-screenshots "slack" --json | jq '.results[] | {file: .screenshot.filename, labels: [.labels[].text]}'
```

### Options

| Flag | Default | Description |
|---|---|---|
| `--mode <m>` | `semantic` | Search mode: `semantic` or `substring` |
| `--k <n>` | `10` | Maximum number of screenshot matches |
| `--window <n>` | `60` | Temporal window: ±N seconds to search for EEG labels |

### HTTP / WebSocket API

```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"eeg_for_screenshots","query":"compiler error","k":10,"window_secs":60,"mode":"semantic"}'
```

### JSON Response

```jsonc
{
  "command": "eeg_for_screenshots",
  "ok": true,
  "query":            "compiler error",
  "mode":             "semantic",
  "k":                10,
  "window_secs":      60,
  "screenshot_count": 3,
  "count":            3,
  "results": [
    {
      "screenshot": {
        "timestamp":    20260315143025,
        "unix_ts":      1741963825,
        "filename":     "20260315/20260315143025.webp",
        "app_name":     "VS Code",
        "window_title": "skill — cli.ts",
        "ocr_text":     "error[E0308]: mismatched types expected…",
        "similarity":   0.87
      },
      "labels": [
        {
          "id": 47, "text": "debugging session",
          "eeg_start": 1741963560, "eeg_end": 1741963860,
          "label_start": 1741963680, "label_end": 1741963680
        },
        {
          "id": 48, "text": "frustrated",
          "eeg_start": 1741963740, "eeg_end": 1741964040,
          "label_start": 1741963860, "label_end": 1741963860
        }
      ],
      "session": {
        "csv_path": "/home/user/.skill/20260315/exg_1741960800.csv",
        "session_start_utc": 1741960800,
        "session_end_utc":   1741964400,
        "device_name":       "Muse-A1B2"
      }
    }
  ]
}
```

---

## Cross-Modal Workflows

Combine the screenshot commands with EEG search for powerful cross-modal analysis:

```bash
# ── "What was on screen during my best focus session?" ────────────────────────
START=$(npx neuroskill sessions --json | jq '.sessions[0].start_utc')
END=$(npx neuroskill sessions --json | jq '.sessions[0].end_utc')
npx neuroskill screenshots-for-eeg --start $START --end $END --window 15

# ── "How did my brain react to error messages?" ──────────────────────────────
npx neuroskill eeg-for-screenshots "error" --window 120

# ── "Find screenshots visually similar to a reference layout" ─────────────────
npx neuroskill search-images --by-image reference-layout.png --k 10

# ── "Find what I was looking at when a specific label was created" ────────────
LABEL_TS=$(npx neuroskill search-labels "deep focus" --json | jq '.results[0].created_at')
npx neuroskill screenshots-around --at $LABEL_TS --seconds 30

# ── "Full chain: OCR → screenshots → EEG → labels → neural search" ───────────
# Step 1: Find screenshots showing code review
npx neuroskill eeg-for-screenshots "pull request" --json | jq '.results[0].session'
# Step 2: Use the session timestamps for a full neural search
npx neuroskill search --start <session_start> --end <session_end> --k 5

# ── "See what was on screen at a search hit timestamp" ────────────────────────
HIT_TS=$(npx neuroskill search --json | jq '.result.results[0].neighbors[0].timestamp_unix')
npx neuroskill screenshots-around --at $HIT_TS

# ── "View the actual screenshot image in a browser" ──────────────────────────
PORT=8375
FILE=$(npx neuroskill screenshots-around --at 1740412800 --json | jq -r '.results[0].filename')
open "http://127.0.0.1:$PORT/screenshots/$FILE"   # macOS
xdg-open "http://127.0.0.1:$PORT/screenshots/$FILE"  # Linux
```
