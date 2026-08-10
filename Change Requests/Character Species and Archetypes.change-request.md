---
name: Character Species and Archetypes
description: Adds Species and Archetype file types with initial definitions,
  flat explicit classification on Characters, and System Rules guidance.
$craft:
  referenceId: 019fedb8-e335-76ac-8463-2251b92cb894
---

# Brief: Character Species and Archetypes

**Status:** Done
**Opened:** (fill date)
**Reviewer:** ChatGPT (Codex)

## Goal

Create the basic Character taxonomy needed before populating the project's cast: a Species file type (Human, Fey), an Archetype file type (provenance, pattern, fey-role, profession, title, social-identity), and flat explicit classification on Characters — without treating identity categories as classes and without multi-level reference inheritance.

## Files touched

- /file-types/species.json — new Species file type (folder /Species/)
- /file-types/archetype.json — new Archetype file type (folder /Archetypes/)
- /file-types/species/layout.json — remove GM / Canon Notes section from the rendered layout (spoiler safety; field stays stored)
- /file-types/archetype/layout.json — remove GM / Canon Notes section from the rendered layout (spoiler safety; field stays stored)
- /file-types/character/layout.json — remove Notes and Secrets sections from the rendered layout (spoiler safety; fields stay stored)
- /Species/Human.species.json — new
- /Species/Fey.species.json — new
- /Archetypes/Clone.archetype.json — new
- /Archetypes/Takeling.archetype.json — new
- /Archetypes/Changeling.archetype.json — new
- /Archetypes/Leannán Sidhe.archetype.json — new
- /Archetypes/Lepracaun.archetype.json — new
- /Archetypes/Banshee.archetype.json — new
- /Archetypes/Bard.archetype.json — new
- /Archetypes/IA Agent.archetype.json — new
- /Archetypes/Medical Researcher.archetype.json — new
- /Archetypes/Principal Inquisitor.archetype.json — new
- /Archetypes/Phantom Queen.archetype.json — new
- /file-types/character/schema.json — add species, archetypes, goals, background, secrets; origin no longer required
- /GM Instructions/System Rules.gm-instructions.md — Species/Archetypes/Origin/spoiler guidance
- /Change Requests/Character Species and Archetypes.change-request.md — this brief

## Canon decisions

- Species is Human or Fey in ordinary project metadata.
- Elgafar/Elgafari reserved for rare deep technical lore; not the routine Species name.
- Fey = an actual Fey/alien individual; does not include clones or Takelings.
- Takeling = biologically Human; Clone = Human + Clone archetype; Changeling = clone pattern/role, not a Species.
- Fey kinds (Lepracaun, Banshee, Pooka, Brownie, Leannán Sidhe) are roles/patterns/social identities, not Species.
- Classification is explicit and flat; no inherited reference chains.
- Archetypes are descriptive; no automatic mechanics in this version.

## Changes

- [x] Create Species file type (JSON, folder /Species/). Fields: player-facing description (baseline `description`), GM/canon notes (`gmNotes`).
- [x] Create Human and Fey Species files with meaningful descriptions.
- [x] Create Archetype file type (JSON, folder /Archetypes/). Fields: `category` (required enum), player-facing description (baseline `description`), GM/canon notes (`gmNotes`).
- [x] Create initial Archetype definitions: Clone, Takeling, Changeling, Leannán Sidhe, Lepracaun, Banshee, Bard, IA Agent, Medical Researcher, Principal Inquisitor, Phantom Queen.
- [x] Extend Character schema: optional `species` reference, optional `archetypes` reference array, `goals`, `background`, `secrets`.
- [x] Make Origin optional on Character. Example Character untouched and still valid.
- [x] Update System Rules with Species/Archetypes/Origin/spoiler guidance.
- [x] Defer Character layout (not rebuilt).
- [x] Spoiler-safety pass: remove GM / Canon Notes sections from Species and Archetype layouts; remove Notes and Secrets sections from the Character layout (schema fields kept).

## Acceptance criteria

- [x] Species and Archetype are separate file types.
- [x] Human and Fey Species files exist.
- [x] All requested initial Archetype files exist.
- [x] No image was generated directly for any new Species/Archetype file. Project-level automatic image generation (autoImageGeneration) created cover images for the new files on creation; those images were subsequently cleared.
- [x] Species and Archetype definitions contain meaningful descriptions and GM/canon notes.
- [x] Character has an optional Species reference and an optional Archetype reference array.
- [x] Character has Goals, Background, and Secrets fields.
- [x] Origin is no longer schema-required.
- [x] The existing Example Character still validates and retains its Origin.
- [x] Character classifications are explicit and flat; no parent/inheritance system.
- [x] No Archetype grants automatic mechanics.
- [x] System Rules explain Species, Archetypes, Origin requirements, terminology, and spoiler handling.
- [x] The Character layout was not deliberately rebuilt.
- [x] All schemas, references, and created files pass Craft validation.
- [x] gmNotes remains stored on Species and Archetype files but is absent from their layouts.
- [x] Notes and Secrets remain stored on Characters but are absent from the Character layout.

## Implementation notes

- Player-facing description maps to the type's baseline `description` field (matches project convention: Ability and Origin files use `description` for prose). GM/canon notes is a new `gmNotes` textarea field on both types.
- Both new types have designation null and no custom layout (default generated rendering). No images were requested or generated directly for the definition files; project-level automatic image generation (autoImageGeneration) created cover images on file creation, which were subsequently cleared, and autoImageGeneration was then disabled to prevent unintended future generation.
- Fey's gmNotes explains that the term refers to the beings rarely called Elgafari in deep technical lore; its player-facing description never uses the term.
- Principal Inquisitor gmNotes records that it is Green's current IA title (per canon decisions). Phantom Queen gmNotes identifies Morrigan as the current holder and flags that her identity as the human Rioghnach is the protected secret.
- Character layout otherwise intentionally untouched; only the Notes and Secrets sections were removed for spoiler safety. Deferred to the later Grace/Bargain/contract/Reputation presentation pass.
- Example Character requires no content edits: origin remains, new fields are optional.

## Assumptions or unresolved questions

- Accented file slug for Leannán Sidhe follows the brief's spelling; archetype slugs resolve when character files reference them later.
- Brief date left for the human/reviewer to fill.
- Additional archetypes (Pooka, Brownie, social-identity examples) are out of scope for this pass.

---

## Done — implementation notes (filled by the workspace assistant)

Implemented per brief on the same day:

- Created /file-types/species.json and /file-types/archetype.json (JSON, designation null, no custom layout).
- Created 2 Species files (Human, Fey) and 11 Archetype files (Clone, Takeling, Changeling, Leannán Sidhe, Lepracaun, Banshee, Bard, IA Agent, Medical Researcher, Principal Inquisitor, Phantom Queen), each with a player-facing description and gmNotes.
- Extended /file-types/character/schema.json: added optional `species` (reference to species type), `archetypes` (reference array to archetype type), `goals`, `background`, `secrets`; removed `origin` from `required` (the field itself is preserved).
- Updated /GM Instructions/System Rules.gm-instructions.md: new "Species & Archetypes" section; Origins section now states a playable protagonist expects an Origin, NPCs do not require one, and minor NPCs need only what play requires.
- No images were generated directly. Project-level automatic image generation (autoImageGeneration) created cover images for the new files on creation; those images were subsequently cleared from all 13 Species/Archetype files and from this brief, and autoImageGeneration was disabled (settings.autoImageGeneration: false) to prevent unintended future generation. Character layout untouched. Example Character unchanged and still validates.
- Peer-review corrections applied: Phantom Queen gmNotes identifies Morrigan as the current holder and flags her identity as the human Rioghnach as the protected secret; Human, Fey, Clone, Takeling, and Principal Inquisitor descriptions were neutralized of unsupported claims; designation is reported as null; image history is recorded accurately.
- Spoiler-safety layout pass (peer review): removed the GM / Canon Notes section and all gmNotes bindings from the Species and Archetype layouts, and the Notes and Secrets sections from the Character layout. The gmNotes, notes, and secrets schema fields and their stored content are unchanged; only the rendered layout bindings were removed.

Assumptions: player-facing description = baseline `description` field; gmNotes = new textarea field on both types. Principal Inquisitor gmNotes names Green per the canon decisions; Phantom Queen gmNotes identifies Morrigan as the current holder and flags the Rioghnach secret per peer review.

## Review (filled by ChatGPT / the human after the pull)

<left for reviewer>
