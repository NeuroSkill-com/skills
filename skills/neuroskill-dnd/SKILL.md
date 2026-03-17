---
name: neuroskill-dnd
description: NeuroSkill Do Not Disturb automation — EEG-driven focus detection that activates OS-level DND when sustained attention exceeds a threshold. Shows rolling average, sample window, and OS state. Supports force-override on/off. Use when managing focus-based DND automation or checking DND status.
---

# NeuroSkill DND (Do Not Disturb) Automation

Show Do Not Disturb automation status, or force-override it.

With no subcommand, shows the full DND config and live state: automation enabled/disabled,
focus threshold, rolling average score, sample window fill, and OS-level Focus state.

With `on` or `off`, immediately activates or deactivates DND, bypassing the EEG threshold.

---

## CLI Examples

```bash
npx neuroskill dnd                                 # show config + live eligibility state
npx neuroskill dnd on                              # force-enable DND (bypass EEG threshold)
npx neuroskill dnd off                             # force-disable DND
npx neuroskill dnd --json                          # raw JSON (pipe to jq)
npx neuroskill dnd --json | jq '{enabled: .enabled, avg_score: .avg_score, threshold: .threshold}'
```

## HTTP API

```bash
# Status:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"dnd"}'

# Force on:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"dnd_set","enabled":true}'

# Force off:
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"dnd_set","enabled":false}'
```

---

## Example Output (Status)

```
⚡ dnd  automation status

  DND automation
    enabled        yes
    threshold      60  avg focus score (0–100) required to activate
    window         60s  (~240 samples at ~4 Hz)
    mode           standard

  Rolling average  (avg of last 240 focus scores)
    ████████████████████░░░░  72.5 / 60
    ▶ above threshold — DND is active

  Sample window
    ████████████████████████  240 / 240 samples

  State
    app activated  yes  (this app set DND)
    OS active      yes  (macOS Assertions.json / defaults read)
```

## JSON Response (Status)

```jsonc
{
  "command": "dnd",
  "ok": true,
  "enabled": true,
  "threshold": 60,
  "duration_secs": 60,
  "window_size": 240,
  "mode_identifier": "standard",
  "avg_score": 72.5,
  "sample_count": 240,
  "dnd_active": true,
  "os_active": true
}
```

## Hidden Fields (visible only with `--full` or `--json`)

| Field | Type | Contents |
|---|---|---|
| `avg_score` | number | Current rolling average focus score (0–100) |
| `sample_count` | number | How many focus score samples have been collected |
| `window_size` | number | Target number of samples for the rolling window |
| `mode_identifier` | string | DND automation mode identifier |
| `dnd_active` | boolean | Whether this app activated DND |
| `os_active` | boolean | Whether the OS reports DND as active |
