---
name: Starting Mission never sets currentBeatKey
description: Starting Mission goes active without a beat key, hiding the journal
  beat objective.
image:
  url: https://media.craftrpgs.com/containers/019fde04-44fd-75c8-8f5e-d680c880596d/images/3eda43cd-1a88-4be6-8dee-6574b56c022d
  generation:
    status: success
    startedAt: 2026-09-09T00:06:13.845Z
    finishedAt: 2026-09-09T00:06:23.030Z
    prompt: A Franco-Belgian Bande Dessinée illustration in the Ligne Claire style.
      A wide shot of a whimsical, surreal landscape where a confused adventurer
      stands before a giant, floating mechanical journal. The journal is open,
      but one of its pages is a blank, white void, representing a missing
      objective. In the background, a mischievous goat is leaping away with a
      golden key in its mouth, leading the adventurer on a "goat-chase." Simple
      shapes, clean black outlines, cell-shading with a limited palette of
      primary colors and flat tones. Minimal texture, bright lighting, and a
      clear, balanced composition. No text, no labels, no watermarks.
$craft:
  referenceId: 01a0837c-8131-7b34-bfdc-6ca4e4accca1
---

# Brief: Starting Mission never sets currentBeatKey

**Status:** Done
**Opened:** 2026-09-08
**Reviewer:** Ingame-oracle

## Problem

Mission Journal `text-3` (`{$item: currentBeatObjective}`) is hidden on fresh starts. `card-3.visible = {$item: currentBeatObjective}` collapses it because `currentBeatObjective` is `""`.

## Cause

`System Rules > Starting the campaign` and the pinned Game Start Instructions start Down the Rabbit-Hole narratively ("begin with goat-chase") but never write `currentBeatKey`. `Mission.currentBeatObjective` needs `status == active` + `storyBeats.find(key == currentBeatKey)`. No key = `""` = hidden.

## Fix

1. `System Rules > Starting the campaign`: apply the full `Selecting a Mission` procedure to the starting Mission.
2. `Game Start Instructions`: explicitly set `status`, structured `startedDate`, and `currentBeatKey = "goat-chase"`.

## Acceptance

Fresh playthrough starting Down the Rabbit-Hole has `status=active`, structured `startedDate`, `currentBeatKey=goat-chase`, non-empty `currentBeatObjective`, visible `text-3`.

---

## Done — implementation notes

- Updated `/GM Instructions/System Rules.gm-instructions.md` > Starting the campaign: starting Mission now applies the full Selecting a Mission procedure.
- Updated Game Start `Start at Granny's Hut` openingInstructions via `game-start set`: set status active, structured startedDate copy, currentBeatKey "goat-chase", begin with goat-chase.
- This project fix does not backfill the live run — that still needs currentBeatKey set on Down the Rabbit-Hole in play.

## Review

<Awaiting reviewer.>
