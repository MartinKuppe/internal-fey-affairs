---
name: Lightweight Character Creation and Checks
description: Adds one chosen Weakness, owned Outfits, clearer Condition names
  and starting values, and a transparent near-diceless 2d6 resolution system
  while preserving The Blacksmith as a ready-to-play protagonist.
$craft:
  referenceId: 01a06db7-3296-7028-991e-35e14f885524
---

# Brief: Lightweight Character Creation and Checks

**Status:** Done
**Opened:** 2026-09-04
**Reviewer:** ChatGPT (Codex)

## Goal

Keep **The Blacksmith** as a complete quick-start protagonist while supplying the durable fields and rules needed for custom character creation. Character creation teaches the lightweight system through three choices: Origin, one narrative Weakness, and one spare Outfit. Replace the opaque success-count dice pool with a sum-based roll that Craft can display clearly.

## Relevant decisions

- Attributes use 0 Weak, 1 Ordinary, and 2 Strong. An Origin makes one Attribute Strong; a different chosen Weakness makes one Attribute Weak.
- Training is worth +2 because an Ability is narrower than an Attribute.
- Every roll names exactly one relevant Condition; if no meaningful Condition and consequence can be named, no roll is made.
- Condition names are Health, Energy, Nerve, Decorum, and Cover. Health begins at 3; the others begin at 2.
- Energy is deliberately broad enough to be restored by sleep, food, warmth, encouragement, or good news when the fiction supports it.
- Common Clothes are always owned. A custom protagonist chooses one of four spare starting Outfits; other Outfits are later unlocks.
- Creation-flow screens and their visual presentation are deliberately left for the human author to configure in the app.

## Files touched

- /.craft/file-types/player-character.json
- /.craft/file-types/origin.json
- /.craft/file-types/outfit.json
- /Characters/Player Characters/The Blacksmith.player-character.json
- /Outfits/Common Clothes.outfit.json
- /Outfits/Dark Outfit.outfit.json
- /Outfits/Fabulous Glamour.outfit.json
- /Outfits/Vest Inside-Out.outfit.json
- /Outfits/Thin Tunic.outfit.json
- /Outfits/Lucky Pompom Hat.outfit.json
- /Outfits/Fancy Pants.outfit.json
- /Outfits/Heavy Black Coat.outfit.json
- /GM Instructions/System Rules.gm-instructions.md
- /Missions/The Encounter.mission.json
- /Missions/Milk and Cream.mission.json

## Changes

- [x] Add a single `weakness` choice to Player Character with seven fixed narrative labels mapped to the seven Attributes.
- [x] Change stored Attribute ratings to the 0–2 scale and update their descriptions and player-facing heading.
- [x] Add `unlockedOutfits` as an array of Outfit references, separate from the currently worn `outfit`.
- [x] Rename Wakefulness → Energy, Composure → Nerve, and Concealment → Cover throughout active schemas, computed displays, rules, outfits, and Mission instructions.
- [x] Set Base Maximums to Health 3 and all four other Conditions 2.
- [x] Add the four spare starting Outfits and their +1/−1 Condition trade-offs.
- [x] Retain the existing four special Outfits as future unlocks and update their Condition keys without inventing unlock requirements or prices.
- [x] Replace the old success-count dice pool with `2d6 + Attribute + 2 if trained + current relevant Condition` against 10/12/14/16.
- [x] Define clean success, success with complication, and advancing setback outcome bands.
- [x] Migrate The Blacksmith to Blacksmith Origin → Body 2, Open Book → Shadow 0, all other Attributes 1; Common Clothes plus Heavy Black Coat unlocked; starting Conditions 3/2/2/2/2.
- [x] Repair The Blacksmith's two pre-existing test Reputation labels to valid existing markers while preserving their reasons: Liar → Untrustworthy; Hobo → Unsightly.
- [x] Leave the Player Character creation flow unset for the human author to build visually in the app.

## Acceptance criteria

- [x] Every active Attribute rule and schema uses 0/1/2 rather than a dice-pool rating.
- [x] The Blacksmith has exactly one Strong, five Ordinary, and one Weak Attribute, and the Weakness does not conflict with the Origin Attribute.
- [x] The Blacksmith may wear only owned Outfits under the rules and begins with Common Clothes plus one spare Outfit.
- [x] A new custom protagonist can be represented with an Origin, Weakness, current Outfit, unlocked Outfit list, trained Abilities, Attributes, and starting Conditions without another schema change.
- [x] All active rules and content use Energy, Nerve, and Cover; old names remain only in historical Change Requests.
- [x] A check always states its formula, target, Condition at risk, and final outcome, and required campaign progress cannot be blocked by a miss.
- [x] No creation-flow UI was added or rebuilt.

---

## Done — implementation notes

- Player Character now stores the two missing creation choices: one `weakness` enum and an `unlockedOutfits` reference list. Origin, trained Abilities, Attributes, and current Outfit remain stored because Abilities and wardrobe ownership can expand during play.
- System Rules now distinguishes the complete Blacksmith quick start from custom creation and gives the GM the exact derived starting values to populate.
- Weakness mapping: Short-Winded/Body; All Thumbs/Hands; Muddle-Headed/Head; Poor Judge of Character/Heart; Flat-Footed/Legs; Squint-Eyed/Eyes; Open Book/Shadow. System Rules forbid choosing the Origin's Strong Attribute as the Weakness.
- Condition storage retained the existing Maximum + Depletion model so outfit and situational modifiers still work. Keys and labels were migrated to Energy, Nerve, and Cover; fallback Base Maximums are now 3 for Health and 2 for the other four.
- Added Thin Tunic (+Energy, −Nerve), Lucky Pompom Hat (+Nerve, −Decorum), Fancy Pants (+Decorum, −Cover), and Heavy Black Coat (+Cover, −Energy). The Blacksmith was assigned Heavy Black Coat as the spare Outfit and Open Book as the Weakness; these are provisional pregen flavour choices that can be changed without schema work.
- The existing player-authored Character layout was preserved. Only the stale visible labels “Attribute Dice”, “Wakefulness”, “Composure”, and “Concealment” were updated; element structure and creationFlow remain untouched.
- The Blacksmith's test Reputation reasons were preserved, but their unsupported labels were normalized to the closest existing schema values: Untrustworthy and Unsightly.
- The local Craft CLI is not installed in this shell. JSON parsing, schema/content consistency, reference targets, active terminology, and layout bindings were checked locally; Craft push/import remains the platform validation gate.

## Review

**Verdict: Passed — no blocking or functional defects.**

Reviewed against the brief: the Player Character schema, The Blacksmith, all 21 Origins, all 9 Outfits, System Rules, all three Missions, the Mission Journal, and the Character layout. Confirmed successes:

- `weakness` enum maps the seven labels to the seven Attributes; Attributes accept 0–2 (0 Weak, 1 Ordinary, 2 Strong); `unlockedOutfits` is an Outfit reference array distinct from the worn `outfit`.
- `conditionState` holds exactly health/energy/nerve/decorum/cover with Base Maximums Health 3, others 2; computed `conditionTracks` matches keys, order, defaults, and `current = clamp(maximum − depletion, 0, 3)`. No removed Condition keys survive in active schema fields or expressions.
- The Blacksmith: Blacksmith Origin, Body 2, Shadow 0, other Attributes 1, Weakness Open Book, Common Clothes worn, Common Clothes + Heavy Black Coat unlocked, starting Conditions 3/2/2/2/2, no Depletion, trained Abilities Repairs/Iron/Weapons. Reputation labels normalized with reasons preserved: Liar → Untrustworthy, Hobo → Unsightly.
- All 21 Origins reference exactly one Attribute and exactly three valid Abilities; no "3 dice" language remains. All references hydrate; no computed values stored in content files.
- Outfits: four spare starting choices (+1/−1 trade-offs) verified; Common Clothes have no modifiers; the four special Outfits remain valid future unlocks with no invented prices or unlock requirements.
- Checks: `2d6 + Attribute + 2 if trained + current relevant Condition` against 10/12/14/16; clean success / complication (miss 1–2) / advancing setback (miss 3+) with Depletion bands; exactly one relevant Condition per roll, Current not Maximum; pre-roll formula/target/Condition/consequence announcement and post-roll outcome report; ordinary actions not rolled; no active rule uses the former dice pool; fail-forward and zero-crossing rules coherent.
- Stale terminology (Wakefulness, Composure, Concealment, Attribute Dice, success-count dice) remains only in historical Change Requests and plain-English usage; The Encounter and Milk and Cream now use Cover and Nerve.
- The player-authored Character layout was preserved; visible labels now say Attributes, Energy, Nerve, Cover; Condition controls point at the correct computed rows; tabs, Grace/Bargain/Reputation displays, and styling unchanged; `creationFlow` remains unset.

Recorded decisions:

- The implementation passed review with no blocking or functional defects.
- The human author has verified in the app that System Rules is pinned.
- Hard-filtering the Outfit selector to `unlockedOutfits` is not supported by the current Craft schema/layout system; GM-rule enforcement is accepted for v1.
- The old internal layout element IDs, the empty `conditionstate-section`, and the minor Mission-kind vocabulary drift (Primary/Secondary vs Story/Side) are harmless and deliberately deferred while the UI and Mission system remain under development.
- The Player Character creation flow remains intentionally unset for the human author to configure later.
