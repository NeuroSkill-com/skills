---
name: neuroskill-protocols-routines
description: Morning routines, workout/gym, hydration, bathroom and movement break protocols — daily rituals and exercise-adjacent interventions.
---

# Routines Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## MORNING ROUTINES

*(Offer at session start when `wakefulness` is low, or the user mentions just waking up.)*

**Gentle Morning Wake-Up** → `wakefulness` < 50, mild grogginess
  Supine stretches (cat-cow, knees-to-chest, spinal twist) + activation (5 deep breaths,
  OR 3 deliberate yawns, OR cold water on face, OR vigorous palm-rubbing for warmth) +
  gratitude anchor + intention for the day. Eases cortisol-awakening response. ~10 min.

**Energising Morning Activation** → `wakefulness` < 35, flat `mood`, low `engagement`
  Sun salutation verbal guide (5 rounds) + cold water face splash + activation (kapalabhati
  30 pumps, OR 20 jumping jacks, OR vigorous arm pumps, OR fast march in place for 60 s)
  + 1-minute power posture. The cold water + movement combination is equally effective
  without any breath technique. ~12 min.

**Morning Clarity Ritual** → low `focus` at day start, `cognitive_load` carryover from yesterday
  Brain-dump journal cue → prioritise top 3 → set daily intention → commitment seal
  (3 slow breaths, OR speak the intention aloud once, OR write it on a sticky note and
  place it visible, OR clap once decisively). The seal is a ritual close — any form works. ~8 min.
  🔧 *API:* `status` → show yesterday's `sleep` summary ("You got 6.5 hours, 18% deep sleep"). `sessions` → streak ("Day 12 in a row!"). `label "morning intention: [user's intention]"`. Later: `search_labels "morning intention"` → review what intentions led to the best focus sessions. `compare` mornings-with-ritual vs mornings-without to show the habit's EEG impact.

**Mindful Morning Transition** → low `integration`, emotional residue from sleep/dreams
  Short body scan + 3 things noticed with each sense + gentle forward fold +
  conscious first sip of water or coffee ritual. ~7 min.

---


---

## WORKOUT & GYM PROTOCOLS

*(Offer when the user mentions gym/training. Also relevant post-workout when `hr` or `stress_index` is elevated.)*

**Pre-Workout Neural Primer** → before training; low `engagement` or `wakefulness`
  Activation (10 diaphragmatic breaths, OR cold water face splash, OR 3 deliberate yawns) +
  dynamic mobility (arm swings, hip circles, leg swings, ankle rolls) + CNS excitation burst
  (10 jumping jacks or rapid foot taps) + session intention. The mobility and excitation
  burst are the core — the opening activation modality is flexible. ~8 min.

**Pre-Workout Focus Lock** → before skill/heavy session; needs calm precision
  Calming phase: box breathing (5 rounds), OR ear massage (60 s per ear), OR slow bilateral
  tapping (90 s). Then: single-lift visualisation (feel the movement). Then: arousal spike
  — sharp inhale + explosive exhale, OR palm clap, OR cold water on face, OR isometric
  full-body clench (10 s) + release. ~6 min.

**Intra-Workout Recovery Micro-Set** → between heavy sets; `hr` elevated, high `stress_index`
  Quick recovery: slow exhale-dominant breaths (4 in / 8 out), OR cold water sip + cold
  wrist splash, OR just walk slowly for 60 s + shake out hands and shoulders. Re-set
  intention for next set. The shake-out + intention is the core. ~90 seconds.

**Post-Workout Cool-Down & Integration** → after training; `hr` still elevated, high `lf_hf_ratio`
  5 min slow walk cue + static stretch sequence (quads, hamstrings, hip flexors, chest,
  lats, calves) + parasympathetic activation: cardiac coherence breathing (6 breaths/min,
  5 min), OR humming through each stretch (vagal toning), OR just slow walking + cold
  water on wrists (passive recovery). Hydration + gratitude note. ~15 min.

**Post-Workout Recovery Reset** → after intense training; high `stress_index` + fatigue
  PMR targeting trained muscle groups → NSDR short version (10 min) →
  somatic scan for sharp pain or compensation → rehydration reminder. ~20 min.

**Mind-Muscle Connection Primer** → low `mu_suppression` before technique-focused training
  Motor imagery: vividly imagine the target movement 5 times (feel the contraction,
  not just see it). Activates motor cortex before physical execution. ~5 min.

---


---

## HYDRATION & WATER BREAKS

*(Keep short. Offer proactively after long sessions, elevated `hr` or `cognitive_load`,
or when the user hasn't mentioned drinking in a while.)*

**Hydration Reminder** → any long session, `hr` mildly elevated, dry-mouth mention
  "Time for a full glass of water." Optionally pair with a 60-second standing break.

**Mindful Water Break** → mid-session, high `cognitive_load`, post-stress spike
  Step away, fill a glass, drink slowly — notice temperature, taste, sensation. ~2 min.

**Hydration + Eye Reset** → long screen block, high `spectral_centroid`
  Water + 20-20-20 vision reset back-to-back. Efficient double reset. ~3 min.

---


---

## BATHROOM & MOVEMENT BREAKS

*(Short and practical. Offer after long unbroken sessions or `stillness` > 0.9 for >45 min.)*

**Bathroom Break Prompt** → high `stillness`, long session, restlessness cue
  "Good time for a bathroom break — stand, walk, come back fresh."

**Break + Reset on Return** → after any bathroom or movement break
  Quick reset: 2 standing breaths, OR full-body shake (5 s), OR cold water on wrists, OR
  squeeze fists and release. Then re-read today's intention → sit back down. <60 s.

**Movement Snack** → `stillness` > 0.9 for >45 min, low `engagement`
  Walk to another room and back (30 s), refill water, 10 calf raises. ~2 min.

---

