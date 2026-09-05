---
name: Automatic Outfit Modifiers
description: Makes every worn Outfit modifier apply mechanically and replaces
  contextual GM adjudication with a short player-facing explanation.
$craft:
  referenceId: 01a073b9-15c7-752b-834f-3af31bda7931
---

# Brief: Automatic Outfit Modifiers

**Status:** Done  
**Opened:** 2026-09-06  
**Reviewer:** ChatGPT (Codex)

## Goal

Make Outfit effects immediate, predictable, and visible on the Player Character sheet. Wearing an Outfit always applies all of its Condition modifiers, even when the abstraction is imperfect.

## Decisions

- Health has base 3; Energy, Nerve, Decorum, and Cover have base 2.
- Effective Maximum is `clamp(base + worn Outfit modifiers, 0, 3)`.
- Current is `clamp(Effective Maximum - Depletion, 0, 3)`.
- Current and Effective Maximum are computed. New Condition state needs only Depletion; legacy stored Maximum values are ignored.
- Outfit modifier rows use `appliesBecause`, a short player-facing explanation. It never gates the mechanical modifier.
- Changing Outfit preserves Depletion and recalculates the sheet immediately.

## Files changed

- `/.craft/file-types/outfit.json`
- `/.craft/file-types/player-character.json`
- `/Outfits/*.outfit.json`
- `/GM Instructions/System Rules.gm-instructions.md`

## Acceptance

- [x] `appliesWhen` is replaced by required `appliesBecause` data and layout bindings.
- [x] Every existing Outfit explanation is rewritten as a reason rather than a circumstance.
- [x] The Player Character sheet derives Condition values from its Outfit reference.
- [x] Stored per-track Maximum fields are removed from the schema and ignored by the computation; Depletion remains durable.
- [x] GM instructions no longer ask the GM to judge or manually apply Outfit modifiers.

## Sync note

The Blacksmith's existing legacy `maximum` values remain in its content but are mechanically inert. Its local sync ledger reports a version newer than the server's current version while `craft pull` reports no remote change, so excluding that unchanged record avoids a false `version_conflict` without editing machine-managed sync state.
