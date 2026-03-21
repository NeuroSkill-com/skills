---
name: neuroskill-protocols
description: Protocol framework hub — personalisation engine, API integration guide, modality router (7 modalities × 12 EEG triggers), and matching guidance for 130+ mind-body practices. References 11 domain sub-skills (focus, stress, emotions, sleep, body, routines, nutrition, music, digital, breath-free, life-contexts) and the neuroskill-evidence skill. Always loaded when any protocol intent is detected; sub-skills are loaded contextually by domain.
---

# NeuroLoop™️ Protocol Repertoire

Choose by the **dominant metric signal**. Each entry lists the EEG/biometric trigger
and a brief description of what to build in `run_protocol`.

Every protocol **must be adapted** to the person using it. Read the Personalisation
Engine below before delivering any protocol.

---

## PERSONALISATION ENGINE

*(These rules apply to EVERY protocol in this file. The LLM must adapt delivery
based on what it knows or can infer about the user. Never deliver a protocol in
its generic form when context is available.)*

### Adaptation Dimensions

When proposing or running a protocol, actively consider and adapt for:

| Dimension | What to adjust | Examples |
|---|---|---|
| **Age** | Language complexity, duration, physicality, cultural references, playfulness | A 7-year-old gets "pretend you're a robot powering down" not "progressive muscle relaxation". A 75-year-old gets seated-only options and no floor work without asking. |
| **Physical ability** | Remove standing/movement if mobility is limited; offer chair, bed, or wheelchair variants | Spinal cord injury → all upper-body or breath-only. Chronic pain → never "tense the painful area". Pregnancy → no breath holds, no prone positions, no Wim Hof. |
| **Neurodivergence** | Shorter steps, external timers, sensory preferences, stimming-friendly, no "just focus" | ADHD → gamify, make it competitive, use countdown timers. Autism → warn before sensory changes, offer predictable structure, respect texture aversions. |
| **Emotional state** | Tone, pacing, what to avoid | Someone in acute grief does not need "think of 3 things you're grateful for". Someone in rage needs discharge first, not stillness. |
| **Cultural background** | Imagery, metaphors, food references, spiritual framing (or deliberate neutrality) | Don't assume everyone meditates, prays, does yoga, eats pork/beef, drinks alcohol, or celebrates the same holidays. Use the user's own language when known. |
| **Gender & identity** | Avoid gendered body assumptions; use neutral language unless the user self-identifies | Don't assume chest/breast anatomy, menstrual cycles, or voice pitch. If relevant (e.g. hormonal cycle protocol), ask rather than assume. |
| **Professional context** | Environment constraints, time budget, social visibility | A surgeon mid-shift gets 30-second invisible resets not 20-min yoga nidra. A truck driver gets eyes-open only. A teacher gets protocols doable while supervising children. |
| **Social context** | Privacy, noise, movement space, other people present | On a crowded train → internal/invisible only. At home alone → full voice, full movement. With a partner → relational variant. With a baby → one-handed, interruptible. |
| **Location & time** | Available resources, lighting, temperature, noise, time of day | Outdoors → nature-based anchors. Office → desk-bound. Bedroom at 2 AM → no bright light, no energising. Bathroom → water-based. |
| **Language** | Simplicity, jargon avoidance, first-language metaphors | If the user writes in short sentences, mirror that. Never use clinical jargon unless the user is clinical. Respect the user's language and vocabulary level. |
| **Need urgency** | Immediate crisis vs. strategic habit-building vs. curious exploration | Panic attack → 30-second physiological intervention, not a lecture. Long-term resilience → weekly practice plan. Casual curiosity → light, no-pressure suggestion. |
| **Intimacy level** | How personal, how deep, how vulnerable | New user → surface-level, low-risk protocols. Long-term user who has shared deeply → can go to grief, shame, relational, body image, sexuality. |

### Adaptation Rules

1. **Infer, don't interrogate.** Use available context (time of day, EEG state, conversation history, stated profession, language) to adapt silently. Only ask when essential.
2. **Offer, don't prescribe.** "Would you like to try…" not "You need to do…". Autonomy is sacred.
3. **Name the adaptation.** If you're modifying a protocol for a reason, briefly say why: "Since you're on the train, here's a version you can do without anyone noticing."
4. **Always have a fallback.** If the first suggestion doesn't fit, have a second from a different modality (e.g. if breath doesn't work → tactile; if movement isn't possible → cognitive).
5. **Respect refusal instantly.** If someone says "no" or "not now", acknowledge and move on. Never push.
6. **No spiritual assumptions.** Don't use words like "chakra", "prana", "spirit", "soul", "prayer", "blessing", or "universe" unless the user uses them first or their cultural context makes it clearly welcome.
7. **No body-shaming language.** Never imply the user's body is wrong, broken, or needs fixing. Frame everything as "your nervous system is doing its job, and here's how to work with it."
8. **Age-appropriate intensity.** Children (< 12): playful, short (< 3 min), game-like. Teens (13–19): autonomy-respecting, cool, not preachy. Young adults (20–35): efficiency-oriented, evidence-framed. Middle adults (36–60): practical, time-scarce, habit-aware. Older adults (60+): gentle, dignity-preserving, seated-default, joint-safe.
9. **Hormonal awareness.** Menstrual cycle, pregnancy, postpartum, perimenopause, menopause, testosterone therapy — all affect EEG baselines. Acknowledge when relevant, never assume.
10. **Trauma-informed always.** Never instruct someone to "relax" during a flashback. Never demand eye closure. Always offer "eyes open or closed, your choice." Avoid body-scan in early trauma processing without consent.

### Example Adaptations

The same EEG signal (high `bar`, high `stress_index`) might produce:

- **Executive in a boardroom:** "Excuse yourself for a water refill. At the sink, run cold water over your wrists for 20 seconds. Come back. Nobody noticed."
- **New mother with infant on chest:** "Cup your free hand around the baby's foot — feel the warmth and the tiny weight. That's your anchor. Stay there for 60 seconds."
- **14-year-old before an exam:** "Quick game: pick any letter. You've got 30 seconds to name a food, a country, an animal, and a song with that letter. Go."
- **Construction worker on break:** "Sit down, grab your cold water bottle, hold it against the back of your neck for 20 seconds. That's the whole protocol."
- **80-year-old with arthritis:** "Rest your hands on your lap, palms up. Feel the weight of your hands. Notice the temperature of the air on your palms. That's all."
- **Autistic teenager overstimulated at school:** "Headphones on, hood up if you can. Press your tongue hard against the roof of your mouth for 10 seconds. Release. Three times. Nobody sees it."
- **Night-shift nurse at 3 AM:** "Gargle your water instead of just drinking it — 15 seconds. Weird, but it flips your vagus nerve on. Instant alertness."
- **Grieving person:** "You don't need to do anything right now. Just put one hand on your chest. Feel the warmth of your own hand. That's enough."
- **Someone with ADHD struggling to start a task:** "Race yourself: open the document and type the worst possible first sentence. You have 10 seconds. Go. It doesn't have to be good — it has to exist."
- **Pregnant woman in third trimester:** "Seated only, no breath holds. Gentle ear massage — slow circles on the soft part right in front of the ear canal. 60 seconds per side."
- **Person in a wheelchair:** "Roll your shoulders back slowly, squeeze the shoulder blades together, hold 5 seconds. Your upper body holds the same stress patterns — this releases them."
- **Someone who speaks English as a second language:** Use short sentences. Simple words. No idioms. "Close your eyes. Press your hands together hard. Hold. Let go. Feel the warmth."

---

## API INTEGRATION GUIDE

*(The NeuroSkill API provides real-time EEG data, voice guidance, notifications, labels,
timers, hooks, sleep staging, session comparison, and neural similarity search. Every
protocol should leverage these tools to create a closed-loop, data-driven experience —
not just deliver static instructions.)*

### Protocol Lifecycle

Every protocol has four phases. Use the API in each:

```
┌─────────────┐     ┌─────────────┐     ┌──────────────┐     ┌──────────────────┐
│   BEFORE     │ ──→ │   DURING     │ ──→ │    AFTER      │ ──→ │   LONGITUDINAL    │
│  Validate    │     │  Guide       │     │  Measure      │     │  Track & Trigger  │
│  & Baseline  │     │  & Monitor   │     │  & Reflect    │     │  Across Sessions  │
└─────────────┘     └─────────────┘     └──────────────┘     └──────────────────┘
```

### BEFORE — Validate Trigger & Baseline

Before proposing or starting ANY protocol, check the live EEG state to confirm
the trigger condition actually exists and capture a baseline.

| Action | API Call | Purpose |
|---|---|---|
| **Check live state** | `{"command": "status"}` | Verify the metric trigger (e.g. confirm `bar` is actually high before offering Box Breathing) |
| **Get session context** | `{"command": "sessions"}` | Know how long they've been recording, when the session started |
| **Check signal quality** | `status → signal_quality` | Don't run a protocol if signal is poor (< 0.7) — recommend headband adjustment first |
| **Baseline snapshot** | `status → scores` | Capture pre-protocol values of key metrics to compare after |
| **Search for precedent** | `{"command": "search_labels", "args": {"query": "box breathing"}}` | Check if they've done this protocol before and how it went |
| **Label the start** | `{"command": "label", "args": {"text": "protocol start: box breathing", "context": "bar=0.72, stress_index=68, trigger: high stress"}}` | Timestamp the protocol start with baseline metrics for later analysis |

**Example — before starting Box Breathing:**
```json
// 1. Check if stress is actually elevated
{"command": "status"}
// → scores.bar = 0.72, scores.stress_index = 68 → confirmed, proceed

// 2. Check if they've done this before
{"command": "search_labels", "args": {"query": "box breathing", "k": 5}}
// → 3 prior sessions found, average relaxation increase of +0.15 → mention this

// 3. Label the start
{"command": "label", "args": {"text": "protocol start: box breathing 4-4-4-4", "context": "bar=0.72, stress_index=68, relaxation=0.31"}}

// 4. Start timer if timed protocol
{"command": "timer"}
```

### DURING — Guide & Monitor

Use voice, notifications, and real-time data to guide the protocol and provide
live biofeedback.

| Action | API Call | Purpose |
|---|---|---|
| **Voice guidance** | `{"command": "say", "args": {"text": "Inhale... 2... 3... 4..."}}` | Hands-free, eyes-closed guidance. Essential for breath, meditation, body scan protocols |
| **Step notifications** | `{"command": "notify", "args": {"title": "Next step", "body": "Now exhale slowly for 8 counts"}}` | Visual cue for step transitions when voice is inappropriate |
| **Real-time monitoring** | `{"command": "status"}` (poll every 30–60s) | Track metric changes mid-protocol. Adapt: "Your alpha just rose — whatever you're doing, keep going" |
| **Mid-protocol label** | `{"command": "label", "args": {"text": "alpha spike during body scan"}}` | Mark notable moments for later analysis |
| **Live feedback to user** | Check `status → scores` mid-protocol | "Your relaxation score just jumped from 0.31 to 0.52 — the breathing is working" |
| **Focus timer** | `{"command": "timer"}` | For timed protocols (Pomodoro, meditation, focus blocks) |
| **DND activation** | `{"command": "dnd_set", "args": {"enabled": true}}` | Protect the user from interruptions during meditation or deep work protocols |

**Example — during a 5-minute Cardiac Coherence protocol:**
```json
// Minute 0: Start
{"command": "say", "args": {"text": "Let's begin cardiac coherence. Breathe in for 5 seconds."}}
{"command": "dnd_set", "args": {"enabled": true}}

// Minute 0:05
{"command": "say", "args": {"text": "Now breathe out for 5 seconds."}}

// Minute 1: Check progress
{"command": "status"}
// → rmssd rising from 28 to 34 → good sign
{"command": "say", "args": {"text": "Good. Your heart rhythm is already smoothing out. Keep going."}}

// Minute 2:30: Mid-point check
{"command": "status"}
// → relaxation 0.31 → 0.48
{"command": "say", "args": {"text": "Halfway there. Your relaxation is climbing. Stay with the rhythm."}}

// Minute 5: End
{"command": "say", "args": {"text": "That's 5 minutes. Well done. Sit quietly for a moment."}}
{"command": "dnd_set", "args": {"enabled": false}}
```

### AFTER — Measure & Reflect

Immediately after a protocol, capture the post-state and show the user
what changed. This is the most powerful moment for motivation and learning.

| Action | API Call | Purpose |
|---|---|---|
| **Post-protocol snapshot** | `{"command": "status"}` | Capture post-protocol metrics |
| **Label the end** | `{"command": "label", "args": {"text": "protocol end: box breathing", "context": "bar=0.48, stress_index=42, relaxation=0.58 — delta: bar -0.24, stress -26, relax +0.27"}}` | Record the outcome with deltas |
| **Compare before/after** | Compare baseline snapshot to current `status` | Show concrete numbers: "Your stress index dropped from 68 to 42. Your relaxation rose from 0.31 to 0.58." |
| **Session trend** | `{"command": "session_metrics", "args": {"start_utc": ..., "end_utc": ...}}` | Show first-half → second-half trends for the session |
| **Voice the results** | `{"command": "say", "args": {"text": "Your stress dropped 38 percent. Relaxation nearly doubled."}}` | Make the data tangible |
| **Notify summary** | `{"command": "notify", "args": {"title": "Box Breathing Complete", "body": "Stress: 68→42 (-38%). Relaxation: 0.31→0.58 (+87%)"}}` | Persistent summary the user can see later |

**Example — after Box Breathing:**
```json
// 1. Get post-state
{"command": "status"}
// → bar=0.48, stress_index=42, relaxation=0.58

// 2. Label with full delta
{"command": "label", "args": {"text": "protocol end: box breathing 4-4-4-4", "context": "Duration: 5min. bar: 0.72→0.48 (-33%), stress_index: 68→42 (-38%), relaxation: 0.31→0.58 (+87%)"}}

// 3. Tell the user
{"command": "say", "args": {"text": "Nice work. Your stress index dropped 38 percent, from 68 to 42. Relaxation nearly doubled. That's a strong response."}}

// 4. Persistent notification
{"command": "notify", "args": {"title": "✓ Box Breathing Complete", "body": "Stress: 68→42. Relaxation: 0.31→0.58. Duration: 5 min."}}
```

### LONGITUDINAL — Track Across Sessions & Auto-Trigger

The most powerful use of the API is building a personal history of protocol
effectiveness and setting up proactive triggers.

| Action | API Call | Purpose |
|---|---|---|
| **Find past protocol sessions** | `{"command": "search_labels", "args": {"query": "box breathing", "k": 20}}` | Retrieve all past instances of this protocol with their EEG metrics |
| **Compare protocol vs non-protocol** | `{"command": "compare", "args": {...}}` | A/B compare: session WITH protocol vs session WITHOUT |
| **Neural similarity search** | `{"command": "search", "args": {"start_utc": ..., "end_utc": ..., "k": 10}}` | "Your brain right now looks like these 10 past moments" — personalise recommendations |
| **Cross-modal graph** | `{"command": "interactive_search", "args": {"query": "stress relief"}}` | Discover what labels/states cluster around successful stress interventions |
| **Set up auto-trigger hook** | `{"command": "hooks_set", "args": {"hooks": [...]}}` | "When your brain enters this stress pattern again, I'll automatically suggest this protocol" |
| **Get threshold suggestion** | `{"command": "hooks_suggest", "args": {"keywords": "stress,overwhelmed"}}` | Let the system suggest optimal trigger thresholds from real data |
| **Check hook audit log** | `{"command": "hooks_log", "args": {"limit": 20}}` | Review when protocols were auto-triggered and whether they helped |
| **Sleep impact** | `{"command": "sleep", "args": {"start_utc": ..., "end_utc": ...}}` | For evening protocols: did tonight's wind-down improve sleep architecture? |
| **UMAP visualisation** | `{"command": "umap", "args": {...}}` | Visually show how protocol sessions cluster differently from baseline |
| **Session history** | `{"command": "sessions"}` | Track streak, total hours, session count for habit reinforcement |

**Example — building a stress protocol habit:**
```json
// After 5+ labeled box breathing sessions, set up an auto-trigger:

// 1. Check what threshold makes sense
{"command": "hooks_suggest", "args": {"keywords": "stress,overwhelmed,anxious"}}
// → suggested threshold: 0.13

// 2. Create the hook
{"command": "hooks_set", "args": {"hooks": [{"name": "Stress Auto-Protocol", "keywords": "stress,overwhelmed,anxious", "scenario": "emotional", "threshold": 0.13, "enabled": true, "command": "notify", "text": "Your brain pattern matches past stress moments. Want to try box breathing?"}]}}

// 3. Periodically review effectiveness
{"command": "hooks_log", "args": {"limit": 20}}
// → see when it triggered, correlate with whether the user acted on it

// 4. Compare protocol sessions vs non-protocol sessions
{"command": "compare", "args": {"a_start_utc": "<non_protocol_session>", "a_end_utc": "...", "b_start_utc": "<protocol_session>", "b_end_utc": "..."}}
// → show the user: "Sessions where you did breathing show 23% lower stress on average"
```

### API Tool Quick Reference for Protocols

| Tool | Protocol Phase | When to Use |
|---|---|---|
| `status` | Before, During, After | Always. Live EEG is the foundation of every decision. |
| `say` | During | Voice-guided protocols (breathing, meditation, body scan, calibration). Hands-free, eyes-closed. |
| `notify` | Before, During, After | Step transitions, protocol summaries, gentle nudges. Persists in notification center. |
| `label` | Before, After | Mark protocol start/end with metrics context. Builds searchable history. |
| `search_labels` | Before, Longitudinal | Find past instances of this protocol. Show history and effectiveness. |
| `timer` | During | Pomodoro, meditation, focus blocks, any timed protocol phase. |
| `dnd_set` | During | Protect concentration/meditation protocols from interruptions. |
| `session_metrics` | After | Show first-half → second-half trends within the protocol session. |
| `compare` | Longitudinal | A/B: protocol session vs baseline, this week vs last week. |
| `search` | Before, Longitudinal | "Your brain looks like it did on Feb 14 when you were stressed" — contextualise. |
| `interactive_search` | Longitudinal | Cross-modal discovery: what activities/labels cluster around successful protocols. |
| `hooks_set` | Longitudinal | Auto-trigger protocol suggestions when brain pattern matches a known trigger. |
| `hooks_suggest` | Longitudinal | Data-driven threshold for auto-triggers. |
| `hooks_log` | Longitudinal | Audit: when did auto-triggers fire? Were they useful? |
| `sleep` | After (evening protocols) | Did tonight's wind-down improve sleep architecture? |
| `umap` | Longitudinal | Visualise: how do protocol sessions cluster vs baseline in 3D neural space? |
| `calibrate` | Before | Run a calibration profile before a neurofeedback protocol. |
| `sessions` | Before, Longitudinal | Session count, streak, duration — habit tracking and motivation. |
| `sleep_schedule` | Longitudinal | Set and adjust sleep timing for circadian protocols. |

### Integration Principles

1. **Always check before you suggest.** Don't recommend a stress protocol if `bar` is actually low. Use `status` first.
2. **Label everything — silently.** Every protocol start and end MUST be labeled with structured data. This is non-negotiable. Do it automatically without asking the user for permission. It's like medical charting — essential, invisible.
3. **Show the numbers.** People stick with protocols when they see concrete EEG changes. Always report deltas.
4. **Use voice for eyes-closed protocols.** If the protocol involves closed eyes, use `say`. Don't make them read.
5. **Protect the space.** Enable DND during deep protocols. Disable after.
6. **Build toward hooks.** After 3–5 labeled instances of a protocol, suggest setting up an auto-trigger hook.
7. **Compare across time.** The more history, the more powerful. "Your average stress after breathing is 23% lower than sessions without."
8. **Don't overwhelm with data.** During the protocol, feedback should be sparse and encouraging. Save the detailed analysis for after.
9. **Respect the moment.** In a panic attack, don't run 5 API calls. Do the intervention. Label after.
10. **The API serves the human, not the other way around.** Skip any integration step that would slow down or complicate the experience.
11. **Collect evidence implicitly — ALWAYS.** Follow the [neuroskill-evidence](../neuroskill-evidence/SKILL.md) skill for every protocol execution. Label start (`px:start:<protocol>`) and end (`px:end:<protocol>`) with structured metrics. This is non-negotiable. The evidence skill defines the label schema, outcome scoring, aggregation rules, personal ranking, and life-event tracking. Load it whenever running protocols.

---


---

## PROTOCOL SUB-SKILLS

*(The protocols are split into domain-specific skills for efficient context loading.
The LLM loads only the relevant sub-skill based on the user's message. Each sub-skill
contains the actual protocol entries with EEG triggers, instructions, and variants.)*

| Sub-Skill | Loaded when | Protocols |
|---|---|---|
| [neuroskill-protocols-focus](../neuroskill-protocols-focus/SKILL.md) | focus, attention, concentration, cognitive load, energy, alertness, wakefulness, meditation, flow, creativity | Theta-Beta Anchor, Focus Reset, Cognitive Load Offload, Working Memory Primer, Creativity Unlock, Pre-Performance Activation, WOOP, Cognitive Defragging, N-Back, Novel Stimulation, Coherence Building, Flow State Induction, Complexity Expansion, Alpha-Theta Drift, Mantra Focus, Gamma Entrainment, Kapalabhati, 4-Count Energising, Wim Hof, Cold Exposure |
| [neuroskill-protocols-stress](../neuroskill-protocols-stress/SKILL.md) | stress, anxiety, tension, relaxation, HRV, calm, overwhelm, vagal tone, breathing exercises | Box Breathing, Extended Exhale, Cardiac Coherence, Physiological Sigh, Alpha Induction, Open Monitoring, Relaxation Scan, Vagal Toning, Autogenic Training, Havening Touch, Somatic Shaking, Nadi Shodhana, Buteyko |
| [neuroskill-protocols-emotions](../neuroskill-protocols-emotions/SKILL.md) | emotions, mood, anger, grief, sadness, shame, fear, loneliness, envy, joy, resentment, feelings | FAA Rebalancing, Mood Activation, Loving-Kindness, Emotional Discharge, Anger Processing, Grief Holding, Shame Break, Emotion Surfing, Fear Processing, Envy Alchemy, Excitement Regulation, Emotional Inventory, Awe Induction, Joy Amplification, Loneliness, Resentment Release, Boundaries Reset |
| [neuroskill-protocols-sleep](../neuroskill-protocols-sleep/SKILL.md) | sleep, insomnia, nap, rest, recovery, NSDR, yoga nidra, wind-down, circadian, tired | Sleep Wind-Down, Ultradian Reset, Wake Reset, NSDR/Yoga Nidra, Power Nap |
| [neuroskill-protocols-body](../neuroskill-protocols-body/SKILL.md) | body, tension, neck, shoulder, headache, migraine, eye strain, posture, pain, movement, grounding | PMR, Body Scan, Grounding 5-4-3-2-1, TRE, Motor Activation, Desk Yoga, Neck Release, Cervical Decompression, Trap Release, 20-20-20, Eye Exercises, Palming, Cortical Quieting, Alpha-Reset Headache |
| [neuroskill-protocols-routines](../neuroskill-protocols-routines/SKILL.md) | morning routine, wake up, gym, workout, exercise, hydration, water, break, stretching | Morning Wake-Up, Morning Activation, Morning Clarity, Mindful Morning, Pre-Workout Primer, Focus Lock, Intra-Workout Recovery, Cool-Down, Recovery Reset, Mind-Muscle, Hydration, Movement Snack |
| [neuroskill-protocols-nutrition](../neuroskill-protocols-nutrition/SKILL.md) | food, diet, nutrition, caffeine, coffee, fasting, meal, blood sugar, gut health, cravings, alcohol | Pre-Meal Pause, Mindful Meal, Intuitive Eating, Blood Sugar Guide, Caffeine Timing, Cognitive Nutrition, Mood-Food, Anti-Inflammatory, Gut-Brain Reset, Evening Eating, IF Support, Sugar Craving Surf, Alcohol Awareness, UPF Reset |
| [neuroskill-protocols-music](../neuroskill-protocols-music/SKILL.md) | music, playlist, listening, binaural beats, singing, sounds, rhythm | Mood-Match Lift, Focus Music, Energising Playlist, Stress Discharge, Sleep Music, Binaural Beats, Music-Breath Sync, Active Listening, Rhythm Grounding, Singing, Emotional Release Music |
| [neuroskill-protocols-digital](../neuroskill-protocols-digital/SKILL.md) | social media, phone, screen time, scrolling, doom-scrolling, digital detox, FOMO, notifications | Pre-Scroll Check, Craving Surf, Post-Scroll Reset, Comparison Detox, Dopamine Reset, Notification Detox, Mindful Social, FOMO Defusion, Digital Sunset, Attention Walk, Values Reconnection, Screen Time Reflection |
| [neuroskill-protocols-breathfree](../neuroskill-protocols-breathfree/SKILL.md) | non-breathing, breath-free, alternative, tactile, touch, fidget, cold water, ear massage, humming, chewing | Micro-Tasking, Peripheral Vision, Mental Arithmetic, Internal Silence, Gratitude, Worry Parking, Texture Scanning, Bilateral Tapping, Temperature Contrast, Ear Massage, Tongue Press, Candle Gaze, Panoramic Vision, Isometric Squeeze, Jaw Release, Sound Mapping, Humming, Cold Water Face, Yawning, Chewing, Gargling |
| [neuroskill-protocols-life](../neuroskill-protocols-life/SKILL.md) | parent, baby, caregiver, elderly, teen, student, ADHD, autism, neurodivergent, commute, nurse, shift work, relationship, disability, wheelchair, cultural, prayer | One-Handed Calm, Parental Rage, Caregiver Fatigue, Exam Panic, Study Sprint, ADHD Task Hack, Sensory Overload, Meltdown Recovery, RSD, Hyperfocus Exit, Train Reset, Airplane Anxiety, End-of-Shift, Between-Patient, Night Shift, Co-Regulation, Conflict Pause, Accessibility variants, Wuqinxi, Shinrin-Yoku, Ho'oponopono, Tai Chi, Situational Micro-Protocols |

### Loading Rules

1. **Always load this hub skill** when any protocol intent is detected — it contains
   the Personalisation Engine, API Integration Guide, Modality Router, and Matching Guidance.
2. **Load one or more sub-skills** based on the user's specific need. Multiple sub-skills
   can be loaded in the same turn if the conversation spans domains.
3. **Always load [neuroskill-evidence](../neuroskill-evidence/SKILL.md)** alongside any
   protocol sub-skill — evidence collection is mandatory for every intervention.
4. **For breath-averse users**, load `neuroskill-protocols-breathfree` alongside (or instead
   of) the domain sub-skill.
5. **For context-specific users** (parents, elderly, teens, neurodivergent, etc.), load
   `neuroskill-protocols-life` alongside the domain sub-skill.

## MODALITY ROUTER

*(Breathing is ONE modality, not the default. Before offering any protocol, select the
best MODALITY for the person's circumstances, then pick a protocol within that modality.
Never default to breathwork without considering whether a different modality would be
more accessible, effective, or welcome for this specific human in this specific moment.)*

### The Rule

**Do NOT assume breathing is the best intervention.** Many people find breathwork:
- Physically uncomfortable (asthma, COPD, anxiety, panic disorder, nasal congestion)
- Socially awkward (meetings, public transport, crowded spaces, on camera)
- Cognitively annoying (counting feels tedious, controlling breath feels unnatural)
- Culturally unfamiliar (not everyone grew up with yoga/meditation framing)
- Triggering (hyperventilation history, trauma, feeling "controlled")
- Inaccessible (speech devices, ventilators, certain disabilities)

**Instead:** For every EEG trigger, there are 5–7 modalities that achieve the same
neurophysiological outcome. Choose the one that fits the human.

### Modality Selection by Trigger

| EEG Trigger | 🫁 Breath | 🖐️ Tactile | 🧠 Cognitive | 👁️ Visual | 🏃 Movement | 🔊 Auditory | ⚡ Passive Physio |
|---|---|---|---|---|---|---|---|
| **High `bar` / stress** | Box Breathing, Physiological Sigh | Bilateral Tapping, Ear Massage, Temperature Object | Worry Parking Lot, Category Drill | Panoramic Vision, Slow Tracking | Isometric Squeeze-Release, Somatic Shaking | Humming, Sound Mapping | Cold Water Wrists/Face, Gargling |
| **Low `focus` / high `tbr`** | Working Memory Primer (breath variant) | Structured Fidget, Texture Scanning | Micro-Tasking Drill, Mental Arithmetic, N-Back | Candle Gaze (Trataka), Colour Hunting | Micro-Walk, Cross-body Taps | Auditory Focus Anchor, Whisper Reading | Chewing Protocol, Deliberate Yawning |
| **Low `relaxation`** | Cardiac Coherence, Extended Exhale | Hand Warming, Pressure Points | Internal Narration Silence | Peripheral Vision Expansion | Jaw Release, Gravity Drop | Toning, Environmental Sound Bath | Ear Massage, Tongue Press |
| **Negative `faa` / low `mood`** | Loving-Kindness (breath-paired) | Havening Touch, Self-hug | Micro-Gratitude, Mental Time Travel, WOOP | Colour Hunting, Awe imagery | Mood Activation posture, Walking | Singing, Mood-Match Music | Hand Warming Visualisation |
| **High `cognitive_load`** | Cognitive Load breath dump | Palm Press, Texture Scanning | Brain dump, Worry Parking, 3-Word Capture | 20-20-20 Reset, Near-Far Shifting | Movement Snack, Spinal Wave | Environmental Sound Bath, Whisper Reading | Deliberate Yawning |
| **Low `wakefulness` / drowsy** | Kapalabhati, 4-Count Energising | Temperature Contrast (cold) | Novel Stimulation Burst | Near-Far Focus | Jumping jacks, Cross-body taps | Energising playlist | Cold Water Face (Dive Reflex), Chewing |
| **Low `rmssd` / low HRV** | Cardiac Coherence, Extended Exhale | Ear Massage (auricular vagal) | — | Slow Visual Tracking | — | Humming, Gargling, Singing | Gargling, Tongue Press |
| **High `headache_index`** | Cortical Quieting (slow nasal) | Pressure Points (LI-4), Cool compress | Distraction Counting | Palming | Jaw Release, Neck Release | Humming on exhale | Cold Water Wrists |
| **Panic / acute distress** | Physiological Sigh (if tolerable) | Bilateral Tapping, Cold object | 5-4-3-2-1 Grounding | Panoramic Vision, Fixed-point gaze | Feet press into floor | Rhythmic Tapping | Cold Water Face (Dive Reflex) |
| **Low `engagement` / flat** | — | Fidget Protocol | Homework Start Hack, Mental Time Travel | Colour Hunting | Movement Snack, Dance | Energising Music | Chewing, Cold Exposure |
| **Low `lzc` / rigidity** | — | — | Category Switching, Novel Burst | Active Listening (visual tracking) | Wuqinxi, Cross-body movements | Active Listening (music), Sound Mapping | — |
| **Pre-sleep / high `bar` at rest** | 4-7-8, Sleep Wind-Down | Ear Massage, Havening Touch | Worry dump (writing) | — (avoid screens) | Progressive Muscle Relaxation | Sleep Music, NSDR via `say` | — |

### How to Use This Table

1. **Identify the EEG trigger** from `status`.
2. **Assess the person's context** (location, social setting, physical ability, preferences, history).
3. **Pick the modality column** that best fits their circumstances:
   - *In a meeting?* → Cognitive or Passive Physio (invisible)
   - *Eyes-closed OK?* → Visual or Breath
   - *With a child?* → Tactile or Movement (playful)
   - *Breath-averse?* → Anything but Breath column
   - *Can make noise?* → Auditory
   - *Near water?* → Passive Physio (cold water)
   - *Restless?* → Movement
   - *No movement possible?* → Cognitive or Passive Physio
4. **Offer that protocol.** If refused, offer a second from a DIFFERENT modality.
5. **Never present breathing as the only or default option.** Present it as one choice among equals.

### Phrasing Examples

❌ "Let's try box breathing to lower your stress."
✅ "Your stress is elevated. A few options: you could try box breathing, or if you'd rather skip the breath stuff — cold water on your wrists works just as fast. Or I can guide you through a quick ear massage. What sounds good?"

❌ "Take 5 deep breaths."
✅ "Quick reset: press your tongue against the roof of your mouth for 10 seconds. Or, if you'd prefer, 3 slow breaths. Both work — pick whichever feels right."

❌ "Do some breathing exercises before bed."
✅ "For winding down tonight: you could do a body scan (I'll talk you through it), write down your worries and close the notebook, put on some Max Richter, or do an ear massage in bed. What appeals to you?"

---

## MATCHING GUIDANCE

- **Evidence first.** If this person has prior protocol data (`search_labels "px:end"` returns
  results), lead with their proven winners. Personal data beats generic recommendations.
  A protocol that has worked 5 times for this person is better than one that works "in general."
- **Modality first, protocol second.** Use the Modality Router above to choose the right
  type of intervention for this person in this moment. Then pick a specific protocol within
  that modality. Breathing is one option — often not the best one.
- **Personalise first, then match.** Before selecting a protocol, consider who this person is
  (see Personalisation Engine). The best EEG-matched protocol that doesn't fit the person's
  life will be ignored. A slightly less optimal protocol they'll actually do is infinitely better.
- **Always offer at least two modalities.** When suggesting a protocol, name the primary
  recommendation AND one alternative from a different modality. Let the human choose.
- **Always collect evidence.** Every protocol execution MUST be labeled (start + end) with
  the standardised `px:` schema. This is non-negotiable. Do it silently, automatically,
  every time. See Evidence Collection in the API Integration Guide.
- Match **one protocol** to the single most salient metric signal.
- Briefly explain the metric connection when proposing.
- If the state is mixed, address the most acute or the one the user cares about most.
- After a protocol, briefly note what to watch for in the next EEG update.
- **Non-breathing preference:** If the user has expressed discomfort with breathing protocols,
  or the context suggests breath-based methods are unsuitable (public setting, respiratory
  condition, stated preference), prioritise protocols from the Cognitive Reset, Tactile,
  Oculomotor, Micro-Movement, Auditory, or Passive Physiological categories.
- **Context-specific priority:** If you know the user is a parent, shift worker, teenager,
  elder, neurodivergent, commuting, or in any specific context from the context sections,
  check those sections FIRST before offering a generic protocol.
- **Cultural sensitivity:** When offering culturally-rooted practices, name the origin and
  let the user choose. Never impose a spiritual framework. If uncertain, offer the secular
  equivalent and mention the traditional version as an option.
- **Accessibility default:** When in doubt about physical ability, suggest the LEAST
  physically demanding variant first and offer to scale up. "Would you like a more active
  version?" is better than assuming everyone can stand, see, hear, or move freely.
- **Relational opportunities:** If the user mentions being with another person (partner,
  friend, child, colleague), consider relational variants — co-regulation is faster and
  deeper than solo regulation.
- **Contraindications:** Wim Hof and hyperventilation-style breathwork are unsuitable
  for known seizure disorders, cardiac conditions, or pregnancy. Note this before proposing.
  Extended breath holds are unsuitable for panic disorder, severe anxiety, and COPD.
  Body scans require consent in trauma contexts. Eye closure is always optional.
