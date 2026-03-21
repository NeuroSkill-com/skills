---
name: neuroskill-hooks
description: NeuroSkill Proactive Hooks — real-time EEG pattern matching with CRUD management, threshold suggestions, scenario gating (cognitive/emotional/physical), WebSocket broadcast triggers, and audit logging. Use when creating, managing, or debugging brain-state hooks and automation pipelines.
---

# NeuroSkill Proactive Hooks

---

## LLM Tool Calls

When calling these commands via the LLM `skill` tool, use `command` + `args`:

```json
{"command": "hooks_status"}
{"command": "hooks_get"}
{"command": "hooks_set", "args": {"hooks": [...]}}
{"command": "hooks_suggest", "args": {"keywords": "focus,stress"}}
{"command": "hooks_log", "args": {"limit": 20}}
```

---

Proactive Hooks are a **real-time pattern matching system** that runs inside the
EEG embedding pipeline. Every 5 seconds, when a new EEG embedding epoch is
computed, the server checks all enabled hooks against the live brain state.

---

## Subcommands

| Subcommand | Description |
|---|---|
| `hooks` (or `hooks status`) | List hooks with scenario + last-trigger metadata |
| `hooks list` | List raw hook rules (name, keywords, threshold, enabled, …) |
| `hooks add <name> [opts]` | Add a new hook rule |
| `hooks remove <name>` | Delete a hook by name |
| `hooks enable <name>` | Enable a hook |
| `hooks disable <name>` | Disable a hook |
| `hooks update <name> [opts]` | Update fields on an existing hook |
| `hooks suggest "kw1,kw2"` | Suggest threshold from matching labels + recent EEG embeddings |
| `hooks log [--limit N --offset M]` | View paginated hook trigger audit log rows |

## Hook Mutation Flags

| Flag | Description |
|---|---|
| `--keywords <csv>` | Comma-separated keywords (e.g. `"focus,deep work,flow"`) |
| `--scenario <s>` | `any` \| `cognitive` \| `emotional` \| `physical` |
| `--command <cmd>` | Command to run on trigger |
| `--hook-text <txt>` | Payload text |
| `--threshold <f>` | Distance threshold (0.01–1.0) |
| `--recent <n>` | Recent-refs limit (10–20) |

---

## CLI Examples

```bash
# Status (default) — hooks with scenario + last trigger
npx neuroskill hooks
npx neuroskill hooks --json
npx neuroskill hooks --json | jq '.hooks[] | {name: .hook.name, scenario: .hook.scenario, last: .last_trigger.triggered_at_utc}'

# List raw hook rules
npx neuroskill hooks list
npx neuroskill hooks list --json

# Add a new hook
npx neuroskill hooks add "Deep Work Guard" --keywords "focus,deep work,flow" --scenario cognitive --threshold 0.14
npx neuroskill hooks add "Stress Alert" --keywords "stress,anxious,overwhelmed" --scenario emotional --threshold 0.12

# Update an existing hook
npx neuroskill hooks update "Deep Work Guard" --keywords "focus,flow" --threshold 0.12

# Enable / disable
npx neuroskill hooks enable "Deep Work Guard"
npx neuroskill hooks disable "Deep Work Guard"

# Remove
npx neuroskill hooks remove "Deep Work Guard"

# Suggest threshold from real EEG/label data
npx neuroskill hooks suggest "focus,deep work"
npx neuroskill hooks suggest "focus" --json | jq '.suggestion.suggested'

# View hook trigger audit log
npx neuroskill hooks log --limit 20 --offset 0
npx neuroskill hooks log --json | jq '.rows[] | {ts: .triggered_at_utc, hook: (.hook_json|fromjson).name, scenario: (.hook_json|fromjson).scenario}'
```

## HTTP / WebSocket API

```bash
# Status
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"hooks_status"}'

# List raw rules
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"hooks_get"}'

# Set hooks (sends the full array)
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"hooks_set","hooks":[...]}'

# Suggest threshold
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"hooks_suggest","keywords":["focus","deep work"]}'

# Audit log
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"hooks_log","limit":20,"offset":0}'
```

---

## How Proactive Hooks Work

```
EEG stream → 5 s epoch → embedding vector → cosine distance to hook references
                                                       ↓
                                              distance ≤ threshold?
                                                       ↓
                                              scenario gate passes?
                                                       ↓
                                       broadcast { event: "hook", payload: {...} }
                                              + audit log in hooks.sqlite
                                              + OS toast notification
```

### Step by Step

1. **Labels as reference patterns** — When you create labels (`npx neuroskill label "deep focus"`),
   the server embeds both the text and the surrounding EEG window. Each hook's
   `keywords` are matched against label texts to build a set of reference EEG
   embeddings (up to `recent_limit` most recent matches).

2. **Live comparison** — Every new 5-second EEG epoch is compared (cosine distance)
   against every enabled hook's reference embeddings. If the closest match is
   within the hook's `distance_threshold`, the hook fires.

3. **Scenario gating** — Before firing, the hook checks the current epoch's metrics
   against the scenario filter:
   - `any` — always passes
   - `cognitive` — requires elevated theta/beta ratio or cognitive load ≥ 55
   - `emotional` — requires stress index ≥ 55, mood ≤ 45, or relaxation ≤ 35
   - `physical` — requires drowsiness ≥ 55, headache/migraine index ≥ 45, or extreme HR

4. **Cooldown** — A hook cannot fire more than once every 10 seconds.

5. **Broadcast** — The server pushes `{ "event": "hook" }` over WebSocket to all
   connected clients.

6. **Audit log** — Every trigger is persisted to `hooks.sqlite` for review via `hooks log`.

---

## Hook Trigger Event Shape (WebSocket)

```jsonc
{
  "event": "hook",
  "payload": {
    "hook":             "Deep Work Guard",
    "scenario":         "cognitive",
    "context":          "labels",
    "distance":         0.0892,
    "label_id":         7,
    "label_text":       "focused reading session",
    "triggered_at_utc": 1740412830,
    "command":          "notify",
    "text":             "You're in deep focus!"
  }
}
```

> **Note:** Hook broadcast events are only available over WebSocket. HTTP transport
> has no push streaming.

---

## Automation Example — React to Hook Triggers

```bash
# Listen for 5 minutes and act on any hook triggers:
npx neuroskill listen --seconds 300 --json | jq -c '.[] | select(.event == "hook") | .payload' | while read -r payload; do
  HOOK=$(echo "$payload" | jq -r '.hook')
  DIST=$(echo "$payload" | jq -r '.distance')
  LABEL=$(echo "$payload" | jq -r '.label_text')
  echo "Hook triggered: $HOOK (dist=$DIST, label=$LABEL)"

  case "$HOOK" in
    "Deep Work Guard")
      npx neuroskill notify "Deep Focus Detected" "Distance: $DIST to '$LABEL'"
      ;;
    "Stress Alert")
      npx neuroskill say "Take a break. You seem stressed."
      ;;
  esac
done
```

## End-to-End Workflow

```bash
# 1. Record EEG while in a specific state and label it:
npx neuroskill label "deep focus coding"
npx neuroskill label "deep concentration"

# 2. Create a hook that fires when your brain returns to that state:
npx neuroskill hooks add "Focus Mode" \
  --keywords "focus,concentration,deep" \
  --scenario cognitive \
  --threshold 0.15

# 3. Verify the threshold makes sense:
npx neuroskill hooks suggest "focus,concentration,deep"

# 4. Listen and watch for triggers:
npx neuroskill listen --seconds 300

# 5. Check the audit log after the session:
npx neuroskill hooks log --limit 10
```

## Python Real-Time Hook Listener

```python
import asyncio, json
import websockets

async def listen_for_hooks(port: int):
    async with websockets.connect(f"ws://127.0.0.1:{port}") as ws:
        print("Listening for hook triggers...")
        async for raw in ws:
            msg = json.loads(raw)
            if msg.get("event") == "hook":
                p = msg["payload"]
                print(f"Hook: {p['hook']} fired! distance={p['distance']:.4f} label=\"{p['label_text']}\"")

asyncio.run(listen_for_hooks(8375))
```
