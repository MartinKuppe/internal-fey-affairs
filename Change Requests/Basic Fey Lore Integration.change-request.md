---
name: Basic Fey Lore Integration
description: Organises foundational Fey canon into searchable GM dossiers,
  concise typed records, and the existing player-facing Lore Card system.
$craft:
  referenceId: 01a063c4-7e26-751d-a63e-d8aacea7c6cf
---

# Brief: Basic Fey Lore Integration

**Status:** Done
**Opened:** 2026-09-02
**Reviewer:** ChatGPT (Codex)

## Goal

Integrate the supplied foundational lore without turning System Rules into an encyclopedia or leaking late campaign secrets to players. Store each fact once at its authoritative depth, then keep typed records and Lore Cards as concise, deliberately scoped projections.

## Relevant canon

- In ordinary metadata and narration use Fey; reserve Elgafar/Elgafari for rare technical lore. Normalise draft variants such as Eldarari and Eldafari to Elgafari.
- Actual Fey are alien; Takelings and Changelings are human. Fey roles such as Brownie, Hob, Boggart, Banshee, and Lepracaun are Archetypes, not Species.
- Glamour is visual light-field technology; Fairy Doors preserve material continuity; Fairy Rings reclaim and reconstruct; threshold fields and Sky Rings are distinct systems.
- Morrigan’s human origin, Elgafari elevation, and Fairy Ring reconstruction/queue behaviour are protected late lore.
- Existing System Rules remain authoritative for Grace ledger operations and plea/thanks resolution.

## Files touched

- /GM Instructions/System Rules.gm-instructions.md
- /GM Instructions/Setting Background.gm-instructions.md
- /GM Instructions/Fey Origins, Biology and Senses.gm-instructions.md
- /GM Instructions/Fey Cognition, Beauty and Society.gm-instructions.md
- /GM Instructions/Grace, Language and Bargains.gm-instructions.md
- /GM Instructions/Fey Technology and Transport.gm-instructions.md
- /GM Instructions/Fey Architecture and Facilities.gm-instructions.md
- /Species/Fey.species.json
- /Archetypes/Banshee.archetype.json
- /Archetypes/Lepracaun.archetype.json
- /Archetypes/Medical Researcher.archetype.json
- /Archetypes/Brownie.archetype.json
- /Archetypes/Hob.archetype.json
- /Archetypes/Boggart.archetype.json
- /Lore Cards/Brownie.lore-card.json
- /Lore Cards/Hob.lore-card.json
- /Lore Cards/Boggart.lore-card.json
- /Grace/*.grace.json

## Changes

- [x] Add six searchable, GM-only reference dossiers organised by subject rather than one giant lore document.
- [x] Add the previously omitted Setting Background dossier covering August 151 AD, the Seventeenth Expedition, the folklore-through-science-fiction premise, and the Iron Flood revelation boundary.
- [x] Add a compact Canon and Knowledge Boundaries section to System Rules; do not duplicate encyclopedia prose there.
- [x] Add the essential setting/genre premise to standing System Rules so the GM does not need to retrieve a dossier merely to avoid generic-fantasy drift.
- [x] Create the missing Brownie, Hob, and Boggart fey-role Archetypes and link their Lore Cards as subjects.
- [x] Update Brownie’s Fey Knowledge with the supplied milk/cream/honey, Takeling/clone, and Brownie–Hob details; preserve all discovery flags.
- [x] Add concise scene-ready projections to Fey, Banshee, Lepracaun, and Medical Researcher GM notes.
- [x] Fill the existing Grace records’ issuer fields, correct their descriptions, and remove the obsolete sample note from Phantom Graces.
- [x] Replace the superseded Fey colour associations with the six canonical occupational palettes: Workers, Scientists, Naturalists, Bureaucrats, Soldiers, and Courtiers.
- [x] Preserve schemas and layouts. Do not create technology Lore Cards without authored tier text or a suitable typed subject.

## Acceptance criteria

- [x] Foundational lore is available to the GM without being pinned wholesale into every turn.
- [x] The standing rules establish Iron Age Ireland and science fiction through Celtic folklore; the fuller setting remains searchable.
- [x] Protected revelations are explicitly marked and do not appear in player-facing descriptions or Common/Deep Lore.
- [x] Brownie, Hob, and Boggart remain roles rather than Species and their cards point to real Archetype records.
- [x] Grace culture does not contradict the existing ledger and bargain mechanics.
- [x] No schema or layout changes are required; all JSON parses, cross-reference shapes follow the current schema, and the diff has no whitespace errors.
- [ ] Run Craft's server-equivalent validation during push; the `craft` executable is not available in this Codex shell.

---

## Done — implementation notes (filled by the workspace assistant)

Implemented locally by Codex. The six new GM Instruction files are intended to remain searchable rather than pinned; System Rules provides the small standing-context pointer and disclosure rules. Exact technical numbers and modern scientific or architectural names are retained only in GM reference material and accompanied by narration guidance.

After Oracle's review identified that the original source paste had omitted the setting section, a sixth searchable dossier, **Setting Background**, was added. System Rules now carries only the short premise needed every turn. The dossier records the Seventeenth Expedition and protects Albion's political division until **Iron Flood**. It also records three resolved canon rulings: Red Cap is canonical (“Recap” was a typo), Elgafar is the fifth planet of Phi Virginis, and Royal architecture uses beige, brown, red, and gold.

After the project Bible's Fey colour taxonomy was revised, **Fey Origins, Biology and Senses** was updated to the six occupational palettes. The older solitary/Banshee/high-rank/servant/administrator associations were removed rather than retained as alternatives. No other project content contained those superseded mappings.

The user-supplied Grace prose was reconciled with the existing implemented system: “please” proposes rather than transfers, System Rules’ maximum-of-plea-and-thanks rule remains authoritative, acknowledgements are normally inert, and apology separates social repair from ledger debt. No new automatic phrase-to-number table was invented.

No new technology Lore Cards were created. The supplied technology sections do not yet provide Common Knowledge / Deep Lore / Fey Knowledge projections, and the current Lore Card subject field has no general technology/object target type. Those cards can be authored later when their reveal tiers and durable subjects are decided.

## Review (filled by ChatGPT / the human after the pull)

Local implementation and structural validation recorded above. All JSON files parse and `git diff --check` reports no errors. Craft's own `craft check` could not run because the CLI executable is not available in this Codex shell; the user's push remains the server validation gate. Review the new dossiers for canon intent before pushing.
