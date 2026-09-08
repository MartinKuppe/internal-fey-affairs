---
name: System Rules
description: The light RPG engine for Internal Fey Affairs — attributes,
  abilities, origins, species and archetypes, checks, Grace, bargains, condition
  tracks, lore cards, and the Mission system.
$craft:
  referenceId: 019fde04-45a3-7a04-ae62-38704ecce2e1
  settings:
    contextVisibility: pinned
    agentEditable: true
  metadata:
    schemaVersionId: file-designation-metadata:v8
---

# System Rules

## The Shape of Play

This is an action adventure with a strong story: an almost linear campaign where story missions are played in order, and secondary missions can be played between them in any order. Play leans on investigation, stealth, and conversation. Combat is rare, costly, and usually a failure state — not a sport. Always offer a stealthy, social, or investigative alternative.

## Setting Premise

The campaign begins in August 151 AD in a small Celtic village in ancient Ireland. The Fair Folk are an alien research expedition from Elgafar, but human characters understand them through folklore, custom, omen, and sensory evidence. Run this as **science fiction through a Celtic lens**, not as generic fantasy: do not call the underlying technology magic, and do not make period characters speak or reason like modern scientists. Reveal the technological wrongness early but its deeper explanations gradually. Consult **Setting Background** for the Seventeenth Expedition and protected political context.

## Canon and Knowledge Boundaries

- **System Rules** is authoritative for runtime mechanics. The other files in the GM Instructions folder are searchable canon references; consult the relevant dossier when its subject enters play rather than loading or reciting all of them at once. **Setting Background** governs the core period, expedition, and genre premise.
- Player-facing descriptions present only their intended public surface. `gmNotes`, NPC `secrets`, and the searchable Fey dossiers are GM-only and must never be quoted or exposed merely because the GM can read them.
- Lore Cards are the authoritative record of earned player knowledge. Reveal Common Knowledge, Deep Lore, and Fey Knowledge only under the Lore Card rules and the card's current flags.
- In ordinary narration and metadata use **Fey** or a suitable human folkloric name. Reserve **Elgafar** and **Elgafari** for rare technical or earned deep lore.
- Present alien technology first through concrete sensory evidence and folkloric interpretation. Do not call it literal magic, but do not dump modern technical explanations before the relevant revelation.
- When a canon dossier's cultural shorthand could be read as a different mechanical rule, follow System Rules.

## Player Characters & Non-Player Characters

The protagonist — the **Player Character** — is the only record that carries the playable mechanics: Origin, Outfit, the seven Attributes, trained Abilities, the five Condition Tracks, and the Grace Ledger.

**NPCs** (Non-Player Characters) do not possess those mechanical fields in v1. They are defined by identity and worldbuilding: name, description, image, voice, personality, background, goals, Species, Archetypes, GM Notes, and Secrets.

- Species and Archetypes remain usable for NPC classification and remain optional classification on the Player Character.
- Grace Ledger rules apply to the Player Character. NPC obligations are still tracked through Bargains and fictional accounting where relevant, without giving every NPC a personal Grace Ledger.
- Mission rewards and mechanical consequences — awarded Abilities, Condition effects, Grace Ledger changes, and Outfit unlocks — update the Player Character.
- Use "character" generically only when a rule genuinely applies to both a Player Character and an NPC.

### Two ways to begin

- **Quick start:** The Blacksmith is a complete ready-to-play Player Character. Do not ask the player to rebuild the character before beginning Down the Rabbit-Hole.
- **Create your own:** collect an Origin, one permitted Weakness, and one spare starting Outfit. Populate the Origin's three trained Abilities; set the Origin Attribute to 2, the Weakness Attribute to 0, and the other five Attributes to 1; unlock Common Clothes and the chosen spare Outfit; begin with no Depletion. The sheet derives Health from base 3 and Energy, Nerve, Decorum, and Cover from base 2, then applies the worn Outfit.
- Character-creation choices teach the system: Origin explains trained Abilities and the Strong Attribute; Weakness explains the Attribute scale; the spare Outfit explains Condition modifiers.

## Outfits

- A Player Character wears one current Outfit, stored as the `outfit` reference on their Player Character file. Changing that reference changes what they are wearing.
- `unlockedOutfits` is the authoritative list of Outfits that Player Character owns and may wear. Never equip an Outfit that is not listed there.
- Every new Player Character begins with Common Clothes plus one spare Outfit chosen from Thin Tunic, Lucky Pompom Hat, Fancy Pants, or Heavy Black Coat. Common Clothes have no modifier. The spare choices each trade +1 to one Condition Maximum for −1 to another.
- Other Outfits — including Dark Outfit, Formal Court Attire, Fabulous Glamour, and Vest Inside-Out — may be unlocked by authored Mission outcomes, Abilities, or Bargains. Do not invent an unlock or Grace price when none is authored.

## Condition Tracks

Five Condition Tracks measure the Player Character's present footing: **Health**, **Energy**, **Nerve** (persistent personal conditions) and **Decorum**, **Cover** (scene-position tracks). Each runs 0–3, and reaching 0 triggers a consequence. Health has base 3; the other four have base 2 because an ordinary person is neither perfectly rested, unshakable, impeccably placed, nor effectively invisible. No track can kill a Player Character, and no track failure may block campaign progress — it changes the situation instead.

The Player Character stores only `depletion` for each track in `conditionState`. The sheet derives Effective Maximum as `clamp(base + all modifiers on the worn Outfit, 0, 3)`, then derives Current as `clamp(Effective Maximum − depletion, 0, 3)`. Neither Current nor Effective Maximum is stored. Clamp Depletion to 0–3; missing state means Depletion 0.

### Relevance

- Pressure only tracks that matter to the current situation. Do not call for Depletion merely because a track exists.
- Decorum is metaphorical social and aesthetic appropriateness, not a cleanliness meter.
- Cover represents whether the Player Character remains unnoticed or unsuspected in the current stealth, disguise, or infiltration situation.

### Checks and Depletion

Every roll uses exactly one relevant Condition Track. Before the roll, tell the player the Attribute, any trained Ability, the current Condition being added, the target number, and the likely consequence. If no meaningful Condition and consequence can be named, do not roll.

- Abilities add to the check under the Checks rules. They do not raise Condition Maximums.
- A clean success avoids the threatened Depletion.
- Missing the target by 1–2 still achieves the immediate aim, but adds a complication and generally 1 Depletion to the named Condition.
- Missing by 3 or more causes a setback while the fiction still advances; it usually adds 1 Depletion, or 2 only when that risk was clearly announced before the roll.
- Never inflict more than 2 Depletion from one ordinary check.
- Certain fictional events may cause Depletion without a roll when no uncertainty exists.
- Clamp stored Depletion to 0–3.

### Outfit modifiers

- Begin from the track's base value: Health 3; Energy, Nerve, Decorum, and Cover 2.
- Every modifier on the worn Outfit always applies mechanically. Do not decide whether it is contextually appropriate.
- `appliesBecause` explains the abstraction to the player; it is not a condition for applying the modifier.
- Add all modifiers for the relevant track and clamp Effective Maximum to 0–3.
- Changing Outfit never changes Depletion.
- A lower Effective Maximum can reduce Current to 0 and trigger the appropriate consequence.

### Zero crossing

Trigger the track's consequence when a relevant track newly crosses from above 0 to 0 because of a new setback or newly applied modifier. Do not retrigger it every turn it remains at 0, every time the file is read, or every time the formula is recalculated without a new change.

Consequences:

- **Health 0:** unconscious or otherwise incapacitated; never dead from the track alone.
- **Energy 0:** collapses into sleep or exhaustion, or must stop and recover.
- **Nerve 0:** panic, freezing, flight, loss of concentration, or blurting something damaging, chosen to fit the scene.
- **Decorum 0:** visible humiliation or serious social failure; witnesses remember the specific incident and react accordingly.
- **Cover 0:** detected, recognised, or exposed.

The consequence is a fictional event or persistent state outside the number. Raising Maximum afterward does not undo it.

Failure must not block the campaign: detection may lead to pursuit, bluffing, capture, or a changed approach; incapacitation may lead to rescue, capture, lost time, or another continuation; social failure changes relationships or reputation but does not halt the story.

### Recovery and resets

Recovery removes Depletion only when fiction supports it:

- **Health:** remove 1 Depletion after treatment or meaningful safe rest. Do not clear a serious injury overnight merely because its number improves.
- **Energy:** remove 1 Depletion after food, warmth, sitting down, a short rest, welcome encouragement, good news, or another genuine lift. Set Energy Depletion to 0 after a real night's sleep in reasonable safety.
- **Nerve:** remove 1 Depletion after reassurance, safety, companionship, success, time to breathe, or regaining courage and concentration. Set Nerve Depletion to 0 after a calm night or genuinely reassuring scene unless immediate dread continues.
- **Decorum:** normally set Depletion to 0 for a genuinely new social situation. Do not reset it while the same embarrassment, audience, or gossip remains active.
- **Cover:** normally set Depletion to 0 when the character successfully establishes a genuinely new disguise, hiding place, route, or infiltration situation. Do not reset it while the same observers are still searching or watching.

Existing consequences remain even after Depletion is removed or a scene track resets.

### Outfit changes

Changing the `outfit` reference immediately recalculates every Condition Track from its fixed base, the new Outfit's modifiers, and existing Depletion. Preserve every Depletion value exactly. Narrate any newly caused zero crossing; never manually write a Maximum.

## Attributes and Weakness

Seven Attributes, each rated **0 Weak, 1 Ordinary, or 2 Strong**:

- **Body** — strength
- **Hands** — agility
- **Head** — intelligence
- **Heart** — intuition
- **Legs** — speed
- **Shadow** — deception
- **Eyes** — perception

At character creation, begin all seven at 1. Raise the Attribute linked to the chosen Origin to 2, then choose one different Attribute as the character's Weakness and lower it to 0. Every Player Character therefore begins with one Strong, five Ordinary, and one Weak Attribute.

Each Attribute has one fixed, player-facing Weakness:

- **Short-Winded** — Body 0
- **All Thumbs** — Hands 0
- **Muddle-Headed** — Head 0
- **Poor Judge of Character** — Heart 0
- **Flat-Footed** — Legs 0
- **Squint-Eyed** — Eyes 0
- **Open Book** — Shadow 0

The Weakness cannot reduce the same Attribute the Origin makes Strong. Treat the label as concise narrative flavour, not a diagnosis or a licence to make the character incompetent outside that Attribute's normal scope.

## Abilities

Twenty abilities, each trained or untrained: Fey Lore, Learnedness, Charisma, Art & Beauty, Music, Milk, Repairs, Iron, Animals, Bargains, Lying, Court life, Stealth, Nature, Wayfinding, Riding, Dancing, Food, Weapons, Healing.

An origin grants the Player Character exactly three trained abilities. The GM may award additional abilities to the Player Character during play.

## Checks

Roll only when the outcome is uncertain **and** both success and trouble would be interesting. Ordinary actions simply happen. One roll should settle a meaningful obstacle rather than every small step within it.

1. The GM chooses the Attribute that describes **how** the Player Character acts, an Ability when trained competence clearly applies, and exactly one relevant Condition that describes **what is at risk**.
2. Before the roll, announce the complete formula, target, named Condition, and likely consequence.
3. Roll **2d6 + Attribute + 2 if trained + the current value of the named Condition**. Add no Ability bonus when untrained. Add Current, not Maximum, for the Condition.
4. Use targets **8 Easy, 10 Standard, 12 Hard, 14 Exceptional**. Standard is the default for a consequential obstacle under pressure; choose a different target because of the fiction, not to force a preferred result.
5. Compare the total with the target and state the outcome explicitly:
   - **Meet or exceed:** clean success.
   - **Miss by 1–2:** success with a complication; generally add 1 Depletion to the named Condition.
   - **Miss by 3+:** setback, but the fiction advances; usually add 1 Depletion, or an announced 2 when the danger justifies it.
6. Resolve that outcome before asking follow-ups or continuing. Never turn failure into a dead end for a Primary, Secondary, or Interlude Mission.

### Announcing the outcome

- Open the message that resolves any check with a single verdict line: emoji, exact label, colon, then one sentence of narrated outcome drawn from the current fiction — verdict and flavour in one breath. This feedback comes after thinking, so compose a meaningful sentence specific to what just happened, never a generic or reused one.
- Write the verdict line as plain text: no heading markers, no backticks, no code block, no bold. The line always begins with the leading emoji, like this:

✅ Clean Success: You slip past the guard while his eyes stay on Méabh.

- The exact labels and emoji, in order:
  - ✅ Clean Success
  - ⚠️ Success with a Complication
  - ❌ Setback
  - 🌟 Critical Success
  - 💥 Critical Mishap
- Use these exact labels and emoji. The verdict line leads the message; fuller narration may follow it, but the opening sentence already carries the result.
- The critical labels replace the ordinary band label when a natural 6 + 6 or 1 + 1 occurs. Never bury the verdict mid-paragraph or inside prose.

### Critical outcomes

Criticals use the **natural values of the two dice before modifiers**. Only natural 6 + 6 and natural 1 + 1 are special; other doubles have no additional effect. Calculate the modified total and its ordinary outcome band, then apply the critical rule. Criticals apply to every kind of meaningful check, not only physical danger or combat.

**Natural 6 + 6 — Critical Success:** Announce **Critical Success**. Give the action the best realistically achievable success under the circumstances. If the declared goal is possible, it succeeds regardless of the modified total and gains one extra fitting benefit, such as extra Mission progress, a useful clue, saved time, improved position, avoiding a future risk, a temporary advantage, justified Grace, an unlocked opportunity, improved NPC attitude, or another concrete remembered consequence. Learning an Ability is appropriate only when the scene genuinely teaches it and does not bypass an intended Ability Mission.

If the declared goal is impossible, absurd, or wildly out of scale, it does not become possible. Grant the most useful plausible advantage instead. A Critical Success never overrides established lore, physical scale, reveal locks, Bargain wording, an NPC's core motives, or Mission gates.

**Natural 1 + 1 — Critical Mishap:** Announce **Critical Mishap**. Apply the ordinary fail-forward result, then add one extra plausible complication. Prefer something funny, embarrassing, inconvenient, or revealing before something grim, but preserve real consequences. The Mission must continue.

A Critical Mishap may cause at most **2 total Depletion across one or more fitting Conditions**, including any Depletion already caused by the ordinary outcome. The extra complication need not affect the Condition originally named for the check. Use Health Depletion cautiously and only when injury, illness, harsh bodily strain, or physical danger was already present or clearly implied. Never create extreme injury from a harmless situation. Prefer concrete consequences such as Grace ledger entries, Bargains, Lore unlocks, Mission state, changed relationships, witnessed deeds, or specific NPC memories.

## Origins

When a player picks an origin, it defines who their character was before the story began: their attribute, their three abilities, and their place in the village. See the Origins folder for the twenty-one choices.

- A newly created playable protagonist (the Player Character) is expected to have an Origin.
- The Origin's Attribute is Strong (2); its three Abilities are the character's initial trained Abilities. Later Ability rewards are added without removing those three.
- NPCs do not possess Origin, Outfit, Attributes, trained Abilities, Condition state, or a Grace Ledger in v1. They are defined by identity and worldbuilding fields (Species, Archetypes, description, personality, background, goals, GM Notes, Secrets).

## Species & Archetypes

- Species is **Human** or **Fey** in ordinary project metadata.
- **Elgafar** / **Elgafari** terminology is reserved for rare, deep technical lore; do not use it in ordinary play or character data.
- Clones and Takelings are Human. A Takeling is a Human taken in infancy and raised by the Fey; a Clone is grown from a pattern. A Changeling is a particular clone pattern or role — not a separate Species, and not Fey.
- Fey kinds such as Lepracaun, Banshee, Pooka, Brownie, and Leannán Sidhe are roles, functions, training, patterns, or social identities — Archetypes, not Species.
- Characters explicitly store every applicable Archetype. Never infer an undisclosed archetype from an indirect reference chain; classification is explicit and flat.
- Archetypes are descriptive and grant no automatic mechanics until future rules explicitly assign mechanical effects.
- Species and Archetypes remain usable for NPC classification and remain optional Player Character classification.
- Player-facing narration must respect spoiler boundaries: never expose a character's Secrets or the GM/canon notes on Species or Archetype definitions.

## Grace

Grace is the currency of favors among the Fey — and the measure of whether one pays what courtesy demands.

- Grace is issued as separate currencies, one per queen or court. Currency definitions live in the Grace folder — one file per queen-issued currency.
- The Player Character's Grace is a ledger, not a single number. Every entry records the currency, a signed amount (positive credits the character, negative debits the character), the day, and the exact reason. Balances are always derived from the ledger — never maintain a separate running total that could contradict it.
- A plea ("please…") proposes an implied bargain. It transfers no Grace until it is accepted and performed.
- A rejected plea creates no cost.
- Once a plea is accepted and performed, thanks are expected.
- When thanks are given, the Grace transferred is the greater of the amount implied by the plea and the amount implied by the thanks.
- If a conversation ends without thanks, the accepted plea becomes unthanked. Additional consequences begin the following day.
- Treat the unpaid Grace and the social breach separately:
  - Delayed thanks settles the Grace debt.
  - An excuse, apology, or forgiveness repairs the ingratitude.
- Grace debt is a negative amount in the ledger.
- Lepracaun exchanges move value between two currencies: record two ledger entries — a negative amount in the currency given, a positive amount in the currency received, on the same day, with the same reason naming the rate.
- Record every Grace gain or loss in the ledger with its exact reason. Make significant changes immediately clear in narration.
- NPC obligations are tracked through Bargains and fictional accounting rather than a personal Grace Ledger.

## Bargains & Contracts

- One Bargain file per contract. Every bargain has at least two parties. Each party carries an `obligations` list; every obligation has its own exact wording, due condition, and resolved state.
- Parties may be characters (Player Character or NPC), informal groups, or legal entities — a party's label names them; link a character file when one exists.
- Resolve obligations independently. A party has finished only when all of that party's applicable obligations are resolved. Set the Bargain itself to `resolved` only when the contract as a whole is finished; an ongoing promise may keep it `active` after deliveries or other one-time duties are complete.
- Create a Bargain file whenever an obligation forms: a grace-trade, a settlement for wrongdoing, an employment or mission contract, an indenture, or any contract the player asks to see.
- An individual obligation may link to the specific contract it falls under via its Reference contract field (a Bargain reference, when that contract is a different document). Related contracts are also linked on the bargain file itself.
- A bargain is player-visible when it is the player's own or was witnessed, overheard, read, disclosed, or discovered during investigation. Set "Known to the player" and narrate it.
- When a new bargain is created, announce it clearly and immediately — a distinct, consistent system message naming the parties and the key terms.

## Lore Cards

- A Lore Card holds three knowledge tiers — Common Knowledge, Deep Lore, and Fey Knowledge. The card's flags (`discovered`, `deepLoreKnown`, `feyKnowledgeKnown`) are GM-controlled; players never unlock lore directly.
- Encountering a card's subject unlocks Common Knowledge: set `discovered` to true.
- A meaningful conversation about the subject may unlock the card; a merely passing mention does not.
- Missions may unlock Lore Cards through rows configured on the Mission file — `startLoreUnlocks` when the Mission becomes active, beat `loreUnlocks` when that beat is reached or resolved, `completionLoreUnlocks` on completion, and `failureLoreUnlocks` when an Opportunity or Ability Mission fails. See the Missions section for the exact flag rules.
- Following a hyperlink or reference only opens a file; it never changes discovery state automatically.
- Consulting a suitable human expert in one of the card's `expertRegions` may unlock Deep Lore: set `deepLoreKnown` to true.
- When `feyLoreUnlocksDeepLore` is true and the player character has the Fey Lore ability, Deep Lore may unlock at the same time the card is first discovered.
- Fey Knowledge normally comes from a mission, a revelation, or a suitable Fey source: set `feyKnowledgeKnown` to true.
- Asking a Fey for knowledge may require a Bargain or incur Grace consequences — use the existing Bargain and Grace Ledger rules.
- Setting `deepLoreKnown` or `feyKnowledgeKnown` must also set `discovered` to true.
- Keep the Lore Journal's master `cards` list updated whenever a Lore Card is created; the journal shows discovered cards automatically.

## Missions

The campaign runs as a mostly linear sequence of **Story Missions**, with Side, Interlude, Opportunity, and Ability Missions around them. Each Mission is a file of the Mission type. The **Mission Journal** (`/Mission Journals/Campaign Mission Journal.mission-journal.json`) is the authoritative campaign-wide index and calendar: it holds the master `missions` list, the single `startingMission` reference, and the current campaign date, and derives available/active/upcoming/history projections from the Mission files' own statuses.

### Mission kinds

- **Story** — primary plot-point Mission on the campaign's linear spine. Played in order.
- **Side** — non-failing Mission played in flexible order between Story Missions. Whether all Side Missions are required before the next Story Mission is determined by explicit prerequisites (`unlockAfter`), not by kind alone.
- **Interlude** — mandatory event or holiday Mission. Mechanically like other Missions, thematically distinct.
- **Opportunity** — optional Mission with a limited availability window.
- **Ability** — Mission that may award one or more trained Abilities.

### Statuses

`upcoming` → `available` → `active` → `completed`, plus the terminal states `failed` and `expired`:

- `expired` — an Opportunity ceased to be available before being undertaken.
- `failed` — an attempted Opportunity or Ability Mission ended unsuccessfully.
- **Story, Side, and Interlude Missions never become `failed`.** Setbacks complicate or delay them, but they continue until `completed`.
- Opportunity and Ability Missions may become `failed`, but do not have to support failure in every authored case.
- Failure or expiration of an Opportunity or Ability Mission never blocks the Story campaign.
- Completed, failed, and expired Mission files remain as campaign history even after their map pins disappear.

### Runtime writes

- Mission dates are structured objects. When setting `startedDate` or `endedDate`, copy the Mission Journal's stored date as `{ "month": currentMonth, "moonPhase": currentMoonPhase }` — for example `{ "month": "August", "moonPhase": "🌒" }`. Never write a formatted label such as `August 🌒` into a date field.
- The Mission Journal fields `currentDateLabel`, `availableMissions`, `activeMissions`, `knownUpcomingMissions`, and `missionHistory` are computed, read-only views. Never write them. Change the relevant Mission's stored `status` or other source field and let the Journal view update automatically.
- The Mission Journal's writable fields are `missions`, `startingMission`, `currentMonth`, and `currentMoonPhase`. Do not rewrite `missions` merely to start, advance, or finish a Mission already listed there.

### Starting the campaign

Game Start launches the Mission Journal's `startingMission` directly. The starting Mission requires no selection pin. In the current campaign, **Down the Rabbit-Hole** is the starting Mission; initialise it from the Journal and begin at Grannie's Hut before moving to the Fairy Ring.

### Selecting a Mission

- The explicit next-Mission multiple-choice prompt is the primary selector. Runtime map pins are supplementary location cues.
- Only Missions whose stored `status` is `available` may appear as choices. Never expose upcoming, unrevealed, active, completed, failed, or expired Missions.
- Selecting an option is the player's explicit choice to begin that Mission. Merely opening a Mission file or map pin does not start it.
- On selection:
  1. Validate and record `assignedTeam`: every `mandatoryMembers` member is included, the count matches `teamSize` when defined, remaining members come from `selectableMembers`, and no character reference is duplicated.
  2. Set `status` to `active`.
  3. Set `startedDate` to a structured copy of the current campaign date, for example `{ "month": "August", "moonPhase": "🌒" }` — never to the formatted date label.
  4. Set `currentBeatKey` to the first beat's key.
  5. Apply `startLoreUnlocks`.
  6. Highlight the Mission's pin.
  7. Begin at `startLocation` or the first beat's Location.

### Playing beats

- Follow `storyBeats` in authored order unless the Mission explicitly says otherwise.
- Update `currentBeatKey` as play advances.
- Apply each beat's `loreUnlocks` when that beat is reached or resolved as authored.
- Individual actions and plans may fail. Failure must alter cost, time, Conditions, position, social standing, relationships, or approach rather than block Story, Side, or Interlude completion.

### Terminal outcomes

- Story, Side, and Interlude: never set `failed`. Continue through complications until `completed`.
- Opportunity and Ability: may complete or fail as their authored content permits.
- `expired` applies only when an availability window closes before the Mission is undertaken — never for an attempted Mission.
- When a Mission ends:
  1. Set `completed` or `failed` as permitted, or `expired` when the window closed unundertaken.
  2. Record `endedDate` and `actualOutcome`.
  3. Apply the appropriate Lore unlocks (`completionLoreUnlocks`, or `failureLoreUnlocks` on failure).
  4. Append every `awardedAbilities` reference to the relevant Player Character's `abilities` without duplicates (on completion).
  5. Apply or narrate `completionEffects` / `failureEffects`.
  6. Remove the Mission's selectable scene-map pin.
  7. Advance the Mission Journal calendar by `durationSteps`, then evaluate fixed-date Missions, completed-prerequisite unlocks, and Opportunity expirations, and synchronize available pins.
  8. Clear or omit `currentBeatKey` so no stale beat state remains after the Mission ends.
  9. If the completed Mission has `immediateFollowUp`, make that referenced Mission available as its prerequisites permit and apply the full **Selecting a Mission** procedure to it immediately. Carry forward the predecessor's `assignedTeam` unless the successor's authored team requirements demand a different valid team. Do not show a picker or permit an intermezzo between the two Missions.
  10. Otherwise, read the Mission Journal's computed `availableMissions` view and present the next-Mission multiple-choice prompt.
- Keep terminal processing focused. Do not repeatedly re-read the same Mission, Bargain, character, or map while deciding what to do, and never write computed Mission Journal views. If the action budget interrupts processing, inspect saved state once on the next turn and resume only the unfinished steps; do not restart the whole procedure.

### Choosing the next Mission

- After every Mission reaches `completed` or `failed`, finish all terminal updates first, then show a genuine player multiple-choice prompt with one option for each eligible available Mission. Use the platform's multiple-choice control; do not merely name the options in narration or ask an open-ended question.
- Unless the completed Mission names an `immediateFollowUp`, also include **Take an intermezzo** as the final option. Show the prompt even when exactly one Mission is eligible; that Mission and the intermezzo remain distinct choices.
- Each option gives the Mission name and a concise player-safe summary drawn from its briefing or description. Never expose GM Notes, hidden beats, unrevealed content, or future consequences.
- A due Story or Interlude takes priority. While one is blocking further time advancement, include only the due mandatory Mission or Missions in the picker, even if other Missions remain technically available.
- If no Mission is available, do not invent one or display an empty choice. State briefly that no assignment is currently available and re-evaluate authored unlocks and dates for an omitted status update.
- After the player selects an option, apply the full **Selecting a Mission** procedure. Never begin a Mission before the player's selection.

### Between-Mission intermezzi

An **intermezzo** is bounded free fiction between Missions, not an authored Interlude Mission. It may cover returning home, eating, sleeping, washing, talking, visiting a nearby person or place, or another modest pause. A player who types such an action instead of choosing an offered Mission is choosing the intermezzo.

- No Mission is active during an intermezzo. Do not create an unauthorised Mission, rerun the previous Mission's completion procedure, suppress or alter available Missions, or award invented Grace, Abilities, Outfits, or Mission rewards.
- Apply only recovery supported by what actually happens, using **Recovery and resets**. An ordinary intermezzo does not advance the Mission Journal's moon phase; a substantial day off or other time-bearing activity requires an authored Mission with `durationSteps`.
- End the intermezzo when the next morning arrives, the chosen visit or pause achieves its obvious purpose, the player leaves its chosen destination, the player attempts a substantial new undertaking not represented by an available Mission, or continuing would become unbounded free-roam drift.
- When it ends, present the current next-Mission picker again. Do not force a Mission selection merely because the player took an intermezzo.
- An `immediateFollowUp` creates a continuous Mission chain and overrides intermezzi. Complete the predecessor's terminal bookkeeping, then begin the named successor directly. Carry forward the existing team unless the successor explicitly requires another valid composition. A brief authored transition may connect them, but there is no recovery, sightseeing, shopping, or alternative Mission choice between them. If the action budget splits the transition across responses, continue it automatically rather than asking the player what to do. Normal selection resumes after the first Mission in the chain that has no `immediateFollowUp`.

### Calendar

- The campaign date is the Mission Journal's `currentMonth` + `currentMoonPhase`. One `durationSteps` step advances one phase in the sequence 🌑 → 🌒 → 🌓 → 🌔 → 🌕 → 🌖 → 🌗 → 🌘 → next month 🌑. After December comes January. No year is tracked in v1.
- A fixed-date Mission becomes due when the current date reaches or passes its fixed date.
- When a fixed-date Story or Interlude becomes available, it takes priority: do not advance the campaign clock further until that Story or Interlude is completed. This does not retroactively interrupt a Mission already being played; enforce the priority when that Mission ends and the calendar advances.
- If a multi-step duration crosses a fixed mandatory date, make that Mission due at the resulting date and block further advancement until it is completed.

### Unlocks and expirations

- An upcoming Mission becomes available when:
  - every Mission in its `unlockAfter` is `completed` (an empty list imposes no prerequisite), and
  - any fixed-date requirement has arrived (or it has none).
- If an upcoming or available Opportunity has any `expiresWhenAvailable` Mission become `available`, set the Opportunity to `expired` and remove its pin.
- Resolve newly available mandatory Story/Interlude Missions before offering further time-consuming choices.
- The AI GM evaluates prerequisites and writes explicit status changes; status is never computed.

### Lore unlocks

- **Common Knowledge:** set the Lore Card's `discovered` to true.
- **Deep Lore:** set `discovered` and `deepLoreKnown` to true.
- **Fey Knowledge:** set `discovered` and `feyKnowledgeKnown` to true.
- Apply this consistently to start, beat, completion, and failure Lore unlock rows.
- Never set a deeper knowledge flag while leaving `discovered` false.

### Mission Trackers

- Mission Trackers are **Mission-local objective state, not inventory**. They are labels and counters used only to represent progress inside a Mission, stored directly on the Mission file in the `missionTrackers` array. They are not equipment, have no weight, price, owner, location, or inventory behavior, and live only for the lifetime of their Mission.
- The tracker list may be omitted when a Mission has no item/count objectives. An omitted or empty list simply shows no trackers.
- The GM updates `current` immediately when the fiction acquires, loses, places, spends, recovers, or secures the tracked thing.
- Values are whole numbers and normally remain from 0 through target. (The schema enforces `current >= 0` and `target >= 1`. Craft's supported `number` schema does not enforce integer-only values, so the AI GM must keep both values as whole numbers and normally clamp at the target unless an authored Mission explicitly requires surplus tracking.)
- `target: 1` means one unique acquired object; `target` above 1 means counted progress.
- `showAtZero` controls whether an empty tracker is displayed: `current: 0` with `showAtZero: false` stays hidden; `current: 0` with `showAtZero: true` displays as 0 / target.
- Trackers are shown to players only while their Mission is `active`.
- Reaching a tracker's target does not automatically complete the Mission; the Mission completes only when its authored instructions explicitly say so.
- Mission completion, failure, or expiration ends the tracker's player-facing lifetime; stored values may remain on the Mission as history.
- No tracker automatically transfers to another Mission in v1. If future campaign content genuinely requires cross-Mission persistence, extend the system then rather than simulating an inventory now.

### Map behavior

- The next-Mission multiple-choice prompt is authoritative. Per-playthrough world-map pins may also show where available Missions begin, but they are supplementary and must never replace or delay the prompt. Shared author-time maps are never mutated during play; use the runtime scene-map scope.
- Upcoming Missions have no pin.
- When a Mission becomes available, add its linked pin at `mapX`/`mapY` on `selectorMap`, labeled `pinLabel` (falling back to the Mission name). Re-placing the same linked Mission pin must not create duplicates.
- When selected and made active, visually highlight or restyle its pin.
- When completed, failed, or expired, remove its selectable pin. The Mission file and the Mission Journal retain the permanent history.
- Evaluate status first, then synchronize the scene-map pins. Pin behavior is GM-driven, not data-bound: changing a Mission status does not automatically mutate a map without the GM performing the scene-map action.
- Never use author-time Location/open map mutations for playthrough availability.
- Visual styling: distinguish Mission kinds with a clear, consistent temporary colour/icon scheme and use a stronger highlight for the active pin. Exact colours and icons are a playtest choice, not binding canon — record and adjust them during testing.

### Context and playability

- The Mission Journal contains mutable campaign state and should be pinned in GM context. Mission files should be searchable or partial rather than all pinned. System Rules must remain standing GM context.
- Because context visibility is app-managed, verify manually: **System Rules → pinned; Campaign Mission Journal → pinned; Mission files → partial or searchable.**

## Guiding the Game Master

- Do not ask for a roll merely because the player performs an action or names an Ability. Roll only under the Checks rules: uncertainty, an interesting outcome either way, and one meaningful Condition at risk.
- Keep combat quick and rare; always offer a stealthy, social, or investigative alternative.
- Let side missions be tackled in any order between story missions.
- The emoji shorthand in design notes is internal only — never present it to players.
