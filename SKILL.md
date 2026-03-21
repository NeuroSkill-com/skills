---
name: neuroskill-index
description: Skill index and overview — lists all available NeuroSkill skills, tools, and explains how skill loading works.
index: true
---

# NeuroSkill Skill Index

NeuroSkill is a biometric AI companion powered by a real-time EEG device.
It reads brainwaves and physiology continuously, and uses that data to inform every response.

NeuroSkill exposes the EEG analysis API through a local WebSocket server and HTTP tunnel.
The `npx neuroskill <command>` CLI is the fastest way to query it from a terminal, shell
script, or any automation pipeline.

Skills are loaded contextually — the LLM loads the relevant skill file into its context
when the user's message matches the skill's domain.

---

## EEG Data & API Skills

| Skill | Loaded when | Description |
|---|---|---|
| [neuroskill-transport](skills/neuroskill-transport/SKILL.md) | transport/connection questions | WebSocket & HTTP transport, port discovery, Quick Start, output modes (`--json` / `--full`), and global CLI flags. |
| [neuroskill-status](skills/neuroskill-status/SKILL.md) | status/device questions | `status` command — full system snapshot: device state (4 device types), signal quality, EEG scores, band powers, ratios, hooks summary, embeddings, labels, sleep summary, and recording history. |
| [neuroskill-sessions](skills/neuroskill-sessions/SKILL.md) | session/history questions | `session` and `sessions` commands — per-session metric breakdowns with first/second-half trends, session listing, and Unix timestamp helpers. |
| [neuroskill-search](skills/neuroskill-search/SKILL.md) | comparison/trend questions | `search` and `compare` commands — ANN search for neurally similar EEG moments across all history, and A/B session comparison with metric deltas and UMAP enqueuing. |
| [neuroskill-sleep](skills/neuroskill-sleep/SKILL.md) | sleep/fatigue context | `sleep` and `umap` commands — EEG-based sleep stage classification (Wake/N1/N2/N3/REM) with efficiency and bout analysis, and 3D UMAP projection for spatial session comparison. |
| [neuroskill-labels](skills/neuroskill-labels/SKILL.md) | label/search context | `label`, `search-labels`, and `interactive` commands — creating EEG text annotations, semantic vector search over labels, and cross-modal 5-layer graph search combining text, EEG similarity, and screenshot discovery with 3D visualization. |
| [neuroskill-streaming](skills/neuroskill-streaming/SKILL.md) | streaming/calibration/TTS context | `say`, `listen`, `notify`, `calibrate`, `calibrations`, `timer`, and `raw` commands — on-device TTS speech, real-time WebSocket event streaming, OS notifications, calibration profile management, focus timer, and raw JSON passthrough. |
| [neuroskill-hooks](skills/neuroskill-hooks/SKILL.md) | hooks/automation context | `hooks` command — Proactive Hooks CRUD, real-time EEG pattern matching with scenario gating (cognitive/emotional/physical), threshold suggestions, WebSocket broadcast triggers, and audit logging. |
| [neuroskill-dnd](skills/neuroskill-dnd/SKILL.md) | DND/focus-mode context | `dnd` command — EEG-driven Do Not Disturb automation, rolling focus score average, OS-level DND state, and force-override on/off. |
| [neuroskill-screenshots](skills/neuroskill-screenshots/SKILL.md) | screenshot/OCR/vision search context | `search-images`, `screenshots-around`, `screenshots-for-eeg`, and `eeg-for-screenshots` commands — search screenshots by OCR text (semantic or substring), by visual similarity (CLIP `--by-image`), find screenshots near a timestamp or EEG session, and cross-modal EEG↔screenshot bridging (find brain state for screen content, or screen content for brain state). |
| [neuroskill-data-reference](skills/neuroskill-data-reference/SKILL.md) | metric field questions | All metric fields — band powers, EEG ratios and indices, core scores, complexity measures, PPG/HRV, motion and artifact markers, sleep stage codes, neurological indices, and consciousness metrics. |
| [neuroskill-recipes](skills/neuroskill-recipes/SKILL.md) | scripting/automation questions | Use-case recipes and scripting patterns — focus monitoring, stress tracking, sleep analysis, cognitive load queries, meditation tracking, cross-modal graph search, A/B comparison, time-range queries, and automation with cron/Python/Node.js/HTTP. |

---

## LLM & AI Skills

| Skill | Loaded when | Description |
|---|---|---|
| [neuroskill-llm](skills/neuroskill-llm/SKILL.md) | LLM/chat/model questions | Built-in on-device LLM inference server — model catalog management (add/download/select/delete GGUF models), vision support (mmproj), streaming WebSocket and HTTP chat, automatic tool calling, GenParams tuning, persistent chat history, and OpenAI-compatible API. |

---

## Protocol & Intervention Skills

| Skill | Loaded when | Description |
|---|---|---|
| [neuroskill-evidence](skills/neuroskill-evidence/SKILL.md) | any intervention, protocol, habit tracking, or "what works for me?" question | Implicit evidence collection and personal effectiveness engine — standardised `px:` label schema, automatic before/after measurement, outcome scoring, personal protocol ranking, life-event tracking, evidence-driven selection rules, and privacy safeguards. All other skills that deliver interventions MUST follow this skill's rules. |
| [neuroskill-protocols](skills/neuroskill-protocols/SKILL.md) | any protocol/exercise/routine intent detected | Protocol framework hub — personalisation engine, API integration guide, modality router, matching guidance, and index of 11 domain sub-skills. Always loaded alongside domain sub-skills. |
| [neuroskill-protocols-focus](skills/neuroskill-protocols-focus/SKILL.md) | focus, attention, cognitive load, energy, alertness, meditation, flow, creativity | Focus, attention, cognition, consciousness, deep meditation, and energy/alertness protocols. |
| [neuroskill-protocols-stress](skills/neuroskill-protocols-stress/SKILL.md) | stress, anxiety, relaxation, HRV, calm, overwhelm, breathing exercises | Stress, relaxation, autonomic regulation, HRV, hemispheric balance, and deep relaxation protocols. |
| [neuroskill-protocols-emotions](skills/neuroskill-protocols-emotions/SKILL.md) | emotions, mood, anger, grief, sadness, shame, fear, loneliness, joy | Emotional regulation, mood, and extended emotional processing protocols (12 specific emotions). |
| [neuroskill-protocols-sleep](skills/neuroskill-protocols-sleep/SKILL.md) | sleep, insomnia, nap, rest, recovery, NSDR, tired | Sleep, circadian, recovery, NSDR/yoga nidra, and power nap protocols. |
| [neuroskill-protocols-body](skills/neuroskill-protocols-body/SKILL.md) | body, tension, neck, headache, eye strain, posture, pain, grounding | Body, somatic, neck/cervical, eye exercise, headache/migraine, and motor protocols. |
| [neuroskill-protocols-routines](skills/neuroskill-protocols-routines/SKILL.md) | morning routine, gym, workout, exercise, hydration, break | Morning routines, workout/gym, hydration, and movement break protocols. |
| [neuroskill-protocols-nutrition](skills/neuroskill-protocols-nutrition/SKILL.md) | food, diet, nutrition, caffeine, fasting, meal, cravings, alcohol | Dietary, nutrition, caffeine timing, fasting, eating, and gut-brain protocols. |
| [neuroskill-protocols-music](skills/neuroskill-protocols-music/SKILL.md) | music, playlist, listening, binaural beats, singing | Music protocols — genre/BPM/artist suggestions for mood, focus, stress, sleep, and emotional release. |
| [neuroskill-protocols-digital](skills/neuroskill-protocols-digital/SKILL.md) | social media, phone, screen time, scrolling, digital detox, FOMO | Social media, digital addiction, screen time, and attention restoration protocols. |
| [neuroskill-protocols-breathfree](skills/neuroskill-protocols-breathfree/SKILL.md) | non-breathing, breath-free, alternative, tactile, fidget, cold water | 30+ non-breathing alternatives — cognitive, tactile, oculomotor, micro-movement, auditory, and passive physiological. |
| [neuroskill-protocols-life](skills/neuroskill-protocols-life/SKILL.md) | parent, elderly, teen, student, ADHD, autism, commute, shift work, relationship, disability, cultural | Context-specific protocols for 11 life situations — parenting, aging, teens, neurodivergent, commuters, manual workers, healthcare, relational, accessibility, cultural, and situational. |

---

## Tools Available to the LLM

The built-in LLM chat has access to these tools (enabled in Settings > LLM > Tools):

| Tool | Purpose |
|---|---|
| `date` | Get current date/time metadata (Unix timestamps, timezone, local/UTC). |
| `location` | Approximate public-IP geolocation (country/region/city/timezone). |
| `bash` | Execute shell commands and return stdout/stderr. |
| `read_file` | Read file contents from disk. |
| `write_file` | Write / create files. |
| `edit_file` | Surgical find-and-replace edits in files. |
| `search_output` | Navigate large tool outputs (paginated regex/head/tail). |
| `web_search` | Search the web and return results. |
| `web_fetch` | Fetch a URL and return its content. |
| `skill` | Query the NeuroSkill EEG API (status, sessions, labels, search, hooks, DND, calibrations, TTS, etc.). |

---

## How Skill Loading Works

Skills are discovered automatically from:
1. `~/.skill/skills/` — user-global skills
2. `<project>/.skill/skills/` — project-local skills
3. The bundled `skills/` directory (this repository)

Each skill has a `SKILL.md` with YAML frontmatter (`name`, `description`). On LLM server
startup, all skills are discovered and their names/descriptions are injected into the system
prompt as an `<available_skills>` XML block. When the user's task matches a skill's description,
the LLM uses the `read_file` tool to load the full instructions from the `SKILL.md` file.
