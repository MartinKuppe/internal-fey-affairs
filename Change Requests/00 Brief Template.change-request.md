---
name: 00 Brief Template
description: Template for change-request briefs exchanged between the human,
  ChatGPT (peer review), and the workspace assistant.
$craft:
  referenceId: 019fed90-bf36-7bd0-91d0-01dca408dd12
---

# Brief: <Short Feature Name>

**Status:** Draft · In Review · Implementing · Done
**Opened:** <date>
**Reviewer:** ChatGPT (Codex)

## Goal

<What are we trying to achieve, in 2–3 sentences? Why does it matter for the game?>

## Files touched

- <e.g. /Characters/Example Character.character.json>
- <e.g. /file-types/character.json — add a field>

## Changes

- [ ] <Exact, actionable change. Name the file, the field, the value. Example: "Add `stats.focus` (number, 1–5, default 2) to the Character schema.">
- [ ] <Another change. The more precise, the fewer implementation rounds.>
- [ ] <If a change is a decision rather than an edit, write the decision and the rationale instead of a checkbox.>

## Acceptance criteria

- [ ] <How do we know it's done? Example: "A new character created with origin X starts with focus 3.">
- [ ] <Testable or observable outcome, not an edit instruction.>

---

## Done — implementation notes (filled by the workspace assistant)

<After implementing: what changed (files and fields), assumptions made, open questions for the reviewer.>

## Review (filled by ChatGPT / the human after the pull)

<Peer-review comments on the diff, plus the next prompt for the workspace assistant.>
