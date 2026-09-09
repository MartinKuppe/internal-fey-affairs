---
name: Checks and Conditions
description: Check resolution, criticals, Depletion, recovery and Outfit
  changes. Consult when these mechanics apply.
$craft:
  referenceId: 01a0879b-2fa5-731b-b003-1e7014ea6cb4
---

# Checks and Conditions

Consult before requesting or resolving a check, changing Conditions, recovery, or Outfit changes.

## Checks

Roll only when the outcome is uncertain and success and trouble would both be interesting. Resolve a meaningful obstacle, not each tiny step.

1. Choose an Attribute for **how** the PC acts, an applicable trained Ability, and exactly one Condition for **what is at risk**. Attributes: Body strength, Hands agility, Head intelligence, Heart intuition, Legs speed, Shadow deception, Eyes perception.
2. Announce the formula, target, Condition and likely consequence. If there are no meaningful stakes, narrate without rolling.
3. Roll **2d6 + Attribute + 2 if trained + Condition Current**. Training is binary; untrained adds 0. Use Current, not Maximum.
4. Targets: **8 Easy, 10 Standard, 12 Hard, 14 Exceptional**. Choose for the fiction, not a preferred result.
5. Resolve before asking follow-ups:
   - Meet target: clean success, no threatened Depletion.
   - Miss by 1–2: achieve the immediate aim with a complication, generally 1 Depletion to the named Condition.
   - Miss by 3+: setback while fiction advances, usually 1 Depletion; 2 only if that risk was announced.
   - Ordinary checks cause at most 2 Depletion. Story, Side and Interlude Missions continue through setbacks.

Apply **One Obstacle, One Roll**: do not repeat a resolved action. A distinct obstacle or a different approach to an unresolved setback may justify a new check.

## Announce the result

Start the resolving message with a plain-text verdict and one scene-specific sentence:

✅ Clean Success: You slip past the guard while his eyes stay on Meabh.

Exact labels: ✅ Clean Success; ⚠️ Success with a Complication; ❌ Setback; 🌟 Critical Success; 💥 Critical Mishap. No heading markup, code or bold. Critical labels replace the ordinary verdict.

## Criticals

Check the natural dice before modifiers. Other doubles have no special effect.

- **6+6:** the best realistically achievable success, regardless of total, plus one fitting benefit. Examples: a clue, time saved, better position, progress, avoided risk, justified Grace or a remembered deed. Learn an Ability only if the scene teaches it without bypassing an Ability Mission. Impossible goals remain impossible; grant a useful plausible advantage instead. Respect lore, scale, NPC core motives, contract wording, reveal locks and mission gates.
- **1+1:** normal fail-forward outcome plus one plausible complication. Prefer funny, embarrassing, inconvenient or revealing consequences before grim ones. At most **2 total Depletion**, including ordinary Depletion, across any fitting Conditions. Health requires bodily danger already present or implied. Record concrete consequences in the appropriate state; never block campaign progress.

## Condition state

Health, Energy and Nerve persist; Decorum and Cover describe scene position. Each ranges 0–3. Decorum means social/aesthetic appropriateness, not just cleanliness; Cover means remaining unnoticed or unsuspected.

Store only `conditionState` Depletion, clamped 0–3; absent Depletion is 0. The sheet derives:
- Effective Maximum = clamp(base + worn Outfit modifiers, 0, 3).
- Current = clamp(Effective Maximum − Depletion, 0, 3).
- Base Health is 3; Energy, Nerve, Decorum and Cover are 2.

Never write Current or Maximum. Abilities do not raise Maximums. Apply pressure only to relevant Conditions; certain fictional events may cause Depletion without a roll.

## Outfits

`outfit` is the worn Outfit; `unlockedOutfits` lists what the PC may equip. Other Outfits may be unlocked by authored Mission outcomes, Abilities or Bargains; invent neither unlocks nor prices.

All worn modifiers apply mechanically. `appliesBecause` explains why, not when. Changing Outfit preserves Depletion exactly and recalculates Current/Maximum; narrate a newly caused zero crossing.

## Reaching zero

A relevant track crossing from above 0 to 0 triggers a consequence after a new setback or modifier—not on every subsequent turn, read or recalculation.

- Health: unconscious/incapacitated, not death from the track.
- Energy: sleep, exhaustion or a forced stop.
- Nerve: panic, freezing, flight, lost concentration or damaging speech.
- Decorum: humiliation/social failure remembered by witnesses.
- Cover: detected, recognised or exposed.

Raising a number afterwards does not erase the event. Continue through rescue, pursuit, bluffing, capture, lost time or another changed situation.

## Recovery and resets

Only recover when the fiction supports it:
- Health: remove 1 Depletion after treatment or meaningful safe rest; serious injury does not vanish just because the number improves.
- Energy: remove 1 after food, warmth, sitting down, short rest, encouragement, good news or another real lift. Clear after a safe night's sleep.
- Nerve: remove 1 after reassurance, companionship, safety, success or time to regain courage. Clear after a calm night or reassuring scene unless immediate dread continues.
- Decorum: normally clear for a new social situation, not while the same embarrassment, audience or gossip remains.
- Cover: normally clear after establishing a new disguise, hiding place, route or infiltration situation, not while the same observers keep searching/watching.

Existing consequences survive recovery and resets.

