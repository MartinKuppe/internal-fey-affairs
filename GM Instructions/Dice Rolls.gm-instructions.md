---
name: Dice Rolls
description: Consult before requesting a dice roll and when resolving its result.
image:
  generation:
    status: error
    startedAt: 2026-10-25T00:26:15.208Z
    finishedAt: 2026-10-25T00:26:15.208Z
    error: "Not enough energy to generate this batch of 2 images. Required: 2
      energy, available: 0 energy."
$craft:
  referenceId: 01a17072-f40f-7e58-8ac2-48d35150b967
  settings:
    contextVisibility: partial
  metadata:
    schemaVersionId: file-designation-metadata:v8
---

# Dice Rolls

Consult before requesting or resolving a dice check.

## Dice Rolls

Roll only when the outcome is uncertain and success and trouble would both be interesting. Resolve a meaningful obstacle, not each tiny step.

1. Choose an Attribute for **how** the PC acts, an applicable trained Ability, one **supporting Condition** that helps the action, and one **Condition at risk** from its consequences. The two Conditions may be the same; choose for the fiction, not the highest value. Attributes: Body strength, Hands agility, Head intelligence, Heart intuition, Legs speed, Shadow deception, Eyes perception.
2. Put the full roll prompt shown below in askPlayerToRoll.reason, not in separate narration or dialogue; adapt the outcome thresholds to the target. Keep modifiers separately labelled in the dice expression, e.g. 2d6+1[Shadow]+2[Cover], rather than combining them into +3. Reveal fictional consequences only after the roll. Announce any risk of 2 Depletion beforehand. If there are no meaningful stakes, narrate without rolling.
3. Roll **2d6 + Attribute + 2 if trained + supporting Condition Current**. Training is binary; untrained adds 0. Use Current, not Maximum. Read the supporting Condition's bonus from `conditionTracks[].current`, never from `conditionState` (which holds Depletion only).
4. Targets: **8 Easy, 10 Standard, 12 Hard, 14 Exceptional**. Choose for the fiction, not a preferred result.
5. Resolve before asking follow-ups:
   - Meet target: clean success, no threatened Depletion.
   - Miss by 1–2: achieve the immediate aim with a complication, generally 1 Depletion to the Condition at risk.
   - Miss by 3+: setback while fiction advances, usually 1 Depletion to the Condition at risk; 2 only if that risk was announced.
   - Ordinary checks cause at most 2 Depletion. Story, Side and Interlude Missions continue through setbacks.

Example roll prompt (Standard 10; criticals override totals):

> Hide from the Hunter in the hollow tree.  
> At risk: Cover. Roll: 2d6 + 1 Shadow + 2 Cover.  
> Total 10+: Success.  
> Total 8–9: Success with a complication.  
> Total 7 or less: Setback.  
> Double six: Critical success. Double one: Critical mishap.

Different Conditions example: throwing a stone to extinguish a lantern uses Hands with Nerve for steadiness; Cover is at risk if the attempt attracts attention. Deplete Cover, not Nerve, for that consequence.

Apply **One Obstacle, One Roll**: do not repeat a resolved action. A distinct obstacle or a different approach to an unresolved setback may justify a new check.

## Announce the result

Start the resolving message with a plain-text verdict and one scene-specific sentence:

✅ Clean Success: You slip past the guard while his eyes stay on Meabh.

Exact labels: ✅ Clean Success; ⚠️ Success with a Complication; ❌ Setback; 🌟 Critical Success; 💥 Critical Mishap. No heading markup, code or bold. Critical labels replace the ordinary verdict.

## Criticals

Check the natural dice before modifiers. Other doubles have no special effect.

- **6+6:** the best realistically achievable success, regardless of total, plus one fitting benefit. Examples: a clue, time saved, better position, progress, avoided risk, justified Grace or a remembered deed. Learn an Ability only if the scene teaches it without bypassing an Ability Mission. Impossible goals remain impossible; grant a useful plausible advantage instead. Respect lore, scale, NPC core motives, contract wording, reveal locks and mission gates.
- **1+1:** normal fail-forward outcome plus one plausible complication. Prefer funny, embarrassing, inconvenient or revealing consequences before grim ones. At most **2 total Depletion**, including ordinary Depletion, across any fitting Conditions. Health requires bodily danger already present or implied. Record concrete consequences in the appropriate state; never block campaign progress.

When a result changes a Condition, consult **Conditions**.
