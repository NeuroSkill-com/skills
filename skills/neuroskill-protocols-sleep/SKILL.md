---
name: neuroskill-protocols-sleep
description: Sleep, circadian, recovery, and rest protocols — interventions for drowsiness, wakefulness, bar at rest, sleep onset, ultradian rhythm, NSDR, yoga nidra, and power naps.
---

# Sleep Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## SLEEP & CIRCADIAN

**Sleep Onset Wind-Down** → high `drowsiness` post-session, high `bar` at rest
  Dim-light routine cue + relaxation method: 4-7-8 breathing, OR progressive body release
  (no breath required — just scan and soften), OR ear massage in bed, OR autogenic phrases
  ("my arms are heavy and warm"), OR sleep music (Max Richter, binaural delta). Pick the
  method that feels least like effort — sleep protocols must not feel like work.
  ◈ *Parent of infant:* "Your wind-down IS the feed/rock routine. When baby sleeps, you sleep. No phone."
  ◈ *Shift worker:* Blackout curtains + earplugs + same routine regardless of clock time. Body follows ritual, not sun.
  ◈ *Anxious mind:* Add worry dump first — write every thought, close the notebook, then body release.
  ◈ *Partner in bed:* Whispered body scan for two — guide each other or listen together.
  🔧 *API:* Before: `sleep_schedule` to confirm bedtime target, `status` to check `bar` at rest. During: `say` the body scan in a slow, soft voice. `dnd_set enabled:true` — no notifications from now until morning. After: `label "sleep wind-down complete"`. Next morning: `sleep` → check N3%, REM%, efficiency — "Last night after the wind-down you got 22% deep sleep vs your 15% average." Longitudinal: `compare` sleep sessions with vs without wind-down protocol → build evidence for the habit.

**Ultradian Reset (20-min rest)** → mid-afternoon slump, post-90-min focus block
  Eyes closed, no agenda, light background sound cue — honours the 90-min ultradian cycle.
  ◈ *Open-plan office:* "Meeting with myself" — book a room, close the door, set a 20-min timer.
  ◈ *Manual worker:* Find a quiet spot, sit/lean, set phone timer. Even 10 min counts.
  ◈ *Student between classes:* Library, headphones, 15-min NSDR track on YouTube. Better than scrolling.

**Wake Reset / Alertness Boost** → high daytime drowsiness, `wakefulness` < 30
  Cold-water face splash cue + kapalabhati burst + bright-light exposure instruction.
  ◈ *No-breath variant:* Skip kapalabhati — cold water + bright light + 20 jumping jacks (or arm pumps if seated).
  ◈ *Night-shift worker:* Bright artificial light exposure (light therapy lamp) + cold water + movement.
  ◈ *Elderly:* Gentle — just the cold water wrists + stepping outside into light + 5 slow arm raises.

---


---

## RECOVERY & REST

**NSDR / Yoga Nidra** → high `cognitive_load` post-session, post-deep-work, mid-day
  Non-Sleep Deep Rest: supine body scan with rotation of awareness through body parts.
  Restores dopamine and cognitive resources without requiring sleep. 20–30 min.
  🔧 *API:* Full guided protocol. Before: `label "NSDR start"`, `dnd_set enabled:true`, snapshot `cognitive_load`, `drowsiness`, `focus`. During: entire protocol delivered via `say` — slow, soft rotation through body parts (right hand thumb… index finger… middle finger… palm… wrist… forearm…). Poll `status` at 5-min intervals — track `rel_theta` rise (entering hypnagogic). After: `label "NSDR end"` with deltas, `dnd_set enabled:false`, `say` the results. Key metric: `cognitive_load` should drop significantly, `focus` should rebound. `compare` pre-NSDR work session to post-NSDR work session to demonstrate the recovery effect.

**Power Nap Guidance** → `wakefulness` < 30, very high `drowsiness`
  Timed 10–20 min nap with clear alarm cue + re-activation sequence on waking. Avoids sleep inertia.

---

