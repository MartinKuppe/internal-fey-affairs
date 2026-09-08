---
name: Split Cream and Milk Missions
description: Splits the combined Milk and Cream tutorial into Cream Tax and Milk
  and Feathers and connects both to the newly authored village locations.
$craft:
  referenceId: 01a0825e-f38d-73fa-9a31-41ce04524662
---

# Brief: Split Cream and Milk Missions

**Status:** Done  
**Opened:** 2026-09-08  
**Reviewer:** ChatGPT (Codex)

## Goal

Replace the combined Milk and Cream Side Mission with two independently selectable tutorials while preserving the existing Mission identity for Cream Tax and adding Milk and Feathers as a new Mission.

## Changes

- [x] Rename the existing SI-01 Mission to **Cream Tax**, retaining its Craft identity.
- [x] Limit Cream Tax to the ten-measure cream obligation, with beats at the Hollow Tree, Tri Mhuilinn Gate, Inside Palisade, and the Hollow Tree delivery.
- [x] Create **Milk and Feathers** as SI-02, covering two cans of milk from the farms north of the palisade.
- [x] Move **Iron and Wax** to SI-03.
- [x] Fix the farm identities: Liam's Farm is fenced and hides the rooster; Finn's Farm is visibly guarded by geese.
- [x] Preserve the choice between one can from each farm and risking a second can from Liam's fenced yard; permit other credible plans and play only the farm beats actually visited.
- [x] Give each Mission only its own tracker and make it resolve only its own obligation under Coinín's Silence.
- [x] Make Cream Tax, Milk and Feathers, and Iron and Wax independently available after Down the Rabbit-Hole.
- [x] Add the new Mission to the Campaign Mission Journal.
- [x] Enrich the four new Location records with player-facing descriptions and GM-facing Mission geography.
- [x] Place Cream Tax's runtime selector coordinates near Tri Mhuilinn Gate and Milk and Feathers' near the two northern farms. Shared author-time map pins remain unchanged.

## Identity and sync note

The existing local file remains temporarily named `Milk and Cream.mission.json` while its content name is `Cream Tax`. This is intentional: changing only the content name preserves the existing Craft file identity during push. The server will rename the canonical file, and the next `craft pull` will synchronize the local filename.

## Acceptance

- [x] Cream Tax never asks for or tracks milk.
- [x] Milk and Feathers never requires entering the palisade or collecting cream.
- [x] Every authored beat points at the appropriate concrete Location record.
- [x] Liam's fence/rooster and Finn's geese cannot be swapped by the GM.
- [x] The Bargain may show cream, milk, and honey completing independently.
