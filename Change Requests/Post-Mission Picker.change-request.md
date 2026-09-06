---
name: Post-Mission Picker
description: Makes an explicit multiple-choice prompt the standard way to select
  the next Mission after every Mission ends.
$craft:
  referenceId: 01a07786-448e-71f8-8ae4-c82ba3a9120e
---

# Brief: Post-Mission Picker

**Status:** Done  
**Opened:** 2026-09-06  
**Reviewer:** ChatGPT (Codex), from the Down the Rabbit-Hole playtest

## Goal

Whenever a Mission ends, present the currently eligible next Missions through the platform's multiple-choice control rather than relying on small map pins or a prose question.

## Rules

- Finish the Mission's state changes, calendar movement, unlocks, expirations, and priority checks before building the choices.
- Build choices from the Mission Journal's computed available-Mission view without writing to that computed field.
- Show one option per eligible available Mission, even when there is only one.
- Selecting an option is the player's explicit instruction to start that Mission.
- Never invent a generic rest or day-off option; those become selectable when authored as Missions.
- Due Story and Interlude Missions retain priority over other available Missions.
- Map pins remain optional supplementary cues rather than the primary picker.

## Acceptance

- [x] The GM is told to invoke a multiple-choice control, not merely narrate options.
- [x] A one-option picker is required.
- [x] Hidden and unavailable Missions cannot leak into the choices.
- [x] Individual Missions contain no duplicated picker instruction; the general lifecycle rule is solely responsible.
