---
name: neuroskill-index
description: Skill index and overview — lists all available NeuroSkill skills, tools, and explains how skill loading works.
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
| [neuroskill-labels](skills/neuroskill-labels/SKILL.md) | label/search context | `label`, `search-labels`, and `interactive` commands — creating EEG text annotations, semantic vector search over labels, and cross-modal 4-layer graph search combining text and EEG similarity. |
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
| [neuroskill-protocols](skills/neuroskill-protocols/SKILL.md) | protocol/exercise/routine intent detected | Full guided-protocol repertoire — 130+ mind-body practices with built-in personalisation engine for any age, culture, profession, ability, neurodivergence, and situation. Covers breathing, meditation, stress, sleep, somatic, emotions, music, eyes, morning/workout/hydration routines, dietary guidance, social-media addiction, non-breathing alternatives, and context-specific protocols for parents/caregivers, elders, teens/students, neurodivergent users, commuters, manual workers, healthcare/shift workers, intimate/relational, accessibility-adapted, culturally diverse practices, and situational micro-protocols. Loaded on-demand when the user asks for help, exercises, routines, or specific practices. |

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
