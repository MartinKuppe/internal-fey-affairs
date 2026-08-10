---
name: Character Species and Archetypes
description: Adds Species and Archetype file types with initial definitions,
  flat explicit classification on Characters, and System Rules guidance.
image:
  url: https://media.craftrpgs.com/containers/019fde04-44fd-75c8-8f5e-d680c880596d/images/e0d1b5a5-0a15-4173-8464-4763197cb7e4
  generation:
    status: success
    startedAt: 2026-08-10T22:09:27.306Z
    finishedAt: 2026-08-10T22:09:41.945Z
    prompt: "A conceptual digital art piece depicting a surreal collage of diverse
      character archetypes. In the center, a shimmering, ethereal Fey entity
      with iridescent skin and floating accents contrasts with a stark, clinical
      Human clone in a white sterile suit. Surrounding them are fragments of
      other identities: a ghostly Banshee shrouded in translucent grey mist, a
      Leprechaun in ornate emerald attire, and a high-tech IA Agent represented
      by a holographic humanoid form. The composition is an organized yet
      artistic taxonomy, with characters separated by thin, glowing geometric
      lines like a biological blueprint. Dark void background with floating
      sparks of gold and neon blue light, cinematic lighting, hyper-detailed
      textures, a mix of organic folklore and futuristic cyberpunk aesthetics."
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
- /Change Requests/01 Character Species and Archetypes.change-request.md — this brief

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

## Acceptance criteria

- [x] Species and Archetype are separate file types.
- [x] Human and Fey Species files exist.
- [x] All requested initial Archetype files exist.
- [x] No image was generated for any new file.
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

## Implementation notes

- Player-facing description maps to the type's baseline `description` field (matches project convention: Ability and Origin files use `description` for prose). GM/canon notes is a new `gmNotes` textarea field on both types.
- Both new types use designation "lore" and no custom layout (default generated rendering). No images requested or generated.
- Fey's gmNotes explains that the term refers to the beings rarely called Elgafari in deep technical lore; its player-facing description never uses the term.
- Principal Inquisitor gmNotes records that it is Green's current IA title (per canon decisions). Phantom Queen gmNotes defines the title generically and does not disclose the current holder's identity — Morrigan's human origin stays a character-file secret.
- Character layout intentionally untouched; deferred to the later Grace/Bargain/contract/Reputation presentation pass.
- Example Character requires no content edits: origin remains, new fields are optional.

## Assumptions or unresolved questions

- Accented file slug for Leannán Sidhe follows the brief's spelling; archetype slugs resolve when character files reference them later.
- Brief date left for the human/reviewer to fill.
- Additional archetypes (Pooka, Brownie, social-identity examples) are out of scope for this pass.

---

## Done — implementation notes (filled by the workspace assistant)

Implemented per brief on the same day:

- Created /file-types/species.json and /file-types/archetype.json (JSON, designation "lore", no custom layout).
- Created 2 Species files (Human, Fey) and 11 Archetype files (Clone, Takeling, Changeling, Leannán Sidhe, Lepracaun, Banshee, Bard, IA Agent, Medical Researcher, Principal Inquisitor, Phantom Queen), each with a player-facing description and gmNotes.
- Extended /file-types/character/schema.json: added optional `species` (reference to species type), `archetypes` (reference array to archetype type), `goals`, `background`, `secrets`; removed `origin` from `required` (the field itself is preserved).
- Updated /GM Instructions/System Rules.gm-instructions.md: new "Species & Archetypes" section; Origins section now states a playable protagonist expects an Origin, NPCs do not require one, and minor NPCs need only what play requires.
- No images generated. Character layout untouched. Example Character unchanged and still validates.

Assumptions: player-facing description = baseline `description` field; gmNotes = new textarea field on both types. Phantom Queen gmNotes intentionally generic to protect Morrigan's secret; Principal Inquisitor gmNotes names Green per the canon decisions.

## Review (filled by ChatGPT / the human after the pull)

<left for reviewer>
