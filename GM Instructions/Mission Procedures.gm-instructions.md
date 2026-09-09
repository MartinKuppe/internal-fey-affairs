---
name: Mission Procedures
description: Searchable runtime procedure for campaign startup, mission teams,
  beats, outcomes, dates, unlocks, trackers, and journal refresh.
$craft:
  referenceId: 01a0879b-2fa5-731b-b003-26f7be756f41
---

# Mission Procedures

Consult when starting or advancing a Mission, changing its state, or processing an ending. Between Missions governs the next choice or immediate follow-up; Checks and Conditions governs checks and recovery.

## Index, kinds, and statuses

Campaign Mission Journal (`/Mission Journals/Campaign Mission Journal.mission-journal.json`) owns the master `missions` list, single `startingMission`, and campaign date. Each Mission stores its own status.

- **Story:** the main plot, played in order.
- **Side:** flexible order between Story Missions; required completion is determined by `unlockAfter`, not kind alone.
- **Interlude:** mandatory event or holiday, otherwise using ordinary Mission mechanics.
- **Opportunity:** optional, with a limited availability window.
- **Ability:** may award trained Abilities.

Normal progression is `upcoming` → `available` → `active` → `completed`. Story, Side, and Interlude never become `failed`: setbacks change cost, time, Conditions, position, relationships, or approach until completion. An attempted Opportunity or Ability Mission may become `failed` only as authored. An unundertaken Opportunity becomes `expired` when its window closes. Failure or expiration must not block the Story campaign. Retain terminal Mission files as history.

## Stored state and dates

- For `startedDate` and `endedDate`, copy the Journal date as `{ "month": currentMonth, "moonPhase": currentMoonPhase }`, e.g. `{ "month": "August", "moonPhase": "🌒" }`. A label such as `August 🌒` is not a date object.
- Journal `currentDateLabel`, `availableMissions`, `activeMissions`, `knownUpcomingMissions`, and `missionHistory` are computed and read-only. Change their source Mission fields instead.
- Writable Journal fields are `missions`, `startingMission`, `currentMonth`, and `currentMoonPhase`. Do not rewrite the master list to advance a Mission already listed.

## Starting the campaign

Game Start launches the Journal's `startingMission` directly, without a picker. Currently this is Down the Rabbit-Hole: apply the activation steps below and begin at Grannie's Hut before moving to the Fairy Ring. Follow the live Game Start opening and the Mission's authored opening beats.

## Activating a Mission

Ordinary selection comes from the explicit multiple-choice prompt described in Between Missions. Opening a Mission file does not start it. On selection, or an authorised direct start:

1. Validate and record `assignedTeam`: include all `mandatoryMembers`, match `teamSize` when defined, choose remaining members from `selectableMembers`, and avoid duplicate character references.
2. Set `status: active`, `startedDate` to the structured current date, and `currentBeatKey` to the first beat's key.
3. Apply `startLoreUnlocks`.
4. Begin at `startLocation` or the first beat's Location.
5. Refresh the Journal display as described below.

## Playing beats

Follow `storyBeats` in authored order unless the Mission says otherwise. Update `currentBeatKey` as play advances and apply beat `loreUnlocks` when reached or resolved as authored. Refresh the Journal after beat or tracker changes. Individual plans may fail; Story, Side, and Interlude Missions continue through those consequences.

## Terminal outcomes

When a Mission ends:

1. Set `completed`, `failed`, or `expired` as permitted above; record structured `endedDate` and `actualOutcome`.
2. Apply `completionLoreUnlocks` on completion or `failureLoreUnlocks` on failure. On completion, append `awardedAbilities` to the relevant Player Character's `abilities` without duplicates.
3. Apply or narrate `completionEffects` / `failureEffects`. Mission completion does not itself resolve a Bargain: record partial deliveries and leave outstanding obligations or ongoing promises active, following Grace, Language and Bargains and the authored contract.
4. Advance the Journal calendar by `durationSteps`; evaluate fixed dates, prerequisites, and Opportunity expirations.
5. Clear or omit `currentBeatKey` and refresh the Journal display.
6. Consult Between Missions: start a completed Mission's `immediateFollowUp`, otherwise read the Journal's computed `availableMissions` and present the next choice.

Keep this processing focused. Avoid repeated reads of the same Mission, Bargain, character, or map. If the action budget interrupts it, inspect saved state once next turn and resume only unfinished steps; do not restart completion or reapply effects.

## Calendar, availability, and expiration

The Journal date is `currentMonth` + `currentMoonPhase`. Each `durationSteps` advances one phase: 🌑 → 🌒 → 🌓 → 🌔 → 🌕 → 🌖 → 🌗 → 🌘 → next month 🌑. December wraps to January; no year is tracked. Zero steps leave the date unchanged.

An upcoming Mission becomes `available` when all `unlockAfter` Missions are `completed` and its fixed date has arrived, if any. An empty prerequisite list imposes no prerequisite. The GM evaluates these conditions and writes status explicitly.

A fixed-date Mission is due when the current date reaches or passes that date. A newly available fixed-date Story or Interlude blocks further clock advancement until completed. Do not interrupt an already active Mission retroactively; apply priority after it ends. If a multi-step duration crosses the mandatory date, make that Mission due at the resulting date. Resolve due mandatory Missions before further time-consuming choices.

If any Mission in an upcoming or available Opportunity's `expiresWhenAvailable` becomes `available`, set that Opportunity to `expired`. Expiration is for an unundertaken Mission, not an attempted one.

## Applying Lore unlock rows

Consult **Lore and Knowledge** for the flags to apply to start, beat, completion and failure unlock rows.

## Mission Trackers

`missionTrackers` holds Mission-local objective labels and counters, not inventory. It may be absent or empty. Trackers have no equipment, ownership, weight, price, or location mechanics and do not transfer between Missions.

- Update `current` immediately when the tracked thing is acquired, lost, placed, spent, recovered, or secured.
- Keep whole numbers: `target >= 1`, `current >= 0`, normally clamped to target unless authored surplus tracking is required.
- `target: 1` represents one unique object; larger targets represent counted progress.
- At zero, `showAtZero: false` hides the tracker; `true` shows `0 / target`.
- Display trackers only while the Mission is `active`. Retain stored values as history afterward.
- Reaching target does not complete the Mission without its authored completion condition.

## Mission presentation

Present Missions through the Mission Journal and narrative picker, not map pins. Do not create or maintain Mission markers; ignore legacy `selectorMap`, `mapX`, `mapY` and `pinLabel` fields. Location pins and character tokens remain in use.

## Journal display and context

After a beat, status, tracker, or date change, refresh the stale sidebar with `scene unpin` followed by `scene pin campaign-mission-journal.mission-journal`. Refresh once after the related updates, not once per field.

This refresh concerns the **scene's displayed file**, not context visibility. Keep Campaign Mission Journal and System Rules pinned in GM context permanently; do not replace the Journal with individual Missions. Mission files remain partial or searchable. Context visibility is app-managed and must be verified in the app.
