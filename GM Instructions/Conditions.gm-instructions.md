---
name: Conditions
description: Consult when changing or recovering Conditions, or changing or unlocking Outfits.
image:
  generation:
    status: error
    startedAt: 2026-10-25T00:26:15.208Z
    finishedAt: 2026-10-25T00:26:15.208Z
    error: "Not enough energy to generate this batch of 2 images. Required: 2
      energy, available: 0 energy."
$craft:
  referenceId: 01a17073-0c57-7e0c-aa9c-8de42944edc2
  settings:
    contextVisibility: searchable
  metadata:
    schemaVersionId: file-designation-metadata:v8
---

# Conditions

Consult when changing Conditions, recovery, or Outfit changes. Dice Rolls governs how roll outcomes are determined; when a result changes a Condition, apply the change here.

## Condition state

Within a Mission, Health, Energy and Nerve persist; Decorum and Cover describe scene position. Each ranges 0–3. Decorum means social/aesthetic appropriateness, not just cleanliness; Cover means remaining unnoticed or unsuspected.

Store only `conditionState` Depletion, clamped 0–3; absent Depletion is 0. The sheet derives:
- Effective Maximum = clamp(base + worn Outfit modifiers, 0, 3).
- Current = clamp(Effective Maximum − Depletion, 0, 3).
- Base Health is 3; Energy, Nerve, Decorum and Cover are 2.

Never write Current or Maximum. Abilities do not raise Maximums. Apply pressure only to relevant Conditions; certain fictional events may cause Depletion without a roll.

## Outfits

`outfit` is the worn Outfit. The sheet's filtered picker offers patterns where `alwaysAvailable` is true (Common Clothes), the PC's Origin is in `startingOrigins`, or `isUnlocked` is true in this instance. Outfit rewards set `isUnlocked` to true on the existing Outfit in the running instance — never create copies. Unlocking does not equip; when the fiction says the PC equips an offered outfit, set the PC's `outfit` reference. Invent neither unlocks nor prices.

All worn modifiers apply mechanically. `appliesBecause` explains why, not when. Changing the worn `outfit` preserves Depletion exactly and recalculates Current/Maximum; narrate a newly caused zero crossing.

## Reaching zero

A relevant track crossing from above 0 to 0 triggers a consequence after a new setback or modifier—not on every subsequent turn, read or recalculation.

- Health: unconscious/incapacitated, not death from the track.
- Energy: sleep, exhaustion or a forced stop.
- Nerve: panic, freezing, flight, lost concentration or damaging speech.
- Decorum: humiliation/social failure remembered by witnesses.
- Cover: detected, recognised or exposed.

Raising a number afterwards does not erase the event. Continue through rescue, pursuit, bluffing, capture, lost time or another changed situation.

## Recovery and resets

At the start of an ordinary Mission, set all of the participating PC's Condition Depletion values to 0. Preserve Depletion between Missions in an authored uninterrupted sequence, including `immediateFollowUp` transitions. Reset only on initial activation, not on resume or completion; worn Outfit modifiers still apply.

Outside this mission-start reset, recover only when the fiction supports it:
- Health: remove 1 Depletion after treatment or meaningful safe rest; serious injury does not vanish just because the number improves.
- Energy: remove 1 after food, warmth, sitting down, short rest, encouragement, good news or another real lift. Clear after a safe night's sleep.
- Nerve: remove 1 after reassurance, companionship, safety, success or time to regain courage. Clear after a calm night or reassuring scene unless immediate dread continues.
- Decorum: normally clear for a new social situation, not while the same embarrassment, audience or gossip remains.
- Cover: normally clear after establishing a new disguise, hiding place, route or infiltration situation, not while the same observers keep searching/watching.

Existing consequences survive recovery and resets.
