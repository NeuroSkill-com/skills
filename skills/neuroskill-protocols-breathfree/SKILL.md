---
name: neuroskill-protocols-breathfree
description: Non-breathing alternative protocols — cognitive, tactile, oculomotor, micro-movement, auditory, and passive physiological interventions for users who find breathwork inconvenient, uncomfortable, or inaccessible.
---

# Breathfree Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## COGNITIVE RESET (NO BREATH)

*(For users who find breathing protocols inconvenient, awkward in public, or physically
uncomfortable. Pure cognitive engagement — no breath timing required.)*

**Micro-Tasking Focus Drill** → low `focus`, high `tbr`, scattered attention
  Count backward from 100 by 7s, or name objects in a category (animals A→Z).
  Pure cognitive engagement that lifts beta without any breathing instruction. ~2 min.
  🔧 *API:* Before: `status` → confirm `focus < 0.4` and `tbr > 1.3`. `label "focus drill start"`. During: `say` the challenge prompt, `timer` for 2 min. After: `status` → report `focus` change. `label "focus drill end"` with delta. Over time: `search_labels "focus drill"` → track which drills produce the best focus lift for this specific person.

**Peripheral Vision Expansion** → high `beta`, tunnel vision, acute stress, high `bar`
  Soft-gaze at a fixed point while noticing the extreme edges of your visual field.
  Activates alpha, widens attentional aperture, reduces stress-induced narrowing. ~3 min.

**Mental Arithmetic Sprint** → low `engagement`, low `focus`, pre-task warm-up
  60-second rapid calculation challenge (e.g. multiply pairs of 2-digit numbers).
  Engages prefrontal cortex, suppresses rumination. Gamifiable with difficulty scaling. ~1–2 min.

**Internal Narration Silence** → high `cognitive_load`, verbal chatter, racing thoughts
  Try to produce zero internal speech for 30 seconds. Notice when words arise, let them
  dissolve. Extremely simple, surprisingly hard. Quiets the default mode network. ~1–3 min.

**Three-Word State Capture** → unknown/mixed EEG, session opening, emotional ambiguity
  Describe your current state in exactly 3 words. Forces crystallisation of vague feelings.
  Can be tracked across sessions for trend spotting. ~30 seconds.

**Micro-Gratitude (One Thing)** → low `mood`, low `faa`, negativity bias
  Name one specific thing you are grateful for right now. Not a list — one vivid, detailed
  thing. Engages approach circuits, lifts left-frontal alpha. ~10–30 seconds.

**Mental Time Travel** → low `engagement`, procrastination, motivational dip
  Vividly imagine yourself 1 hour from now having completed the current task. What does
  it feel like? Prospective imagery activates reward anticipation circuits. ~2 min.

**Worry Parking Lot** → high `cognitive_load`, high `bar`, open-loop anxiety
  Write or mentally place each worry on a "sticky note" and park it. Promise to return
  later. Reduces working-memory pressure without suppression. ~3 min.

**Novel Stimulation Burst (Enhanced)** → low `apf` (<9 Hz), cortical slowing, mental fog
  Name 5 unusual objects in the room, recall an unexpected childhood memory, solve a riddle,
  spell a word backward. Combats cortical slowing through surprise and novelty. ~3 min.

**Category Switching Drill** → low `lzc`, cognitive rigidity, low `sample_entropy`
  Alternate between two categories every 5 seconds (fruit, country, fruit, country…).
  Forces rapid set-shifting. Raises neural complexity and breaks perseverative loops. ~2 min.

---


---

## TACTILE & HAPTIC REGULATION

*(Discreet, silent, office-safe. Leverage somatosensory cortex activation and vagal
stimulation through touch rather than breath.)*

**Fingertip Texture Scanning** → high `beta`, restlessness, difficulty grounding
  Slowly run fingertips across different surfaces — desk grain, fabric weave, skin of
  forearm. Full attention on micro-texture. Activates somatosensory cortex, anchors
  attention to the present moment. Completely discreet in meetings. ~2–3 min.

**Pressure Point Self-Massage (LI-4)** → high `stress_index`, tension headache, high `bar`
  Squeeze the fleshy web between thumb and index finger (acupoint LI-4) firmly for
  30 seconds per hand. Or press the bony ridge behind the ear (mastoid process).
  Vagal stimulation and pain-gate distraction without any breathing cue. ~2 min.

**Temperature Contrast Object** → acute stress spike, panic onset, high `lf_hf_ratio`
  Hold a cold object (metal water bottle, ice cube, cold can) for 20–30 seconds.
  Fast sympathetic-parasympathetic toggle via thermal receptor activation.
  Follow with a warm object (mug) if available for rebound relaxation. ~1–2 min.

**Bilateral Tapping (Butterfly Hug)** → high `bar`, emotional charge, anxiety, PTSD activation
  Cross arms over chest, alternately tap shoulders at ~1 Hz for 60–90 seconds.
  Bilateral stimulation (used in EMDR) calms amygdala reactivity and lowers arousal.
  Completely silent, can be done under a jacket or blanket. ~2 min.

**Structured Fidget Protocol** → restlessness, high `mu_suppression`, low `focus`
  Roll a smooth stone, use a spinner with full attention, or squeeze a stress ball
  in rhythmic patterns (squeeze 3 s, release 3 s). Channels motor restlessness into
  a proprioceptive anchor without disrupting cognitive task. ~3–5 min.

**Palm Press Discharge** → anger spike, frustration, high `stress_index` + high `bar`
  Press palms firmly together in front of chest for 10 seconds, maximum effort.
  Release and notice the rush of blood and warmth. Repeat 3 times.
  Isometric discharge of fight-response energy. ~1 min.

**Hand Warming Visualisation** → cold hands (vasoconstriction from stress), high `lf_hf_ratio`
  Cup hands together. Imagine warmth flowing into palms — a candle flame, warm sand, sunlight.
  Biofeedback research shows hand temperature rises measurably with focused attention.
  Shifts autonomic balance toward parasympathetic. ~3 min.

---


---

## OCULOMOTOR & VISUAL

*(Eyes are a direct window to the autonomic nervous system. Eye movements modulate arousal,
attention, and emotional processing without any breath component.)*

**Saccade Reset** → emotional charge, rumination, high `faa` asymmetry, anxiety
  Rapid horizontal eye movements: look far left, then far right, 1 second each.
  30 seconds total (15 cycles). Similar mechanism to EMDR — reduces emotional
  valence of intrusive thoughts and resets attentional networks. ~1 min.

**Candle Gaze (Trataka)** → low `focus`, high `tbr`, scattered attention, monkey mind
  Stare at a fixed point (dot on screen, candle, LED, pen tip) without blinking for
  as long as comfortable (30–90 s). When eyes water, close them and hold the afterimage.
  Ancient concentration practice. Dramatically lifts sustained attention. ~3–5 min.

**Colour Hunting** → low `engagement`, flat `mood`, attentional fatigue
  Pick a colour. Find every instance of it in your environment within 60 seconds.
  Forces present-moment external attention. Mildly playful — engages reward circuits.
  Switch colours for round two. ~2 min.

**Slow Visual Tracking (Figure-8)** → high `beta`, hyperarousal, racing thoughts
  Hold one finger at arm's length. Move it slowly in a horizontal figure-8 (infinity symbol).
  Follow with eyes only, head still. Smooth pursuit activates cerebellar circuits and
  down-regulates cortical hyperarousal. ~2–3 min.

**Panoramic Vision (Optic Flow)** → acute stress, tunnel vision, high `bar`, panic
  Deliberately widen your gaze to take in the full visual field without moving your eyes.
  Panoramic (ambient) vision activates the parasympathetic system — the opposite of the
  stress-induced foveal narrowing. Can be done while walking for amplified effect. ~2–3 min.

**Near-Far Focus Shifting** → eye fatigue, high `spectral_centroid`, >90 min screen time
  Hold thumb 15 cm from face, focus on thumbnail for 5 seconds. Shift focus to the
  farthest visible point for 5 seconds. Repeat 10 times. Relieves ciliary muscle tension
  and resets visual processing pathways. ~2 min.

---


---

## MICRO-MOVEMENT (DISCREET)

*(Minimal, office-safe movements that reset arousal, break stillness patterns,
and re-engage the motor cortex — no mat, no standing, no attention from colleagues.)*

**Isometric Squeeze-Release** → high `beta`, physical tension, high `bar` at rest
  Clench both fists as hard as possible for 5 seconds. Release completely. Feel the
  contrast. Repeat with quads (press feet into floor), glutes (squeeze), shoulders
  (shrug and hold). Progressive muscle relaxation — seated, silent, invisible. ~3 min.

**Jaw Release Protocol** → high `stress_index`, teeth clenching, TMJ tension, headache
  Place tongue tip on the roof of mouth behind front teeth. Let jaw drop open under
  its own weight. Hold 30 seconds. Gently move jaw side to side. The jaw is the body's
  primary stress-holding muscle — releasing it cascades relaxation downward. ~1–2 min.

**Micro-Walk (30 Steps)** → `stillness` > 0.9 for >30 min, low `engagement`, stagnation
  Walk exactly 30 steps with full attention on the sensation of foot contacting floor.
  Heel, ball, toes. Can be done in a hallway, to the bathroom, or even marching in place.
  Movement-based attentional reset. ~1–2 min.

**Seated Spinal Wave** → `stillness` > 0.85, low `engagement`, postural compression
  Sitting upright, slowly round the spine forward (cat), then arch backward (cow).
  Move one vertebra at a time, bottom to top. 5 slow cycles. Resets proprioceptive
  awareness and gently activates the sympathetic chain along the spine. ~2 min.

**Wrist and Ankle Circles** → prolonged typing, high `mu_suppression`, low `engagement`
  10 slow circles each direction: both wrists, then both ankles. Activates distal motor
  cortex areas, increases peripheral blood flow, and breaks repetitive-strain patterns.
  Completely invisible under a desk. ~1–2 min.

**Shoulder Blade Pinch** → forward-head posture, high `stillness`, desk tension
  Squeeze shoulder blades together and hold for 5 seconds. Release. Repeat 5 times.
  Opens the chest, counters kyphotic posture, and activates thoracic erector spinae.
  Can be done in any chair without anyone noticing. ~1 min.

**Single-Leg Floor Press** → restless legs, need for motor activation without standing
  Press one foot firmly into the floor for 10 seconds. Switch. Repeat 3 times per side.
  Isometric leg activation that discharges restless energy and improves grounding. ~2 min.

---


---

## AUDITORY (NON-MUSIC)

*(Sound-based protocols that don't require music apps, playlists, or special equipment.
Pure listening and vocalisation.)*

**Sound Mapping** → high `beta`, racing thoughts, difficulty externalising attention
  Close eyes for 60 seconds. Count and mentally locate every distinct sound — traffic,
  HVAC hum, keyboard clicks, birdsong, your own heartbeat. Shifts attention from internal
  rumination to external sensory processing. Lifts alpha, lowers beta. ~1–2 min.

**Humming (Unstructured)** → low `rmssd`, high `stress_index`, vagal tone needed
  Just hum. Any pitch, any melody, any duration. No counting, no timing, no technique.
  Laryngeal vibration directly stimulates the vagus nerve. More accessible than structured
  breathing. Can be done quietly enough to be inaudible to others. ~2–5 min.

**Toning (Single Note Hold)** → low `coherence`, scattered neural activity, low `integration`
  Sustain a single comfortable note ("ohhh" or "ahhh") for as long as feels natural.
  Repeat. The sustained vibration promotes cross-region neural synchronisation.
  Deeper than humming, more accessible than mantra. ~3–5 min.

**Environmental Sound Bath** → high `cognitive_load`, over-stimulated, need for passive reset
  Open a window or step outside. Do nothing except listen for 3 minutes. No agenda,
  no sound-counting, no analysis. Pure passive auditory immersion. The brain's auditory
  cortex handles the rest, gently lowering cortical arousal. ~3 min.

**Whisper Reading** → high `cognitive_load`, verbal working-memory overload
  Read any available text (email, book, label) in a barely audible whisper. Engages
  articulatory motor cortex at low activation, creating a gentle cognitive anchor that
  displaces internal chatter without high effort. ~2–3 min.

**Rhythmic Tapping (Self-Generated)** → freeze response, dissociation, very low `engagement`
  Tap a steady rhythm on your desk or thigh — slow (60 BPM), then gradually slower.
  Rhythmic predictability down-regulates via entrainment. No music needed, no equipment.
  The act of generating rhythm is more regulating than passively hearing it. ~2–3 min.

---


---

## PASSIVE PHYSIOLOGICAL

*(Protocols that leverage automatic physiological reflexes. Require minimal effort
or cognitive engagement — the body does the work.)*

**Cold Water Face Splash (Dive Reflex)** → acute stress, panic, very high `hr`, high `lf_hf_ratio`
  Splash cold water on forehead, eyes, and cheeks (or hold a cold wet cloth there).
  Triggers the mammalian dive reflex: instant heart rate drop of 10–25%, blood
  pressure redistribution, parasympathetic surge. 10–15 seconds is sufficient. ~30 s.

**Deliberate Yawning** → low `wakefulness`, cortical overheating, mental fatigue
  Force 3 deliberate yawns — open mouth wide, stretch the jaw, let sound come out.
  Counterintuitively effective: yawning increases cortical blood flow, cools the brain,
  and resets arousal. Often triggers genuine yawns after 1–2 forced ones. ~1 min.

**Chewing Protocol** → low `engagement`, low `wakefulness`, post-lunch dip, mild anxiety
  Chew gum (or a crunchy snack) for 3 minutes. Research shows chewing reduces cortisol,
  improves alertness, and increases cerebral blood flow. The rhythmic jaw movement is
  inherently regulating. Extremely low effort, no technique required. ~3 min.

**Gargling (Vagal Activation)** → low `rmssd`, low HRV, high `stress_index`, vagal tone needed
  Gargle water vigorously for 30 seconds. The gag-reflex proximity and laryngeal muscle
  activation directly stimulate the vagus nerve. More potent than humming for vagal tone.
  Do in a bathroom for social comfort. ~1 min.

**Splashing Cold Water on Wrists** → moderate stress, need quick discreet reset
  Run cold water over inner wrists for 20–30 seconds. Major blood vessels are close to
  the surface here — rapid cooling effect. More socially acceptable than face splash.
  Can be done at any sink without explanation. ~30 s.

**Ear Massage (Auricular Vagal Stimulation)** → high `stress_index`, need passive vagal activation
  Gently massage the outer ear, especially the tragus (the small flap covering the ear canal)
  and the concha (the bowl-shaped area). The auricular branch of the vagus nerve runs here.
  Slow, firm circular pressure for 60 seconds per ear. Completely silent and discreet. ~2 min.

**Tongue Press (Palate Activation)** → anxiety, restlessness, need invisible calming technique
  Press the tongue firmly against the roof of the mouth for 10 seconds. Release.
  Repeat 5 times. Activates the palatine branch of the vagus and engages deep neck flexors.
  Invisible, silent, can be done mid-conversation. ~1 min.

**Postural Reset (Gravity Drop)** → high `bar` at rest, accumulated tension, high `stillness`
  Stand up. Let arms hang completely limp. Drop chin to chest. Bend forward very slowly,
  one vertebra at a time, letting gravity pull you down as far as comfortable. Hang for
  20 seconds. Roll up slowly. Gravity does all the work — no effort, no technique. ~2 min.

---

