---
name: neuroskill-sleep
description: NeuroSkill `sleep` and `umap` commands — EEG-based sleep stage classification (Wake/N1/N2/N3/REM) with efficiency and bout analysis, and 3D UMAP projection of session embeddings for spatial comparison. Use when analysing sleep quality or visualising neural state separation between sessions.
---

# NeuroSkill `sleep` and `umap` Commands

---

## LLM Tool Calls

When calling these commands via the LLM `skill` tool, use `command` + `args`:

```json
{"command": "sleep", "args": {"start_utc": 1740380100, "end_utc": 1740412800}}
{"command": "umap", "args": {"a_start_utc": 1740380100, "a_end_utc": 1740382665, "b_start_utc": 1740412800, "b_end_utc": 1740415510}}
{"command": "umap_poll", "args": {"job_id": 5}}
```

---

## `sleep` — Sleep Stage Classification

Classify EEG epochs into sleep stages (Wake / N1 / N2 / N3 / REM) using
relative band-power ratios and simplified AASM heuristics.

Auto-range: all sessions from the last 24 hours.
By index: `sleep 0` = most recent session, `sleep 1` = previous, etc.

```bash
npx neuroskill sleep                          # auto: last 24h of sessions
npx neuroskill sleep 0                        # most recent session's sleep data
npx neuroskill sleep 1                        # previous session
npx neuroskill sleep --start 1740380100 --end 1740415510
npx neuroskill sleep --json | jq '.summary'
npx neuroskill sleep --json | jq '.analysis'
npx neuroskill sleep --json | jq '.summary | {n3: .n3_epochs, rem: .rem_epochs}'
```

**HTTP:**
```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"sleep","start_utc":1740380100,"end_utc":1740415510}' | jq '.summary'
```

### JSON Response

```jsonc
{
  "command": "sleep",
  "ok": true,
  "summary": {
    "total_epochs": 1054,
    "wake_epochs": 134,
    "n1_epochs": 89,
    "n2_epochs": 421,
    "n3_epochs": 298,
    "rem_epochs": 112,
    "epoch_secs": 5
  },
  "analysis": {
    "efficiency_pct": 85.2,
    "onset_latency_min": 12.5,
    "rem_latency_min": 62.0,
    "transitions": 38,
    "awakenings": 11,
    "stage_minutes": { "wake": 11, "n1": 7, "n2": 35, "n3": 25, "rem": 9 },
    "bouts": {
      "WAKE": { "count": 11, "mean_min": 1.0, "max_min": 3.5 },
      "N3":   { "count": 6,  "mean_min": 4.2, "max_min": 9.0 },
      "REM":  { "count": 4,  "mean_min": 2.3, "max_min": 4.5 }
    }
  },
  "epochs": [
    { "utc": 1740380100, "stage": 0, "rel_delta": 0.18, "rel_theta": 0.21, "rel_alpha": 0.38, "rel_beta": 0.17 }
    // ... one entry per 5-second epoch
  ]
}
```

> **Stage codes:** `0` = Wake, `1` = N1, `2` = N2, `3` = N3, `4` = REM.

### Hidden Fields (visible only with `--full` or `--json`)

| Hidden field | Contents |
|---|---|
| `epochs[]` | Per-epoch classification for every 5-second window — can be thousands of entries |

```bash
npx neuroskill sleep --json | jq '.epochs | length'
npx neuroskill sleep --json | jq '.epochs[0]'
npx neuroskill sleep --json | jq '[.epochs[] | select(.stage == 3)] | length'  # N3 epoch count
npx neuroskill sleep --json | jq '[.epochs[] | {utc: .utc, stage: .stage}]'    # hypnogram data
```

### Good Sleep Targets (healthy adult, ~8h)

- N3 (slow-wave): 15–25% of total sleep
- REM: 20–25%
- Sleep efficiency: > 85%
- Sleep onset: < 20 min

---

## `umap` — 3D UMAP Projection

Compute a 3D UMAP projection of EEG embedding vectors from two sessions.
Runs GPU-accelerated UMAP; the CLI polls for progress and prints a live bar.
Results are cached so re-running the same ranges is instant.

Auto-range: last two sessions (same as `compare`).

```bash
npx neuroskill umap                           # auto: last 2 sessions
npx neuroskill umap --a-start 1740380100 --a-end 1740382665 \
                 --b-start 1740412800 --b-end 1740415510
npx neuroskill umap --json | jq '.result.points | length'
npx neuroskill umap --json | jq '.result.points[0]'
npx neuroskill umap --json | jq '[.result.points[] | select(.session == "A")] | length'
npx neuroskill umap --json | jq '.result.analysis.separation_score'
```

**HTTP (two requests — enqueue then poll):**
```bash
# Step 1 — enqueue:
JOB=$(curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"umap","a_start_utc":1740380100,"a_end_utc":1740382665,"b_start_utc":1740412800,"b_end_utc":1740415510}')
JOB_ID=$(echo $JOB | jq '.job_id')

# Step 2 — poll until complete:
until [ "$(curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d "{\"command\":\"umap_poll\",\"job_id\":$JOB_ID}" | jq -r '.status')" = "complete" ]; do
  sleep 2
done
```

### JSON Response

```jsonc
{
  "status": "complete",
  "elapsed_ms": 8432,
  "result": {
    "points": [
      { "x": 1.23, "y": -0.45, "z": 2.01, "session": "A", "utc": 1740380105, "label": null },
      { "x": 1.31, "y": -0.38, "z": 1.94, "session": "A", "utc": 1740380110, "label": "eyes closed" },
      { "x": -0.87, "y": 1.34, "z": -1.22, "session": "B", "utc": 1740412805 }
    ],
    "n_a": 513, "n_b": 541, "dim": 3,
    "analysis": {
      "separation_score": 1.84,      // higher = better A/B separation
      "inter_cluster_distance": 2.31,
      "intra_spread_a": 0.82,
      "intra_spread_b": 0.94,
      "centroid_a": [1.23, -0.45, 2.01],
      "centroid_b": [-0.87, 1.34, -1.22],
      "n_outliers_a": 3,
      "n_outliers_b": 5
    }
  }
}
```

### Hidden Fields

| Hidden field | Contents |
|---|---|
| `result.points[]` | 3D coordinates for every embedding epoch — typically 500–2000+ entries |

```bash
npx neuroskill umap --json | jq '.result.points | length'
npx neuroskill umap --json | jq '[.result.points[] | select(.session == "B")]'
npx neuroskill umap --json | jq '[.result.points[] | select(.label != null)]'  # labeled points only
```

> **Interpreting separation score:**
> - `> 1.5` — sessions are neurally distinct (different brain states)
> - `< 0.5` — similar brain state across both sessions

---

## `sleep-schedule` — Sleep Schedule Configuration

View or update the sleep schedule used for session classification and sleep staging.

```bash
# Show current schedule:
npx neuroskill sleep-schedule
npx neuroskill sleep-schedule --json

# Update with explicit times:
npx neuroskill sleep-schedule set --bedtime 23:00 --wake 07:00

# Apply a named preset:
npx neuroskill sleep-schedule set --preset early_bird
npx neuroskill sleep-schedule set --preset night_owl
```

### Available Presets

| Preset | Bedtime | Wake | Duration |
|---|---|---|---|
| `default` | 23:00 | 07:00 | 8 h |
| `early_bird` | 21:30 | 05:30 | 8 h |
| `night_owl` | 01:00 | 09:00 | 8 h |
| `short_sleeper` | 00:00 | 06:00 | 6 h |
| `long_sleeper` | 22:00 | 08:00 | 10 h |

### HTTP

```bash
# Get:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"sleep_schedule"}'

# Set:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"sleep_schedule_set","bedtime":"23:00","wake_time":"07:00","preset":"default"}'
```

### JSON Response

```jsonc
{
  "bedtime": "23:00",
  "wake_time": "07:00",
  "preset": "default",
  "duration_minutes": 480
}
```
