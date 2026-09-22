---
name: Wardrobe Migration
description: Player Character outfit migration to runtime wardrobe copies with
  Common Clothes baseline
image:
  url: https://media.craftrpgs.com/containers/019fde04-44fd-75c8-8f5e-d680c880596d/images/88bf39cf-c162-4669-87b3-c1524820e27e
  generation:
    status: success
    startedAt: 2026-09-22T23:27:45.872Z
    finishedAt: 2026-09-22T23:27:50.404Z
    prompt: "A confident adventurer stands before an open travel wardrobe, selecting
      a neatly folded outfit while a faint, translucent duplicate of the
      garments hovers beside it like a preserved snapshot. Behind them, orderly
      racks of common clothes and a few spare pieces suggest choices ready to
      unlock; a small, glowing thread links the chosen copy to its original
      ensemble. Compose as a clear, balanced tabletop-RPG cover image, with the
      character and wardrobe as the focal point and subtle visual cues of
      transformation and continuity. Franco-Belgian ligne claire comic
      illustration: simple readable shapes, crisp black outlines, restrained
      cell shading, a limited palette of warm browns, cream, and muted teal,
      minimal texture. Inviting, clever mood; no text, no labels, no
      watermarks."
$craft:
  referenceId: 01a0cb72-32b9-7514-a599-1ad10c53365f
---

# Brief: Wardrobe Migration
**Status:** Done
**Opened:** 2026-09-22

## Goal
Retain `outfit` as authoring fallback; add `wornUnlockedOutfit` (Unlocked Outfit copy); selector edits only the copy; outfit-dependent computed fields prefer copy when present.

## Changes
- Created Unlocked Outfit type (snapshot: image, description, sourceOutfit→outfit, conditionModifiers + computed rows).
- Created /Unlocked Outfits/Common Clothes Wardrobe (source: common-clothes).
- Player-character schema: added wornUnlockedOutfit; outfitImage/outfitDescription/outfitModifiers/conditionTracks prefer worn copy when set.
- Layout: Outfit card selector now binds /wornUnlockedOutfit with fileTypeSlug unlocked-outfit; removed duplicate bottom selector.
- All 21 pregens: wornUnlockedOutfit=common-clothes-wardrobe; outfit unchanged; common-clothes removed from unlockedOutfits (spare kept).
- GM instructions: Character Creation, Checks and Conditions, Mission Procedures updated with runtime wardrobe protocol.
- Creation flow generationInstructions updated.
- Scratch probe, 4 scratch wardrobe files, scratch types removed. Production baseline + Outfit files retained.

## Verification
- API/data (verified): Bandit + Wood Gatherer reads show worn copy resolves, outfitImage/description/modifiers/tracks derived from copy; grep confirms all 21 worn refs resolve with referenceId; /Unlocked Outfits/ holds exactly the baseline copy.
- UI (per prior probe, passed): picker shows copies live; restricted selector; editor display with Common Clothes.
- Requires fresh-game test (not verifiable in authoring project): instance inherits Common Clothes copy; post-selection spare copies created with sourceOutfit dedupe; unset worn initializes to matching copy; later unlocks add missing copies; never clear wardrobe/reset worn on resume; other playthroughs' copies do not accumulate in authoring.
