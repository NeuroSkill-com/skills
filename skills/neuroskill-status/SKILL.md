---
name: neuroskill-status
description: NeuroSkill `status` command — full system snapshot including device state, signal quality, EEG scores, band powers, ratios, embeddings, labels, hooks summary, sleep summary, and recording history. Use when checking current EEG state, device connection, or session metadata.
---

# NeuroSkill `status` Command

Full snapshot: device state, session, signal quality, scores, bands, embeddings, labels,
hooks (with latest trigger), sleep summary, and recording history.

## Supported Devices

| Device | Channels | Sample Rate | Connection |
|---|---|---|---|
| **Muse** (S, 2, 2016) | 4 (TP9, AF7, AF8, TP10) | 256 Hz | BLE |
| **OpenBCI Ganglion** | 4 | 200 Hz | BLE |
| **Neurable MW75 Neuro** | 12 | 500 Hz | BLE + RFCOMM |
| **Hermes V1** | 8 (Fp1, Fp2, AF3, AF4, F3, F4, FC1, FC2) | 250 Hz | BLE GATT |

The DSP pipeline dynamically scales to the active channel count (4, 8, or 12).

---

```bash
npx neuroskill status
npx neuroskill status --json
npx neuroskill status --json | jq '.scores.focus'
npx neuroskill status --json | jq '.scores.bands'
npx neuroskill status --json | jq '.device.battery'
npx neuroskill status --json | jq '.signal_quality'
npx neuroskill status --json | jq '.sleep'
npx neuroskill status --json | jq '.history.current_streak_days'
npx neuroskill status --json | jq '.hooks'
npx neuroskill status --json | jq '.hooks.latest_trigger'

# Poll every N seconds (keeps connection open, re-prints a fresh snapshot each time):
npx neuroskill status --poll 5              # refresh every 5 s
npx neuroskill status --poll 10 --json     # JSON snapshot every 10 s
```

**HTTP:**
```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"status"}'
```

---

## Full JSON Response

```jsonc
{
  "command": "status",
  "ok": true,
  "device": {
    "state": "connected",          // "connected" | "connecting" | "disconnected"
    "name": "Muse-A1B2",          // or "MW75-Neuro-XXXX", "Hermes-XXXX", "Ganglion-XXXX"
    "battery": 73,                 // percent
    "firmware": "1.3.4",
    "eeg_samples": 195840,         // cumulative samples this run
    "ppg_samples": 30600,          // Muse only (PPG sensor)
    "imu_samples": 122400,
    "eeg_channels": 4              // 4 (Muse/Ganglion), 8 (Hermes), or 12 (MW75)
  },
  "session": {
    "start_utc": 1740412800,       // Unix seconds (UTC)
    "duration_secs": 1847,
    "n_epochs": 369                // 5-second embedding epochs computed so far
  },
  "signal_quality": {
    // Keys vary by device — Muse: tp9/af7/af8/tp10; Hermes: fp1/fp2/af3/af4/f3/f4/fc1/fc2
    // MW75: ft7/t7/tp7/cp5/p7/c5/ft8/t8/tp8/cp6/p8/c6
    "tp9": 0.95,                   // 0–1; >=0.9 = good, >=0.7 = acceptable
    "af7": 0.88,
    "af8": 0.91,
    "tp10": 0.97
  },
  "scores": {
    // Core scores (0–1 unless noted):
    "focus": 0.70,
    "relaxation": 0.40,
    "engagement": 0.60,
    "meditation": 0.52,
    "mood": 0.55,
    "cognitive_load": 0.33,
    "drowsiness": 0.10,
    "hr": 68.2,                    // bpm (from PPG)
    "snr": 14.3,                   // signal-to-noise ratio in dB
    "stillness": 0.88,             // 0–1; 1 = perfectly still
    // Band powers (relative, sum ~ 1):
    "bands": {
      "rel_delta": 0.28,
      "rel_theta": 0.18,
      "rel_alpha": 0.32,
      "rel_beta":  0.17,
      "rel_gamma": 0.05
    },
    // EEG ratios & spectral indices:
    "faa": 0.042,                  // Frontal Alpha Asymmetry (positive = approach)
    "tar": 0.56,                   // Theta/Alpha Ratio
    "bar": 0.53,                   // Beta/Alpha Ratio
    "tbr": 1.06,                   // Theta/Beta Ratio
    "apf": 10.1,                   // Alpha Peak Frequency (Hz)
    "coherence": 0.614,
    "mu_suppression": 0.031
  },
  "embeddings": {
    "today": 342,
    "total": 14820,
    "recording_days": 31
  },
  "labels": {
    "total": 58,
    "recent": [
      { "id": 42, "text": "meditation start", "created_at": 1740413100 }
    ]
  },
  "sleep": {
    // Last 48 h sleep staging summary:
    "total_epochs": 1054,
    "wake_epochs": 134,
    "n1_epochs": 89,
    "n2_epochs": 421,
    "n3_epochs": 298,
    "rem_epochs": 112,
    "epoch_secs": 5
  },
  "hooks": {
    "total": 3,                    // total configured hooks
    "enabled": 2,                  // how many are enabled
    "latest_trigger": {            // most recent trigger across all hooks (null if never)
      "hook": "Deep Work Guard",   // hook name that fired
      "triggered_at_utc": 1740413100,
      "distance": 0.0892,          // cosine distance to matched reference
      "label_id": 7,
      "label_text": "focused reading session"
    }
  },
  "history": {
    "total_sessions": 63,
    "recording_days": 31,
    "current_streak_days": 7,
    "total_recording_hours": 94.2,
    "longest_session_min": 187,
    "avg_session_min": 89
  }
}
```

---

## Hidden Fields (visible only with `--full` or `--json`)

| Hidden field | Type | Contents |
|---|---|---|
| `scores.faa`, `scores.tar`, `scores.bar` ... | numbers | EEG ratios and spectral indices not surfaced in the default summary |
| `calibration.actions[]` | array | Full ordered list of calibration step objects |
| `labels.recent[]` | array | Full label objects; summary only prints text + timestamp |
| `hooks.latest_trigger` | object | Most recent hook trigger: hook name, timestamp, distance, label_id, label_text |
| `history.today_vs_avg` | object | Per-metric today-vs-7-day-avg comparison table |

```bash
npx neuroskill status --json | jq '.history.today_vs_avg'
npx neuroskill status --json | jq '.calibration.actions'
npx neuroskill status --json | jq '.labels.recent'
npx neuroskill status --json | jq '.hooks.latest_trigger'
```
