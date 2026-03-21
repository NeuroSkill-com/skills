---
name: neuroskill-evidence
description: Implicit evidence collection, personal effectiveness tracking, and data-driven recommendation engine for NeuroSkill. Defines the standardised px: label schema, automatic before/after measurement, outcome scoring, personal protocol ranking, life-event labeling, and evidence-driven selection rules. Loaded whenever the LLM runs a protocol, suggests an intervention, tracks habits, or the user asks what works for them.
---

# NeuroSkill Evidence Collection

*(This skill defines how the LLM silently collects, aggregates, and uses evidence
about what actually helps each person. Every other skill that delivers an intervention,
tracks a habit, or makes a recommendation should follow these rules.)*

---

## Core Principle

**Every intervention is a natural experiment.** The EEG headband is already running.
The baseline already exists. The post-state will arrive automatically. All the LLM
needs to do is snapshot, label, and remember.

The user should NEVER be asked: "Can I label this?" or "Should I measure the outcome?"
Just do it. The labeling and measurement are invisible infrastructure. The only thing
the user sees are the insights that emerge over time.

**The goal is population-of-one science.** Not "does box breathing reduce stress in
general?" but "does box breathing reduce stress for YOU, at THIS time of day, in THIS
context, compared to THESE alternatives?"

---

## Who Uses This Skill

Every skill that does any of the following MUST follow the evidence collection rules:

| Skill | How it uses evidence |
|---|---|
| **neuroskill-protocols** | Label every protocol execution (start + end + deltas). Rank protocols by personal effectiveness. Select proven winners. |
| **neuroskill-hooks** | Label hook trigger events. Track whether the user acted on the hook and whether it helped. Tune thresholds from real outcomes. |
| **neuroskill-streaming** | Label calibration sessions. Track TTS-guided protocol outcomes. |
| **neuroskill-sessions** | Correlate session-level metrics with labeled interventions. Identify which sessions had protocols and which didn't. |
| **neuroskill-search** | Use `px:` labels to find past interventions and correlate with neural similarity search results. |
| **neuroskill-labels** | The evidence system IS the label system. All `px:` labels flow through `label` and `search_labels`. |
| **neuroskill-sleep** | Label evening routines. Correlate sleep architecture with pre-sleep interventions. |
| **neuroskill-dnd** | Label DND activations. Track whether protected focus periods produced better outcomes. |
| **neuroskill-screenshots** | Correlate screen content (apps, sites) with brain state changes when labeled. |
| **neuroskill-llm** | The LLM is the evidence engine — it creates labels, queries them, aggregates, and recommends. |
| **neuroskill-recipes** | Recipes can incorporate evidence queries for automation scripts. |

---

## Standardised Label Schema

### Prefix System

All evidence labels use the `px:` prefix (protocol/experience execution):

| Prefix | Meaning | Example |
|---|---|---|
| `px:start:<name>` | Intervention began | `px:start:box_breathing` |
| `px:end:<name>` | Intervention ended | `px:end:box_breathing` |
| `px:note:<name>` | Observation (no start/end pair) | `px:note:poor_sleep` |
| `px:skip:<name>` | Offered but declined | `px:skip:kapalabhati` |
| `px:auto:<name>` | Auto-triggered by hook | `px:auto:stress_alert` |

The `px:` prefix makes all evidence data instantly queryable:
```json
{"command": "search_labels", "args": {"query": "px:end", "k": 50}}
{"command": "search_labels", "args": {"query": "px:end:box_breathing", "k": 20}}
{"command": "search_labels", "args": {"query": "px:start", "k": 50, "mode": "context"}}
```

### Context Format

The `context` field uses pipe-separated `key=value` pairs for machine-parseable
structured data:

```
modality=breath | trigger=high_bar | bar=0.72 | stress_index=68 | relaxation=0.31 | focus=0.45 | hr=78 | mood=0.40 | faa=-0.03 | rmssd=26
```

### Required Fields

**Start label context — always include:**

| Field | Description | Example |
|---|---|---|
| `modality` | Intervention type | `breath`, `tactile`, `cognitive`, `visual`, `movement`, `auditory`, `passive_physio`, `music`, `dietary`, `relational`, `environmental` |
| `trigger` | EEG trigger that prompted this | `high_bar`, `low_focus`, `low_rmssd`, `high_drowsiness`, `low_mood`, `high_cognitive_load`, `user_request` |
| `bar` | Beta/Alpha Ratio at start | `0.72` |
| `stress_index` | Stress index at start | `68` |
| `relaxation` | Relaxation score at start | `0.31` |
| `focus` | Focus score at start | `0.45` |
| `hr` | Heart rate at start | `78` |
| `mood` | Mood score at start | `0.40` |
| `faa` | Frontal Alpha Asymmetry at start | `-0.03` |
| `rmssd` | HRV (RMSSD) at start | `26` |

**End label context — always include (in addition to the above at end-state):**

| Field | Description | Example |
|---|---|---|
| `duration_min` | Duration in minutes | `5` |
| `delta_bar` | Change in BAR | `-0.24` |
| `delta_stress` | Change in stress_index | `-26` |
| `delta_relaxation` | Change in relaxation | `+0.27` |
| `delta_focus` | Change in focus | `+0.07` |
| `delta_hr` | Change in heart rate | `-10` |
| `delta_mood` | Change in mood | `+0.15` |
| `delta_faa` | Change in FAA | `+0.04` |
| `delta_rmssd` | Change in RMSSD | `+12` |
| `outcome` | Overall result | `positive`, `neutral`, `negative` |

### Outcome Determination

Outcome is based on the **target metric** for the intervention's trigger:

| Trigger | Target metric | Positive if | Negative if |
|---|---|---|---|
| `high_bar` / stress | `bar`, `stress_index` | Dropped ≥ 10% or ≥ 0.05 abs | Rose ≥ 10% |
| `low_focus` | `focus` | Rose ≥ 10% or ≥ 0.05 abs | Dropped ≥ 10% |
| `low_relaxation` | `relaxation` | Rose ≥ 10% or ≥ 0.05 abs | Dropped ≥ 10% |
| `low_mood` | `mood`, `faa` | Rose ≥ 10% or ≥ 0.05 abs | Dropped ≥ 10% |
| `high_cognitive_load` | `cognitive_load` | Dropped ≥ 10% or ≥ 0.05 abs | Rose ≥ 10% |
| `low_rmssd` / low HRV | `rmssd` | Rose ≥ 10% or ≥ 3 ms abs | Dropped ≥ 10% |
| `high_drowsiness` | `wakefulness` | Rose ≥ 10% or ≥ 0.05 abs | Dropped ≥ 10% |
| `high_headache` | `headache_index` | Dropped ≥ 10% or ≥ 5 abs | Rose ≥ 10% |
| `user_request` | User's stated goal | Ask user or infer from context | — |
| All other | Neutral unless clear change | — | — |

If target metric change is between thresholds → `neutral`.

### Full Label Examples

**Starting a protocol:**
```json
{"command": "label", "args": {
  "text": "px:start:box_breathing",
  "context": "modality=breath | trigger=high_bar | bar=0.72 | stress_index=68 | relaxation=0.31 | focus=0.45 | hr=78 | mood=0.40 | faa=-0.03 | rmssd=26"
}}
```

**Ending a protocol (success):**
```json
{"command": "label", "args": {
  "text": "px:end:box_breathing",
  "context": "modality=breath | trigger=high_bar | duration_min=5 | bar=0.48 | stress_index=42 | relaxation=0.58 | focus=0.52 | hr=68 | mood=0.55 | faa=0.01 | rmssd=38 | delta_bar=-0.24 | delta_stress=-26 | delta_relaxation=+0.27 | delta_focus=+0.07 | delta_hr=-10 | delta_mood=+0.15 | delta_faa=+0.04 | delta_rmssd=+12 | outcome=positive"
}}
```

**Noting a life event:**
```json
{"command": "label", "args": {
  "text": "px:note:caffeine_intake",
  "context": "type=coffee | bar=0.45 | focus=0.52 | stress_index=38 | hr=72 | mood=0.55"
}}
```

**User declined a suggestion:**
```json
{"command": "label", "args": {
  "text": "px:skip:kapalabhati",
  "context": "reason=breath_averse | trigger=low_wakefulness | offered_alternative=cold_water_face"
}}
```

**Hook auto-triggered:**
```json
{"command": "label", "args": {
  "text": "px:auto:stress_alert",
  "context": "hook=Stress Auto-Protocol | distance=0.089 | bar=0.78 | stress_index=72 | suggested=ear_massage"
}}
```

---

## Measurement Flow

### For Formal Protocols (mandatory, every time)

```
1. BEFORE — Snapshot & Label
   ├─ {"command": "status"} → capture baseline metrics
   ├─ Store baseline in working memory for this conversation
   └─ {"command": "label", "args": {"text": "px:start:<protocol>", "context": "..."}}

2. DURING — Monitor (optional, per protocol)
   ├─ Poll status every 30–60s for live feedback if appropriate
   └─ Label notable moments: {"text": "px:note:alpha_spike_during_body_scan"}

3. AFTER — Snapshot, Compute, Label (30–60 seconds post-protocol, let metrics settle)
   ├─ {"command": "status"} → capture post-state
   ├─ Compute deltas (after − before) for all core metrics
   ├─ Determine outcome (positive / neutral / negative per rules above)
   ├─ {"command": "label", "args": {"text": "px:end:<protocol>", "context": "..."}}
   └─ Report to user (brief, natural language — see Surfacing Rules)

4. REMEMBER — Store in working memory
   └─ "This person did box_breathing. Outcome: positive. delta_bar: -0.24."
      Use this to inform future suggestions in this conversation.
```

### For Life Events (implicit, when mentioned)

When the user mentions any of the following, silently label it:

| User says / does | Label | Follow-up measurement |
|---|---|---|
| "I had coffee" / "drinking tea" | `px:note:caffeine_intake` | Check `focus`, `bar` at +30 min and +60 min if still in conversation. Label `px:note:caffeine_effect_30m` with deltas. |
| "Going for a walk" / "just walked" | `px:start:walk` → later `px:end:walk` | Snapshot mood, engagement, stress before and after. |
| "Just ate" / "having lunch" | `px:note:meal` | Check `drowsiness`, `focus` at +30 min. Label `px:note:post_meal_30m`. |
| "Bad sleep" / "slept poorly" | `px:note:poor_sleep` | Snapshot today's baseline. Correlate with `sleep` command data. |
| "Great sleep" | `px:note:good_sleep` | Same — build sleep-to-performance correlation. |
| "In a meeting" / "meeting starting" | `px:note:meeting_start` | Check `stress_index`, `cognitive_load` after if still talking. |
| "Meeting done" | `px:note:meeting_end` | Snapshot post-meeting state. |
| "Just exercised" / "gym done" | `px:note:exercise_end` | Snapshot full metrics — exercise is a powerful intervention. |
| "Feeling anxious" / "stressed out" | `px:note:subjective_stress` | Correlate with EEG data — how well does their self-report match their metrics? |
| "Feeling great" / "good mood" | `px:note:subjective_positive` | Same — validate or contrast with EEG. |
| App switch visible in `status` | `px:note:app_context` with `status → apps.top_24h` | Correlate screen content with brain state over time. |
| "Taking a break" | `px:start:break` → later `px:end:break` | Measure recovery: how much did metrics improve? |
| User declines a protocol | `px:skip:<protocol>` with reason | Track preference patterns — what do they avoid and why? |

**Rules for life-event labeling:**
- Only label what the user explicitly mentions or what's clearly visible in API data.
- NEVER infer ("you sound upset" → don't label unless they said it).
- Keep labels factual, not interpretive.
- Don't label every message — only intervention-like events.

### For Hook Triggers

When a hook fires (visible in `listen` events or `hooks_log`):

```json
{"command": "label", "args": {
  "text": "px:auto:stress_alert",
  "context": "hook=Stress Auto-Protocol | distance=0.089 | bar=0.78 | stress_index=72 | suggested=ear_massage | user_acted=pending"
}}
```

If the user follows up with a protocol → label normally and link:
```json
{"command": "label", "args": {
  "text": "px:start:ear_massage",
  "context": "modality=tactile | trigger=hook_auto | hook=Stress Auto-Protocol | ..."
}}
```

If the user ignores the hook → update later:
```json
{"command": "label", "args": {
  "text": "px:note:hook_ignored",
  "context": "hook=Stress Auto-Protocol | bar=0.78 | reason=user_busy"
}}
```

This tracks hook effectiveness: how often does the user act on auto-triggers,
and do they help when they do?

---

## Evidence Aggregation & Personal Ranking

### When to Aggregate

| Data state | Action |
|---|---|
| **0 protocol executions** | No aggregation possible. Just run protocols and collect data. |
| **1 execution** | Report immediate before/after delta. "Let's see how this works for you." |
| **2–4 executions** of same protocol | Pattern recognition: "This is the 3rd time — average stress drop of 0.18 each time." |
| **5+ executions** of same protocol | Compute personal success rate and average delta. Use in recommendations. |
| **10+ total executions** across multiple protocols | Build cross-protocol ranking. Identify modality preferences. |
| **20+ total executions** | Full personal effectiveness profile with time-of-day, trigger-specific, and modality insights. |

### How to Aggregate

```
1. Query all protocol outcomes:
   {"command": "search_labels", "args": {"query": "px:end", "k": 100, "mode": "context"}}

2. Parse context fields from each result.

3. Group by protocol name (extracted from label text after "px:end:").

4. For each protocol, compute:
   - n_executions: count of px:end labels
   - success_rate: % with outcome=positive
   - avg_delta_target: mean delta of the trigger's target metric
   - avg_delta_mood: mean mood change (secondary wellbeing signal)
   - avg_duration: mean duration_min
   - best_time: most common hour-of-day (from label created_at) for positive outcomes
   - worst_time: most common hour for neutral/negative outcomes
   - best_trigger: trigger value most associated with positive outcomes
   - modality: extract from context

5. Rank by: success_rate × |avg_delta_target|
   (higher success rate AND larger effect size = better rank)

6. Group by modality to find modality preferences:
   - "Tactile interventions: 83% average success rate"
   - "Breath interventions: 68% average success rate"
   - → This person responds better to tactile
```

### Personal Protocol Ranking Output

When the user asks "what works for me?" or after enough data accumulates:

```
Based on your 47 protocol sessions over 3 weeks:

YOUR TOP 5
1. Cold water face splash — 92% success (12/13), avg stress drop: 31%
   Best: mornings. Trigger: acute stress.
2. Ear massage — 85% success (6/7), avg stress drop: 28%
   Best: any time. Trigger: moderate stress.
3. Box breathing — 78% success (7/9), avg stress drop: 22%
   Best: mornings (89% before noon, 62% after). Trigger: high bar.
4. Bilateral tapping — 75% success (3/4), avg stress drop: 19%
   Best: evenings. Trigger: emotional charge.
5. Micro-tasking drill — 71% success (5/7), avg focus lift: 24%
   Best: post-lunch. Trigger: low focus.

MODALITY PREFERENCE
  Passive physiological: 88% avg success ← your sweet spot
  Tactile: 82% avg success
  Cognitive: 71% avg success
  Breath: 68% avg success

LEAST EFFECTIVE FOR YOU
  Peripheral vision expansion — 40% success (2/5). Consider retiring.
  Humming — 50% success (3/6). Works evenings only.

LIFE PATTERNS
  Coffee at 9 AM → focus peaks at 9:45 (+18% avg)
  Coffee after 2 PM → sleep N3% drops 15% that night
  Post-lunch walk → drowsiness 34% lower than no-walk days
  Social media > 20 min → focus crashes avg 0.22 for 45 min after
```

---

## Evidence-Driven Selection Rules

These rules apply to ALL skills that suggest interventions:

### 1. Check Before You Suggest

Before recommending any protocol or intervention, search for past evidence:

```json
{"command": "search_labels", "args": {"query": "px:end:<candidate_protocol>", "k": 20}}
```

If results exist, compute success rate. If < 50% success for this person after 4+ tries,
do NOT suggest it as the primary option.

### 2. Lead with Proven Winners

If personal data shows cold water has 92% success for stress and box breathing has 68%,
suggest cold water first:

> "Your stress is spiking. Cold water on your wrists has worked really well for you —
> 92% of the time it drops your stress by about 30%. Want to do that, or try something
> different today?"

### 3. Retire Consistent Failures

If a protocol has outcome=negative or outcome=neutral 4+ consecutive times:

- Stop suggesting it proactively.
- If the user specifically asks for it, note the history:
  "We've tried that 5 times and it hasn't shifted your numbers. Happy to try again,
  but I also have alternatives that work better for you."

### 4. Explore Occasionally

Even with strong data, occasionally suggest something new (every 5th–8th intervention):

> "Your go-to for stress is cold water — and it works great. But today, want to try
> bilateral tapping? It targets the same pathway differently and might give us new data."

Mark exploratory suggestions: `px:note:exploration | protocol=bilateral_tapping | reason=modality_diversity`

### 5. Track Modality Preferences

Aggregate success rates by modality. If tactile interventions average 85% success and
breath averages 65%, bias toward tactile for new triggers too — even ones where the
person hasn't tried tactile specifically.

### 6. Time-of-Day Patterns

Parse `created_at` timestamps from `px:end` labels. Group by hour. Report:

> "Box breathing works best for you in the morning — 89% success before noon,
> but only 62% in the afternoon. In the afternoon, cold water works better (90%)."

### 7. Trigger-Specific Effectiveness

The same protocol may work for one trigger but not another. Track this:

> "Ear massage is great for your stress (85% success) but doesn't help much
> for your focus dips (45% success). For focus, try micro-tasking instead."

### 8. Correlate Life Events with Outcomes

Use `px:note:` labels to build non-protocol insights:

> "Days when you walk after lunch: average afternoon focus 0.61.
> Days without a walk: average afternoon focus 0.44.
> That walk is worth +38% focus."

> "Your sleep is 18% better (more N3) on days you do the wind-down protocol
> vs days you skip it."

### 9. Cross-Protocol Compound Effects

Some interventions work better in combination. Track sequences:

> "When you do morning clarity ritual + study focus sprint, your average focus
> is 0.72. When you skip the morning ritual, the same sprint averages 0.58."

### 10. Respect the Learning Curve

Some protocols improve with practice. Track change over time:

> "Your first 3 box breathing sessions averaged 15% stress reduction.
> Your last 3 averaged 28%. You're getting better at this."

---

## Surfacing Evidence to the User

### When to Show Data (proactively)

| Trigger | What to surface |
|---|---|
| **After a protocol** (always) | Brief delta: "Stress dropped from 68 to 42." |
| **After 3+ sessions of same protocol** | Pattern: "This is working — 3 for 3." |
| **After 5+ sessions** | Success rate: "78% success rate with this one." |
| **When choosing between protocols** | Compare: "Option A works 85% for you, option B 65%." |
| **First time in a while** | Remind: "Last time you did this was 2 weeks ago — it worked well then (stress −31%)." |
| **End of week / milestone** | Summary: "This week: 8 protocols, 6 positive outcomes. Top performer: ear massage." |
| **When asked "what works?"** | Full personal ranking (see above). |
| **When declining a suggestion** | Offer evidence for the alternative: "No problem. Cold water has a 92% track record for you — want to try that?" |

### When NOT to Show Data

| Situation | Why not |
|---|---|
| **During an active protocol** | Don't interrupt the experience with analytics. Sparse encouraging feedback only. |
| **First time doing anything** | Don't say "no data yet" — just do it and collect. |
| **Acute crisis** | Do the intervention. Analytics after. |
| **User is overwhelmed** | Simpler is better. One number, not a table. |
| **Negative outcome just happened** | Don't say "this one usually fails for you." Frame constructively: "That one didn't land — let's try something that usually works better." |

### Tone of Evidence Delivery

**Do:**
- "Based on your data…" (personal, specific)
- "In your experience…" (respects their autonomy)
- "Your numbers show…" (factual, not judgmental)
- "Over your last 5 sessions…" (temporal, concrete)

**Don't:**
- "Studies show…" (generic, not personal)
- "You should…" (prescriptive)
- "You failed at…" (blaming)
- "Your data proves…" (overstating — small sample sizes are suggestive, not proof)

---

## Privacy & Transparency

- **Never hide that evidence is collected.** If the user asks "what are you tracking?"
  explain fully and immediately: "I label the start and end of every protocol we do,
  along with your EEG metrics at that moment, so I can learn what actually works for
  you over time. All the data is local on your machine."
- **The user owns all data.** Labels are in their local database, fully searchable
  via `search-labels`, deletable at any time.
- **Don't over-label.** Only label things the user explicitly mentions or that are
  clearly visible in API data (app switches, session boundaries). Never infer private
  states: "You seem angry" → don't label unless they said it.
- **Aggregate, don't surveil.** The purpose is population-of-one effectiveness
  research, not behaviour monitoring. Frame it that way if asked.
- **Data never leaves the device.** All labels and metrics are stored locally.
  No cloud aggregation, no sharing with anyone.
- **Deletable at any time.** The user can delete any label, any time, via the
  dashboard or `search-labels` → delete.

---

## Quick Reference for Other Skills

### Minimum Viable Evidence Collection (copy-paste pattern)

For any skill that delivers an intervention, this is the minimum:

```
BEFORE:
  status → snapshot baseline
  label "px:start:<intervention_name>" with context metrics

AFTER:
  status → snapshot post-state
  compute deltas
  determine outcome (positive / neutral / negative)
  label "px:end:<intervention_name>" with context metrics + deltas + outcome
```

### Querying Past Evidence

```json
// All protocol outcomes
{"command": "search_labels", "args": {"query": "px:end", "k": 100}}

// Specific protocol outcomes
{"command": "search_labels", "args": {"query": "px:end:ear_massage", "k": 20}}

// All protocol starts (to count attempts vs completions)
{"command": "search_labels", "args": {"query": "px:start", "k": 100}}

// All skipped/declined protocols
{"command": "search_labels", "args": {"query": "px:skip", "k": 50}}

// All life event notes
{"command": "search_labels", "args": {"query": "px:note", "k": 100}}

// All hook auto-triggers
{"command": "search_labels", "args": {"query": "px:auto", "k": 50}}

// Context search (find by metric values)
{"command": "search_labels", "args": {"query": "outcome=positive", "k": 100, "mode": "context"}}
```

### Protocol Name Reference

Use these consistent snake_case names in `px:` labels:

| Category | Names |
|---|---|
| **Breath** | `box_breathing`, `extended_exhale`, `cardiac_coherence`, `physiological_sigh`, `kapalabhati`, `four_count_energising`, `wim_hof`, `nadi_shodhana`, `buteyko`, `temazcal_breath`, `bhramari` |
| **Tactile** | `ear_massage`, `bilateral_tapping`, `pressure_point_li4`, `temperature_contrast`, `texture_scanning`, `structured_fidget`, `palm_press`, `hand_warming`, `havening_touch` |
| **Cognitive** | `micro_tasking_drill`, `mental_arithmetic`, `internal_narration_silence`, `three_word_capture`, `micro_gratitude`, `mental_time_travel`, `worry_parking`, `novel_stimulation`, `category_switching`, `woop`, `cognitive_defragging`, `n_back` |
| **Visual** | `peripheral_vision`, `candle_gaze`, `colour_hunting`, `slow_tracking`, `panoramic_vision`, `near_far_focus`, `saccade_reset`, `twenty_twenty_twenty` |
| **Movement** | `isometric_squeeze`, `jaw_release`, `micro_walk`, `spinal_wave`, `wrist_ankle_circles`, `shoulder_blade_pinch`, `floor_press`, `progressive_muscle_relaxation`, `somatic_shaking`, `desk_yoga`, `neck_release` |
| **Auditory** | `sound_mapping`, `humming`, `toning`, `sound_bath`, `whisper_reading`, `rhythmic_tapping`, `singing`, `active_listening` |
| **Passive Physio** | `cold_water_face`, `cold_water_wrists`, `deliberate_yawning`, `chewing`, `gargling`, `tongue_press`, `gravity_drop` |
| **Meditation** | `alpha_induction`, `open_monitoring`, `alpha_theta_drift`, `mantra_focus`, `gamma_entrainment`, `loving_kindness`, `nsdr_yoga_nidra`, `body_scan`, `grounding_54321` |
| **Music** | `mood_match_lift`, `focus_music`, `energising_playlist`, `stress_discharge_playlist`, `sleep_music`, `binaural_beats`, `music_breath_sync`, `emotional_release_music` |
| **Relational** | `co_regulation`, `conflict_pause`, `gratitude_expression`, `touch_co_regulation`, `post_argument_repair` |
| **Cultural** | `wuqinxi`, `shinrin_yoku`, `hooponopono`, `ubuntu_reflection`, `dadirri`, `tai_chi_micro`, `dhikr`, `centering_prayer`, `mala_meditation` |
| **Life events** | `caffeine_intake`, `meal`, `walk`, `exercise`, `break`, `meeting_start`, `meeting_end`, `poor_sleep`, `good_sleep`, `app_switch`, `subjective_stress`, `subjective_positive` |
| **System** | `hook_ignored`, `hook_acted`, `exploration`, `day_end_summary` |
