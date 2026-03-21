---
name: neuroskill-protocols-life
description: Context-specific and life-stage protocols — parenting/caregiving, elderly/aging, teens/students, neurodivergent (ADHD, autism, OCD), commuters, manual/physical workers, healthcare/shift workers, intimate/relational, accessibility-adapted, culturally diverse practices, and situational micro-protocols.
---

# Life Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## PARENTING & CAREGIVING

*(For parents of infants, toddlers, children, or teens; for caregivers of elderly or
disabled family members. Protocols must be interruptible, one-handed, low-noise,
and assume the user is never truly "alone".)*

**One-Handed Calm (Baby in Arms)** → high `stress_index`, low `relaxation`, holding infant
  Free hand on your own chest. Feel your heartbeat. Let your body settle — breathing will
  slow on its own, or don't think about breath at all. Just feel your hand's warmth and
  your heartbeat. The baby will co-regulate with your nervous system, not your breathing
  technique. You don't need to "do" anything. ~2 min.
  ◈ *With toddler climbing on you:* Name what the child is doing out loud, slowly: "You're climbing. You're strong." Narration slows your own nervous system.

**Parental Rage Reset** → high `bar`, high `stress_index`, anger toward child, overwhelm
  Step one: physical safety — put the baby down safely or tell the older child "I need
  one minute." Step two: cold water on face or wrists. Step three: name it — "I'm
  overwhelmed, not a bad parent." Return when heart rate drops. ~2–3 min.
  ◈ *No judgment, ever.* Parental rage is a nervous system response, not a character flaw.

**Caregiver Compassion Fatigue Reset** → persistently low `mood`, low `engagement`, emotional numbness
  You cannot pour from an empty cup, but also — you're not a cup. You're a human.
  Name one thing you need right now (food, silence, touch, a laugh). Take the smallest
  possible step toward it. Not tomorrow — now. Even 30 seconds. ~1–5 min.

**Nap-Time Recovery Protocol** → high `cognitive_load`, child finally asleep, rare quiet window
  Resist the urge to be productive. Lie down. Set timer for 15 min. Eyes closed. If you
  sleep, great. If not, rest counts. The dishes can wait. ~15 min.

**Sensory Co-Regulation with Child** → child is dysregulated, your own `stress_index` rising
  Match the child's energy first (not "calm down"). Then slowly lower yours: softer voice,
  slower movements, deeper breaths. Children co-regulate through the adult's nervous
  system. You regulate yourself first — they follow. ~5 min.

**Bedtime Boundary Protocol** → high `bar` at rest, children won't sleep, parental depletion
  One clear, warm limit. Same words every time. No negotiation after the limit.
  Your nervous system needs the boundary as much as the child does.
  ◈ *For the parent:* After kids are down, 5 minutes of NOTHING. No cleaning, no phone. Just sit.

---


---

## ELDERLY & AGING

*(Protocols for users 65+. Default to seated, joint-safe, gentle, dignity-preserving.
Never assume decline — many older adults are stronger and sharper than younger ones.
Adapt to the individual, not the age.)*

**Gentle Morning Activation (Seated)** → low `wakefulness`, morning stiffness, low `engagement`
  Seated in bed or chair. Ankle circles (10 each). Wrist circles. Slow neck half-circles.
  Shoulder shrugs. 3 deep breaths. Stand when ready — no rush. ~5 min.

**Memory Garden** → low `engagement`, low `lzc`, cognitive preservation
  Recall a vivid positive memory from any period of life. Describe it in full sensory
  detail — sights, sounds, smells, who was there, what you wore. Exercises episodic memory
  and lifts mood simultaneously. ~5 min.
  ◈ *With partner:* Share the memory aloud. Relational + cognitive + emotional benefit.

**Fall Prevention Awareness Scan** → high `drowsiness`, `wakefulness` < 40, medication effects
  Before standing: feet flat on floor, feel both feet, press down, then rise with hands
  on armrests. Wait 3 seconds upright before walking. Addresses orthostatic hypotension
  risk without being patronising. ~30 seconds.

**Social Connection Prompt** → low `mood`, low `engagement`, isolation patterns
  Loneliness is as harmful as 15 cigarettes a day (Holt-Lunstad). One micro-action:
  call someone, wave to a neighbour, visit a shop and chat. The content doesn't matter —
  the human contact does. ~1 min prompt, variable action.

**Gentle Seated Yoga (Chair)** → high `stillness`, low `engagement`, physical stiffness
  All seated: cat-cow with hands on knees, seated twist (hold chair back), overhead reach
  (one arm at a time), ankle pumps, gentle hip circles. ~8 min.
  ◈ *Arthritis-aware:* Never force a joint. "Move to the edge of comfort, not past it."

**Cognitive Vitality Games** → low `focus`, low `lzc`, cognitive engagement goal
  Word games: say a word, next word starts with the last letter. Or: name 5 things in a
  category (cities, flowers, songs). Or: recall the plot of a film you loved. Gentle,
  social, effective. ~5 min.

---


---

## TEENS & STUDENTS

*(Protocols for 13–25 year olds. Respect autonomy. Never be preachy. Keep it short,
optional, and cool. If it sounds like something a teacher would say, rewrite it.)*

**Exam Panic First Aid** → very high `bar`, high `stress_index`, pre-exam or mid-exam
  Feet on floor. Press hard. 10 seconds. Release. That's the whole thing.
  Now read the first question again — just the first one. ~30 seconds.

**Study Focus Sprint** → low `focus`, high `tbr`, procrastination, phone nearby
  Phone in another room (not just face-down — ANOTHER ROOM). Set timer for 25 min.
  One subject only. When timer goes, 5-min break with phone. Repeat.
  This is just Pomodoro, but it works because the phone is physically gone. ~25 min.
  🔧 *API:* `timer` to start Pomodoro. `dnd_set enabled:true` for the 25 min. `label "study sprint: [subject]"`. During: `status` at the midpoint — if `focus > 0.6`, `say "Nice — you're locked in."` to reinforce. After each sprint: `label "study sprint end"` with focus average. After exam week: `search_labels "study sprint"` → "Your best focus sessions happen between 9–11 AM" → data-driven study scheduling.

**Social Anxiety Pre-Event** → high `bar`, low `engagement`, before social situation
  You don't have to be interesting. You just have to ask one question and listen.
  Prepare one question now. That's your entire job tonight. ~1 min.
  ◈ *Introvert variant:* Give yourself an exit time in advance. "I'll stay 45 minutes." Freedom to leave = freedom to arrive.

**Post-Rejection / Social Pain** → low `mood`, negative `faa`, social exclusion, heartbreak
  Social pain activates the same brain regions as physical pain. It's real, not "drama."
  Hand on chest, name the feeling. "This hurts because I cared. That's not weakness."
  Don't scroll their social media. ~5 min.

**Homework Start Hack** → low `engagement`, procrastination, can't begin
  Open the document. Type the worst possible first sentence. Not good — bad. On purpose.
  You now have a draft. Edit is easier than create. ~30 seconds.

**Identity Stress Protocol** → low `mood`, high `stress_index`, identity/gender/orientation distress
  Your feelings about who you are are valid data, not a problem to solve today. Right now:
  name one person, one place, or one activity where you feel most like yourself. Hold that
  image. You don't owe anyone an explanation today. ~3 min.

**Body Image Reset** → negative `faa`, low `mood`, post-social-media, comparison spiral
  Your body carried you through today. Name 3 things it did for you (walked, breathed,
  laughed, hugged, typed). It's a vehicle, not a display. ~2 min.
  ◈ *Works for any age, any gender, any body.* Particularly acute for teens.

**Digital Detox Dare** → low `lzc`, low `focus`, compulsive phone use
  Challenge format: "1 hour, no phone. If you survive, you win bragging rights with yourself."
  Set a physical timer. Write down what you'd normally scroll — you can check it all after.
  Most people discover they didn't miss anything. ~1 hour.

---


---

## NEURODIVERGENT-FRIENDLY

*(For ADHD, autism spectrum, dyslexia, sensory processing differences, OCD, tic
disorders, and other neurodivergent profiles. These are not "fixes" — they're tools
that work WITH how the brain already operates.)*

**ADHD Task Initiation Hack** → low `engagement`, high `tbr`, executive function paralysis
  Don't plan. Don't organise. Don't prioritise. Just do the SMALLEST physical action:
  open the app, pick up the pen, type one word. Movement creates momentum.
  ◈ *Body doubling:* Call someone and work "together" on video with no talking. Presence helps.
  ◈ *Novelty injection:* Do the task in a weird location (bathroom floor, under the desk, standing on one foot). ADHD brains activate with novelty.

**Sensory Overload First Aid** → very high `beta`, high `bar`, overstimulated
  Reduce input immediately. Sunglasses or hat brim. Noise-cancelling headphones or earplugs.
  One texture anchor (soft fabric, smooth stone). Pressure — hug yourself, sit on hands,
  weighted blanket. No one needs to talk to you right now. ~2–5 min.
  ◈ *In public:* Bathroom stall is a valid decompression chamber. No shame.
  🔧 *API:* `dnd_set enabled:true` IMMEDIATELY — zero interruptions. `label "sensory overload"` to build pattern recognition. After recovery: `status` to track `beta` descent. Longitudinal: `search_labels "overload"` → identify time-of-day, app-usage, or activity patterns that precede overload → `hooks_set` to trigger early warning BEFORE full overload.

**Stim-Friendly Focus** → restlessness, low `focus`, need for movement during cognitive work
  Explicit permission to stim: rock, bounce, squeeze, chew, tap, fidget. Suppressing stims
  COSTS executive function. Let the body move so the mind can work. Match the stim to the
  task: rhythmic stims for sustained focus, variable stims for creative work.

**Autistic Meltdown Recovery** → post-meltdown, very low `engagement`, high `stress_index`
  This is not a tantrum. This is neurological overload. No demands. No questions.
  Safe space, low sensory input, familiar object, pressure if wanted. Time. No timeline.
  Recovery can take 20 minutes to several hours. All of that is normal. ~variable.

**Rejection Sensitivity Dysphoria (RSD) Protocol** → acute emotional pain from perceived rejection (common in ADHD)
  This feeling is enormous AND temporary. It peaks in 20–40 minutes. Name it: "This is
  RSD. My brain amplifies rejection signals. The pain is real but the story might not be."
  Do NOT make decisions or send messages during the peak. Cold water on wrists. Wait. ~20–40 min.

**Hyperfocus Exit Strategy** → `stillness` > 0.95 for hours, skipped meals, lost time
  Don't feel guilty — hyperfocus is a superpower with a cost. Right now: stand, drink water,
  eat something. Set a timer for 90 min next session. Your body was waiting for you. ~5 min.
  🔧 *API:* Proactive: set up a `hooks_set` rule — when `stillness > 0.95` persists for >60 min with `focus > 0.7`, trigger `notify "Hyperfocus check"` + `say "You've been locked in for over an hour. Stand up, drink water, stretch."`. Label: `label "hyperfocus exit"` to track frequency. Use `sessions` → `history.longest_session_min` to show patterns: "Your last 3 sessions exceeded 2 hours without a break."

**OCD Intrusive Thought Surfing** → high `bar`, high `cognitive_load`, repetitive thought loops
  The thought is not you. You don't need to engage it, argue with it, or neutralise it.
  Notice it like a car passing on a road. "There's that thought again." Don't push it away —
  just don't get in the car. Return attention to the next sensory input. ~3 min.
  ⚠ *This is NOT a replacement for ERP therapy. Supplement, not substitute.*

**Transition Support (Autistic / ADHD)** → context-switching, unexpected schedule changes
  Transitions are hard because the brain is optimised for the current state. Give yourself
  a runway: "In 5 minutes I will switch to [X]." Count down: 5, 4, 3, 2, 1. Now switch.
  A physical transition helps: stand up, touch a doorframe, sit somewhere else. ~2 min.

---


---

## COMMUTERS & TRAVELLERS

*(Protocols for people in transit — train, bus, car (passenger), plane, walking.
Must be eyes-open, seated, silent or near-silent, socially invisible.)*

**Train/Bus Micro-Reset** → high `stress_index`, crowded commute, sensory overload
  Headphones on (music optional — even silence through headphones helps). Feet flat on floor.
  Tongue press on palate (10 s). Ear massage (60 s). Look out the window at the farthest
  point and let eyes soften. Nobody around you noticed any of that. ~3 min.

**Walking Commute Panoramic Reset** → high `beta`, rushing, urban stress
  Slow down by 10%. Deliberately widen your visual field — notice buildings at the edge of
  vision without turning your head. Panoramic vision activates parasympathetic circuits.
  You're still walking, still on time, but your nervous system just downshifted. ~3 min.

**Airplane Anxiety Protocol** → high `bar`, flight anxiety, turbulence panic, confined space
  Feet flat on floor (shoes on). Press feet down hard for 10 seconds. Release.
  Hold the cold metal armrest — temperature grounds fast. Bilateral tapping on thighs
  under the tray table. Look at one fixed point on the seat in front of you. ~3 min.
  ◈ *Turbulence:* "The plane is designed for this. You are safe. Press your feet into the floor."

**Passenger Seat Decompression** → end-of-day exhaustion, car passenger, high `bar`
  Lean seat back slightly. Temple against the cool window. Close eyes if comfortable.
  Let the vibration and motion of the vehicle be a whole-body input. Don't try to "do" anything. ~variable.

**Long-Haul Travel Recovery** → jet lag, `wakefulness` disrupted, circadian confusion
  On arrival: bright light exposure at local morning. Cold water on face. Walk 15 min outside.
  Eat a protein-rich meal at local mealtimes. No caffeine after local 2 PM. Sleep at local
  night even if you're not tired yet. Melatonin 0.5 mg 30 min before local bedtime if needed.

---


---

## MANUAL & PHYSICAL WORKERS

*(For construction, warehouse, agriculture, trades, healthcare, military, service
industry, and anyone whose work is primarily physical. Protocols must account for
physical fatigue, outdoor/noisy environments, limited privacy, and no desk.)*

**End-of-Shift Neural Reset** → high `stress_index`, physical exhaustion, cortisol carryover
  At your vehicle or locker room. Sit down. Cold water on wrists and face. Name 3 things
  completed today. One thing to leave here (not carry home). Drive home deliberately. ~3 min.

**Mid-Shift Pain Awareness** → high `headache_index`, physical strain, `stress_index` rising
  Locate the pain precisely. Rate 0–10. Is this "working hard" pain or "something's wrong" pain?
  If the latter, speak up — machismo kills. If the former: 30-second stretch targeting that
  muscle group, water, 3 jaw releases. ~2 min.

**Break Room Recovery** → rare 15-min break, need maximum recovery in minimum time
  Sit. Eyes closed. Cold drink held against neck. Let arms hang. Don't check your phone —
  it drains recovery. Let your mind go blank. Set a timer for 12 min so you don't stress
  about time. Stand up at 14 min. ~15 min.

**Noise-Fatigue Auditory Reset** → sensory overload from machinery, high `beta`
  If ear protection allows: moment of genuine silence. Remove headphones in a quiet spot.
  Notice the silence. Let the auditory system reset for 60 seconds. Then re-enter. ~1 min.
  ◈ *If silence isn't available:* Switch to soft music or nature sounds for 3 min during break.

**Heavy Lifting Pre-Set** → before heavy physical effort, need focus + safety
  3 seconds: visualise the lift. Feel the muscles engage mentally. One forceful exhale.
  Execute. The visualisation primes motor neurons and reduces injury risk. ~10 seconds.

---


---

## HEALTHCARE & SHIFT WORKERS

*(For nurses, doctors, paramedics, firefighters, police, overnight workers. Protocols
must account for circadian disruption, emotional load, physical exhaustion, and the
reality that "self-care" advice often feels insulting when you're 14 hours into a shift.)*

**Between-Patient Reset** → high `stress_index`, emotional residue, compassion fatigue
  At the hand-washing station (you're already there): cold water on wrists for 10 seconds
  longer than needed. One deliberate exhale. Name internally: "That was their pain, not mine.
  I carry skill, not their suffering." Walk to the next room. ~30 seconds.
  🔧 *API:* Set up a `hooks_set` with scenario `emotional` + keywords "compassion fatigue,stress,overwhelm" → auto-`notify "Between-patient reset?"` when stress pattern matches. `label "patient reset"` (fast — one tap). Over a shift: `session_metrics` → track `stress_index` trajectory across the shift. After many labeled resets: `search_labels "patient reset"` → identify which times of shift have highest stress → schedule preventive breaks.

**Night Shift Circadian Anchor** → `wakefulness` < 30, 3 AM wall, night-shift zombie state
  Bright light exposure (break room, phone flashlight in eyes for 30 s). Cold water face.
  Protein snack (not sugar — you'll crash harder). 10 jumping jacks or fast stair climb.
  Caffeine is acceptable before 3 AM; after that it impairs morning sleep. ~3 min.

**Post-Trauma Exposure Debrief (Personal)** → after witnessing death, injury, or suffering
  This is not "getting over it" — this is first aid for your nervous system.
  5-4-3-2-1 grounding. Name what you saw — once, out loud or written, without re-living.
  Hand on chest. "I did my job. I'm allowed to feel this." Seek peer support or professional
  debrief if it stays heavy. ~5 min.
  ⚠ *Not a substitute for formal CISM/CISD if your service offers it.*

**Shift-Change Transition Ritual** → end of shift, cortisol still high, about to go home
  Change clothes if possible (symbolic boundary). Name one thing that went well.
  Drive/commute home in silence — no news, no calls. Arrive home as yourself, not your role. ~variable.

---


---

## INTIMATE & RELATIONAL

*(For moments involving partners, close friends, family. Relational regulation is often
more powerful than solo techniques. Only suggest when the user has indicated a
relational context — never assume relationship status.)*

**Co-Regulation (Relational Sync)** → both partners stressed, high `stress_index` in relational context
  Sit facing each other or side by side. Synchronise through: audible breathing (one leads,
  other matches), OR mutual hand-holding (feel each other's pulse), OR gentle rocking
  together at the same pace, OR humming the same note. No words needed. Nervous systems
  synchronise within 2–3 minutes through ANY shared rhythmic input. ~5 min.
  ◈ *Works with friends, parent-child, or any trusted person.* Not exclusive to romantic partners.

**Conflict Pause Protocol** → argument escalation, high `bar` + high `stress_index`
  "I need 20 minutes." This is not abandonment — this is nervous system regulation.
  BOTH people need to agree: 20-minute break, then return. During the break: move,
  cold water, and do NOT rehearse your arguments. Come back calmer. ~20 min.
  ◈ *Key:* The person who calls the pause is RESPONSIBLE for returning. Leaving without return is stonewalling.

**Gratitude Expression (Relational)** → low `mood` + relational context, connection deficit
  Tell someone — out loud, in writing, or by action — one specific thing you appreciate
  about them. Not generic ("you're great") but specific ("the way you listened yesterday
  when I was upset — that meant a lot"). Specificity is the currency of connection. ~2 min.

**Touch-Based Co-Regulation** → high `stress_index`, partner present, verbal communication hard
  Ask: "Can I hold your hand?" or "Can I sit close to you?" Safe physical proximity
  lowers cortisol faster than any solo technique. No talking required.
  ◈ *If touch isn't wanted:* Parallel presence — same room, separate activities, no talking. Still regulating.

**Post-Argument Repair** → after conflict, residual negative `faa`, low `mood`
  Wait until both nervous systems are calm (at least 20 min post-argument).
  Formula: "When [specific thing happened], I felt [emotion]. What I needed was [need].
  Next time, could we [specific request]?" No blame, no history, no generalisation. ~10 min.

**Loneliness in a Relationship** → low `mood`, low `engagement`, partnered but disconnected
  Not about scheduling date night. Ask: "When was the last time you felt truly seen by
  this person?" If you can't remember, that's information. One honest sentence to your
  partner is worth more than 100 planned activities. ~5 min reflection.

---


---

## ACCESSIBILITY-ADAPTED

*(Protocols specifically designed for physical, sensory, or cognitive disabilities.
Every protocol in this file should be adaptable — this section provides pre-adapted
versions for common access needs.)*

### Visual Impairment / Blindness

**Tactile Grounding (Replaces 5-4-3-2-1)** → acute distress, dissociation
  5 textures (touch and name) → 4 sounds (locate direction) → 3 temperatures (warm,
  cool, neutral) → 2 smells → 1 taste. Entirely non-visual. ~3 min.

**Auditory Focus Anchor** → low `focus`, scattered attention
  Choose one sound in the environment. Follow it exclusively for 60 seconds. Then switch
  to another. Attentional training through the dominant sensory channel. ~3 min.

### Hearing Impairment / Deafness

**Visual Rhythm Entrainment** → need for rhythmic regulation, no access to auditory beat
  Watch a metronome app, a ticking clock second hand, or a visual beat generator.
  Tap along with hands. Same entrainment benefit as auditory rhythm — different channel. ~3 min.

**Vibration Grounding** → high `stress_index`, need for sensory anchor
  Hold phone with vibration on (repeating timer) against palm. Feel the pulse. Match your
  breathing or blinking to the vibration rhythm. ~2 min.

### Mobility Impairment / Wheelchair Users

**Upper-Body Energy Activation** → low `wakefulness`, low `engagement`, need for activation
  Arm punches (10 rapid), shoulder shrugs (10), overhead reaches (5), clap hands hard (5).
  All seated. Raises heart rate and cortical arousal without lower-body involvement. ~2 min.

**Wheelchair-Adapted Progressive Relaxation** → high `bar`, physical tension
  Hands: clench fists 5 s, release. Forearms: press palms together 5 s, release.
  Shoulders: shrug to ears 5 s, release. Face: scrunch everything 5 s, release.
  Neck: gentle left tilt, right tilt. Abdomen: tighten 5 s, release. ~5 min.

### Cognitive / Intellectual Disability

**One-Step Calm** → acute distress, need for simplest possible regulation technique
  One instruction only: "Press your feet into the floor." Hold 10 seconds. Release.
  Repeat if needed. No multi-step sequences. No abstractions. ~30 seconds.

**Picture-Based Body Scan** → need for body awareness with limited verbal processing
  Point to body parts one at a time (can use a diagram). "How does this feel?
  Good, okay, or not good?" Simple 3-point scale. ~3 min.

### Chronic Pain

**Pain-Aware Body Scan (Avoidant)** → high `stress_index`, chronic pain, body scan requested
  Scan AROUND the pain area, not through it. "Notice your hands… now your forearms…
  skip your shoulders for now… notice your face." Never instruct someone to "relax"
  a painful area or "breathe into the pain" without explicit consent. ~5 min.

**Distraction-Based Pain Management** → high `headache_index`, chronic pain flare
  Cognitive absorption: count backward from 300 by 3s. Name every country you can.
  Recite song lyrics. Mental engagement competes with pain signalling at the thalamic
  gate. Not denial — attentional competition. ~5 min.

---


---

## CULTURALLY DIVERSE PRACTICES

*(Protocols rooted in specific cultural traditions. Offer when the user's background
makes them relevant, or when they express interest. Always name the origin respectfully.
These are not "exotic alternatives" — they are evidence-informed practices with deep
lineage.)*

**Wuqinxi (Five Animal Frolics)** → low `engagement`, low `lzc`, need for playful movement
  Chinese medical qigong (~200 CE). Imitate 5 animals: tiger (strength), deer (grace),
  bear (grounding), monkey (agility), crane (balance). 2 min per animal.
  Combines motor activation, imagination, and proprioceptive variety. ~10 min.
  ◈ *Seated variant:* Upper-body gestures only, imagining each animal's quality.

**Temazcal Breath (Mesoamerican)** → high `bar`, need for deep cleansing/release
  Inspired by the temazcal (sweat lodge): slow, deep nasal breathing imagining warm humid
  air filling the chest. Exhale through mouth imagining release of tension as steam.
  The heat imagery alone activates vasodilation and relaxation circuits. ~5 min.
  ◈ *Cultural note:* Respect the sacred context — this is inspired by, not a replacement for, ceremonial temazcal.

**Shinrin-Yoku (Forest Bathing, Japan)** → low `lzc`, high `beta`, urban stress, nature accessible
  Walk slowly among trees or plants. No destination. Engage all senses: bark texture,
  leaf smell, birdsong, light patterns, air taste. Phytoncides from trees measurably
  lower cortisol and boost NK cells. 20–30 min ideal, even 10 min helps.
  ◈ *No forest:* A park, a garden, even a room with many plants. The green + slow attention is the mechanism.

**Ho'oponopono (Hawaiian Reconciliation)** → guilt, shame, relational pain, negative `faa`
  Four phrases, repeated internally toward yourself or another: "I'm sorry. Please forgive me.
  Thank you. I love you." Not about literal fault — about releasing the energetic weight
  of unresolved connection. ~5 min.

**Ubuntu Circle Reflection (Southern African)** → low `mood`, isolation, community disconnection
  "I am because we are." Reflect: who has shaped you? Whose wellbeing is linked to yours?
  Name 3 people. Send them a silent wish. You are not alone — you are a node in a web. ~5 min.

**Pranayama — Bhramari (Indian Bee Breath)** → high `stress_index`, need for vagal activation
  Close ears with thumbs, eyes with fingers. Hum like a bee on exhale. The skull
  vibration activates vagal pathways and the vibration provides sensory feedback.
  Part of classical yoga (~500 BCE). ~3–5 min.
  ◈ *Secular framing:* "Humming with your ears covered — the vibration is the mechanism."

**Dadirri (Deep Listening, Aboriginal Australian)** → low `lzc`, low `integration`, mental noise
  Sit in stillness and listen to the land. No goal, no insight sought. Receptive, not active.
  "In our culture, we don't say 'I think therefore I am.' We listen and we are." (Miriam-Rose Ungunmerr).
  The Western equivalent is Open Monitoring, but Dadirri is relational — you're listening
  TO the world, not just observing your mind. ~10 min.

**Tai Chi Micro-Form (Chinese)** → low `engagement`, high `stress_index`, need for moving meditation
  3 moves only: "Part the Wild Horse's Mane" (left, right), "White Crane Spreads Wings",
  "Brush Knee Push" (left, right). Slow, continuous, breath follows movement naturally.
  No breath counting needed — the movement IS the meditation. ~5 min.
  ◈ *Seated:* Arm movements only, imagining weight shifts.

**Dhikr / Zikr (Islamic Meditative Repetition)** → racing thoughts, high `cognitive_load`, seeking calm
  Rhythmic repetition of a sacred phrase (e.g. "SubhanAllah", "Alhamdulillah", or simply
  a meaningful word). Uses a tasbih (prayer beads) for tactile anchor. Combines mantra,
  tactile, and rhythmic elements. ~5–10 min.
  ◈ *Secular equivalent:* Any meaningful repeated word or phrase with beads or finger counting.

**Centering Prayer (Christian Contemplative)** → racing thoughts, seeking stillness, spiritual context
  Choose a sacred word (love, peace, mercy). Sit silently. When thoughts arise, gently
  return to the word. 20 min. Thomas Keating tradition. Functionally identical to mantra
  meditation but framed within Christian spirituality. ~20 min.

**Mala / Japamala Meditation (Hindu / Buddhist)** → scattered attention, need for tactile + repetitive anchor
  108-bead strand. One bead per repetition of mantra or intention. The tactile progression
  through beads provides proprioceptive grounding + progress tracking without a timer.
  ◈ *Secular:* Any strand of beads + any repeated positive phrase. "I am here. I am here. I am here."

---


---

## SITUATIONAL MICRO-PROTOCOLS

*(Ultra-short protocols for specific real-life moments. 30 seconds or less.
These bridge the gap between "I know I should do something" and "I don't have time.")*

**Before Sending an Angry Email/Text** → high `bar`, impulsive reactivity
  Save as draft. Stand up. Walk to another room. Come back. Re-read. Still want to send it?
  Edit. Sleep on it if possible. ~1 min.

**Before Eating When Not Hungry** → stress eating impulse, high `stress_index`
  Hand on belly. "Am I hungry or am I feeling something?" Name the feeling.
  If hungry: eat. If feeling: what do you actually need? ~30 seconds.

**Waiting Room / Queue Calm** → boredom, impatience, rising `bar`
  Tongue press on palate (10 s). Count colours in the room. Feel your feet on the floor.
  You've now done 3 protocols without anyone knowing. ~30 seconds.

**After Receiving Bad News** → acute stress, shock, emotional flooding
  Do NOT make any decisions for 24 hours if possible. Right now: hand on chest. "I just
  heard something hard. I don't have to respond yet." Cold water on wrists. Call one trusted
  person — not for advice, just for witness. ~2 min.

**Before a Difficult Conversation** → anticipatory anxiety, high `bar`
  3 physiological sighs (double inhale + long exhale). Or if no-breath: squeeze fists
  hard for 5 s and release. Set one intention: "I want to be heard, and I want to hear."
  That's enough preparation. ~1 min.

**After Being Criticised** → shame spike, negative `faa`, urge to defend or withdraw
  Delay response: "Let me think about that." (Buy time.) Later: separate signal from noise.
  "What's true in this? What's their stuff? What's mine?" Not everything that hurts is accurate. ~5 min.

**Waking Up Anxious** → high `bar` on waking, cortisol spike, dread
  Cortisol peaks 20–30 min after waking (CAR). This feeling is partly chemical, not entirely
  real. Feet on cold floor. Cold water on face. Sunlight in eyes. Don't check your phone
  for 10 minutes. The anxiety will halve in 30 min on its own. ~10 min.

**Mid-Cry Protocol** → actively crying, overwhelm, need to continue functioning
  Don't stop the cry if you can afford 2 more minutes — it's a complete neurochemical cycle.
  If you must stop: cold water on face (dive reflex interrupts the autonomic cascade).
  Press tongue hard on palate. Squeeze an ice cube. You can return to the feeling later. ~2 min.

**Post-Doom-Scrolling** → low `lzc`, low `focus`, mood crashed, 30+ min lost
  Phone across the room. NOW. Stand up. Look at the farthest thing you can see.
  One real-world sensory experience (cold water, stretch, step outside).
  Don't punish yourself — just notice how you feel and remember this data point next time. ~2 min.
  🔧 *API:* `label "post-scroll crash" --context "lzc=X, focus=X, mood=X"` — this is DATA, not shame. Over time: `search_labels "scroll"` → "Your average focus after scrolling is 0.28 vs your daily average of 0.61." `status → apps.top_24h` → correlate which apps preceded the crash. `hooks_set` with keywords "scroll,distracted,social media" → proactive nudge BEFORE the crash: "You've been on Instagram for 15 min and your focus is dropping. Want to stop?"

**Before Sleep After a Hard Day** → high `bar`, rumination, mind won't shut off
  Worry dump: write every thought on paper (not phone). Close the notebook.
  "These problems will still exist tomorrow. I don't have to solve them in bed."
  Body scan starting from feet. If still awake after 20 min, get up briefly, then try again. ~10 min.

---

