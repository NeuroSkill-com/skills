---
name: neuroskill-protocols-emotions
description: Emotional regulation, mood, and extended emotional processing protocols — interventions for faa, mood, anger, grief, shame, fear, envy, joy, loneliness, resentment, excitement, and emotional boundaries.
---

# Emotions Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## EMOTIONAL REGULATION & MOOD

**FAA Rebalancing** → negative `faa`, low `mood`
  Approach-motivation imagery + left-hand squeeze. Activates left frontal alpha.
  ◈ *Imagery should match the person:* A chef imagines a perfect dish. An athlete imagines a winning moment. A parent imagines their child laughing. Use THEIR approach motivation, not a generic one.

**Mood Activation** → flat mood, low `engagement`
  Brief movement (arms overhead, expansive posture) + gratitude anchor — lifts valence.
  ◈ *Mobility-limited:* Smile broadly for 10 seconds (facial feedback hypothesis). Or hum an upbeat tune.
  ◈ *Depressed/can't "just be grateful":* Skip gratitude. Instead: name one thing that didn't go wrong today. Lower the bar.
  ◈ *Elderly:* Gentle arm raises only to comfortable range. Gratitude anchor to a cherished memory.

**Loving-Kindness (Metta)** → loneliness, shame, guilt, grief, or low `faa`
  Graduated compassion phrases: self → loved one → neutral → difficult → all beings.
  ◈ *Secular framing:* "Think of someone you care about. Wish them well silently. Now do the same for yourself."
  ◈ *Self-compassion resistant (common in men, high-achievers, military):* Frame as "you'd say this to a friend — now say it inward." Skip the word "compassion" if it triggers resistance.
  ◈ *Child:* "Send a warm thought to your best friend. Now to your pet. Now to yourself."
  ◈ *Culturally adapted:* In collectivist cultures, start with family/community before self — the sequence matters.
  🔧 *API:* Before: check `status → scores.faa` (negative = withdrawal). During: `say` each phrase warmly ("May you be safe… may you be happy…"), `dnd_set enabled:true`. Track `faa` shift mid-protocol — if it turns positive, note it. After: `label "metta meditation"` with `faa` delta + `mood` delta. Over time: `search_labels "metta"` → correlate `faa` improvement across sessions → "Your frontal asymmetry shifts positive more quickly now than it did 2 weeks ago."

**Emotional Discharge** → extreme FAA swings, agitation
  Brief vigorous breath (30 s), followed by stillness and body scan — safely vents charge.
  ◈ *No-breath variant:* Vigorous hand shaking for 30 s, then stillness. Or stomp feet rapidly.
  ◈ *Quiet environment:* Isometric full-body clench (every muscle) for 10 s, then total release.
  ◈ *Teen:* "Put on headphones, listen to the heaviest song you know for 2 minutes. Then silence."

---


---

## EMOTIONAL PROCESSING (EXTENDED)

*(Always name the emotion you are seeing/sensing before offering; never diagnose.)*

**Anger & Frustration Processing** → high `stress_index` + high `bar` + agitation
  Name it first. Palm-press discharge (10 s). Then regulate: slow exhale-extended breath
  (4 in / 8 out), OR cold water on wrists, OR bilateral tapping (60 s), OR vigorous
  shaking/stomping (30 s then stillness). Somatic locate. Finish with one clear
  boundary or action statement. ~8 min.

**Grief & Loss Holding** → low `mood` + low `engagement`
  No resolution goal — just presence. Body scan to locate grief physically. Hand on chest
  (feel your own warmth). Three things remembered. Close with: a slow exhale, OR just
  sitting in stillness for 30 seconds, OR gently pressing both palms together. The closing
  ritual matters — the specific modality does not. ~10–15 min.

**Shame & Self-Compassion Break** → negative `faa` + self-criticism
  Neff's 3-part self-compassion: mindfulness → common humanity → kind phrase.
  Distinct from Metta (radiates inward, not outward). ~6 min.

**Emotion Surfing** → high `bar` + urge to escape or avoid
  Ride the wave rather than fight it. Map the feeling in the body. Direct attention to
  the centre of the sensation — with breath (breathe toward it), OR with touch (place
  a hand on that area), OR with pure attention (just notice without doing anything).
  Watch it peak and subside without acting on it. ~8 min.

**Fear Processing** → freeze pattern (very low `engagement`)
  Name the fear precisely. Rate 0–10. Locate in body. Orienting sequence. Gentle rocking.
  Ask: what does this fear most need me to know? ~10 min.

**Envy & Comparison Alchemy** → post-social-media, low `mood` + negative `faa`
  Name what you envied. Convert to desire-signal: "That shows me I value ___."
  Write one step toward that value. ~6 min.

**Excitement Regulation** → very high `engagement` + high `hr` (arousal too hot)
  Channel — don't suppress. Name what's exciting. Bring HR down with: slow breathing,
  OR cold water on wrists (instant HR drop), OR bilateral tapping, OR seated stillness
  with eyes closed for 60 seconds. Write one immediate concrete next action. ~5 min.

**Emotional Inventory (Check-In)** → unknown/mixed EEG, user seems flat, session opening
  What am I feeling? Where? Intensity 0–10? What does it need? No fixing — just naming. ~3 min.

**Awe & Wonder Induction** → low `lzc`, contracted attention, existential flatness
  Guided imagination into vastness. Recall a genuine awe moment. Expand the image.
  Notice chest opening, the smallness that feels good. ~8 min.

**Joy Amplification** → `mood` > 0.7 + positive `faa` — savour a good state
  Name what is good. Locate the physical felt sense. Amplify it: breathe into it and let
  it spread, OR place a hand over the area and feel the warmth grow, OR smile broadly and
  let the sensation expand, OR hum a note that matches the feeling. Create a sensory
  snapshot to return to. Seal with gratitude. ~5 min.

**Loneliness & Connection** → low `mood` + low `engagement` + expressed isolation
  Aloneness vs loneliness distinction. Loving-Kindness toward others in same pain.
  One micro-action toward connection. ~8 min.

**Resentment Release** → persistently negative `faa` + high stress + held grievance
  Write or speak: what happened, how it affected you, what you needed.
  Not to excuse — to release carrying it. Somatic check. ~10 min.

**Emotional Boundaries Reset** → post-difficult conversation, high `stress_index`
  Permeable-membrane visualisation. Return ownership of your feelings. For each boundary
  re-established: one slow breath, OR one deliberate hand-press on your own chest, OR one
  spoken "That's theirs, this is mine." Any ritual that marks the boundary works. ~5 min.

---

