---
name: neuroskill-screenshots
description: NeuroSkill screenshot search commands — `search-images` for OCR text search across captured screenshots (semantic or substring mode) and `screenshots-around` for finding screenshots near a specific timestamp. Screenshots are captured periodically, embedded with CLIP vision, OCR-indexed, and served via the API. Use when searching for past screen content, finding what was on screen at a specific time, or exploring screenshot history.
---

# NeuroSkill Screenshot Search

NeuroSkill can periodically capture screenshots of the active window, embed them
with CLIP vision (ONNX), run OCR text extraction, and index both modalities in
separate HNSW indices for similarity search.

> **Note:** Screenshot capture must be enabled in Settings → Screenshots.
> Screenshots are served via `GET /screenshots/<day>/<filename>` on the API server
> and appear as indicators on the History day-view heatmap grid.

---

## `search-images` — Search Screenshots by OCR Text

Search all captured screenshots using OCR-extracted text. Supports semantic
(embedding-based) and substring (literal match) search modes.

```bash
npx neuroskill search-images "compiler error"
npx neuroskill search-images "login page" --k 5
npx neuroskill search-images "TODO" --mode substring
npx neuroskill search-images "dashboard" --json | jq '.results[].filename'
npx neuroskill search-images "slack message" --mode semantic --k 10
npx neuroskill search-images "code review" --json | jq '.results[] | {file: .filename, app: .app_name, sim: .similarity}'
```

### Options

| Flag | Default | Description |
|---|---|---|
| `--mode <m>` | `semantic` | Search mode: `semantic` (embedding similarity) or `substring` (literal text match) |
| `--k <n>` | `20` | Maximum number of results to return |
| `--json` | — | Output raw JSON (pipe-safe) |
| `--full` | — | Human-readable summary + colorized JSON |

### HTTP / WebSocket API

```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"search_screenshots","query":"compiler error","k":20,"mode":"semantic"}'
```

### JSON Response

```jsonc
{
  "command": "search_screenshots",
  "ok": true,
  "mode": "semantic",
  "k": 20,
  "count": 3,
  "results": [
    {
      "timestamp": 1740412800,
      "unix_ts": 1740412800,
      "filename": "20260224_080000.png",
      "app_name": "Visual Studio Code",
      "window_title": "cli.ts - skill",
      "ocr_text": "error: cannot find module...",
      "similarity": 0.87
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
| `--json` | — | Output raw JSON |
| `--full` | — | Summary + colorized JSON |

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
  "count": 2,
  "results": [
    {
      "timestamp": 1740412790,
      "unix_ts": 1740412790,
      "filename": "20260224_075950.png",
      "app_name": "Firefox",
      "window_title": "GitHub - Pull Request",
      "ocr_text": "Merge pull request #42..."
    }
  ]
}
```

---

## Cross-referencing with EEG Data

Screenshots are timestamped, so you can correlate them with EEG sessions:

```bash
# Find what was on screen during a focus dip:
# 1. Get session timestamps
START=$(npx neuroskill sessions --json | jq '.sessions[0].start_utc')
END=$(npx neuroskill sessions --json | jq '.sessions[0].end_utc')

# 2. Search for EEG moments similar to your current state
npx neuroskill search --start $START --end $END --json | jq '.result.results[0].neighbors[0].timestamp'

# 3. Find screenshots around that EEG moment
npx neuroskill screenshots-around --at <timestamp_from_step_2> --seconds 30

# Search for screenshots of a specific app during work:
npx neuroskill search-images "Visual Studio Code" --mode substring --k 10
```
