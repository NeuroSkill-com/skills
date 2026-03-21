---
name: neuroskill-protocols-focus
description: Focus, attention, cognition, consciousness, deep meditation, and energy/alertness protocols — EEG-triggered interventions for tbr, focus, engagement, cognitive_load, lzc, wakefulness, apf, and pac_theta_gamma.
---

# Focus Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## ATTENTION & FOCUS

**Theta-Beta Neurofeedback Anchor** → high `tbr` (>1.5), low `focus`
  Rhythmic counting + breath anchor designed to suppress theta and lift beta.
  ◈ *Child (6–12):* Count colourful animals — "3 blue elephants, 2 red parrots…"
  ◈ *Teen/ADHD:* Speed challenge — count backward from 100 by 7s, beat your last time.
  ◈ *Elder:* Gentle pace, use familiar sequences (days of week backward, months backward).
  🔧 *API:* Before: verify `status → scores.tbr > 1.5`. During: poll `status` every 60s, use `say` to report "Your theta-beta ratio just dropped from 1.6 to 1.3 — keep going". After: `label "neurofeedback anchor complete"` with tbr delta. Longitudinal: `search_labels "neurofeedback"` to track improvement trend.

**Focus Reset** → scattered `engagement`, high `cognitive_load` mid-session
  90-second reset: choose your modality — breath hold + intention, OR eyes-closed + ear massage + intention, OR isometric squeeze (fists 5s, release) + intention. All achieve the same attentional interrupt.
  ◈ *Eyes-open variant (driving, supervising kids):* Fix gaze on one object, soften vision, set intention silently.
  ◈ *Public/meeting variant:* "Bathroom break" — 90 seconds alone, palms on cool surface, one sentence of intention.
  ◈ *No-breath:* Tongue press 10s → release → set intention. Nobody notices.

**Cognitive Load Offload** → `cognitive_load` > 0.7, end of deep work block
  Brain-dump journaling cue + priority sorting — reduces working-memory pressure.
  ◈ *Non-writer variant:* Voice memo — speak every open loop aloud for 60 seconds, then delete the recording.
  ◈ *Parent variant:* Sticky notes on the fridge — one task per note, peel off when done. Physical offload.
  ◈ *Manual worker:* End-of-shift ritual — name 3 things done, 1 thing for tomorrow, walk to car.

**Working Memory Primer** → low `pac_theta_gamma`, pre-task warm-up
  3-back verbal sequence + slow theta-pace breath (inhale 4 s, exhale 6 s, hold 2 s).
  ◈ *Child:* "I went to the shop and bought…" memory chain game.
  ◈ *No-breath variant:* Pure N-back — tap the desk every time you hear a repeated number in a spoken sequence.

**Creativity Unlock** → high `beta`, low `rel_alpha`, creative block
  Mind-wandering permission: eyes soft-open, no goal, follow any thought loosely 5 min.
  ◈ *Structured variant (for those uncomfortable with "no goal"):* Doodle on paper, let the hand move freely.
  ◈ *Movement variant:* Walk without destination — let feet choose turns. Works outdoors or in a building.
  ◈ *Musical variant:* Play, hum, or listen to unfamiliar music with no analysis.

**Pre-Performance Activation** → low `engagement` before a presentation or challenge
  Power posture + activation + single positive-outcome visualisation. Activation options:
  fast breathing (3 sharp inhale-exhales), OR 10 rapid arm pumps, OR vigorous hand
  clapping (5 s), OR cold water on face/wrists. Choose what fits the setting.
  ◈ *Athlete:* Sport-specific motor imagery — feel the movement, not just see it.
  ◈ *Student before exam:* "You already studied. Recall one thing you know cold. That's your anchor."
  ◈ *Introvert before socialising:* Visualise one specific warm moment with someone you trust. Carry that feeling in.
  ◈ *Wheelchair user:* Arms wide, chest open, chin up — power posture is about expansion, not standing.
  ◈ *No breath, no movement:* Pure visualisation — vividly imagine the successful outcome for 60 seconds. Mental rehearsal alone activates motor and reward circuits.

---


---

## COGNITIVE PERFORMANCE & MOTIVATION

**WOOP / Mental Contrasting** → low engagement before a task, pre-challenge motivation dip
  Wish → Outcome → Obstacle → Plan. 5-min imagination exercise. Improves follow-through.

**Cognitive Defragging** → high `spectral_centroid` + `cognitive_load` + context-switching
  Structured unloading: speak or write every open loop, then categorise and park each.

**Dual-N-Back Warm-Up** → low `pac_theta_gamma` + low `sample_entropy` (rigid patterns)
  Spoken-number 2-back tracking. 3 min. Lifts theta-gamma coupling before demanding work.

**Novel Stimulation Burst** → low `apf` (<9 Hz), cortical slowing
  Name 5 unusual objects, recall an unexpected memory, solve a riddle. Combats cortical slowing.

---


---

## CONSCIOUSNESS & INTEGRATION

**Coherence Building** → low `coherence` (<0.4), low `integration`
  Synchronised attention on heartbeat — promotes cross-region coupling. Pair with: slow
  breathing, OR humming/toning (auditory), OR rhythmic bilateral tapping, OR slow hand
  warming visualisation. The heartbeat attention is the active ingredient; the pairing
  modality is flexible.

**Flow State Induction** → mid-range `focus` (0.5–0.7) + `engagement` rising
  Challenge-skill calibration cue + single-task commitment + distraction clearing ritual.
  🔧 *API:* Before: `status` → confirm sweet spot (`focus` 0.5–0.7 + `engagement` rising). `dnd_set enabled:true`. `label "flow induction start"`. During: do NOT poll frequently — let them flow. One silent `status` check at 15 min. After: `label "flow state end"` with `focus`, `engagement`, `lzc`, `coherence`. Over time: `search "flow"` to find neural signature → `hooks_set` to alert when conditions are ripe: "Your brain is entering the flow zone — protect this time." `umap` to visualise flow sessions vs distracted sessions in 3D.

**Complexity Expansion (LZC boost)** → low `lzc`, low `integration`, cognitive rigidity
  Rapid alternating-attention task (sound / visual / body) — forces neural variety.

---


---

## DEEP MEDITATION

**Alpha-Theta Drift** → low `lzc` + moderate `drowsiness`, trauma integration, deep creativity
  Eyes closed, allow attention to drift between alpha and theta. No effort to stay focused.
  Reaches the integrative border between conscious/unconscious.

**Mantra / Single-Point Focus** → high `rel_theta` + low `focus`, monkey-mind, verbal chatter
  One internally repeated syllable (e.g. "so-hum") timed to breath. Quiets DMN chatter.

**Gamma Entrainment (40 Hz)** → low `integration`, low `rel_gamma`
  Sustained 40 Hz humming or audiovisual cue. Promotes gamma synchrony. 3–5 min sessions.

---


---

## ENERGY & ALERTNESS

**Kapalabhati Energiser** → low `bar`, low `engagement`, sluggish cognition, low `wakefulness`
  20–30 rapid forceful exhales (passive inhale). Brief — powerful alertness lift.
  ◈ *Non-breath energisers with equal alertness effect:* Cold water on face/wrists (fastest). 20 jumping jacks or rapid arm pumps. Vigorous hand clapping for 10 seconds. Rapid alternating nostril tapping (without breathing pattern). Chewing ice. Any of these spike cortical arousal without breath technique.

**4-Count Energising Breath** → low `engagement`, post-lunch dip, low `wakefulness`
  Inhale 4, hold 4, exhale 4, hold 4 — brisk pace (~1 s per count) to activate.
  ◈ *Non-breath alternative:* Isometric full-body clench (every muscle for 10 s, release, repeat 3×). Or: stand up, march in place for 30 seconds, sit back down. Or: 3 deliberate yawns (counterintuitively activating). Or: chew gum vigorously for 2 minutes.

**Wim Hof Breathwork** → near-zero `engagement`, very low `wakefulness`, full reset needed
  30 deep power breaths, breath retention, recovery breath. Dramatic alertness + mood lift.
  ⚠ Not for known seizure disorders, cardiovascular concerns, or pregnancy.
  ◈ *Non-breath full reset:* Cold water face immersion 30 s (dive reflex — equally dramatic autonomic shift). Or: bright light therapy lamp + 20 jumping jacks + cold water. The breath-free route takes slightly longer but avoids all contraindications.

**Cold Exposure Micro-Protocol** → low `wakefulness`, autonomic torpor, low `bar`
  Guided cold-water face/wrist immersion cue (20–30 s) — sympathetic spike then parasympathetic rebound.
  ◈ *This IS a non-breath protocol.* Excellent first-line alternative for anyone who can't or won't do breath-based energising. Works for all ages, abilities, and settings with a water source.

---

