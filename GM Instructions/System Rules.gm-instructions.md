---
name: System Rules
description: The light RPG engine for Internal Fey Affairs — attributes,
  abilities, origins, species and archetypes, checks, Grace, bargains,
  reputation markers, condition tracks, lore cards, and the Mission system.
$craft:
  referenceId: 019fde04-45a3-7a04-ae62-38704ecce2e1
  settings:
    agentEditable: true
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

The protagonist — the **Player Character** — is the only record that carries the playable mechanics: Origin, Outfit, the seven Attributes, trained Abilities, the five Condition Tracks, the Grace Ledger, and Reputation Markers.

**NPCs** (Non-Player Characters) do not possess those mechanical fields in v1. They are defined by identity and worldbuilding: name, description, image, voice, personality, background, goals, Species, Archetypes, GM Notes, and Secrets.

- Species and Archetypes remain usable for NPC classification and remain optional classification on the Player Character.
- Reputation Markers belong to the Player Character and are publicly perceptible to NPCs; they are not NPC stat blocks. NPCs do not carry Reputation Markers of their own.
- Grace Ledger rules apply to the Player Character. NPC obligations are still tracked through Bargains and fictional accounting where relevant, without giving every NPC a personal Grace Ledger.
- Mission rewards and mechanical consequences — awarded Abilities, Condition effects, Grace Ledger changes, and Reputation Marker changes — update the Player Character.
- Use "character" generically only when a rule genuinely applies to both a Player Character and an NPC.

## Outfits

- A Player Character wears one current Outfit, stored as the `outfit` reference on their Player Character file. Changing that reference changes what they are wearing.
- All five initial Outfits — Common Clothes, Dark Outfit, Formal Court Attire, Fabulous Glamour, Vest Inside-Out — are available to every player character in v1. Outfits may carry Condition Modifiers that adjust Condition Track Maximums (see Condition Tracks below); no other outfit bonuses or penalties, no unlock conditions, and no Grace costs exist yet.

## Condition Tracks

Five Condition Tracks measure how much the Player Character can take before the fiction turns against them: **Health**, **Wakefulness**, **Composure** (persistent personal conditions) and **Decorum**, **Concealment** (scene-position tracks). Each track runs 0–3: 3 is strong, and reaching 0 triggers a consequence. No track can kill a player character, and no track failure may block campaign progress — it changes the situation instead.

The Player Character stores `conditionState`: per track, a `maximum` (the currently applicable Effective Maximum after outfit, environmental, and situational modifiers) and `depletion` (accumulated damage, fatigue, strain, embarrassment, or exposure that remains when Maximum changes). Current is always derived — `current = clamp(maximum − depletion, 0, 3)` — and is never stored. Clamp stored Depletion to 0–3. A Player Character without `conditionState` is treated as Maximum 3, Depletion 0.

### Relevance

- Pressure only tracks that matter to the current situation. Do not call for Depletion merely because a track exists.
- Decorum is metaphorical social and aesthetic appropriateness, not a cleanliness meter.
- Concealment represents whether the player character remains unnoticed in the current stealth situation.

### Checks and Depletion

Before a risky check, tell the player which track is threatened, how much Depletion is at stake, and what reaching 0 would mean.

- Abilities affect the check under the existing rules. They do not raise track Maximums.
- Normally: success avoids the threatened Depletion; an ordinary failure adds 1 Depletion and advances the fiction with a complication; an especially severe or clearly announced failure may add 2 Depletion.
- Never inflict more than 2 Depletion from one ordinary check.
- Certain fictional events may cause Depletion without a roll when no uncertainty exists.
- Clamp stored Depletion to 0–3.

### Maximum changes

- Begin from Base Maximum 3.
- Apply only outfit/environment/situation modifiers whose circumstances currently apply.
- Clamp the Effective Maximum to 0–3 and store that result as the track's `maximum`.
- Changing Maximum never changes Depletion.
- Positive Maximum modifiers may be wasted at the cap; this is intentional.
- A Maximum reduction may cause Current to reach 0.

### Zero crossing

Trigger the track's consequence when a relevant track newly crosses from above 0 to 0 because of a new setback or newly applied modifier. Do not retrigger it every turn it remains at 0, every time the file is read, or every time the formula is recalculated without a new change.

Consequences:

- **Health 0:** unconscious or otherwise incapacitated; never dead from the track alone.
- **Wakefulness 0:** falls asleep or must rest.
- **Composure 0:** panic, freezing, flight, or blurting something damaging, chosen to fit the scene.
- **Decorum 0:** visible humiliation or serious social failure; may create an appropriate existing Reputation Marker such as Unsightly or Disrespectful.
- **Concealment 0:** detected.

The consequence is a fictional event or persistent state outside the number. Raising Maximum afterward does not undo it.

Failure must not block the campaign: detection may lead to pursuit, bluffing, capture, or a changed approach; incapacitation may lead to rescue, capture, lost time, or another continuation; social failure changes relationships or reputation but does not halt the story.

### Recovery and resets

Recovery removes Depletion only when fiction supports it:

- **Health:** treatment, healing, or sufficient rest.
- **Wakefulness:** sleep or equivalent genuine recovery.
- **Composure:** reassurance, safety, time, or regaining control.
- **Decorum:** normally resets for a genuinely new social situation.
- **Concealment:** normally resets when the character successfully establishes a genuinely new hiding or infiltration situation.

Existing consequences remain even after Depletion is removed or a scene track resets.

### Outfit changes

When the Outfit or relevant circumstances change:

1. Read the Outfit's modifier definitions.
2. Decide which conditions apply.
3. Recalculate affected Maximums from Base Maximum 3.
4. Preserve every Depletion value exactly.
5. Narrate any newly caused zero crossing.

## Attributes

Seven attributes, each rated 1–5 dice:

- **Body** — strength
- **Hands** — agility
- **Head** — intelligence
- **Heart** — intuition
- **Legs** — speed
- **Shadow** — deception
- **Eyes** — perception

At character creation, the attribute linked to the Player Character's origin starts at 3 dice; the other six start at 2.

## Abilities

Twenty abilities, each trained or untrained: Fey Lore, Learnedness, Charisma, Art & Beauty, Music, Milk, Repairs, Iron, Animals, Bargains, Lying, Court life, Stealth, Nature, Wayfinding, Riding, Dancing, Food, Weapons, Healing.

An origin grants the Player Character exactly three trained abilities. The GM may award additional abilities to the Player Character during play.

## Checks

1. The GM chooses the attribute for the task, and an ability when one clearly applies.
2. The player rolls that many d6s — the attribute rating, plus one extra die if trained in the ability.
3. Every die showing 4 or more is a success.
4. The GM sets the difficulty as the number of successes needed: 1 easy, 2 standard, 3 hard, 4+ very hard.
5. Resolve the outcome from the number of successes before asking follow-ups or continuing.

## Origins

When a player picks an origin, it defines who their character was before the story began: their attribute, their three abilities, and their place in the village. See the Origins folder for the twenty-one choices.

- A newly created playable protagonist (the Player Character) is expected to have an Origin.
- NPCs do not possess Origin, Outfit, Attributes, trained Abilities, Condition state, a Grace Ledger, or Reputation Markers in v1. They are defined by identity and worldbuilding fields (Species, Archetypes, description, personality, background, goals, GM Notes, Secrets).

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

- One Bargain file per contract. Every bargain has at least two parties; each party carries its exact obligation wording, its due condition, and its resolved state.
- Parties may be characters (Player Character or NPC), informal groups, or legal entities — a party's label names them; link a character file when one exists.
- Create a Bargain file whenever an obligation forms: a grace-trade, a settlement for wrongdoing, an employment or mission contract, an indenture, or any contract the player asks to see.
- A party's obligation may link to the specific contract it falls under via its Reference contract field (a Bargain reference, when that contract is a different document). Related contracts are also linked on the bargain file itself.
- A bargain is player-visible when it is the player's own or was witnessed, overheard, read, disclosed, or discovered during investigation. Set "Known to the player" and narrate it.
- When a new bargain is created, announce it clearly and immediately — a distinct, consistent system message naming the parties and the key terms.

## Reputation Markers

- Markers belong to the Player Character and are publicly perceptible to NPCs; they are universal markers, not NPC stat blocks. NPCs do not carry their own Reputation Markers.
- Markers are simply present or absent — no scores, no scopes, no severity.
- Initial markers: Ungrateful, Disrespectful, Unsightly, Violent, Cruel, Untrustworthy, Oath-Breaker, Generous, Respectful, Handsome, Elegant, Merciful, Trustworthy, Oath-Keeper.
- Each active marker carries a short reason explaining how it was acquired.
- Markers influence NPC reactions and fictional position. Never add automatic hidden numerical modifiers.
- Narrate when a marker is acquired or removed.

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

### Starting the campaign

Game Start launches the Mission Journal's `startingMission` directly. The starting Mission requires no selection pin. (Actual Game Start wiring and the first Mission are deferred until campaign content exists.)

### Selecting a Mission

- Only `available` Mission pins are presented for selection.
- Opening a Mission pin displays its brief but does not itself start the Mission. The player explicitly chooses to begin it.
- On selection:
  1. Validate and record `assignedTeam`: every `mandatoryMembers` member is included, the count matches `teamSize` when defined, remaining members come from `selectableMembers`, and no character reference is duplicated.
  2. Set `status` to `active`.
  3. Set `startedDate` to the current campaign date.
  4. Set `currentBeatKey` to the first beat's key.
  5. Apply `startLoreUnlocks`.
  6. Highlight the Mission's pin.
  7. Begin at `startLocation` or the first beat's Location.

### Playing beats

- Follow `storyBeats` in authored order unless the Mission explicitly says otherwise.
- Update `currentBeatKey` as play advances.
- Apply each beat's `loreUnlocks` when that beat is reached or resolved as authored.
- Individual actions and plans may fail. Failure must alter cost, time, Conditions, position, reputation, relationships, or approach rather than block Story, Side, or Interlude completion.

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

- Missions are selected through per-playthrough pins on the world map. Shared author-time maps are never mutated during play; use the runtime scene-map scope.
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

- When the user says they are using a skill or performing an action, ask them to roll an appropriate check before resolving the outcome.
- Keep combat quick and rare; always offer a stealthy, social, or investigative alternative.
- Let side missions be tackled in any order between story missions.
- The emoji shorthand in design notes is internal only — never present it to players.
