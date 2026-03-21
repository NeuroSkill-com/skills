---
name: neuroskill-labels
description: NeuroSkill `label`, `search-labels`, and `interactive` commands — creating EEG text annotations, semantic vector search over labels, and cross-modal 5-layer graph search combining text, EEG similarity, and screenshot discovery with 3D visualization. Use when annotating EEG moments or searching for past states by description.
---

# NeuroSkill Label Commands

---

## LLM Tool Calls

When calling these commands via the LLM `skill` tool, use `command` + `args`:

```json
{"command": "label", "args": {"text": "meditation start"}}
{"command": "label", "args": {"text": "breathwork", "context": "box breathing 4-4-4-4"}}
{"command": "search_labels", "args": {"query": "deep focus", "k": 10}}
{"command": "search_labels", "args": {"query": "stress", "mode": "context"}}
{"command": "interactive_search", "args": {"query": "flow state"}}
```

---

## `label` — Create a Timestamped Annotation

Create a timestamped text annotation on the current EEG moment.
Labels are stored in the database, shown in the dashboard, and searchable via `search-labels`.

```bash
npx neuroskill label "meditation start"
npx neuroskill label "eyes closed"
npx neuroskill label "feeling anxious"
npx neuroskill label "coffee just finished"
npx neuroskill label "task switch: coding → email"
npx neuroskill label "phone notification distracted me"
npx neuroskill label --json "focus block start"   # just print the label_id

# --context: attach a long-form body (searchable via search-labels --mode context or --mode both):
npx neuroskill label "breathwork" --context "box breathing 4-4-4-4, 10 min"
npx neuroskill label "meditation" --context "loving-kindness, 20 min, candle focus"

# --at: backdate the annotation to a specific Unix second:
npx neuroskill label "retrospective note" --at 1740412800
```

**HTTP:**
```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"label","text":"meditation start"}'

LABEL_ID=$(curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"label","text":"focus block start"}' | jq '.label_id')
echo "Created label #$LABEL_ID"
```

**Response:** `{ "command": "label", "ok": true, "label_id": 42 }`

---

## `search-labels` — Semantic Search Over Annotations

Semantic (vector) search across all your EEG annotations.
The query is embedded and compared against the label HNSW index.

```bash
npx neuroskill search-labels "deep focus"
npx neuroskill search-labels "relaxed meditation" --k 10
npx neuroskill search-labels "low energy" --mode context
npx neuroskill search-labels "flow state" --mode both --k 5
npx neuroskill search-labels "creative work" --json | jq '.results[].text'
npx neuroskill search-labels "morning routine" --json | jq '.results[] | {text, sim: .similarity}'
```

**Modes:**
- `text` (default) — searches the label short-text HNSW index
- `context` — searches the long-context HNSW (requires context fields to be set)
- `both` — runs both indexes, deduplicates by best cosine distance

**HTTP:**
```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"search_labels","query":"deep focus","k":10,"mode":"text"}'
```

### JSON Response

```jsonc
{
  "command": "search_labels",
  "ok": true,
  "query": "deep focus",
  "mode": "text",
  "model": "Xenova/bge-small-en-v1.5",
  "k": 10,
  "count": 3,
  "results": [
    {
      "label_id": 7,
      "text": "focused reading session",
      "context": "",
      "distance": 0.1204,
      "similarity": 0.8796,         // 1 − distance
      "eeg_start": 1740412800,
      "eeg_end": 1740413100,
      "created_at": 1740412810,
      "embedding_model": "bge-small-en-v1.5",
      "eeg_metrics": {
        "focus": 0.74,
        "relaxation": 0.38,
        "engagement": 0.62,
        "hr": 66.1,
        "mood": 0.58,
        "rel_alpha": 0.35,
        "rel_beta": 0.19
      }
    }
  ]
}
```

### Hidden Fields

| Hidden field | Contents |
|---|---|
| `results[].eeg_metrics` | Full EEG metrics for the label window — summary shows only 5 fields |
| `results[].context` | Long-context string — only a truncated preview in the summary |

```bash
npx neuroskill search-labels "deep focus" --json | jq '.results[0].eeg_metrics'
npx neuroskill search-labels "stress" --json | jq '[.results[].eeg_metrics.tbr]'
```

---

## `interactive` — Cross-Modal 5-Layer Graph Search

Combines semantic text search, EEG similarity search, temporal label proximity,
and screenshot discovery into a single directed graph:

```
"deep focus"  →  text_label nodes       (semantically similar annotations)
                      ↓
              eeg_point nodes           (raw EEG moments from label time windows)
                      ↓
              found_label nodes         (labels near those EEG moments in time)
                      ↓
              screenshot nodes          (screenshots near EEG timestamps, ranked by
                                         window-title / OCR-text proximity to query)
```

### Output Formats

| Flag | Output |
|---|---|
| _(none)_ | Colored human-readable summary |
| `--full` | Summary **+** colorized JSON |
| `--json` | Raw JSON: `{ query, nodes, edges, dot, svg, svg_col, svg_3d }` |
| `--dot` | Graphviz DOT source — pipe to `dot -Tsvg` or `dot -Tpng` |

```bash
npx neuroskill interactive "deep focus"
npx neuroskill interactive "meditation" --k-text 8 --k-eeg 8 --k-labels 5 --reach 15
npx neuroskill interactive "flow state" --json | jq '.nodes | length'
npx neuroskill interactive "focus" --json | jq '[.nodes[] | select(.kind == "text_label") | .text]'
npx neuroskill interactive "low focus" --json | jq '[.nodes[] | select(.kind == "eeg_point") | .timestamp_unix]'
npx neuroskill interactive "stress" --json | jq '[.nodes[] | select(.kind == "found_label") | .text]'

# Extract discovered screenshots:
npx neuroskill interactive "coding" --json | jq '[.nodes[] | select(.kind == "screenshot") | {file: .filename, app: .app_name, ocr_sim: .ocr_similarity}]'

# Render graph (requires graphviz):
npx neuroskill interactive "deep focus" --dot | dot -Tsvg > graph.svg
npx neuroskill interactive "meditation" --dot | dot -Tpng > graph.png
npx neuroskill interactive "focus" --json | jq -r '.dot' | dot -Tsvg > graph.svg

# Save the 3D perspective SVG:
npx neuroskill interactive "work" --json | jq -r '.svg_3d' > graph_3d.svg
```

### Pipeline Parameters

| Flag | Default | Range | Description |
|---|---|---|---|
| `--k-text <n>` | 5 | 1–20 | k for text-label HNSW search |
| `--k-eeg <n>` | 5 | 1–20 | k for EEG-similarity HNSW per text label |
| `--k-labels <n>` | 3 | 1–10 | k for label-proximity per EEG point |
| `--reach <n>` | 10 | 1–60 | Temporal window (minutes) around each EEG point |

**HTTP:**
```bash
curl -s -X POST http://127.0.0.1:8375/ \
  -H "Content-Type: application/json" \
  -d '{"command":"interactive_search","query":"deep focus","k_text":5,"k_eeg":5,"k_labels":3,"reach_minutes":10}'
```

### JSON Response Structure

```jsonc
{
  "command": "interactive_search",
  "ok": true,
  "query": "deep focus",
  "nodes": [
    { "id": "query",    "kind": "query",       "text": "deep focus",           "distance": 0.0    },
    { "id": "tl_0",    "kind": "text_label",   "text": "focused reading",      "distance": 0.1204 },
    { "id": "ep_...",  "kind": "eeg_point",    "text": null, "timestamp_unix": 1740413565, "distance": 0.0231 },
    { "id": "fl_42",   "kind": "found_label",  "text": "eyes closed",          "distance": 0.133  },
    {
      "id": "ss_20260224080530", "kind": "screenshot", "timestamp_unix": 1740413130,
      "distance": 0.05, "filename": "20260224/20260224080530.webp",
      "app_name": "VS Code", "window_title": "main.rs",
      "ocr_text": "fn dispatch(app: &AppHandle…", "ocr_similarity": 0.42,
      "proj_x": 0.31, "proj_y": -0.18, "proj_z": 0.72
    }
  ],
  "edges": [
    { "from_id": "query", "to_id": "tl_0",   "kind": "text_sim",   "distance": 0.1204 },
    { "from_id": "tl_0",  "to_id": "ep_...", "kind": "eeg_bridge", "distance": 0.0231 },
    { "from_id": "ep_...", "to_id": "fl_42", "kind": "label_prox", "distance": 0.133  },
    { "from_id": "ep_...", "to_id": "ss_20260224080530", "kind": "screenshot_prox", "distance": 2.0 },
    { "from_id": "ep_...", "to_id": "ss_20260224080530", "kind": "ocr_sim", "distance": 0.42 }
  ],
  "dot": "digraph interactive_search { ... }",
  "svg": "<svg ...>...</svg>",
  "svg_col": "<svg ...>...</svg>",
  "svg_3d": "<svg ...>...</svg>"
}
```

### Node Kinds

| Kind | Layer | Color | Description |
|---|---|---|---|
| `query` | 0 | violet | The embedded search keyword (always exactly 1) |
| `text_label` | 1 | blue | Annotations semantically similar to the query |
| `eeg_point` | 2 | amber | Raw EEG moments from label time windows |
| `found_label` | 3 | emerald | Annotations discovered near EEG moments in time |
| `screenshot` | 4 | pink | Screenshots near EEG timestamps, ranked by window-title / OCR proximity |

### Edge Kinds

| Kind | Connects | Distance meaning |
|---|---|---|
| `text_sim` | query → text_label | Cosine distance in text embedding space |
| `eeg_bridge` | text_label → eeg_point | Cosine distance in EEG embedding space |
| `eeg_sim` | eeg_point → eeg_point | Cosine distance (cross-edge) |
| `label_prox` | eeg_point → found_label | Temporal proximity (fraction of reach window) |
| `screenshot_prox` | eeg_point → screenshot | Temporal proximity (seconds between EEG epoch and screenshot) |
| `ocr_sim` | eeg_point → screenshot | Cosine similarity between query text and screenshot OCR text |

### Screenshot Node Fields

Screenshot nodes include additional fields not present on other node kinds:

| Field | Type | Description |
|---|---|---|
| `filename` | string | Relative path to the screenshot image file |
| `app_name` | string | Application name at capture time |
| `window_title` | string | Window title at capture time |
| `ocr_text` | string | OCR-extracted text from the screenshot |
| `ocr_similarity` | number | Cosine similarity (0–1) between query text and OCR text |

### 3-D Projection Fields

All nodes with text embeddings include PCA projection coordinates:

| Field | Type | Description |
|---|---|---|
| `proj_x` | number | PCA axis 1, normalised to [-1, 1] |
| `proj_y` | number | PCA axis 2, normalised to [-1, 1] |
| `proj_z` | number | PCA axis 3, normalised to [-1, 1] |

### SVG Outputs

The response includes three pre-rendered SVG visualizations:

| Field | Description |
|---|---|
| `svg` | PCA-scatter layout for found_labels |
| `svg_col` | Column-per-EEG-parent layout |
| `svg_3d` | 3-D perspective-projected dark-themed SVG with depth cues (scale, opacity, drop shadows) and grid floor — includes screenshot nodes |

> **Empty results:** If no labels have been embedded yet, only the query node is returned.
> Annotate moments with `label` first, then run `search-labels` to verify, then re-run `interactive`.
