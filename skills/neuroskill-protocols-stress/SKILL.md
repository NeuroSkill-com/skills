---
name: neuroskill-protocols-stress
description: Stress, relaxation, autonomic regulation, HRV, hemispheric balance, and deep relaxation protocols — EEG-triggered interventions for bar, stress_index, relaxation, rmssd, lf_hf_ratio, faa asymmetry, and vagal tone.
---

# Stress Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## STRESS & AUTONOMIC REGULATION

**Box Breathing (4-4-4-4)** → high `bar`, low `relaxation`
  Classic sympathetic brake. Four equal phases; expand to 5-5-5-5 if well-tolerated.
  ◈ *Breath-averse:* Replace with Ear Massage or Bilateral Tapping from non-breathing sections.
  ◈ *Child:* "Smell the flower (in), blow out the candle (out)" — make it vivid and visual.
  ◈ *Asthma/respiratory condition:* Shorter counts (2-2-2-2), no breath holds. Or skip to tactile.
  ◈ *Crowded space:* Breathe normally but count silently — the counting alone helps.
  🔧 *API:* Before: `status` → confirm `bar > 0.5`, snapshot `relaxation` baseline, `label "protocol start: box breathing" --context "bar=X, relaxation=X"`. During: `say "Inhale... 2... 3... 4..."` for each phase, `dnd_set enabled:true`, poll `status` at 60s/120s to track `rmssd` rise. After: `status` → compare to baseline, `label "protocol end: box breathing" --context "bar delta, relaxation delta"`, `say` the results, `notify` summary. After 5 sessions: `hooks_suggest "box breathing,calm"` → create auto-trigger hook for when `bar` spikes again.

**Extended Exhale (4-7-8)** → acute stress spike, high `lf_hf_ratio`
  Fastest breath-based parasympathetic trigger. Short inhale, long hold, very long exhale.
  ◈ *Panic/hyperventilation risk:* Skip the hold. Just make the exhale twice as long as the inhale (3-0-6).
  ◈ *Pregnant:* No extended holds — use 4-0-8 (no hold) variant.
  ◈ *Non-breath alternatives with equal parasympathetic speed:* Cold Water Face Splash (Dive Reflex) — fastest non-breath option, 10–15 seconds. Bilateral Tapping — 60 seconds. Gargling — 30 seconds. Any of these achieve parasympathetic activation without breath control.

**Cardiac Coherence (~6 breaths/min)** → low `rmssd` (<30 ms), high `stress_index`, low HRV
  5-second inhale / 5-second exhale, 5 minutes. Maximises HRV and vagal tone.
  ◈ *Music-paired variant:* Use a track at 60 BPM as a breath pacer instead of counting.
  ◈ *Tech-assisted:* Many HRV apps (e.g. Elite HRV, Welltory) show a visual breath pacer.
  🔧 *API:* This protocol has the clearest measurable outcome — `rmssd` should rise. Before: snapshot `rmssd`, `stress_index`. During: `say` each inhale/exhale phase, poll `status` at 60s intervals — report `rmssd` changes verbally ("Your heart rate variability is climbing — from 28 to 36"). After: `label` with full HRV delta. Longitudinal: `compare` sessions with coherence vs without — build the evidence that it works for this person.

**Physiological Sigh** → rapid stress onset, overwhelm
  Double inhale through nose + long slow exhale. 1–3 cycles only.
  ◈ *Best "stealth" breath technique:* Looks like a natural sigh. Usable in meetings, on camera, on a date.
  ◈ *Child:* "Sniff-sniff like a bunny, then blow out like you're cooling soup."
  ◈ *If breath control feels impossible right now:* Press tongue to palate hard for 10 seconds (same vagal pathway, zero breath involvement). Or squeeze an ice cube / hold a cold can. Or press both feet into the floor as hard as you can for 10 seconds. The goal is parasympathetic activation — breath is one route, not the only route.

---


---

## RELAXATION & ALPHA PROMOTION

**Alpha Induction** → high `beta`, high `bar`, low `relaxation`, post-stress
  Eyes-closed, open-focus attention (notice the space around objects). Lifts `rel_alpha`.
  🔧 *API:* Prime feedback protocol. Before: snapshot `rel_alpha` and `bar`. During: `say` guidance, poll `status` every 30s — when `rel_alpha` rises, give verbal feedback ("Alpha just jumped — your brain is letting go"). This real-time feedback accelerates the learning. After: `label` with alpha delta. `notify "Alpha up X%"`. Longitudinal: `compare` rest sessions to track alpha trend over weeks.

**Open Monitoring** → low `lzc` (<40), low `integration`, mental narrowing
  Non-directed awareness — let thoughts and sounds arise without following. Raises LZC.

**Relaxation Scan** → high cortical arousal, `headache_index` > 30
  Slow progressive softening: scalp → jaw → shoulders → hands → belly.

---


---

## AUTONOMIC & VAGAL

**Vagal Toning (Humming / Gargling)** → low `rmssd` (<25 ms), low HRV, high `stress_index`
  Sustained audible humming on exhale (30–60 s) or vigorous gargling.
  Directly stimulates the vagus nerve via laryngeal vibration.

---


---

## DEEP RELAXATION (SOMATIC)

**Autogenic Training** → chronic physical tension, high `stress_index`, difficulty releasing
  Self-hypnotic phrase repetition: warmth and heaviness in limbs, heartbeat calm,
  breathing free, abdomen warm, forehead cool. ~15 min.

**Havening Touch** → acute emotional distress spike, trauma activation
  Gentle self-administered touch on face, arms, palms while recalling and down-grading
  distressing content. Disrupts the amygdala-cortex encoding loop.

**Somatic Shaking** → post-adrenaline state, stored tension after stress spike
  Intentional full-body shaking for 2–3 min. Distinct from TRE (no posture induction).

---


---

## HEMISPHERIC BALANCE & BREATHING

**Nadi Shodhana (Alternate Nostril)** → FAA asymmetry (`|faa|` > 0.1), low `coherence`
  Alternating nostril breath at ~6-second cycles. Targets left-right frontal balance.

**Buteyko CO2 Retraining** → habitual over-breathing, high `lf_hf_ratio`
  Reduced-volume nasal breathing + air-hunger training. Raises CO2 tolerance.

---

