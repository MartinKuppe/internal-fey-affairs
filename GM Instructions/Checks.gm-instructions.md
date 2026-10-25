---
name: Checks
description: Roll formula, current-value lookup, targets, outcome bands,
  criticals, prompts, and result announcements.
image:
  generation:
    status: error
    startedAt: 2026-10-25T00:26:15.208Z
    finishedAt: 2026-10-25T00:26:15.208Z
    error: "Not enough energy to generate this batch of 2 images. Required: 2
      energy, available: 0 energy."
$craft:
  referenceId: 01a17072-f40f-7e58-8ac2-48d35150b967
---

# Checks

Consult before requesting or resolving a check.

## Checks

Roll only when the outcome is uncertain and success and trouble would both be interesting. Resolve a meaningful obstacle, not each tiny step.

1. Choose an Attribute for **how** the PC acts, an applicable trained Ability, and exactly one Condition for **what is at risk**. Attributes: Body strength, Hands agility, Head intelligence, Heart intuition, Legs speed, Shadow deception, Eyes perception.
2. Show the formula, at-risk Condition and outcome bands by final total, as below; adapt the thresholds to the target. Reveal fictional consequences only after the roll. Announce any risk of 2 Depletion beforehand. If there are no meaningful stakes, narrate without rolling.
3. Roll **2d6 + Attribute + 2 if trained + Condition Current**. Training is binary; untrained adds 0. Use Current, not Maximum. Read the Condition bonus from `conditionTracks[].current`, never from `conditionState` (which holds Depletion only).
4. Targets: **8 Easy, 10 Standard, 12 Hard, 14 Exceptional**. Choose for the fiction, not a preferred result.
5. Resolve before asking follow-ups:
   - Meet target: clean success, no threatened Depletion.
   - Miss by 1–2: achieve the immediate aim with a complication, generally 1 Depletion to the named Condition.
   - Miss by 3+: setback while fiction advances, usually 1 Depletion; 2 only if that risk was announced.
   - Ordinary checks cause at most 2 Depletion. Story, Side and Interlude Missions continue through setbacks.

Example roll prompt (Standard 10; criticals override totals):

> Hide from the Hunter in the hollow tree.  
> At risk: Cover. Roll: 2d6 + 1 Shadow + 2 Cover.  
> Total 10+: Success.  
> Total 8–9: Success with a complication.  
> Total 7 or less: Setback.  
> Double six: Critical success. Double one: Critical mishap.

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