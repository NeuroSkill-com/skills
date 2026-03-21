---
name: neuroskill-skills-readme
description: README for the neuroskill-skills npm package — installation, skill listing, and contribution guide.
---

# neuroskill-skills

[![npm](https://img.shields.io/npm/v/neuroskill-skills)](https://www.npmjs.com/package/neuroskill-skills)

A collection of NeuroSkill EXG skills for [NeuroLoop™️](https://neuroloop.io) — a biometric AI companion powered by a real-time EXG device (Muse or OpenBCI). Skills are loaded contextually: the harness injects the relevant skill file into the system prompt when the user's message matches the skill's domain.

---

## Skills

### EXG Data & API Skills

| Skill | Description |
|---|---|
| [`neuroskill-transport`](skills/neuroskill-transport/SKILL.md) | WebSocket & HTTP transport layer, port discovery, Quick Start, output modes (`--json` / `--full` / default), and every global CLI flag. |
| [`neuroskill-status`](skills/neuroskill-status/SKILL.md) | `status` command — full system snapshot: device state, signal quality, EXG scores, band powers, ratios, embeddings, labels, 48 h sleep summary, and recording history. |
| [`neuroskill-sessions`](skills/neuroskill-sessions/SKILL.md) | `session` and `sessions` commands — per-session metric breakdowns with first-half → second-half trend arrows, full session listing across all days, and Unix timestamp helpers. |
| [`neuroskill-search`](skills/neuroskill-search/SKILL.md) | `search` and `compare` commands — ANN search for neurally similar EXG moments across all history, and A/B session comparison with metric deltas, trend directions, and UMAP enqueuing. |
| [`neuroskill-sleep`](skills/neuroskill-sleep/SKILL.md) | `sleep` and `umap` commands — EXG-based sleep stage classification (Wake / N1 / N2 / N3 / REM) with efficiency, onset latency, bout analysis; and GPU-accelerated 3D UMAP projection of session embeddings for spatial state comparison. |
| [`neuroskill-labels`](skills/neuroskill-labels/SKILL.md) | `label`, `search-labels`, and `interactive` commands — creating timestamped EXG text annotations, semantic vector search over labels (HNSW), and a cross-modal 4-layer graph search combining text similarity, EXG similarity, and temporal label proximity. Supports Graphviz DOT export. |
| [`neuroskill-streaming`](skills/neuroskill-streaming/SKILL.md) | `say`, `listen`, `notify`, `calibrate`, `calibrations`, `timer`, and `raw` commands — on-device TTS speech, real-time WebSocket event streaming (EXG, PPG, IMU, scores, labels), OS notifications, calibration profile management, focus timer, and raw JSON passthrough for unlisted commands. |
| [`neuroskill-data-reference`](skills/neuroskill-data-reference/SKILL.md) | Complete metric field reference — EXG band powers, ratios & indices (FAA, TBR, BAR, TAR, APF, SEF95, coherence, …), core scores (focus, relaxation, engagement, meditation, mood, cognitive load, drowsiness), complexity measures (Hjorth, permutation entropy, Higuchi FD, DFA, sample entropy, PAC), PPG/HRV fields, motion & artifact markers, sleep stage codes, neurological correlate indices, and consciousness metrics. |
| [`neuroskill-recipes`](skills/neuroskill-recipes/SKILL.md) | Use-case recipes and scripting patterns — shell snippets for focus monitoring, stress tracking, sleep quality analysis, cognitive load queries, meditation tracking, cross-modal graph search, A/B session comparison, time-range queries, and automation with cron / Python / Node.js / HTTP. |
| [`neuroskill-hooks`](skills/neuroskill-hooks/SKILL.md) | `hooks` command — Proactive Hooks CRUD, real-time EXG pattern matching with scenario gating (cognitive/emotional/physical), threshold suggestions, WebSocket broadcast triggers, and audit logging. |
| [`neuroskill-dnd`](skills/neuroskill-dnd/SKILL.md) | `dnd` command — EXG-driven Do Not Disturb automation, rolling focus score average, OS-level DND state, and force-override on/off. |
| [`neuroskill-screenshots`](skills/neuroskill-screenshots/SKILL.md) | `search-images`, `screenshots-around`, `screenshots-for-eeg`, and `eeg-for-screenshots` commands — search screenshots by OCR text (semantic/substring), by visual similarity (CLIP `--by-image`), find screenshots near a timestamp or EEG session, and cross-modal EEG↔screenshot bridging. |

### LLM & AI Skills

| Skill | Description |
|---|---|
| [`neuroskill-llm`](skills/neuroskill-llm/SKILL.md) | Built-in on-device LLM inference server — model catalog management, vision support (mmproj), streaming WebSocket and HTTP chat, automatic tool calling, GenParams tuning, persistent chat history, and OpenAI-compatible API. |

### Protocol & Intervention Skills

| Skill | Description |
|---|---|
| [`neuroskill-evidence`](skills/neuroskill-evidence/SKILL.md) | Implicit evidence collection and personal effectiveness engine — standardised `px:` label schema for automatic before/after measurement, outcome scoring (positive/neutral/negative), personal protocol ranking by success rate and effect size, life-event tracking (caffeine, meals, walks, meetings, sleep quality), evidence-driven selection rules, modality preference detection, time-of-day patterns, and privacy safeguards. All skills that deliver interventions follow this skill's rules. |
| [`neuroskill-protocols`](skills/neuroskill-protocols/SKILL.md) | Full guided-protocol repertoire — 130+ mind-body practices with built-in personalisation engine for any age, culture, profession, ability, neurodivergence, and situation. Organised across: attention & focus, stress & autonomic regulation, emotional regulation & mood, relaxation & alpha promotion, sleep & circadian, body & somatic, consciousness & integration, cognitive performance & motivation, headache & migraine, energy & alertness, hemispheric balance & breathing, deep relaxation, recovery & rest, deep meditation, extended emotional processing, autonomic & vagal, motor & embodiment, neck & cervical relief, eye exercises & visual recovery, morning routines, workout & gym, hydration, bathroom & movement breaks, music, social-media & digital-addiction, dietary protocols, non-breathing alternatives, and context-specific collections for parents/caregivers, elders, teens/students, neurodivergent users, commuters, manual workers, healthcare/shift workers, intimate/relational, accessibility-adapted, culturally diverse practices, and situational micro-protocols. |

---

## Install

```bash
# From npm (recommended)
npx skills add NeuroSkill-com/skill

# From GitHub
npx skills add NeuroSkill-com/skill

# From a local clone
npx skills add ./skill

# Install a single skill
npx skills add NeuroSkill-com/skill --skill neuroskill-status

# List all available skills without installing
npx skills add NeuroSkill-com/skill --list
```

---

## How Contextual Loading Works

On every user message the harness:
1. Runs `npx neuroskill status` and injects the live EXG snapshot into the system prompt.
2. Detects domain signals in the user's prompt (stress, sleep, focus, protocols, …).
3. Runs the relevant neuroskill commands in parallel (`neurological`, `session`, `search-labels`, …).
4. If protocol intent is detected, reads `skills/neuroskill-protocols/SKILL.md` and injects the full protocol repertoire for that turn.
5. Injects the skill index (`SKILL.md`) so the model always knows every capability available.

---

## Tools Available to the Agent

| Tool | Purpose |
|---|---|
| `neuroskill_run` | Run any neuroskill EXG command and return its output. |
| `neuroskill_label` | Create a timestamped EXG annotation for the current moment (supports `--context` and `--at` flags). |
| `run_protocol` | Execute a multi-step guided protocol with OS notifications, per-step timing, and EXG labelling. |
| `prewarm` | Kick off a background `npx neuroskill compare` so results are ready when needed. |
| `memory_read` | Read the agent's persistent memory file. |
| `memory_write` | Write or append to the agent's persistent memory file. |
| `web_fetch` | Fetch a URL and return its content. |
| `web_search` | Search the web and return results. |

## Citing

If you use this library in academic work, please cite it as:

```bibtex
@software{kosmyna2026openbci,
  author    = {Nataliya Kosmyna and Eugene Hauptmann},
  title     = {{neuroskill}: Skills to model Human State of Mind using NeuroSkill™️},
  year      = {2026},
  version   = {0.0.1},
  url       = {https://github.com/NeuroSkill-com/skill},
  license   = {AI100},
}
```

## License

[AI100](/LICENSE)
