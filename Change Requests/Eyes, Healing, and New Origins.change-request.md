---
name: Eyes, Healing, and New Origins
description: Adds the Eyes attribute, Healing ability, and Fairy Doctor, Druid,
  and Herbalist origins, completing the seven-attribute V2 origin roster.
$craft:
  referenceId: 019ff7eb-af42-757d-af35-39226a7b1010
---

# Brief: Eyes, Healing, and New Origins

**Status:** Done
**Opened:** 2026-08-12
**Reviewer:** ChatGPT (Codex)

## Goal

Expand character creation from six to seven main attributes by adding Eyes (perception), add Healing to the trained abilities, and complete the V2 roster with three origins that use them.

## Relevant canon

- Eyes governs perception.
- Healing is a practical trained ability; it does not introduce a new supernatural healing system.
- The three new origins and their grants are fixed by the V2 roster.
- Tailor and Grocer belong under Eyes in V2.

## Files touched

- `/file-types/character.json`
- `/Characters/Example Character.character.json`
- `/Attributes/Eyes.attribute.json`
- `/Abilities/Healing.ability.json`
- `/Origins/Fairy Doctor.origin.json`
- `/Origins/Druid.origin.json`
- `/Origins/Herbalist.origin.json`
- `/Origins/Tailor.origin.json`
- `/Origins/Grocer.origin.json`
- `/GM Instructions/System Rules.gm-instructions.md`
- `/Change Requests/Eyes, Healing, and New Origins.change-request.md`

## Changes

- [x] Add optional `attributes.eyes` to Character with the same 1–5 range and default 2 as the other attributes.
- [x] Add Eyes (Perception) and Healing records.
- [x] Add Fairy Doctor (Hands; Healing, Fey Lore, Bargains).
- [x] Add Druid (Head; Healing, Learnedness, Nature).
- [x] Add Herbalist (Eyes; Healing, Nature, Wayfinding).
- [x] Reassign Tailor and Grocer to Eyes without changing their abilities or descriptions.
- [x] Give Example Character Eyes 2 and update the rules counts and lists.

## Acceptance criteria

- [x] The Character sheet's existing dynamic Attribute Dice grid displays Eyes whenever it is stored.
- [x] All three new origins grant exactly three valid Ability references and one valid Attribute reference.
- [x] The V2 roster contains seven attributes, twenty abilities, and twenty-one origins.
- [x] Existing character and origin content remains otherwise unchanged.

---

## Done — implementation notes

Added the five new records without images or identity metadata; Craft will mint their identities when pushed. Character's attribute object now accepts Eyes with the established range/default, and Example Character stores Eyes 2 so the dynamic attribute grid displays all seven attributes immediately. Tailor and Grocer now reference Eyes. System Rules now state the correct seven-attribute, twenty-ability, and twenty-one-origin totals.

No layout rewrite was necessary: Character already renders the dynamic `_attributesEntries` collection. No check-resolution, Condition Track, outfit, Grace, contract, lore-card, Species, or Archetype behavior changed.

## Review

Implemented locally by Codex; validation results reported in the handoff.
