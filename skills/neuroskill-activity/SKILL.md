---
name: neuroskill-activity
description: NeuroSkill activity tracking — file interactions, terminal commands, focus sessions, meetings, clipboard, productivity scoring, weekly digest, brain awareness, and multi-monitor awareness. Tracks files, terminal commands (50+ categories), dev loops, zone switches, EEG-correlated struggle prediction, and task type detection. Use when asking about coding activity, file history, productivity, meetings, terminal usage, dev workflow, or brain state.
---

# NeuroSkill Activity Tracking

---

## LLM Tool Calls

When calling these commands via the LLM `skill` tool, use the `activity` command with a subcommand in `args`:

```json
{"command": "activity", "args": "summary"}
{"command": "activity", "args": "score"}
{"command": "activity", "args": "files"}
{"command": "activity", "args": "top-files"}
{"command": "activity", "args": "sessions"}
{"command": "activity", "args": "meetings"}
{"command": "activity", "args": "digest"}
{"command": "activity", "args": "terminal-commands"}
{"command": "brain", "args": "flow"}
{"command": "brain", "args": "stuck"}
{"command": "brain", "args": "terminal-impact"}
{"command": "brain", "args": "dev-loops"}
{"command": "terminal", "args": "status"}
{"command": "terminal", "args": "commands"}
{"command": "terminal", "args": "install zsh"}
```

---

## Available Subcommands

| Subcommand | Description |
|---|---|
| `summary` | Today's daily summary (files, edits, lines, projects, avg EEG focus) |
| `score` | Productivity score (0-100) with component breakdown |
| `files` | Recent file interactions (last 7 days) |
| `top-files` | Most-used files by interaction count |
| `top-projects` | Most-used projects |
| `languages` | Language breakdown by interaction count |
| `sessions` | Focus sessions (contiguous work blocks split by 5-min idle) |
| `meetings` | Detected meetings (Zoom, Teams, Slack, Google Meet, etc.) |
| `clipboard` | Recent clipboard events (macOS, metadata only) |
| `builds` | Terminal build/test outcomes |
| `heatmap` | Hourly edit distribution (0-23) |
| `switches` | Context switch rate (file switches per minute) |
| `digest` | Weekly 7-day activity digest |
| `stale` | Files edited but untouched for 7+ days |
| `bands` | Current EEG band powers |
| `window` | Currently active window |
| `input` | Last keyboard/mouse activity |
| `windows` | Recent window changes |
| `timeline` | Unified timeline of all events |
| `terminal-commands` | Recent terminal commands with exit codes and categories |

---

## CLI Examples

```bash
npx neuroskill activity summary              # today's stats
npx neuroskill activity score                # productivity score
npx neuroskill activity files                # recent file interactions
npx neuroskill activity top-files            # most-used files
npx neuroskill activity sessions             # focus sessions
npx neuroskill activity meetings             # detected meetings
npx neuroskill activity digest               # weekly summary
npx neuroskill activity stale                # abandoned files
npx neuroskill activity heatmap              # hourly activity
npx neuroskill activity timeline             # unified event timeline
npx neuroskill activity terminal-commands    # recent terminal commands
npx neuroskill activity --json               # raw JSON
```

## Terminal Integration

Track every terminal command across all shells and terminals — VS Code, iTerm, Terminal.app, tmux, ssh.

### CLI

```bash
npx neuroskill terminal                     # hook status for all shells
npx neuroskill terminal status              # installed/health per shell + command count
npx neuroskill terminal install zsh          # install zsh hook (also: bash, fish, powershell)
npx neuroskill terminal uninstall zsh        # remove hook from ~/.zshrc
npx neuroskill terminal commands             # recent commands (last hour) with exit codes
npx neuroskill terminal impact               # focus delta by command category (brain-fused)
npx neuroskill terminal loops                # edit-build-test dev loop detection
```

### Shell Hook Installation

The daemon serves hook scripts and installs them into shell rc files:

```bash
# Via CLI
npx neuroskill terminal install zsh
npx neuroskill terminal install bash
npx neuroskill terminal install fish
npx neuroskill terminal install powershell

# Via HTTP API
curl -X POST http://127.0.0.1:18444/v1/activity/install-shell-hook \
  -H "Content-Type: application/json" -d '{"shell":"zsh"}'

# Check status
curl -X POST http://127.0.0.1:18444/v1/activity/shell-hook-status \
  -H "Content-Type: application/json" -d '{"shell":"zsh"}'

# Get hook script content
curl http://127.0.0.1:18444/v1/activity/shell-hook?shell=zsh
```

Hooks are self-contained scripts stored in `~/.skill/shell-hooks/`. No path dependencies — works from DMG, NSIS, or tar app bundles.

### Supported Shells

| Shell | OS | Hook method |
|---|---|---|
| zsh | macOS, Linux | `add-zsh-hook preexec/precmd` |
| bash | macOS, Linux, Windows (Git Bash, WSL) | `trap DEBUG` + `PROMPT_COMMAND` |
| fish | macOS, Linux | `fish_preexec/fish_postexec` events |
| PowerShell | Windows, macOS, Linux | `prompt` override + `Get-History` |

### Command Categorization (50+ patterns)

| Category | Example commands |
|---|---|
| build | `cargo build`, `npm run build`, `make`, `tsc`, `go build` |
| test | `cargo test`, `npm test`, `pytest`, `jest`, `vitest` |
| run | `cargo run`, `npm start`, `node`, `python`, `go run` |
| git | `git commit`, `git push`, `git rebase`, `git stash` |
| docker | `docker`, `docker-compose`, `kubectl`, `helm` |
| deploy | `terraform`, `fly`, `vercel`, `netlify` |
| install | `npm install`, `pip install`, `brew`, `cargo add` |
| navigate | `cd`, `ls`, `find`, `grep`, `rg`, `fd` |
| debug | `gdb`, `lldb`, `node --inspect` |
| network | `ssh`, `curl`, `wget` |

## HTTP API (REST endpoints)

```bash
# Today's summary
curl -s -X POST http://127.0.0.1:8375/v1/activity/daily-summary \
  -H "Content-Type: application/json" \
  -d '{"dayStart": 1714003200}'

# Productivity score
curl -s -X POST http://127.0.0.1:8375/v1/activity/productivity-score \
  -H "Content-Type: application/json" \
  -d '{"day_start": 1714003200}'

# Files in time range (for session replay)
curl -s -X POST http://127.0.0.1:8375/v1/activity/files-in-range \
  -H "Content-Type: application/json" \
  -d '{"from_ts": 1714000000, "to_ts": 1714090000}'

# Weekly digest
curl -s -X POST http://127.0.0.1:8375/v1/activity/weekly-digest \
  -H "Content-Type: application/json" \
  -d '{"day_start": 1713398400}'

# Meetings in range
curl -s -X POST http://127.0.0.1:8375/v1/activity/meetings-in-range \
  -H "Content-Type: application/json" \
  -d '{"from_ts": 1713398400, "to_ts": 1714003200}'
```

---

## Brain Awareness Subcommands

| Subcommand | Description |
|---|---|
| `flow` | Real-time flow state (focus >70 for 10+ min, low switching) |
| `fatigue` | Fatigue detection (focus decline over continuous work) |
| `streak` | Deep work streak (consecutive days with 60+ min focus) |
| `task` | Current task type (coding, debugging, testing, infrastructure, deploying, git) |
| `stuck` | Struggle prediction (undo rate, velocity drop, focus decline, terminal failures) |
| `report` | Daily brain report (morning/afternoon/evening focus + productivity score) |
| `optimal` | Best/worst coding hours (7-day hourly EEG averages) |
| `breaks` | Natural focus cycle detection + suggested break interval |
| `load` | Cognitive load by language (low focus + high undos) |
| `load-files` | Cognitive load by file |
| `recovery` | Post-meeting focus recovery time |
| `interruptions` | Interruption recovery analysis |
| `correlation` | Code-EEG correlation (coding patterns vs brain state) |
| `terminal-impact` | How terminal commands affect EEG focus by category |
| `context-cost` | Focus level at editor/terminal/panel transitions |
| `dev-loops` | Edit-build-test cycle detection with pass rate and focus trend |

### CLI Examples

```bash
npx neuroskill brain flow              # current flow state
npx neuroskill brain stuck             # am I stuck?
npx neuroskill brain task              # what am I doing?
npx neuroskill brain terminal-impact   # how do terminal commands affect focus?
npx neuroskill brain context-cost      # cost of switching between editor/terminal/panel
npx neuroskill brain dev-loops         # detected edit-build-test cycles
npx neuroskill brain report            # today's brain report
npx neuroskill brain optimal           # best hours to code
```

---

## Data Collected

| Category | What | Storage |
|---|---|---|
| **File interactions** | File path, app, project, language, git branch, duration, lines added/removed, EEG focus/mood, undo count | `file_interactions` table |
| **Edit chunks** | 5-second granular edits within files (lines added/removed, undo estimate) | `file_edit_chunks` table |
| **Focus sessions** | Contiguous work blocks split by 5-min idle gaps | `focus_sessions` table |
| **Meetings** | Zoom, Teams, Slack, Google Meet, FaceTime, Discord, Webex detection | `meeting_events` table |
| **Clipboard** | Source app, content type, size (content never stored) | `clipboard_events` table |
| **Build events** | Terminal build/test commands and pass/fail outcomes | `build_events` table |
| **Terminal commands** | Command text, cwd, exit code, duration, category, EEG focus at start/end | `terminal_commands` table |
| **Dev loops** | Edit-build-test cycles with iteration count, pass/fail, focus trend | `dev_loops` table |
| **Zone switches** | Editor/terminal/panel transitions with EEG focus snapshot | `zone_switches` table |
| **Layout snapshots** | Periodic tab count, editor groups, terminals | `layout_snapshots` table |
| **Browser tabs** | Page title extracted from browser window titles | `browser_title` column |
| **Secondary monitors** | Windows visible on non-primary screens | `secondary_windows` table |

## Productivity Score Components

| Component | Weight | Description |
|---|---|---|
| Edit velocity | 0-25 | Lines changed per hour (cap: 200 lines/hr) |
| Deep work | 0-25 | Minutes in focus sessions >15 min (cap: 120 min) |
| Context stability | 0-25 | Low context-switch rate (0 switches = max) |
| EEG focus | 0-25 | Average EEG focus score during file work |

## Privacy

- Clipboard monitoring records **metadata only** (source app, type, size) — content is never stored
- File content is never stored — only paths, line counts, and content hashes for diff detection
- All data stored locally in `~/.skill/activity.sqlite`
- Configurable retention period (default: 90 days)
- Tracking can be independently toggled: active window, input activity, file tracking, clipboard
