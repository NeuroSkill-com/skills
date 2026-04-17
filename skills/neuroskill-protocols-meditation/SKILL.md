---
name: neuroskill-protocols-meditation
description: "Meditation protocols — guided, unguided, mantra, visualization, loving-kindness, body scan, open awareness, and EEG-biofeedback meditation with real-time neural tracking."
---

# Meditation Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## FOCUSED ATTENTION MEDITATION

**Breath-Counting Meditation** → high `bar`, scattered attention, low `focus`
  Count breaths 1–10, restart on losing count. Builds sustained attention.
  ◈ *Beginner:* Start with 1–5 counts. Use `say` to announce each count as an anchor.
  ◈ *Child:* "Count your belly breaths like counting sheep — start over if you forget!"
  ◈ *Breath-averse:* Count heartbeats or ambient sounds instead of breaths.
  🔧 *API:* Before: `status` → snapshot `focus`, `bar`, `rel_theta/rel_alpha` baseline, `label "protocol start: breath counting" --context "focus=X, bar=X"`. During: poll `status` every 60s — report focus shifts verbally ("Focus score climbing — you're settling in"). After: `status` → compare to baseline, `label "protocol end: breath counting" --context "focus delta, bar delta"`.

**Single-Point Focus (Trataka)** → low `focus`, high `beta`, mental restlessness
  Gaze at a fixed point (candle flame, dot, object) without blinking as long as comfortable. Then close eyes and hold the afterimage.
  ◈ *Screen-based:* Use a small dot on screen if no candle available.
  ◈ *Eye strain risk:* Limit to 2–3 min, blink gently if needed.
  🔧 *API:* Track `focus` and `rel_alpha` — alpha typically rises sharply when eyes close after gazing.

**Mantra Meditation** → high mental chatter, difficulty with silent practice, low `relaxation`
  Silently repeat a word or phrase (e.g., "Om", "So Hum", personal mantra). Return to mantra when mind wanders.
  ◈ *Secular:* Use neutral words like "calm", "peace", "one", or counting.
  ◈ *Vocalized variant:* Chanting aloud activates vagal tone via laryngeal vibration (same pathway as humming).
  ◈ *Child:* Pick a favourite word — "butterfly", "sunshine" — and whisper it like a secret.
  🔧 *API:* Before: snapshot `bar`, `relaxation`, `rel_alpha`. During: `say` the mantra softly at intervals if vocalized. Poll `status` at 60s — watch for `rel_alpha` rise and `bar` drop. After: `label` with deltas.

---

## OPEN AWARENESS MEDITATION

**Open Monitoring / Choiceless Awareness** → low `lzc` (<40), low `integration`, mental rigidity
  Sit without a focal object. Let thoughts, sensations, and sounds arise and pass without engaging.
  Raises neural complexity (LZC) and integration.
  ◈ *Beginner bridge:* Start with 2 min focused attention, then release into open monitoring.
  ◈ *Restless:* Label experiences silently ("thinking", "hearing", "feeling") to stay non-reactive.
  🔧 *API:* Track `lzc`, `integration`, `rel_alpha`, `rel_theta`. Open monitoring raises LZC more than focused attention. Before: snapshot all. During: poll every 90s. After: `compare` with focused sessions to see which raises complexity more for this person.

**Non-Dual Awareness** → advanced meditators, high baseline `focus`, seeking deeper states
  Rest as awareness itself — no meditator, no object, no effort. Notice the noticing.
  ◈ *Prerequisites:* Stable focused attention and open monitoring practice.
  ◈ *Pointer:* "What is aware right now? Look for the looker."
  🔧 *API:* Watch for simultaneous high `rel_alpha`, high `lzc`, low `bar`, high `coherence` — the signature of effortless awareness. `label` any state shifts with context.

---

## VISUALIZATION MEDITATION

**Visualization / Guided Imagery** → stress, pre-performance anxiety, creative blocks
  Mentally construct a detailed scene (beach, forest, mountain, safe place). Engage all senses.
  ◈ *Low visualization ability:* Focus on felt sense (warmth, breeze) rather than visual detail.
  ◈ *Performance prep:* Visualize successful execution of upcoming task in detail.
  🔧 *API:* `say` the guided imagery script at natural pace. Poll `status` every 60s — track `relaxation`, `bar`, `rel_alpha`. After: `label` with subjective vividness rating.

**Yoga Nidra / NSDR (Non-Sleep Deep Rest)** → fatigue, low energy, need recovery without sleep
  Systematic guided body rotation + intention setting while hovering between wake and sleep.
  20–45 min. Distinct from progressive relaxation — uses rotation of awareness, not tensing.
  ◈ *Short version:* 10 min micro-nidra focusing on body rotation only.
  ◈ *Insomnia:* Use at bedtime — if sleep happens, that's fine.
  🔧 *API:* `dnd_set enabled:true`. `say` body rotation cues slowly. Poll `status` at 120s intervals — watch for theta dominance (`rel_theta` > 0.3) indicating hypnagogic entry. After: `label "protocol end: yoga nidra" --context "theta peak, relaxation delta"`. `sleep` command can confirm if actual sleep occurred.

---

## COMPASSION & EMOTIONAL MEDITATION

**Loving-Kindness (Metta)** → low mood, loneliness, self-criticism, interpersonal stress
  Silently direct well-wishes: "May I be happy, may I be safe, may I be healthy, may I live with ease." Expand to loved ones → neutral people → difficult people → all beings.
  ◈ *Resistance:* Start with a beloved pet or child — easier entry than self-directed kindness.
  ◈ *Physical anchor:* Place hand on heart while sending wishes.
  ◈ *Child:* "Send a warm golden light from your heart to someone you love."
  🔧 *API:* Track `valence`, `relaxation`, `faa` (frontal asymmetry). Left-dominant FAA shift correlates with positive affect. `label` with emotional target (self/other/difficult).

**Tonglen (Giving & Taking)** → empathic fatigue, avoidance of difficult emotions
  Inhale suffering (dark, heavy); exhale relief (light, spacious). Paradoxically reduces distress by reversing avoidance.
  ◈ *Overwhelm risk:* Start with minor discomfort, not trauma. Scale gradually.
  ◈ *Non-breath version:* Visualize absorbing and transforming without breath coordination.

**Gratitude Meditation** → negativity bias, rumination, low `valence`
  Bring to mind 3–5 things you're grateful for. Dwell on each — feel the gratitude in the body.
  ◈ *Specificity matters:* "The warmth of coffee this morning" > "I'm grateful for life."
  🔧 *API:* Snapshot `valence`, `bar` before and after. `label` with gratitude targets for longitudinal tracking.

---

## BODY-BASED MEDITATION

**Body Scan Meditation** → physical tension, disconnection from body, high `stress_index`
  Systematic attention sweep from crown to toes (or vice versa). Notice sensations without changing them.
  ◈ *Speed variants:* Quick scan (3 min) for check-ins, deep scan (20 min) for full release.
  ◈ *Difficulty feeling body:* Focus on contact points (feet on floor, back on chair) first.
  🔧 *API:* `say` body region cues at slow pace. Track `relaxation`, `stress_index`, `bar`. After: `label` with tension regions discovered.

**Walking Meditation** → restlessness, inability to sit still, post-meal sluggishness
  Slow deliberate walking. Attention on: lifting, moving, placing each foot.
  ◈ *Indoor:* 10 steps forward, pause, turn, 10 steps back. Repeat.
  ◈ *Outdoor:* Add awareness of environment — sounds, air, ground texture.
  ◈ *Stealth:* Slow your normal walk by 20% and attend to feet. No one notices.
  🔧 *API:* Motion sensors may show reduced `acceleration` variance. Track `focus` and `bar`.

---

## EEG-BIOFEEDBACK MEDITATION

**Neurofeedback-Guided Meditation** → meditators wanting objective feedback on practice quality
  Use real-time EEG data to guide and validate meditation depth.
  🔧 *API:* This is the flagship meditation use case for NeuroSkill.
  1. Before: `status` → full baseline snapshot.
  2. `dnd_set enabled:true` — silence interruptions.
  3. Choose target metric based on meditation style:
     - Focused attention → track `focus` score
     - Open monitoring → track `lzc`, `integration`
     - Relaxation → track `relaxation`, `rel_alpha`
     - Deep states → track `rel_theta`, `consciousness` indices
  4. During: poll `status` every 60–90s. Use `say` for gentle audio cues:
     - "Focus deepening" when target metric improves
     - "Mind wandering detected" when `focus` drops or `bar` spikes
     - Avoid interrupting flow — only cue on significant shifts
  5. After: `label "meditation session" --context "style, duration, peak metrics"`.
  6. Longitudinal: `compare` meditation sessions over time. `search` for neurally similar peak states. Build evidence of which style produces deepest states for this person.

**Alpha-Theta Crossover Training** → deep meditation, creativity, integrative states
  Goal: achieve alpha-theta crossover where `rel_theta` exceeds `rel_alpha`.
  Historically used in neurofeedback clinics for peak performance and trauma resolution.
  🔧 *API:* Poll `status` at 30s intervals. When `rel_theta > rel_alpha`, `say "Crossover — you're in deep territory"`. `label` crossover moments. Track crossover duration across sessions.

---

## SESSION DESIGN GUIDELINES

| Duration | Structure | Best for |
|---|---|---|
| 3–5 min | Single technique, no transitions | Micro-reset, beginners, acute stress |
| 10–15 min | Settle (2 min) → main practice → close (1 min) | Daily practice, moderate depth |
| 20–30 min | Settle → focused attention (5 min) → main practice → open awareness (3 min) → close | Experienced practitioners, deep work |
| 45–60 min | Full sequence: body scan → breathwork → focused → open → non-dual → integration | Retreat-style, advanced |

**Transition rule:** Always begin with 1–2 min of settling (feel the body, notice the breath) before any technique. Abrupt starts increase resistance.

**Ending rule:** Never end abruptly. Use 30–60s of "widening" — open awareness, feel the room, hear ambient sounds — before opening eyes.

---
