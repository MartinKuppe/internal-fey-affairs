---
name: Lore Card System
description: Adds Lore Card and Lore Journal file types, a campaign Lore
  Journal, discovery and unlocking GM rules, and three sample cards (Brownie,
  Hob, Boggart). Peer-review corrections applied and verified.
$craft:
  referenceId: 019ff086-a7e9-7558-9db8-e96b6554dd3b
---

# Brief: Lore Card System

**Status:** Done
**Opened:** (fill date)
**Reviewer:** ChatGPT (Codex)

## Goal

Add player-facing Lore Cards with three gated knowledge tiers (Common Knowledge, Deep Lore, Fey Knowledge), conditional mystery/revealed imagery, a thin Lore Journal index that derives discovered cards from the cards' own flags, and concise GM rules for discovery and unlocking.

## Relevant canon

- Supplied Common/Deep/Fey text for Brownie, Hob, and Boggart (transcribed verbatim into the sample cards).
- Fey kinds such as Brownie are roles/functions/training patterns/social identities — Archetypes, not Species. No Brownie/Hob/Boggart Archetype files are created in this pass; empty `subjects` arrays are acceptable until world records exist.
- Grace is issued as separate queen/court-linked currencies. There is NO numeric `graceCost` on Lore Cards; asking a Fey for knowledge may create a contextual Bargain and Grace consequence under the existing Bargain and Grace Ledger rules.
- No Mission file type exists yet. Mission-granted unlocks are deferred; the future Mission type should be able to reference the Lore Cards it grants.
- Lore Card state is project-global (one shared content library, no per-campaign file copies) — suitable for this single-campaign project, but it does not isolate discoveries between multiple simultaneous campaigns.
- No stored `partyHasFeyLore` flag. When relevant, the GM inspects the actual player character's Ability references (Fey Lore).
- Sample card images stay empty. Deep Lore and Fey Knowledge unlock independently.

## Files touched

- /file-types/lore-card.json — new Lore Card file type (schema + layout)
- /file-types/lore-journal.json — new Lore Journal file type (schema + layout)
- /Lore Cards/Brownie.lore-card.json — new sample card
- /Lore Cards/Hob.lore-card.json — new sample card
- /Lore Cards/Boggart.lore-card.json — new sample card
- /Lore Journals/Lore Journal.lore-journal.json — new campaign journal
- /GM Instructions/System Rules.gm-instructions.md — add a Lore Cards section
- /Change Requests/Lore Card System.change-request.md — this brief

## Changes

- [x] Create the Lore Card file type (folder /Lore Cards/): `subjects` (optional multi-target reference array — species, archetype, character, equipment, location), `commonKnowledge`, `deepLore`, `feyKnowledge`, `expertRegions` (string array), `mysteryImage` (image), `revealedImage` (image), `discovered` / `deepLoreKnown` / `feyKnowledgeKnown` (booleans, default false), `feyLoreUnlocksDeepLore` (boolean, default false), `unlockGuidance` (GM-facing), `gmNotes` (GM-facing).
- [x] Do NOT add a numeric `graceCost` field (Grace is queen/court-currency based; consequences go through Bargain/Grace Ledger rules).
- [x] Create the Lore Journal file type (folder /Lore Journals/): stored `cards` reference array (master list, updated when cards are authored) and computed `discoveredCards` — filtered to `discovered` cards via array-method expression with `load()`, each row retaining the original card reference plus name and known-level labels.
- [x] Create one Lore Journal for the current campaign listing Brownie, Hob, and Boggart.
- [x] Create Brownie, Hob, and Boggart Lore Cards from the supplied canon; all flags false (undiscovered), no images, empty `subjects`.
- [x] Add a concise Lore Cards section to the System Rules GM instructions (discovery triggers, expert regions, Fey Lore, Fey Knowledge sources, Bargain/Grace consequences, flag coupling, journal maintenance, no automatic link-based discovery).
- [x] Player-facing Lore Card layout: identity + current appropriate image above three fixed tabs (Common Knowledge / Deep Lore / Fey Knowledge); locked tabs show static non-secret hints; flags are used only in `visible` conditions — never rendered as toggles or values; `unlockGuidance`, `gmNotes`, and `feyLoreUnlocksDeepLore` are never bound.

## Acceptance criteria (revised after peer review)

- [x] New schemas validate against all created files.
- [x] Every layout binding resolves to a real schema field; the Lore Card layout uses only supported components (no `Checkbox`).
- [x] The Lore Card player-facing layout never binds `gmNotes`, `unlockGuidance`, `feyLoreUnlocksDeepLore`, or `subjects`; the three knowledge flags appear only in `visible` conditions.
- [x] Expert Regions appear only inside the locked Deep Lore panel and only after the card is discovered.
- [x] The Lore Journal layout has no binding to `/cards`; the only player-facing card collection is `discoveredCards`.
- [x] All three master references are fully hydrated (slug, referenceId, fileTypeSlug "lore-card").
- [x] The `discoveredCards` computed field's schema matches every possible returned row; unknown knowledge levels yield `""` (not `null`); the nested `card` property is declared as a Lore Card reference.
- [x] Undiscovered cards do not appear in the journal; the non-empty branch was tested (Brownie discovered → appears exactly once; restored → empty again).
- [x] No hidden lore, GM notes, unlock fields, or discovery controls appear in player-facing layouts.
- [x] Mystery/revealed images use image fields with URL-based conditional visibility; the neutral header appears when the appropriate URL is absent.
- [x] No affected content file contains a base `image` field or generation metadata; all three sample cards end with every discovery flag false.

---

## Done — implementation notes (filled by the workspace assistant)

**Original implementation** (first pass) created both file types, the campaign journal, the three sample cards, the GM rules section, and this Change Request, as described in the Changes list above.

**Peer-review corrections applied (second pass)**

The peer review detected and corrected the following:

- **Exposed GM/authoring fields in the Lore Card layout.** The stored player-facing layout had been auto-populated with sections for `gmNotes` (GM / Canon Notes), `unlockGuidance` (Unlock Guidance), a `Checkbox` bound to `feyLoreUnlocksDeepLore`, a `Subjects` FileReferenceGrid bound to `/subjects`, and an always-visible Expert Regions section. The layout was rewritten so none of those fields are bound and only supported components remain. The schema fields are unchanged — they stay as GM/authoring data.
- **Unsupported `Checkbox` component.** The `feyloreunlocksdeeplore-checkbox` element used a component not in the supported catalog; removed entirely.
- **Exposed master card list in the Lore Journal layout.** A second FileReferenceGrid (`cards-references`) bound directly to the full `/cards` list. Removed; the only player-facing card collection is the computed `discoveredCards` grid, and the layout has no binding to `/cards`.
- **Incomplete Lore Card references.** The journal's `cards` entries were stored as incomplete `{slug}` drafts rather than fully hydrated references. The journal was rewritten after the target files existed; raw stored-form grep now shows all three entries fully hydrated (`slug` + `referenceId` + `fileTypeSlug: "lore-card"`).
- **Nullable values conflicting with the computed row schema.** `deepKnown`/`feyKnown` returned `null` while declared as strings, and `name` could be null. The expression now returns `""` for unknown levels and `load(card)?.name ?? ""`; the nested `card` property is declared as a Lore Card reference (`referencedFileTypeSlug: "lore-card"`) in the computed items schema.
- **Automatically generated cover images despite `autoImageGeneration: false`.** Generated base `image` fields (with `generation` metadata) existed on all three sample cards, the journal, and this Change Request. All were removed; no replacements were generated. Empty optional `mysteryImage`/`revealedImage` fields are now omitted from content rather than stored as empty objects (the platform accepts omission; `null` remains invalid for image fields). The layout's hero conditions now test the actual image URL (`/mysteryImage/url`, `/revealedImage/url`) and fall back to the neutral header when the URL is absent.
- **Expert Regions placement.** Moved from an always-visible section into the locked Deep Lore panel, gated on `discovered` — they cannot appear on an undiscovered card, and are presented as guidance on where an expert may be found (via a computed `expertRegionsText` display string).

**Platform behavior discovered during the correction pass:** a schema change triggers a schema→layout sync that can re-add auto-generated per-field sections and overwrite a just-written custom layout when the two happen close together. The layouts were therefore re-written after the schema was final and verified by read-back to persist.

**Non-empty branch test (required by peer review):**
- Temporarily set Brownie's `discovered` flag to true.
- Journal read-back: `discoveredCards` contained exactly one row — Brownie — with a fully hydrated Lore Card reference (`slug` + `referenceId` + `fileTypeSlug`), `name: "Brownie"`, and `deepKnown: ""` / `feyKnown: ""` (schema-compatible). Hob and Boggart did not appear.
- Restored Brownie's `discovered` flag to false.
- Journal read-back: `discoveredCards` is `[]` again. All three sample cards end undiscovered.

**Platform limitations observed**

- State is project-global; no per-campaign isolation (acceptable for this single-campaign project).
- Discovery is never automatic — every unlock is a GM edit.
- `designation: "lore"` does not persist via assistant tooling (reports null).
- Image fields reject `null`; empty optional image fields are omitted from content.
- Schema→layout sync can re-add generated sections after schema edits; re-write the layout after schema changes and verify by read-back.

**Validation results (final)**

- All writes passed platform write-time schema/layout validation.
- Layout read-back confirms: the Lore Card layout binds only name/description, the three lore texts, and the computed expert-regions text, using the flags solely in `visible` conditions; the journal layout binds only `/discoveredCards` (no `/cards`).
- Raw stored-content grep confirms: no `media.craftrpgs` URLs, no `generation` metadata, and no `referenceId`-less drafts remain in the affected content files; the journal stores fully hydrated references.
- The non-empty branch test above succeeded and was restored.

Assumptions (unchanged): `feyLoreUnlocksDeepLore` = true on the three samples; `expertRegions` = ["Ireland"] placeholder; file names/slugs (brownie, hob, boggart); no Archetype files created; future Mission type should add a Lore Card reference field for granted cards.

## Review (filled by ChatGPT / the human after the pull)

<left for reviewer>
