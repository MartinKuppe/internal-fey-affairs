---
name: Connect Origin Pregens to Campaign
description: Finish live character selection and selected-protagonist
  integration for the 21 locally prepared pregens.
$craft:
  referenceId: 01a0cb01-1cb5-76d5-a286-e3249e25eddc
---

# Connect Origin Pregens to Campaign

## Already prepared locally

There are 21 Player Characters in `/Characters/Player Characters/`, one per Origin. The Blacksmith retains his identity and portrait; the other twenty reuse their Origin artwork. Each has the Origin's three Abilities, one Attribute at 2, the assigned Weakness at 0, and five Attributes at 1. All wear Common Clothes and own one spare. Grace Ledgers, known Bargains and signed Bargains are empty; all Condition Depletion is zero. Do not recreate these characters or generate images.

The storyteller pregen is **The Seanchai**, linked to the existing **Seanchaí** Origin. Keep him distinct from the GM persona.

## Oracle: finish in the authoring project, not this game instance

1. Read the **live Game Start designation metadata**. The local Game Start export has previously contained obsolete opening instructions: do not replace the live script with that export. Preserve the current static opening, dialogue, locations, goat prompt and transition into Down the Rabbit-Hole.
2. Mark all 21 pregens playable and offer them as alternatives in the existing Game Start. Bind player dialogue cards to the selected character through a supported mechanism; if per-character openings are required, preserve the same script for each. No unselected pregen should join the party or speak for the player.
3. Remove the hardcoded Blacksmith from `mandatoryMembers` in all four missions. Down the Rabbit-Hole keeps teamSize 1: the selected protagonist. Cream Tax, Milk and Feathers, and Iron and Wax keep teamSize 2: that protagonist plus mandatory Meabh. Add all pregens to `selectableMembers` if required by the current schema and team procedure.
4. Update Mission Procedures to use exactly the lobby-selected PC for the protagonist slot and its `assignedTeam` reference. Conditions, rewards, Grace and Bargain participation belong to that PC, not automatically the Blacksmith. Preserve mission states and story beats.
5. Update Character Creation's Blacksmith-only quick-start wording to cover all pregens. They are alternative protagonists in the same household/story role, not 21 simultaneously present characters. Preserve the manual character-creation option; selecting a pregen should not repeat completed choices.
6. Check the opening, Granny's Hut, and related directions for protagonist-specific assumptions. Keep actual world smiths and smithcraft clues, but do not give every Origin the Blacksmith's expertise. Preserve Grannie's family relationship and the existing plot.

If selected-character binding requires an unsupported feature or a new design decision, report that specific blocker before altering the opening structure.

## Verify and report

Verify 21 playable choices with the existing portraits. Test Blacksmith and one non-Blacksmith in fresh games: correct speaker/portrait, selected protagonist in the mission team, zero starting Grace and Bargains, Common Clothes worn, assigned spare unlocked, correct Attributes and Abilities. Report any remaining manual setup. Do not modify an existing playthrough as a substitute for fixing the authoring project.
