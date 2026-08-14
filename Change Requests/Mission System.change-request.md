---
name: Mission System
description: Adds a lightweight Mission system for Internal Fey Affairs —
  Mission and Mission Journal file types, a mostly linear campaign structure
  with Story/Side/Opportunity/Interlude/Ability kinds, a moon-phase campaign
  calendar, per-playthrough world-map pin selection, Lore Card unlocks driven by
  Missions, GM lifecycle rules, and a campaign Mission Journal dashboard.
$craft:
  referenceId: 01a001e7-37f1-7bb4-a69c-26831e0f8435
---

# Brief: Mission System

**Status:** Done
**Opened:** (fill date)
**Reviewer:** ChatGPT (Codex)

## Goal

Create a lightweight Mission system for Internal Fey Affairs: a mostly linear sequence of Story Missions; Side Missions playable in any order between Story Missions; mandatory Interludes (holidays/village events); time-limited Opportunities; Ability Missions that may award trained Abilities; Mission selection through per-playthrough pins on the world map; and a campaign-wide Mission Journal holding the authoritative date and Mission index. Build the structures, layouts, and GM rules only — do NOT invent actual campaign Missions, mission lore, map geography, or a replacement game opening in this pass.

## Relevant canon

- The campaign shape is already canon: "an almost linear campaign where story missions are played in order, and secondary missions can be played between them in any order" (System Rules, Shape of Play). The Lore Card rules already anticipate Mission-based unlocks ("once the future Mission system supports it") — replace that wording with the implemented mechanism.
- Lore Card state is project-global (single-campaign project) — Mission state likewise lives directly on Mission files; no per-campaign file copies or isolation engine.
- The starting Mission must NOT be a boolean flag on Mission files. The single authoritative starting-Mission reference belongs on the Mission Journal.
- No actual Missions, map geography, or Game Start changes in this pass.
- Mission kinds are player-facing terms: Story, Side, Opportunity, Interlude, Ability. Statuses: upcoming, available, active, completed, failed, expired.
- Grace, Bargain, Reputation, relationship, Condition, Outfit, and world-state consequences live in `completionEffects`/`failureEffects` prose — no universal structured reward engine in v1.

## Files touched

- /Change Requests/Mission System.change-request.md — this brief
- /file-types/mission.json — new Mission file type (schema; layout at /file-types/mission/layout.json)
- /file-types/mission-journal.json — new Mission Journal file type (schema; layout at /file-types/mission-journal/layout.json)
- /Missions/ — new root folder (auto-created; no permanent files in this pass)
- /Mission Journals/Campaign Mission Journal.mission-journal.json — new campaign journal
- /GM Instructions/System Rules.gm-instructions.md — add a Missions section; replace the "future Mission system" wording in Lore Cards
- Temporary validation Mission files (deleted before completion)

## Changes

- [x] **Mission file type** (designation null, JSON). Built-in name/description/image only for identity/presentation (image = thumbnail/cover; no separate thumbnail field).
- [x] **Mission identity fields:** `code` (required string, authoring ID like `ST-01` — never a reference mechanism), `kind` (required enum Story/Side/Opportunity/Interlude/Ability), `status` (required enum upcoming/available/active/completed/failed/expired, default upcoming), `revealedToPlayer` (boolean, default false), `briefing` (longer player-facing brief), `objective` (required player-facing overall objective).
- [x] **Date shape (reused everywhere dates are stored):** object with `month` (enum January–December, calendar order) and `moonPhase` (enum 🌑🌒🌓🌔🌕🌖🌗🌘, in order). No year field in v1.
- [x] **Mission time fields:** `fixedDate` (optional date object — absent means availability is controlled by prerequisites/state, not a calendar date), `durationSteps` (required non-negative number, treated as whole moon-phase steps, may be 0), `startedDate` (optional runtime date), `endedDate` (optional runtime date), plus computed `fixedDateLabel` (top-level string, e.g. `November 🌒`, empty when no fixed date — never store the formatted duplicate).
- [x] **Calendar sequence:** 🌑 → 🌒 → 🌓 → 🌔 → 🌕 → 🌖 → 🌗 → 🌘 → next month 🌑; after December comes January.
- [x] **Availability fields:** `unlockAfter` (optional Mission reference array — available only when every referenced Mission is completed; no reverse "unlocks" lists on predecessors), `expiresWhenAvailable` (optional Mission reference array, intended for Opportunities — if this Mission is upcoming/available and any referenced Mission becomes available, this Mission becomes expired). Status itself is NOT computed; the AI GM evaluates prerequisites and writes explicit status changes.
- [x] **Team and location fields:** `teamSize` (optional positive whole number, exact team size), `mandatoryMembers` (optional Character reference array), `selectableMembers` (optional Character reference array), `assignedTeam` (optional Character reference array — runtime record set when the Mission becomes active), `startLocation` (optional Location reference). GM rules require: every mandatory member included; assigned count matches `teamSize` when defined; remaining members come from `selectableMembers`; no duplicated Character references.
- [x] **Runtime map placement fields (do not author/mutate shared Location maps):** `selectorMap` (Location reference for the selection map), `mapX`/`mapY` (numbers constrained 0–1), `pinLabel` (optional player-safe label; fallback to Mission name). Starting Mission may omit these because Game Start launches it directly. No routine pin colour/icon fields in v1 — kind and status determine styling via GM rules.
- [x] **Story beats:** required `storyBeats` array; each beat requires `key` (stable unique short id), `title`, `location` (optional Location reference), `objective` (player-relevant), `events` (GM-facing), `gmInstructions` (GM-facing), `loreUnlocks` (optional array of rows requiring `card` — Lore Card reference — and `level` enum Common Knowledge/Deep Lore/Fey Knowledge). Runtime `currentBeatKey` (the active beat's stable key; set on start, updated as beats advance; omitted when not active). No per-beat completed flag in v1.
- [x] **GM material and consequences:** `gmNotes`, `startLoreUnlocks`, `completionLoreUnlocks`, `failureLoreUnlocks` (optional lore unlock rows), `awardedAbilities` (optional Ability reference array granted on completion), `completionEffects` / `failureEffects` (GM-facing prose — Grace/Bargain/Reputation/relationship/Condition/Outfit/world-state consequences live here), `debriefing` (player-facing terminal text), `actualOutcome` (runtime prose after play). No universal structured reward engine.
- [x] **Lore unlock behavior (System Rules):** Common Knowledge → set card `discovered` true; Deep Lore → set `discovered` + `deepLoreKnown`; Fey Knowledge → set `discovered` + `feyKnowledgeKnown`. Apply consistently to start/beat/completion/failure rows. Never set a deeper flag while `discovered` is false.
- [x] **Mission Journal file type** (designation null, JSON). Stored: `missions` (Mission reference array — master list, update when a permanent Mission is authored), `startingMission` (optional single Mission reference — authoritative campaign start), `currentMonth` (optional month enum), `currentMoonPhase` (optional phase enum). Computed: `currentDateLabel` (top-level string, empty until date initialized), `availableMissions` (Mission reference + name/code/kind/description/fixed-date label/status where stored status is available), `activeMissions` (equivalent for active), `knownUpcomingMissions` (status upcoming AND revealedToPlayer true), `missionHistory` (completed/failed/expired with terminal status and ended date where available). All projections use direct one-level `load()` reads from referenced Mission files; never stored in journal content.
- [x] **Campaign Mission Journal file:** /Mission Journals/Campaign Mission Journal.mission-journal.json — empty `missions` array, no invented starting Mission, no invented current date, no image.
- [x] **Mission layout (player-facing):** image/title/short description with graceful fallback; Code; Kind; Status; Fixed date when present; Duration in moon steps; Objective; Briefing when present; Start Location; Team size; Mandatory and selectable members; Assigned team while active or afterward; Debriefing and actual outcome only for appropriate terminal statuses. Must NOT expose: `gmNotes`, beat `events`, beat `gmInstructions`, hidden Lore unlock definitions, unlock/expiration Mission references, completion/failure effect instructions, unrevealed future info, raw map coordinates, or raw authoring/runtime fields players don't need. Do not display the complete story-beat plan; expose only a safe computed current-beat objective (`currentBeatObjective`), leaving beat progress GM-facing in v1 otherwise.
- [x] **Mission Journal layout (campaign dashboard):** current date prominently displayed; Active Mission section visible without opening a tab; Available / Known Upcoming / History sections (tabs acceptable); read-only reference grids/cards; hide empty sections gracefully. World map remains the primary picker; the dashboard is the persistent overview and fallback index. No binding to the raw `missions` master list.
- [x] **Map behavior (System Rules):** shared author-time maps are never mutated during play; use the runtime scene-map scope. Upcoming Missions have no pin. When a Mission becomes available, add its linked pin at `mapX`/`mapY` on `selectorMap`, labeled `pinLabel` (fallback Mission name); re-placing the same linked pin must not create duplicates. When active, visually highlight/restyle the pin. When completed/failed/expired, remove the selectable pin; the Mission file and Journal retain permanent history. Evaluate status first, then synchronize pins. Pin behavior is GM-driven, not data-bound (status change alone never mutates a map). Never use author-time Location/open map mutations for playthrough availability. Do not invent exact colours/icons as binding canon — use a clear, consistent temporary distinction by kind and a stronger highlight for active status; record that styling remains a playtest choice.
- [x] **Mission lifecycle and GM rules (System Rules, concise but complete):** starting the campaign (Game Start launches the Journal's `startingMission` directly; starting Mission needs no selection pin; actual Game Start rewriting and the first Mission deferred until campaign content exists); selecting a Mission (only available pins presented; opening a pin displays the brief but does not start the Mission; player explicitly chooses to begin; on selection validate + record `assignedTeam`, set status active, set `startedDate` to the current campaign date, set `currentBeatKey` to the first beat, apply `startLoreUnlocks`, highlight the pin, begin at `startLocation` or the first beat's Location); playing beats (follow authored order unless stated otherwise; update `currentBeatKey`; apply beat `loreUnlocks` when reached/resolved; actions may fail — failure alters cost/time/Conditions/position/reputation/relationships/approach, never blocks Story/Side/Interlude completion); terminal outcomes (Story/Side/Interlude never `failed`; Opportunity/Ability may complete or fail as authored; `expired` only for a window that closes before undertaking, never for an attempted Mission; when a Mission ends: set status as permitted, record `endedDate` + `actualOutcome`, apply the appropriate Lore unlocks, append `awardedAbilities` to the relevant Character without duplicates, apply/narrate effects, remove the selectable pin, advance the Journal calendar by `durationSteps`, evaluate fixed-date Missions / completed-prerequisite unlocks / Opportunity expirations, synchronize available pins).
- [x] **Calendar priority:** one phase per step in the eight-phase sequence; 🌘 wraps to next month's 🌑; December wraps to January. A fixed-date Mission becomes due when the current date reaches/passes its fixed date. When a fixed-date Story or Interlude becomes available, it takes priority: do not advance the campaign clock further until that Story/Interlude is completed. This does not retroactively interrupt a Mission already being played — enforce the priority when that Mission ends and the calendar advances. If a multi-step duration crosses a fixed mandatory date, make that Mission due at the resulting date and block further advancement until completed.
- [x] **Unlocks and expirations:** an upcoming Mission becomes available when every `unlockAfter` Mission is completed AND any fixed-date requirement has arrived (empty list = no prerequisite). If an upcoming/available Opportunity has any `expiresWhenAvailable` Mission become available, set the Opportunity to expired and remove its pin. Resolve newly available mandatory Story/Interlude Missions before offering further time-consuming choices. Failure/expiration of Opportunity or Ability Missions never blocks the Story spine.
- [x] **Context and playability:** Mission Journal is mutable campaign state and should be pinned in GM context; Mission files searchable or partial rather than all pinned; System Rules remains standing GM context. Because context visibility is app-managed, include a final manual checklist: System Rules → pinned; Campaign Mission Journal → pinned; Mission files → partial or searchable.
- [x] **No Game Start metadata changes** in this pass (no real starting Mission exists yet); record that wiring Game Start to the first Mission remains a future campaign-content step.

## Acceptance criteria

- [x] Both schemas and layouts read back correctly.
- [x] No custom layout exposes GM-only Mission material (gmNotes, beat events/gmInstructions, hidden lore unlock rows, unlock/expiration references, completion/failure effects, raw map coordinates, unrevealed future info).
- [x] An upcoming Mission with `revealedToPlayer: false` shows only the generic locked presentation and exposes no Mission-specific player information; revealed upcoming and all non-upcoming statuses display normally (verified in the peer-review pass).
- [x] Mission Journal computed projections use only one-level `load()` reads.
- [x] Computed fields are absent from stored content.
- [x] Temporary test Missions are removed.
- [x] Campaign Mission Journal contains no invented date or starting Mission.
- [x] A temporary test confirms: all completed prerequisites permit availability; an incomplete prerequisite prevents it; Opportunity expiration occurs when any lock Mission becomes available; Story/Side/Interlude are never assigned failed by the rules; Ability/Opportunity may use failed; Lore unlock levels map to the existing Lore Card flags correctly; a duration step advances the moon sequence; 🌘 wraps to the next month's 🌑; a due Story or Interlude prevents further time advancement; map coordinates accept only 0–1; Mission Journal available/active/history projections return the correct Missions.
- [x] No conflict artifacts or unwanted generated imagery are present.
- [x] Existing Character, Lore Card, Lore Journal, Location, Game Start, Condition, Outfit, Grace, Bargain, Species, Archetype, Attribute, Ability, and Origin data remain valid and otherwise unchanged.

---

## Done — implementation notes (filled by the workspace assistant)

### What shipped

- **Two new JSON file types, both `designation: null`:**
  - **Mission** (`/file-types/mission.json`; schema `/file-types/mission/schema.json`; custom player-facing layout `/file-types/mission/layout.json`). Root folder `/Missions/` auto-created and left empty this pass.
  - **Mission Journal** (`/file-types/mission-journal.json`; schema `/file-types/mission-journal/schema.json`; dashboard layout `/file-types/mission-journal/layout.json`). Root folder `/Mission Journals/`.
- **Mission schema fields (exact):** identity — `code` (required), `kind` (required enum Story/Side/Opportunity/Interlude/Ability), `status` (required enum upcoming/available/active/completed/failed/expired, default upcoming), `revealedToPlayer` (bool, default false), `briefing`, `objective` (required). Time — `fixedDate`/`startedDate`/`endedDate` (reused date object: `month` enum January–December, `moonPhase` enum 🌑🌒🌓🌔🌕🌖🌗🌘), `durationSteps` (required, min 0, default 0). Availability — `unlockAfter` (Mission ref array), `expiresWhenAvailable` (Mission ref array). Team/location — `teamSize` (min 1), `mandatoryMembers`/`selectableMembers`/`assignedTeam` (Character ref arrays), `startLocation` (Location ref). Map placement — `selectorMap` (Location ref), `mapX`/`mapY` (numbers, min 0, max 1), `pinLabel`. Beats — required `storyBeats` (each beat: required `key`, `title`, `objective`, `events`, `gmInstructions`; optional `location`, optional `loreUnlocks` rows of `card` (Lore Card ref) + `level` enum Common Knowledge/Deep Lore/Fey Knowledge), runtime `currentBeatKey`. GM material — `gmNotes`, `startLoreUnlocks`, `completionLoreUnlocks`, `failureLoreUnlocks`, `awardedAbilities` (Ability ref array), `completionEffects`, `failureEffects`, `debriefing`, runtime `actualOutcome`. Computed — `fixedDateLabel` ("November 🌒", "" when absent), `currentBeatObjective` (current beat's player-facing objective, "" when inactive — the only beat info exposed).
- **Mission Journal schema fields:** stored — `missions` (Mission ref array, master list), `startingMission` (single Mission ref), `currentMonth` (month enum), `currentMoonPhase` (phase enum). Computed — `currentDateLabel`, `availableMissions`, `activeMissions`, `knownUpcomingMissions`, `missionHistory`; every projection uses direct one-level `load()` reads from referenced Mission files and each row carries the hydrated Mission reference plus name/code/kind/description/fixed-date label/status (history adds terminal status and `endedDateLabel`).
- **Campaign journal:** `/Mission Journals/Campaign Mission Journal.mission-journal.json` — `missions: []`, no starting Mission, no current date, no image (an auto-added base image was removed).
- **Mission layout (player-facing):** hero with graceful image fallback; Kind + Status badges; Code + Duration (moon steps); Fixed date when present; Objective; Briefing when present; computed Current Objective only when a beat is active; Start Location; Team size; Mandatory / Selectable / Assigned team grids; Debriefing and Actual Outcome only for terminal statuses (completed/failed/expired). Never binds `gmNotes`, `storyBeats`, beat `events`/`gmInstructions`, any Lore unlock rows, `unlockAfter`, `expiresWhenAvailable`, `completionEffects`, `failureEffects`, `selectorMap`, `mapX`, `mapY`, `pinLabel`, `currentBeatKey`, or other raw runtime/authoring fields; the complete beat plan is never displayed.
- **Mission Journal layout (dashboard):** prominent Campaign Date card (label, or a muted "not set" note), Active Mission section outside the tabs, then Available / Known Upcoming / History tabs; all sections are read-only FileReferenceGrids over the computed projections with empty-state text; no binding to the raw `missions` master list.
- **System Rules:** new **Missions** section (kinds; statuses incl. the Story/Side/Interlude-never-failed invariant; starting the campaign; selection incl. team validation; beat play and fail-forward; terminal outcomes and the end-of-Mission checklist; calendar sequence and priority; unlocks/expirations; lore unlock flag mapping; scene-map behavior; context checklist). The Lore Cards bullet now references the implemented unlock rows instead of the "future Mission system". Frontmatter description updated to mention missions.

### Runtime versus computed state

- Stored on Mission files: everything except `fixedDateLabel` and `currentBeatObjective`. Stored on the Journal: `missions`, `startingMission`, `currentMonth`, `currentMoonPhase`. All five journal projections are computed at read time and never written into the file (read-back lists them as computedFields; stored content holds only name/missions/description).
- `status` is explicitly written by the AI GM — never a computed field; journal projections merely reflect stored statuses.

### Mission kinds and status semantics

- Kinds: Story (linear spine, calendar priority), Side (flexible order; prerequisites via `unlockAfter`, not kind), Interlude (mandatory, calendar priority), Opportunity (time-limited; may expire/fail), Ability (may award `awardedAbilities`).
- Statuses: upcoming → available → active → completed, plus failed (Opportunity/Ability only) and expired (window closed before undertaking; never for an attempted Mission). Story/Side/Interlude never become failed — setbacks complicate or delay, and the rules say so. Failure/expiration of Opportunity/Ability never blocks the Story. Terminal files remain as campaign history after pins disappear.

### Calendar behavior

- Date = Journal `currentMonth` + `currentMoonPhase`; one step per `durationSteps`; sequence 🌑→🌒→🌓→🌔→🌕→🌖→🌗→🌘→ next month 🌑; December→January; no year in v1. Fixed-date Missions become due when the date reaches/passes them; a due Story/Interlude takes priority and blocks further advancement (enforced when the current Mission ends, never retroactively). Multi-step durations crossing a fixed mandatory date make that Mission due at the result. Validated: `fixedDateLabel` "November 🌒", `currentDateLabel` "November 🌘" → "December 🌑" after the wrap.

### Runtime scene-map behavior

- Rules-only this pass (no shared map edits): pins live on the runtime scene-map scope; upcoming Missions have no pin; becoming available adds a linked pin at `mapX`/`mapY` on `selectorMap` with `pinLabel` fallback and no duplicates; active highlights/restyles the pin; terminal statuses remove the pin while the file + journal keep history. Status is evaluated first, then pins are synchronized. Pin behavior is GM-driven — a status change never mutates a map by itself. Author-time Location/open maps are never used for playthrough availability. Temporary kind/active styling is a playtest choice, not canon.

### Lore and Ability reward behavior

- Unlock rows map: Common Knowledge → `discovered`; Deep Lore → `discovered` + `deepLoreKnown`; Fey Knowledge → `discovered` + `feyKnowledgeKnown`; never set a deeper flag while `discovered` is false. Applied consistently for start/beat/completion/failure rows. On completion, `awardedAbilities` are appended to the Character's `abilities` without duplicates. Other consequences live in `completionEffects`/`failureEffects` prose.

### Validation performed

All writes passed platform schema/layout validation. Behavioral tests used temporary Mission files (deleted before completion):

- Journal projections returned exactly the right Missions: available → ST-01; known-upcoming → SI-02 (upcoming + revealed); history → AB-04 completed with endedDateLabel "December 🌑"; OP-03 (upcoming, unrevealed) in none; active empty.
- Prerequisite gating: while ST-01 was not completed, SI-02 stayed upcoming/not available; after ST-01 completed and the GM set SI-02 available per the unlock rule, it appeared in availableMissions.
- Opportunity expiration: OP-03 set expired after its `expiresWhenAvailable` lock became available → history with status expired.
- Status semantics demonstrated: Story completed, Opportunity expired, Ability failed (failed allowed for Ability/Opportunity; rules forbid failed for Story/Side/Interlude).
- Lore flag mapping: `discovered` → journal "Discovered"; + `deepLoreKnown` → "Deep Lore known"; + `feyKnowledgeKnown` → "Fey Knowledge known"; all restored to false.
- Calendar: `currentDateLabel` rendered "November 🌘" then "December 🌑" (🌘 → next month 🌑 wrap).
- mapX/mapY bounds: writing 1.5 rejected ("must be <= 1"); writing −0.1 rejected ("must be >= 0").
- Computed fields absent from stored content (journal stores only name/missions/description; mission files store no `fixedDateLabel`/`currentBeatObjective`).
- Cross-Mission references hydrate to slug + referenceId (one batch-order draft was re-resolved by a fresh write).
- Final state: /Missions/ empty; journal has no date, starting Mission, or image; Boggart flags false; Example Character and all other pre-existing files untouched.

### Platform limitations encountered

- Same-batch writes: a Mission reference written in the same batch as its target resolved as a draft; re-writing the same content afterwards re-hydrated it. Sequence reference-creating writes after the target exists.
- The platform auto-added a generated base image to the Campaign Mission Journal and to this Change Request despite no image request; both were removed and no replacements generated.
- Status semantics (Story never failed, expired only for unundertaken windows, calendar priority) are GM-rule invariants — the schema cannot mechanically enforce them; they live in System Rules.
- Context visibility is app-managed (types were created searchable); the manual checklist below is the record.

### Manual context-visibility checklist

- System Rules → pinned
- Campaign Mission Journal → pinned
- Mission files → partial or searchable

### Deferred work

- Actual campaign Missions (Story/Side/Interlude/Opportunity/Ability content)
- Actual world-map construction and per-playthrough pin playtesting
- First-Mission / Game Start wiring (Game Start metadata intentionally untouched this pass)
- Final pin colours/icons (temporary scheme is a playtest choice)
- Structured rewards beyond Ability and Lore references (Grace/Bargain/Reputation/etc. stay in effect prose)
- Sophisticated branching, retries, or multiple-campaign isolation

### Peer-review corrections (second pass)

Applied per review prompt; architecture, schemas, references, projections, map model, and the Mission Journal layout were preserved except where the corrections required changes.

- **Unrevealed upcoming Missions are concealed.** The Mission layout's hero and body are now visible only when `revealedToPlayer` is true OR `status` is not `upcoming`; an upcoming Mission with `revealedToPlayer: false` renders a generic locked card ("This Mission has not yet surfaced in the campaign…") that binds no Mission-specific fields — name, description, image, code, kind, objective, briefing, dates, locations, team, and beat info all stay hidden. Available, active, and terminal Missions are unaffected by a stale `revealedToPlayer` value.
- **Current-beat state is active-only.** `currentBeatObjective` now returns a beat objective only while `status` is `active` and empty otherwise, even with a stale `currentBeatKey`. The layout's Current Objective section is visible only when the Mission is active AND the computed objective is non-empty. System Rules now clears/omits `currentBeatKey` at the end of a Mission (new step 8 of the terminal-outcome procedure). `storyBeats` gained `minItems: 1` because starting a Mission requires a first beat; an empty array is rejected by schema validation.
- **Journal date display corrected.** `currentDateLabel` is now empty unless BOTH `currentMonth` and `currentMoonPhase` are present; it can never produce a partial value such as `November ` or a phase without a month.
- **Change Request repaired (final).** The duplicate frontmatter block and orphaned image-generation metadata (`startedAt`, `finishedAt`, `prompt`) were removed, and the remaining body-level YAML frontmatter block was deleted entirely — the stored Markdown body begins directly with `# Brief: Mission System`. The description lives in Craft's separate Description metadata field; no `name`, `$craft`, or YAML delimiters appear inside the body. Note: no `$craft.referenceId` was present in the file's raw content to preserve — flagged for the reviewer rather than invented.
- **Verified (this pass):** unrevealed upcoming Mission exposes no mission-specific player information (layout read-back: hero/body gated, locked card binds nothing); revealed upcoming displays normally; available/active/completed/failed/expired are unlocked regardless of `revealedToPlayer`; `currentBeatObjective` is non-empty only while active (temp-file test with a stale `currentBeatKey`: active → objective, completed/upcoming → ""); empty `storyBeats` rejected by validation; `currentDateLabel` empty with month-only and with phase-only, full label when both present; the Change Request body contains no frontmatter block and begins directly with `# Brief: Mission System`, with the description in the separate Description metadata field, so the pulled checkout contains exactly one opening frontmatter block — the checkout-generated block containing `$craft.referenceId` — followed directly by `# Brief: Mission System`; no generation metadata remains; /Missions/ is empty and the Campaign Mission Journal has no date, starting Mission, or image.

## Review (filled by ChatGPT / the human after the pull)

<left for reviewer>
