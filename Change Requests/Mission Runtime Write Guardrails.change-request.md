---
name: Mission Runtime Write Guardrails
description: Prevents the GM from writing formatted dates or computed Mission
  Journal projections during play.
$craft:
  referenceId: 01a0777d-bd97-7031-8933-85c654ce3a41
---

# Brief: Mission Runtime Write Guardrails

**Status:** Done  
**Opened:** 2026-09-06  
**Reviewer:** ChatGPT (Codex), from first-mission playtest errors

## Problem

During Down the Rabbit-Hole, the GM attempted to store `August 🌒` in the structured `startedDate` field and attempted to write the Mission Journal's computed `activeMissions` projection. Both writes correctly failed validation.

## Resolution

- Runtime Mission dates are written as objects containing `month` and `moonPhase` copied from the Journal.
- Formatted date labels are display-only and are never stored in date fields.
- The Journal's date label and Mission projections are explicitly read-only.
- Starting, advancing, or ending a Mission changes the Mission's stored fields; the Journal projections update automatically.

## Acceptance

- [x] The lifecycle rules contain an exact valid `startedDate` example.
- [x] The GM is explicitly forbidden from writing computed Journal projections.
- [x] The Mission schema descriptions reinforce the same date shape.
- [x] No schema is weakened and no computed projection is converted into stored state.
