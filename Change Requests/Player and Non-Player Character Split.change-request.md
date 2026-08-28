---
name: Player and Non-Player Character Split
$craft:
  referenceId: 01a04a1b-11a0-7528-9daa-44879e8ccc74
---

# Change Request: Player and Non-Player Character Split

**Status:** Done (peer-review corrections applied, incl. playability)
**Opened:** (on implementation)

## Goal

Replace the single `character` file type with two independent, non-inherited types — **Player Character** (`player-character`) and **Non-Player Character** (`non-player-character`) — so the protagonist's game mechanics and NPC identity/worldbuilding live on separate records. Migrate the two existing records and every reference that targeted the old `character` type without broadening player-only mechanics to NPCs.

## Relevant canon

- The protagonist (the Player Character) is the only record that carries playable mechanics: Origin, Outfit, Attributes, trained Abilities, Condition Tracks, Grace Ledger, and Reputation Markers. NPCs do not possess those mechanical fields in v1.
- Reputation Markers belong to the Player Character and are publicly perceptible to NPCs; they are not NPC stat blocks.
- Grace Ledger rules apply to the Player Character; NPC obligations use Bargains and fictional accounting.
- Species and Archetypes remain usable for NPC classification and remain optional Player Character classification.
- Méabh is Human and a Takeling. Her existing image must be preserved; do not generate replacement artwork.

## Files touched

- `/file-types/character.json` — **retired** (deleted, recoverable from trash).
- `/file-types/player-character.json` — **new** type (designation `character`).
- `/file-types/non-player-character.json` — **new** type (designation `character`).
- `/Characters/Player Character prototype.character.json` → `/Player Characters/Example Character.player-character.json`.
- `/Characters/Méabh.character.json` → `/Non-Player Characters/Méabh.non-player-character.json`.
- Schema/layout migrations: `/file-types/bargain/schema.json`, `/file-types/lore-card/schema.json` (+ two subject fields), `/file-types/mission/schema.json`, `/file-types/mission/layout.json` (privacy-safe restore), `/file-types/lore-card/layout.json` (privacy-safe restore), `/file-types/player-character/layout.json` (condition-track editPath removed).
- Content updates: `/Bargains/One Year Employment.bargain.json`, Game Start `Tavern Start` (via the supported `game-start` metadata command).
- `/GM Instructions/System Rules.gm-instructions.md` — PC/NPC split + description restored.
- **New:** `/Change Requests/Player and Non-Player Character Split.change-request.md`.

## Changes

### Architectural decision

Two independent file types with Craft's shared `character` designation. No wrapper, interface, `ICharacter`, actor, or shared-record type was created, and no `role` flag was added to a single Character type. Fields that may point to either type use native multi-type references (`reference.targets`) listing both `player-character` and `non-player-character`.

### Player Character type (`/file-types/player-character.json`)

Role field set (protagonist mechanics): `origin` (ref `origin`), `outfit` (ref `outfit`), `attributes` (Body, Hands, Head, Heart, Legs, Shadow, Eyes; each 1–5), `abilities` (ref array `ability`), `conditionState` + computed `conditionTracks` (five fixed tracks, current derived from maximum−depletion), `graceLedger` (ref array `grace`), `markers` (Reputation Markers), optional `species` (ref `species`) and `archetypes` (ref array `archetype`), and the base `name`.

No NPC-authoring fields (`personality`, `background`, `goals`, `GM Notes`, `secrets`) and no obsolete D&D fields. The base `image` injected by the platform is unused by the Player Character sheet; the portrait was intentionally **not** migrated.

### Non-Player Character type (`/file-types/non-player-character.json`)

Role field set (identity/worldbuilding): base `name`, base `description`, base `image`, `voice`, `personality`, `background`, `goals`, `species` (ref `species`), `archetypes` (ref array `archetype`), `gmNotes` (title **GM Notes**), and `secrets` (AI/GM-only). None of the player mechanics and no placeholder versions of them.

### Reference migration

- **Bargain** `parties[].refs` — now a multi-type reference targeting both `player-character` and `non-player-character`.
- **Lore Card** subjects — split into **two** reference arrays (`subjects` for species/archetype/outfit/location; `characterSubjects` for player/non-player characters) to keep Outfit valid while respecting the 5-type cap; neither is rendered on the player-facing card.
- **Mission** `mandatoryMembers`, `selectableMembers`, `assignedTeam` — now multi-type references accepting both Player Character and Non-Player Character; team-size and assignment rules unchanged. The Mission layout is the privacy-safe version (only the three team grids changed for this split).

Player-only mechanics remain Player Character–only (`awardedAbilities` → `ability`, appended to Player Character `abilities`; Origin/Outfit/Condition/Grace/Reputation rules scoped per System Rules).

### Mission and Lore Card privacy-safe layouts

Both layouts were restored to their pre-split, player-facing forms. Mission renders no GM-only administration; Lore Card keeps the three knowledge tabs and reveal behavior and renders/edits none of `gmNotes`, `subjects`, `characterSubjects`, `unlockGuidance`, or `feyLoreUnlocksDeepLore`, and contains no `Checkbox`.

### Player Character Condition Track layout (provisional)

The provisional layout displays the computed `current` value read-only; the earlier `editPath` writing into `conditionState/{key}/depletion` was **removed**. No second stored current value.

### Existing record migration

There were exactly two Character records (confirmed before acting).

**Player Character prototype → Example Character** (`/Player Characters/Example Character.player-character.json`)
- Preserved: `origin` (blacksmith), `outfit` (common-clothes), `abilities` (repairs, iron, weapons), seven `attributes`, full `conditionState`.
- No graceLedger, markers, species, or archetypes were present; none invented.
- Prototype prose (description, personality, notes) and portrait were **not** migrated; the auto-generated cover image on the new record was **removed** (with generation metadata).

**Méabh → Non-Player Character** (`/Non-Player Characters/Méabh.non-player-character.json`)
- Preserved: name, `species` (human), `archetypes` (`takeling`), original image URL (`890eed6e-…`), and the background-removed representation stored under `overlayUrl` (`8e1b70c5-…`).
- Méabh is **not playable** (not in Game Start `startingCharacters`; not a `player-character` designation).

### Playability / Game Start

Using Craft's supported `game-start` metadata command (the only surface for the play configuration, which lives as designation metadata outside the VFS content files):

- Set `Tavern Start.startingCharacters` to the new Player Character record via its VFS path (`/Player Characters/Example Character.player-character.json`); the command reports **1 playable character** and `game-start get` lists `example-character.player-character` as the **sole** starting character. Per the design, being offered in `startingCharacters` is the supported mechanism for a playable pregen in the lobby.
- Méabh is absent from `startingCharacters`, so she is **not** offered/playable.
- Read-back via `game-start get`: sole starting character = `example-character.player-character`.

### Old Character type disposition

The retired originals and `/file-types/character.json` were removed after migration via Craft's delete mechanism (recoverable from trash). The `character` file type no longer exists; `/Characters/` holds no live files. A full-VFS search finds no live reference to the retired type or its deleted records.

## Acceptance criteria (verified read-back)

- [x] Both new types exist with `designation: character`.
- [x] Player Character contains the player mechanics and no NPC-only custom fields.
- [x] Non-Player Character contains the NPC identity/lore fields and none of the player mechanical fields.
- [x] Example Character exists only as the live Player Character; `game-start get` reports it as the sole starting character (offerable/playable in the lobby).
- [x] Méabh exists only as the live Non-Player Character and is not offered/no Game Start reference (not playable).
- [x] No live VFS content references the retired `character` type or its deleted records.
- [x] Méabh's original image URL and its background-removed representation are both present.
- [x] Example Character contains no generated image.
- [x] The Change Request contains no generated image.
- [x] Mission layout retains its spoiler protection and exposes no GM-only mission administration.
- [x] Lore Card layout exposes no GM Notes, subject administration, unlock guidance, or Fey Lore administration; every component supported; no `Checkbox`.
- [x] Outfit remains a valid Lore Card subject (`subjects` → species, archetype, outfit, location; `characterSubjects` → both character types).
- [x] Player Character Condition Track current values are derived and read-only in the provisional layout (no `editPath`).
- [x] The One Year Employment bargain links both Example Character (`player-character/example-character`) and Méabh (`non-player-character/méabh`).
- [x] The System Rules description is restored.
- [x] The Change Request's Done section agrees with the actual read-back results.

## Done — implementation notes (filled by the workspace assistant)

**Final verification (read back after the playability correction):**

- `game-start get` → `startingCharacters: ["example-character.player-character"]` — the new Player Character is the **sole** starting character; the `game-start set` command reports **1 playable character**. The friendly projection `example-character.player-character` identifies the new Player Character record, whose hydrated reference ID confirms as `01a04a10-fa51-764c-b20e-02ee5da32f8f` (verified via the Bargain's stored reference). Méabh is absent from `startingCharacters`, so she is not offered/playable.
- Full-VFS grep for the retired reference ID `019fde04-455f-74bc-99e2-43a11d3b7b84` and for `player-character-prototype` / `Player Character prototype` returns **no live content** — the only remaining occurrence is historical prose in this Change Request document.
- Example Character reads back with origin/outfit/abilities/attributes/conditionState and **no image**.
- Méabh reads back with name, human species, Takeling archetype, original image `890eed6e-…`, and background-removed `8e1b70c5-…` under `overlayUrl`.
- Bargain `parties[0].refs[0]` = `player-character/example-character`; `parties[1].refs[0]` = `non-player-character/méabh`; Green stays label-only.
- Mission team schema/layout uses `reference.targets` / `fileTypeSlugs` for both character types; layout is privacy-safe (no GM sections).
- Lore Card `subjects` (species/archetype/outfit/location) and `characterSubjects` (both character types); layout privacy-safe, no `Checkbox`.
- Player Character condition-row has no `editPath`.
- System Rules description restored.

**About raw Game Start reference verification:** the Game Start play configuration (starting characters, opening) is designation metadata stored **outside the VFS content files**; it is not greppable and the `game-start` command is the only supported reader/writer, projecting friendly slugs and character counts rather than raw reference IDs. The supported command has been applied and read back showing the new Player Character as the sole starting character. The retired reference ID does not appear in any live VFS content. If a fresh checkout of the game-start designation metadata were to still contain the retired ID while the tool reports the new record, that would be an unresolved Craft synchronization issue to flag rather than mark complete — per that standard, verification here is based on the tool's read-back and the absence of the retired ID in all readable project content.

## Review (filled by ChatGPT / the human after the pull)

See peer-review corrections addressed above, including playability.