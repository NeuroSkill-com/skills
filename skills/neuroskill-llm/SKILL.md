---
name: neuroskill-llm
description: NeuroSkill built-in LLM inference server — on-device OpenAI-compatible chat with llama.cpp, model catalog management (add/download/select/delete GGUF models), vision support (mmproj), streaming WebSocket and HTTP chat, automatic tool calling (bash, read, write, edit, web_search, web_fetch), GenParams tuning, and persistent chat history. Use when managing local LLM models, chatting, or integrating with the OpenAI-compatible API.
---

# NeuroSkill LLM — On-Device Inference Server

Control the built-in on-device LLM inference server (OpenAI-compatible, powered by llama.cpp).
All subcommands route to the WebSocket/HTTP API.

---

## Subcommands

| Subcommand | Description |
|---|---|
| `llm status` | Server state (stopped/loading/running), model name, context window |
| `llm start` | Load the active model and start the inference server |
| `llm stop` | Stop the server and free GPU/CPU memory |
| `llm catalog` | List all GGUF models with download states and active selections |
| `llm add <repo> <filename>` | Add an external HF model to the catalog and download it |
| `llm add <hf-url>` | Add from a full HuggingFace URL |
| `llm add ... --mmproj <file>` | Also add and download a vision projector from the same repo |
| `llm select <filename>` | Set the active text model |
| `llm mmproj <filename\|none>` | Set the active vision projector (or `none` to disable) |
| `llm autoload-mmproj <on\|off>` | Toggle auto-loading of vision projector on start |
| `llm download <filename>` | Download a model (fire-and-forget; poll catalog for progress) |
| `llm pause <filename>` | Pause an in-progress download |
| `llm resume <filename>` | Resume a paused download |
| `llm cancel <filename>` | Cancel an in-progress download |
| `llm delete <filename>` | Delete a locally-cached model file |
| `llm downloads` | List all downloads with status and progress |
| `llm refresh` | Re-probe the HF Hub cache for externally downloaded models |
| `llm fit` | Check which models fit in available RAM/VRAM |
| `llm logs` | Print the last 500 LLM server log lines |
| `llm chat` | **Interactive multi-turn REPL** — type `exit` to quit (**WebSocket only**) |
| `llm chat "message"` | Single-shot: send one message, stream the reply, and exit |

---

## CLI Examples

```bash
# Server lifecycle
npx neuroskill llm status
npx neuroskill llm start
npx neuroskill llm stop

# Model management
npx neuroskill llm catalog
npx neuroskill llm catalog --json | jq '.entries[] | select(.state == "downloaded")'
npx neuroskill llm add bartowski/Phi-4-mini-reasoning-GGUF Phi-4-mini-reasoning-Q4_K_M.gguf
npx neuroskill llm add bartowski/Phi-4-mini-reasoning-GGUF Phi-4-mini-reasoning-Q4_K_M.gguf --mmproj mmproj-Phi-4-mini-reasoning-BF16.gguf
npx neuroskill llm select "Qwen_Qwen3.5-4B-Q4_K_M.gguf"
npx neuroskill llm mmproj "mmproj-Qwen_Qwen3.5-4B-BF16.gguf"
npx neuroskill llm mmproj none
npx neuroskill llm download "Qwen3-1.7B-Q4_K_M.gguf"
npx neuroskill llm delete "Qwen3-1.7B-Q4_K_M.gguf"
npx neuroskill llm fit

# Interactive chat REPL
npx neuroskill llm chat
npx neuroskill llm chat --system "You are a concise EEG assistant."
npx neuroskill llm chat --temperature 0.3 --max-tokens 512

# Single-shot chat
npx neuroskill llm chat "What EEG frequency bands are associated with meditation?"
npx neuroskill llm chat "Explain delta waves" --temperature 0.3 --max-tokens 256

# Image input (requires vision model + mmproj)
npx neuroskill llm chat "What do you see?" --image screenshot.png
npx neuroskill llm chat "Compare these" --image session1.png --image session2.png
```

### Interactive REPL Commands

| Command | Effect |
|---|---|
| `/clear` | Clear conversation history (system prompt is kept) |
| `/history` | Print all messages in the current conversation |
| `/help` | Show REPL command help |
| `exit` or `quit` | End the session |

---

## WebSocket Streaming Protocol — `llm_chat`

`llm_chat` returns **multiple frames** per request. Tokens stream as `delta` frames;
generation ends with `done` or `error`.

```js
ws.send(JSON.stringify({
  command:  "llm_chat",
  messages: [
    { role: "system", content: "You are a concise EEG assistant." },
    { role: "user",   content: "What does high theta power indicate?" },
  ],
  // Optional: session_id: 42  (continue existing conversation)
}));

// Short-hand for single user message:
ws.send(JSON.stringify({ command: "llm_chat", message: "Hello!" }));
```

### Server Frames

```jsonc
// Session frame (first):
{ "command": "llm_chat", "type": "session", "session_id": 42 }

// Delta frames (one per token batch):
{ "command": "llm_chat", "type": "delta", "text": "High theta" }

// Final done frame:
{
  "command": "llm_chat", "ok": true, "type": "done",
  "finish_reason": "stop", "prompt_tokens": 42,
  "completion_tokens": 87, "n_ctx": 4096, "session_id": 42
}
```

---

## HTTP REST API

```bash
# Status / start / stop
curl -s http://127.0.0.1:8375/llm/status | jq '{status, model_name, n_ctx}'
curl -s -X POST http://127.0.0.1:8375/llm/start
curl -s -X POST http://127.0.0.1:8375/llm/stop

# Catalog
curl -s http://127.0.0.1:8375/llm/catalog | jq '.entries[] | select(.state == "downloaded") | .filename'

# Non-streaming chat
curl -s -X POST http://127.0.0.1:8375/llm/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"What is EEG coherence?"}' | jq '{text, finish_reason, session_id}'

# Continue existing session
curl -s -X POST http://127.0.0.1:8375/llm/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Can you elaborate?","session_id":42}'

# With system prompt and GenParams
curl -s -X POST http://127.0.0.1:8375/llm/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Summarize in one line.","system":"Be extremely brief.","temperature":0.3}'

# Image input (base64 data-URL)
curl -s -X POST http://127.0.0.1:8375/llm/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"What do you see?","images":["data:image/png;base64,iVBORw0KGgo..."]}'

# Logs
curl -s http://127.0.0.1:8375/llm/logs | jq '.logs[-10:]'
```

---

## WebSocket Commands

| Command | Required | Optional | Description |
|---|---|---|---|
| `llm_status` | — | — | Server state + model info |
| `llm_start` | — | — | Start inference server |
| `llm_stop` | — | — | Stop server + free resources |
| `llm_catalog` | — | — | Full model catalog |
| `llm_add_model` | `repo`, `filename` | `download`, `mmproj` | Add external HF model |
| `llm_select_model` | `filename` | — | Set active text model |
| `llm_select_mmproj` | `filename` | — | Set active vision projector |
| `llm_download` | `filename` | — | Start model download |
| `llm_pause_download` | `filename` | — | Pause download |
| `llm_resume_download` | `filename` | — | Resume download |
| `llm_cancel_download` | `filename` | — | Cancel download |
| `llm_delete` | `filename` | — | Delete cached model |
| `llm_downloads` | — | — | List downloads with progress |
| `llm_refresh_catalog` | — | — | Re-probe HF cache |
| `llm_hardware_fit` | — | — | Check model fit in RAM/VRAM |
| `llm_logs` | — | — | Last 500 log entries |
| `llm_chat` | `messages` or `message` | GenParams | Streaming chat |

## GenParams Reference

| Field | Type | Default | Description |
|---|---|---|---|
| `temperature` | float | 0.8 | Sampling temperature (0 = deterministic) |
| `top_k` | int | 40 | Top-K sampling |
| `top_p` | float | 0.9 | Nucleus sampling threshold |
| `repeat_penalty` | float | 1.1 | Repetition penalty |
| `seed` | uint | 0xDEADBEEF | RNG seed for reproducible output |
| `max_tokens` | uint | 2048 | Maximum tokens to generate |
| `thinking_budget` | uint/null | 512 | Max tokens in `<think>` block (0 = skip, null = unlimited) |

---

## Built-in Tool Calling

When the LLM server is running, `llm_chat` supports **automatic tool calling** — the
model can invoke built-in tools and return results within the conversation.

| Tool | Description |
|---|---|
| `date` | Get current date/time metadata |
| `location` | Approximate public-IP geolocation |
| `bash` | Execute shell commands |
| `read_file` | Read file contents from disk |
| `write_file` | Write / create files |
| `edit_file` | Surgical find-and-replace edits |
| `search_output` | Navigate large tool outputs (paginated) |
| `web_search` | Search the web |
| `web_fetch` | Fetch a URL and return its content |
| `skill` | Query the NeuroSkill EEG API (status, sessions, labels, search, hooks, etc.) |

Tool calls are detected in the model's output, executed server-side, and results are
fed back automatically. The CLI `llm chat` handles this transparently.

---

## Chat Persistence

All conversations are **automatically persisted** to SQLite
(`~/.skill/chats/chat_history.sqlite`):

- WebSocket / HTTP chats appear in the Chat window sidebar.
- The server returns a `session_id` for multi-turn persistence across connections.
- New sessions are auto-titled from the first user message.
- Tool calls executed during conversation are also persisted.

---

## Server Status Values

| `status` | Meaning |
|---|---|
| `"stopped"` | No model loaded |
| `"loading"` | Model being loaded from disk |
| `"running"` | Model ready; chat and `/v1/*` endpoints are live |

## Download State Values

| `state` | Meaning |
|---|---|
| `"not_downloaded"` | Not present locally |
| `"downloading"` | In progress; check `progress` (0.0–1.0) |
| `"downloaded"` | Cached locally; ready to use |
| `"cancelled"` | Download was cancelled |
| `"failed"` | Download failed; `status_msg` has details |

> The LLM server also exposes an OpenAI-compatible HTTP API at `/v1/chat/completions`,
> `/v1/completions`, and `/v1/embeddings`. Use any OpenAI client library by pointing
> it at `http://127.0.0.1:<port>`.
