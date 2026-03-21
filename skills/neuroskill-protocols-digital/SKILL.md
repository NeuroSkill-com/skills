---
name: neuroskill-protocols-digital
description: Social media, digital addiction, screen time, and attention restoration protocols — interventions for doom-scrolling, phone compulsion, FOMO, comparison, dopamine reset, and digital sunset.
---

# Digital Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## SOCIAL MEDIA & DIGITAL ADDICTION PROTOCOLS

*(Never shame. Approach with curiosity and warmth. Post-scroll EEG often shows low `focus`,
low `lzc`, scattered `engagement`, crashed `faa` or `mood`.)*

**Pre-Scroll Intention Check** → before opening any social media app
  Pause (3 breaths, OR tongue press 10 s, OR just close your eyes for 5 seconds).
  Ask: why am I opening this? Set a time limit (10 min) and exit cue. ~1 min.
  The pause is the intervention — the modality of the pause doesn't matter.

**Craving Surf (Urge Surfing)** → compulsive urge to check phone, dopamine craving spike
  Observe the urge. Locate in body. Rate 0–10. Watch it rise and fall without acting.
  Most urges peak in 90 seconds if not fed. ~3 min.

**Post-Scroll Brain Reset** → after unintended long scroll; low `focus`, low `lzc`, mood crash
  Screen away. Reset: 5 slow breaths, OR stand up and shake your whole body for 10 seconds,
  OR cold water on face, OR just look at the farthest thing you can see for 30 seconds.
  Body scan — notice quality of mind. 2-min outdoor/window gaze. One deliberate
  re-engagement with something meaningful. ~8 min.

**Comparison Detox** → post-social-media, low `mood` + negative `faa`
  Name what triggered the comparison. Envy Alchemy reframe. Write one real, unperformable
  good thing in your own life. ~5 min.

**Dopamine Palette Reset** → habitual phone checking, low baseline `engagement`, low `mood`
  Short-form content spikes dopamine → raises hedonic baseline → real life feels flat.
  Identify 3 sustained-reward activities. Commit to one today. No screen. ~5 min to set intention.

**Notification Detox** → high context-switching, low `focus`, attention fragmented
  Silence non-essential notifications, move social apps off home screen, phone face-down.
  Frame as an experiment, not a permanent rule. ~3 min.

**Mindful Social Media Session** → user wants to use social media intentionally
  Set timer (10–15 min max). Choose one specific purpose. Close when done or timer fires.
  Notice how you feel afterward — better or worse? ~15 min capped.

**FOMO Defusion** → worry about missing out, compulsive checking, high `bar`
  Name the fear precisely. Examine actual probability and stakes. Offer a rational
  counter-statement. Ground back to present with sensory anchor. ~5 min.

**Digital Sunset Protocol** → elevated `bar` at rest, pre-sleep phone use
  Hard stop on all screens 60–90 min before sleep. Replace with dim light + music + paper.
  Guide setup of phone Downtime / Digital Wellbeing schedule. ~5 min setup + 60 min offline.
  🔧 *API:* `sleep_schedule` → get bedtime. Set a `hooks_set` rule: 90 min before bedtime, if `bar > 0.5`, trigger `notify "Digital Sunset"` + `say "Time to put screens away. Your sleep starts now."`. `dnd_set enabled:true`. `label "digital sunset"`. Next morning: `sleep` → compare nights with digital sunset vs without. Over time: build the data case that screens before bed cost measurable deep sleep.

**Attention Restoration Walk** → post-scroll, low `lzc`, attention depleted
  Go outside. No phone, no podcasts, no agenda. Let eyes move naturally.
  Attention Restoration Theory: nature restores directed attention. Even 10 min helps. ~10–20 min.

**Values Reconnection** → persistent comparison spiral, low `mood`, inadequacy after scrolling
  Social media optimises for envy. Counter by reconnecting to personal values.
  Ask: what kind of person am I trying to be? One small aligned action now. Write it down. ~8 min.

**Screen Time Reflection** → end of day; user curious about usage patterns
  Invite check of screen time stats (Settings → Screen Time / Digital Wellbeing).
  Review without judgment. Ask: does this match how you want to spend attention? ~5 min.

---

