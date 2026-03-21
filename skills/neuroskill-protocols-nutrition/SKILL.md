---
name: neuroskill-protocols-nutrition
description: Dietary, nutrition, caffeine, fasting, and eating protocols — EEG-correlated food guidance for focus, mood, blood sugar, sleep, gut-brain axis, cravings, and alcohol awareness.
---

# Nutrition Protocols

*(This is a domain sub-skill of the neuroskill-protocols repertoire. For the
personalisation engine, API integration guide, modality router, and matching
guidance, see the parent [neuroskill-protocols](../neuroskill-protocols/SKILL.md) skill.
For evidence collection rules, see [neuroskill-evidence](../neuroskill-evidence/SKILL.md).)*

## DIETARY PROTOCOLS

*(Food shapes EEG state — blood sugar, gut-brain signalling, caffeine, inflammation, and meal
timing show up in `focus`, `mood`, HRV, and `drowsiness`. Deliver guidance conversationally
unless a timed practice is appropriate.)*

### Mindful Eating & Awareness

**Pre-Meal Pause** → any meal; `stress_index` elevated before eating, autopilot eating
  Pause before the first bite: three slow breaths, OR put both hands flat on the table
  for 5 seconds, OR just look at the food and name one colour and one texture.
  Check: physically or emotionally hungry? Rate hunger 0–10. Phone down. ~60 seconds.

**Mindful Meal Protocol** → rushed eating, high `cognitive_load` before meal
  Utensils down between bites. Chew 15–20 times. Pause halfway to re-check hunger.
  No screens, no reading. ~20 min.

**Intuitive Eating Check-In** → emotional eating, stress eating, binge urges
  Four questions: (1) Physically hungry? (2) Actually feeling? (3) Truly need?
  (4) Will this food meet that need? ~2 min reflection.

**Eating Speed Reset** → frequent post-meal `drowsiness`, bloating, overeating
  Timer: make the meal last at least 15 min. Fullness signal takes 15–20 min to arrive.

### Energy & Cognitive Performance Nutrition

**Post-Meal Energy Crash Protocol** → `drowsiness` spike post-meal, `wakefulness` drop, mid-afternoon slump
  5-min brisk walk after eating. Avoid high-GI carbs at lunch. Cold water face splash.
  10 min sunlight if possible. Power Nap Protocol if needed.

**Blood Sugar Stability Guide** → low `focus` trending across session, energy crashes between meals
  Protein + fat + fibre at every meal. Avoid refined carbs alone. Eat every 3–4 h.
  Suggest by diet: Western: nuts, eggs, avocado, oily fish, legumes. South Asian: dal,
  paneer, nuts, yoghurt with vegetables. East Asian: tofu, miso, edamame, fish, seaweed.
  Latin: black beans, eggs, avocado, nopales. African: groundnuts, lentils, moringa, fish.
  Vegan: tempeh, lentils, chia, nuts, nutritional yeast. Avoid: white bread, sugary drinks, refined carbs alone.

**Caffeine Timing Protocol** → afternoon focus crash; high `bar`; coffee question
  Window: 90–120 min after waking (let cortisol peak first). Cut-off: no caffeine after 2 pm.
  Max effective dose: 100–200 mg per sitting. Stack with L-theanine to smooth the curve.
  If `bar` is elevated and `relaxation` low, switch to matcha/green tea.
  🔧 *API:* `label "caffeine intake"` every time they drink coffee. Over weeks: `search_labels "caffeine"` → correlate with `focus`, `bar`, `stress_index`, and next-night `sleep` quality. Build personalised evidence: "Coffee after 1 PM costs you 12% deep sleep on average" or "Your best focus sessions start 45 min after coffee labels."

**Pre-Focus Block Nutrition** → before planned deep work session
  Eat 45–60 min before (not immediately). Ideal: moderate protein + low-GI carbs + healthy fat.
  Avoid: heavy meal, pure sugar, alcohol. Hydrate first.

**Cognitive Nutrition Briefing** → general brain performance nutrition question
  Tier 1 (daily): oily fish (DHA/EPA), leafy greens, berries, nuts, olive oil, eggs (choline).
  Tier 2 (support): dark chocolate >85%, turmeric, green tea, legumes.
  Tier 3 (limit): ultra-processed food, refined sugar, trans fats, alcohol.

### Mood & Mental Health Nutrition

**Mood-Food Connection** → persistently low `mood`, low `faa`
  Gut-brain axis: ~90% of serotonin produced in the gut. Support with omega-3, fermented
  foods, tryptophan-rich foods, and magnesium. By culture: Western: salmon, kefir, turkey,
  dark chocolate, almonds. East Asian: natto, miso, mackerel, sesame, tofu. South Asian:
  turmeric lassi, fermented idli/dosa, mustard greens, fish. Latin: ceviche, fermented
  tepache, pepitas, black beans. African: fermented injera, sardines, moringa, groundnuts.
  Vegan: flaxseed, tempeh, sauerkraut, pumpkin seeds, spinach. Reduce: sugar, alcohol, ultra-processed food.

**Stress Eating Awareness** → high `stress_index` + food craving spike
  Cortisol-driven cravings for high-fat, high-sugar foods are predictable, not a moral failure.
  Use Intuitive Eating Check-In first. If physically hungry — protein-rich. If emotional — Emotions protocol.

**Anti-Inflammatory Eating Guide** → `headache_index` > 25, chronic stress, cognitive fog
  Omega-3, polyphenols, turmeric + black pepper, zinc, Vit D.
  Avoid: vegetable seed oils, refined sugar, processed meat, excess alcohol.
  EEG changes visible in 2–4 weeks — suggest logging meals alongside sessions.

**Gut-Brain Axis Reset** → persistently low `mood`, high `lf_hf_ratio`
  30-day guidance: add one fermented food daily, 30 different plant foods per week,
  minimise emulsifiers. Changes take weeks — set expectations.

### Sleep & Evening Nutrition

**Evening Eating Protocol** → late eating habit, poor sleep quality, elevated `bar` at rest pre-sleep
  Last meal 2–3 h before sleep. Avoid: alcohol (fragments REM), caffeine after 2 pm,
  heavy meals late, large sugar load. If hungry: tryptophan snack (banana + nut butter,
  warm milk, almonds). Herbal tea: chamomile, passionflower, or valerian.

**Post-Workout Nutrition Window** → after training; recovery focus, `hr` still elevated
  Anabolic window: 30–60 min post-workout. Protein 20–40 g. Moderate carbs to refill glycogen.
  Rehydrate (500 ml minimum). Avoid: fat-heavy meal, alcohol (blunts protein synthesis).

### Fasting & Meal Timing

**Intermittent Fasting Support** → user in fasting window; hunger, low energy, focus complaints
  Fasting EEG: higher `lzc` and better `focus` 2–3 h in (ketones are clean brain fuel).
  Tips: black coffee/green tea (does not break a fast), electrolytes (sodium, magnesium),
  stay busy. If `cognitive_load` is very high, consider a small protein snack (50–70 cal).

**Breaking the Fast Mindfully** → end of fasting window; first meal of the day
  Avoid breaking with pure sugar or refined carbs. Ideal: eggs, avocado, nuts, yoghurt,
  oily fish, a small piece of fruit. Pre-Meal Pause protocol first. Eat slowly.

**Time-Restricted Eating Reflection** → user exploring IF; meal timing curiosity
  Common patterns: 16:8, 14:10, 5:2. Suggest correlating meal timing with EEG session
  data to find the user's personal optimal window.

### Cravings & Compulsive Eating

**Sugar Craving Surf** → intense craving for sweet/processed food; stress-driven
  Apply urge surfing to the craving. Rate 0–10. Watch it peak (60–90 s if not fed).
  Drink water — dehydration mimics hunger. If persists: dark chocolate >70%.

**Alcohol Awareness Protocol** → high `stress_index` evening; user mentions drinking; poor sleep quality
  EEG-visible impact: disrupts `rem_epochs`, raises daytime `drowsiness` next day, lowers `rmssd`.
  Offer the data, not a lecture. Suggest dry-week EEG comparison if the user is curious.

**Ultra-Processed Food (UPF) Reset** → persistent low `mood`, high `bar`, mostly packaged diet
  One small shift: replace one UPF item per day with a whole-food equivalent.
  Not a diet plan — an accumulating investment over 30 days without feeling like restriction.

---

