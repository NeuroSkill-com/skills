---
name: neuroskill-protocols-mindfulness
description: "Mindfulness protocols — present-moment awareness, informal mindfulness, mindful eating, mindful listening, RAIN, STOP, noting practice, and EEG-tracked mindfulness with real-time neural feedback."
---

# Mindfulness Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## FORMAL MINDFULNESS PRACTICES

**Noting Practice** → high mental chatter, rumination, low `focus`, inability to disengage from thoughts
  Label each experience as it arises: "thinking", "hearing", "feeling", "planning", "remembering". The label creates distance from the content.
  ◈ *Beginner:* Use broad categories only (thinking / feeling / sensing).
  ◈ *Advanced:* Add granularity — "judging", "anticipating", "resisting", "wanting".
  ◈ *Child:* "Name what your brain is doing like a nature narrator — 'now it's worrying, now it's daydreaming!'"
  ◈ *Non-verbal variant:* Instead of labeling, gently nod internally at each experience — acknowledge without words.
  🔧 *API:* Before: `status` → snapshot `focus`, `bar`, `lzc`, `label "protocol start: noting" --context "focus=X, bar=X, lzc=X"`. During: poll `status` every 60s — `focus` should stabilise, `lzc` should rise as meta-awareness develops. After: `label` with deltas. Longitudinal: `compare` noting sessions vs breath-counting — noting often outperforms for rumination-prone users.

**Mindful Breathing (Awareness, Not Control)** → stress, autopilot mode, need to ground, moderate `bar`
  Observe the breath exactly as it is — do not regulate, count, or shape it. Notice temperature at nostrils, rise and fall of chest/belly, pauses between breaths.
  ◈ *Key distinction from breathwork:* Zero effort to change the breath. If it's shallow, let it be shallow.
  ◈ *Anchor point options:* Nostrils (sharpest sensation), belly (easiest for beginners), whole-body breathing (advanced).
  ◈ *Breath-averse:* Observe heartbeat or ambient sounds instead — same principle of non-reactive observation.
  🔧 *API:* Track `relaxation`, `rel_alpha`, `bar`. Unlike breathwork protocols, expect gradual rather than rapid shifts — the mechanism is attentional, not physiological. `label` with anchor point used.

**Sitting Mindfulness (Full Spectrum)** → daily practice, building equanimity, moderate-to-advanced practitioners
  Structured sitting in layers: breath (5 min) → body sensations (5 min) → sounds (5 min) → thoughts and emotions (5 min) → choiceless awareness (5 min).
  ◈ *Shorter version:* Pick 2–3 layers, 3 min each.
  ◈ *Difficulty with thoughts layer:* Treat thoughts as sounds — they arise, have a texture, and fade.
  🔧 *API:* `dnd_set enabled:true`. `say` layer transition cues. Track metric shifts across layers — `focus` typically peaks in breath layer, `lzc` peaks in choiceless layer. `label` each transition for granular session analysis.

---

## INFORMAL MINDFULNESS (DAILY LIFE)

**Mindful Eating** → rushed meals, stress eating, cravings, disconnection from hunger/satiety
  One meal or snack with full attention: observe colour, texture, smell before tasting. Chew slowly (20+ chews). Notice flavour changes, swallowing impulse, satiety signals.
  ◈ *Minimum viable:* First three bites of any meal, fully attended.
  ◈ *Social meals:* Alternate — one mindful bite, then conversation, repeat.
  ◈ *Child:* "Be a food detective — what's the secret flavour hiding in this bite?"
  🔧 *API:* `label "mindful eating start"` and `label "mindful eating end"` to bracket. Compare `bar` and `relaxation` during mindful vs rushed meals over time.

**Mindful Listening** → interpersonal stress, poor attention in conversations, high `bar` during meetings
  Listen to another person (or ambient sounds) without planning a response, judging, or drifting. Just receive.
  ◈ *Conversation:* Let 1 full second of silence pass after they finish before responding.
  ◈ *Sound-based solo:* Sit and identify all distinct sounds in your environment, one at a time.
  ◈ *Music variant:* Listen to one song as if hearing it for the first time — track individual instruments.
  🔧 *API:* Track `focus`, `bar`. `label` with context (conversation/ambient/music).

**Mindful Walking** → autopilot commuting, restlessness, afternoon energy dip
  Walk at normal pace but place attention on: feet contacting ground, leg muscles, air on skin, peripheral vision.
  ◈ *Distinction from walking meditation:* Normal speed, normal context (commute, hallway). Not a separate practice — a way of being while walking.
  ◈ *Micro-version:* Be fully present for just 10 steps. Then allow autopilot. Repeat throughout the day.
  🔧 *API:* `label "mindful walking" --context "location, duration"`. Compare `focus` snapshots during vs after.

**Mindful Transitions** → context-switching fatigue, high `bar` between tasks, scattered attention
  At every natural transition (finishing an email, leaving a room, starting a meal), pause for 3 breaths and arrive in the new moment.
  ◈ *Digital:* Before opening a new app or tab, pause and ask "what am I looking for?"
  ◈ *Doorway practice:* Every time you walk through a doorway, feel your feet for one step.
  🔧 *API:* Set up a `hook` — when `bar` spikes after sustained `focus` drops (context switch detected), `say "Pause — take three breaths before the next thing"`. `label` transitions for pattern analysis.

---

## STRUCTURED MINDFULNESS FRAMEWORKS

**STOP Practice** → acute reactivity, emotional hijack, impulse to act, sudden `bar` spike
  **S**top what you're doing. **T**ake one breath. **O**bserve (body, thoughts, emotions). **P**roceed with awareness.
  Total time: 15–30 seconds.
  ◈ *Before emails:* STOP before hitting send on any emotionally charged message.
  ◈ *Conflict:* STOP before responding to provocation.
  ◈ *Child:* "Freeze like a statue, take a dragon breath, look around, then go."
  🔧 *API:* Ideal for hooks: `hooks_suggest` → trigger STOP when `bar` spikes above threshold within 10s. `say "STOP. One breath. What do you notice?"`. `label "STOP practice" --context "trigger, bar level"`.

**RAIN (Recognize, Allow, Investigate, Nurture)** → difficult emotions, resistance, self-judgment, emotional avoidance
  **R**ecognize what's happening ("I notice anxiety"). **A**llow it to be there without fixing. **I**nvestigate with curiosity — where in the body? what does it need? **N**urture with self-compassion — hand on heart, kind inner voice.
  ◈ *Stuck at Allow:* Say "This too belongs" or "It's OK to feel this."
  ◈ *Stuck at Investigate:* Ask "If this feeling had a shape/colour/temperature, what would it be?"
  ◈ *Stuck at Nurture:* Ask "What would I say to a friend feeling this?"
  🔧 *API:* Track `valence`, `bar`, `faa`, `relaxation`. `say` each RAIN step as a prompt with pauses. After: `label "RAIN" --context "emotion, valence delta, bar delta"`. Longitudinal: `search-labels "RAIN"` to see which emotions recur and whether response patterns improve.

**5-4-3-2-1 Grounding** → dissociation, derealization, panic, overwhelm, very high `bar`
  Name: 5 things you see, 4 you hear, 3 you touch, 2 you smell, 1 you taste. Forces sensory present-moment contact.
  ◈ *Simplified:* Just "name 3 things you see and 2 you hear" if full sequence is too much.
  ◈ *Eyes-closed variant:* 5 body sensations, 4 sounds, 3 textures within reach, 2 temperatures, 1 taste.
  ◈ *Child:* Make it a game — "Quick! Spot 5 blue things!"
  🔧 *API:* `say` each prompt with pauses. Track `bar` — should drop rapidly across the sequence. `label "grounding 5-4-3-2-1" --context "bar start=X, bar end=X"`.

**Three-Minute Breathing Space (MBCT)** → mid-day reset, between meetings, cognitive overwhelm
  Minute 1: Awareness — "What is my experience right now?" (thoughts, feelings, sensations).
  Minute 2: Gathering — narrow attention to breath at belly.
  Minute 3: Expanding — widen attention to whole body and posture.
  ◈ *Origin:* Core practice from Mindfulness-Based Cognitive Therapy (MBCT).
  ◈ *Calendar integration:* Schedule 2–3 per day as recurring calendar events.
  🔧 *API:* `dnd_set enabled:true`. `say` each minute transition. `timer 3m`. Track `focus` and `bar` across the three phases. `label "3-min breathing space"`. After: `notify "Breathing space complete"`.

---

## MINDFUL AWARENESS OF THOUGHTS

**Thought Defusion** → rumination, catastrophizing, over-identification with thoughts, low `valence`
  Observe thoughts as mental events, not facts. Techniques:
  - "I notice I'm having the thought that..." (prefix)
  - Visualize thoughts as leaves on a stream, clouds passing, or train cars going by
  - Say the anxious thought in a cartoon voice (breaks identification)
  ◈ *Key insight:* The goal is not to stop thinking. It's to change your relationship to thinking.
  ◈ *Repetitive thought:* Count how many times the same thought arises in 5 minutes — the counting itself creates distance.
  🔧 *API:* Track `bar`, `valence`, `rel_beta` (high beta often correlates with ruminative loops). `label "thought defusion" --context "theme, repetition count, valence shift"`.

**Awareness of Impermanence** → attachment, clinging to states, frustration that "it's not working"
  During any practice, notice that every sensation, thought, and emotion changes on its own. Nothing you observe stays constant for more than a few seconds.
  ◈ *Practice:* Watch a single body sensation closely — notice it pulse, shift, fade, return.
  ◈ *Meta-application:* When meditation "isn't working", notice that even that judgment is impermanent.
  🔧 *API:* Real-time EEG makes impermanence visible — `status` snapshots 60s apart always differ. `say "Notice: your brain state just shifted. Nothing stays."` Use metric variance itself as a teaching tool.

---

## COMPASSION-BASED MINDFULNESS

**Self-Compassion Break (Kristin Neff)** → self-criticism, shame, perfectionism, failure response
  Three phrases: "This is a moment of suffering" (mindfulness). "Suffering is part of life" (common humanity). "May I be kind to myself" (self-kindness).
  ◈ *Physical anchor:* Both hands on heart, or one hand on heart and one on belly.
  ◈ *Personalize:* Replace third phrase with what you most need to hear right now.
  🔧 *API:* Track `valence`, `bar`, `faa`. `say` each phrase with 10s pauses. `label "self-compassion break" --context "trigger, valence delta"`.

**Equanimity Practice** → reactivity to pleasant/unpleasant, emotional volatility, high `valence` variance
  Notice pleasant, unpleasant, and neutral experiences arising. For each: "This too is impermanent. I don't need to chase or push away."
  ◈ *Body-based:* Scan for pleasant sensations (warmth, comfort) and unpleasant ones (tension, itch) — hold both with equal attention.
  🔧 *API:* Track `valence` stability over session. Equanimity practice should narrow valence variance. `compare` sessions to verify.

---

## MINDFULNESS IN DIFFICULT MOMENTS

**Urge Surfing** → cravings, addictive impulses, compulsive checking, digital addiction triggers
  When an urge arises, don't act on it or fight it. Observe it as a wave: it builds, peaks, and naturally subsides (usually within 15–20 minutes).
  ◈ *Track the intensity:* Rate 1–10 every 2 minutes. Watch the number fall.
  ◈ *Body location:* Where is the urge felt physically? Chest tightness? Restless hands?
  ◈ *Digital:* When reaching for phone, pause and surf the urge for 60 seconds before deciding.
  🔧 *API:* `timer` for 15 min. Poll `status` every 2 min — track `bar` and `focus`. `label "urge surfing" --context "urge type, peak intensity, duration until subsided"`. Longitudinal: `search-labels "urge surfing"` to identify patterns.

**Mindful RAIN for Pain** → chronic pain, acute discomfort, pain-related anxiety
  Apply RAIN specifically to physical pain: Recognize the pain. Allow it without bracing. Investigate — where exactly? what quality (sharp/dull/throbbing/burning)? does it have edges? Nurture — breathe warmth toward the area, soften surrounding muscles.
  ◈ *Key:* Separate the sensation from the narrative ("this will never end", "something is wrong").
  ◈ *Discovery:* Pain often has texture and movement when closely observed — it's rarely the solid block it seems.
  🔧 *API:* Track `bar`, `stress_index`. Pain-related anxiety (`bar`) often drops faster than the pain itself. `label "mindful pain" --context "location, quality, bar delta"`.

---

## SESSION DESIGN GUIDELINES

| Context | Protocol | Duration |
|---|---|---|
| Acute reactivity | STOP or 5-4-3-2-1 | 15–60 sec |
| Mid-day reset | Three-Minute Breathing Space | 3 min |
| Daily informal | Any informal practice (eating, walking, transitions) | Varies |
| Daily formal | Mindful breathing → noting or sitting mindfulness | 10–20 min |
| Emotional difficulty | RAIN or Self-Compassion Break | 5–15 min |
| Craving/impulse | Urge Surfing | 10–20 min |
| Deepening practice | Sitting mindfulness (full spectrum) | 20–45 min |

**Integration rule:** Formal practice builds the skill. Informal practice is where the skill lives. Prioritize weaving mindfulness into daily activities over adding more cushion time.

**Attitude foundations:** All practices rest on: non-judging, patience, beginner's mind, trust, non-striving, acceptance, letting go. When practice feels forced, return to these.

---
