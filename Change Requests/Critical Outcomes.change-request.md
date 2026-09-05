---
name: Critical Outcomes
description: Adds rare natural-dice Critical Successes and Critical Mishaps
  while preserving realistic limits and fail-forward play.
$craft:
  referenceId: 01a073e7-c317-73f6-8308-fe11f05c6c1e
---

# Brief: Critical Outcomes

**Status:** Done  
**Opened:** 2026-09-06  
**Reviewer:** ChatGPT (Codex), from Hitchcock's mechanics handoff

## Goal

Make natural double sixes and double ones rare and memorable without turning the game into a combat engine, slapstick engine, or means of bypassing story logic.

## Rules

- Criticals inspect the natural dice before modifiers; other doubles have no special effect.
- Natural 6 + 6 grants the best realistically achievable success and one extra fitting benefit. It cannot make the impossible possible or override canon, physical scale, reveal locks, Bargains, NPC motives, or Mission gates.
- Natural 1 + 1 adds one plausible complication to the ordinary fail-forward result. The Mission continues.
- A Critical Mishap may cause at most 2 total Depletion across any fitting Conditions, including ordinary outcome Depletion.
- Health Depletion requires bodily danger, injury, illness, or harsh strain already present or clearly implied.
- The GM explicitly announces “Critical Success” or “Critical Mishap” before resolving it.

## Files changed

- `/GM Instructions/System Rules.gm-instructions.md`
- `/.craft/file-types/player-character.json` — compact Dice Rule card only

## Acceptance

- [x] The global rule covers both natural-dice triggers and their order relative to modifiers.
- [x] Impossible actions receive a plausible advantage rather than impossible success.
- [x] Critical Mishaps remain proportional, fail-forward, and cautious with Health.
- [x] The Player Character sheet includes a compact reminder.
- [x] Individual Missions contain no duplicated critical rules.
