---
name: Mission Trackers
description: Adds lightweight Mission-local tracker counters for item-like
  Mission objectives (facility ledgers, counted goods, recovered barrels,
  secured evidence), and repairs the player-facing Mission layout that exposed
  GM-only and raw authoring fields.
$craft:
  referenceId: 01a03917-2719-751c-bc0d-82a6953368c0
---

# Brief: Mission Trackers

**Status:** Done
**Opened:** (fill date)
**Reviewer:** ChatGPT (Codex)

## Goal

Add a lightweight, Mission-local tracker system for item-like Mission objectives: labels and counters used only to represent progress inside a Mission (e.g. "Facility ledger", "Méisrin of cream: 0 / 10", "Barrels recovered: 0 / 7"). These are not inventory items. Also repair the current Mission layout regression that exposes GM-only and authoring fields on the player-facing Mission page.

## Relevant canon

- Tracker data is Mission-local objective state, not inventory. Trackers belong directly to their Mission and live only for its lifetime.
- Do not create an Item, Mission Object, or Mission Tracker file type. Do not add anything to Character inventory or Outfit state. Do not add tracker state to the Campaign Mission Journal. No cross-Mission persistence in v1.
- A tracker has no image, description, weight, location, owner, equipment behavior, price, or other inventory fields. No separate unit field in v1 — the unit is included in the player-facing label (e.g. "Méisrin of cream").
- Reaching a tracker's target does not automatically complete the Mission unless the authored Mission instructions explicitly say so.
- Trackers are player-visible while a Mission is `active`, but not player-editable; the AI GM updates values through Mission state.
- The player-facing Mission page must not bind or display GM-only or raw authoring/runtime fields (see Changes). An upcoming Mission with `revealedToPlayer: false` shows only the generic locked presentation and exposes no Mission-specific information, including trackers.

## Files touched

- /Change Requests/Mission Trackers.change-request.md — this brief
- /file-types/mission.json — extend Mission schema with an optional `missionTrackers` array and a computed `displayedTrackerCount`; rebuild the player-facing layout at /file-types/mission/layout.json
- /GM Instructions/System Rules.gm-instructions.md — add a concise **Mission Trackers** subsection under Missions
- Temporary validation Missions in /Missions/ (deleted before completion)

## Changes

- [x] **Mission schema — `missionTrackers` (optional array, no backfill needed since no permanent Mission files exist).** Each row holds exactly: `key` (required string, minLength 1, stable authoring identifier unique within the Mission), `label` (required string, minLength 1, player-facing text incl. unit), `current` (required number, minimum 0 — the AI GM keeps it a whole number), `target` (required number, minimum 1 — the AI GM keeps it a whole number), `showAtZero` (required boolean). No `stackable`, `acquired`, `completed`, `visible`, `active`, `unit`, `image`, `description`, related-Mission references, or persistent-across-Missions state.
- [x] **Computed `displayedTrackerCount`** — count of trackers currently shown while active (`showAtZero` true, or `current > 0`). Used to collapse the section when nothing would render. Never stored.
- [x] **Semantics:** `target: 1` = one unique acquired object; `target > 1` = counted objective; `current: 0` + `showAtZero: false` stays hidden; `current: 0` + `showAtZero: true` shows 0 / target; a unique tracker at `current: 1` displays its label as an acquired object, not "1 / 1". Counts stay 0–target unless a Mission explicitly requires surplus tracking (normally clamped at target). Tracker `key` is an authoring identifier, not player-facing text.
- [x] **Mission layout rebuild (player-facing only).** Removed every GM-only / raw authoring binding: `gmNotes`, `storyBeats`, beat `events`/`gmInstructions`/`loreUnlocks`, `startLoreUnlocks`, `completionLoreUnlocks`, `failureLoreUnlocks`, `unlockAfter`, `expiresWhenAvailable`, `completionEffects`, `failureEffects`, `selectorMap`, `mapX`, `mapY`, `pinLabel`, `currentBeatKey`, and the `revealedToPlayer` editing control. Added a **Mission Trackers** section, visible only while `status` is `active` and `displayedTrackerCount` is greater than 0, repeating over `missionTrackers` (not numbered fields). Counted trackers render read-only `Progress` (the standard objective/progress component) with `showValue`; target-1 unique objects render as a `Badge` label (no "1 / 1"); both honor `showAtZero`. Tracker values are not player-editable.
- [x] **Unrevealed-Mission protection preserved.** Hero/body gated on `revealedToPlayer` or status not `upcoming`; the generic locked card binds nothing. The locked card shows no name/image/description/code/kind/objective/briefing/date/location/team/trackers.
- [x] **System Rules — Mission Trackers subsection.** Mission-local objective state, not inventory; list may be omitted; GM updates `current` immediately on acquire/lose/place/spend/recover/secure; whole numbers, normally 0–target; target 1 = unique object, > 1 = counted; `showAtZero` controls empty display; shown to players only while `active`; reaching target does not auto-complete; terminal statuses end player-facing lifetime while stored values may remain as history; no automatic cross-Mission transfer in v1.
- [x] Temporary validation Missions (counted, unique, hidden-until-progress, omitted-array) created and deleted after verifying behavior.

## Acceptance criteria

- [x] Mission schema and layout read back correctly; `missionTrackers` optional, omitted array valid.
- [x] Negative `current` rejected (must be >= 0); `target < 1` rejected (must be >= 1). The schema enforces `current >= 0` and `target >= 1`; Craft's supported `number` schema does not enforce integer-only values, so keeping both values as whole numbers is a documented System Rules obligation for the AI GM (no integer-only constraint supported).
- [x] Counted trackers use the standard read-only `Progress` component with current/target (0 / target when `showAtZero: true`; 3 / 10 verified). Unique target-1 objects display as a label/badge, never "1 / 1".
- [x] Zero-display works in both modes (counted `showAtZero: true` → displayed at 0; hidden tracker at 0 → not counted/displayed until `current` > 0).
- [x] Trackers never appear outside active Missions; omitted/empty array leaves no blank heading or empty panel.
- [x] Unrevealed upcoming Missions expose no tracker information.
- [x] No GM-only or raw authoring fields remain bound anywhere in the player-facing Mission layout.
- [x] No Mission Journal or Character fields added; no new item-like file type or content files created; no temporary Mission files remain; no unrelated schemas, layouts, content, maps, or Game Start metadata changed.

---

## Done — implementation notes (filled by the workspace assistant)

### Final schema shape

- New editable field **`missionTrackers`** (optional array of objects). Each row:
  - `key` — required string, `minLength: 1`, stable authoring identifier unique within the Mission
  - `label` — required string, `minLength: 1`, player-facing label (unit included)
  - `current` — required number, `minimum: 0`
  - `target` — required number, `minimum: 1`
  - `showAtZero` — required boolean
  - All five required; no other fields added.
- New computed field **`displayedTrackerCount`** (number):
  `($self.missionTrackers ?? []).filter(t => t.showAtZero === true || (t.current ?? 0) > 0).length` — the count of trackers that would render while active. Used to collapse the section when nothing shows; never stored.

### Standard layout component selected and why

The counted objective uses **`Progress`** (`label`, `value`, `max`, `showValue: true`), the standard read-only progress/tracker component in the platform's component set — it shows "value / max" without any editing affordance, matching the brief's "display, not an editing surface" requirement. A target-1 unique object renders as a **`Badge`** (variant `secondary`) showing just the label — the smallest standard possession-style component, avoiding any "1 / 1" display. Both rows live inside one `repeat` over `missionTrackers`, with per-row `visible` conditions choosing the counted vs. unique presentation (counted when `target > 1` and visible per `showAtZero`/`current`; unique when `target === 1` and visible per the same rule). No formatted display strings were stored to satisfy the layout.

### Player visibility rules

- The **Mission Trackers** section is visible only while `status` is `active` **and** `displayedTrackerCount > 0`. It collapses completely when the array is absent/empty or when no tracker would render.
- Trackers are read-only in the layout; the AI GM updates `current` through Mission state.
- Trackers never appear for upcoming/available/completed/failed/expired Missions.
- The unrevealed-upcoming protection is unchanged: an upcoming Mission with `revealedToPlayer: false` renders only the generic locked card and binds no Mission-specific field (including trackers).

### Mission-layout privacy fields removed

The rebuild removed all of: `gmNotes`, the complete `storyBeats`, beat `events`, beat `gmInstructions`, beat `loreUnlocks`, `startLoreUnlocks`, `completionLoreUnlocks`, `failureLoreUnlocks`, `unlockAfter`, `expiresWhenAvailable`, `completionEffects`, `failureEffects`, `selectorMap`, `mapX`, `mapY`, `pinLabel`, `currentBeatKey`, and the `revealedToPlayer` editing control. The visible layout now shows: hero (image/name/description, gated), Kind/Status badges, Code, Duration (moon steps), Fixed date, Objective, Briefing, Current Objective (active only), Mission Trackers (active only), Start Location, Team size, Mandatory/Selectable/Assigned team, and Debriefing + Actual Outcome for terminal statuses. This restores the spoiler boundary intended by the original Mission System brief.

### GM update / lifecycle rules (System Rules subsection)

Mission Trackers are Mission-local objective state, not inventory. The list may be omitted when a Mission has no item/count objectives. The GM updates `current` immediately when the fiction acquires, loses, places, spends, recovers, or secures the tracked thing. Values are whole numbers, normally 0–target. `target: 1` = unique object; `target > 1` = counted. `showAtZero` controls empty display. Trackers show only while `active`; reaching target does not auto-complete; completion/failure/expiration ends the tracker's player-facing lifetime while stored values may remain as history; no automatic cross-Mission transfer in v1.

### Validation performed

- Schema/layout written and read back cleanly; `missionTrackers` appears under editable and `displayedTrackerCount` under computed.
- Temporary Missions created and exercised, then deleted:
  - **Counted visible** (Méisrin of cream, `current 0 / target 10`, `showAtZero: true`): counted as displayed and reads 3 / 10 after `current` set to 3.
  - **Unique hidden** (Facility ledger, `current 0 / target 1`, `showAtZero: false`): not displayed at 0; displayed with count 1 at `current: 1` (no "1 / 1").
  - **Hidden-until-progress** (Evidence samples secured, `current 0 / target 3`, `showAtZero: false`): not displayed at 0; would display from 1 / 3.
  - **Omitted array** (`_TrackerNoTrackers`, no `missionTrackers`): valid; `displayedTrackerCount` 0; no blank section.
- Constraints: writing `current: -1` rejected ("must be >= 0"); writing `target: 0` rejected ("must be >= 1").
- `/Missions/` empty after deletion; no permanent Mission files or new file types created; no Mission Journal, Character, Outfit, or unrelated schema/layout/content changes.

### Genuine platform limitation

The schema system provides `minimum`/`maximum` (and other JSON Schema constraints) but not an integer-only type constraint. Craft's supported `number` schema enforces `current >= 0` and `target >= 1` but does not itself enforce integer-only values. Keeping both values as whole numbers is therefore an explicit System Rules obligation for the AI GM — record counts as whole numbers and normally clamp at target — rather than something the schema enforces.

## Review (filled by ChatGPT / the human after the pull)

<left for reviewer>
