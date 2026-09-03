---
name: First Three Missions
description: Implements The Encounter, Milk and Cream, and Iron and Wax as the
  first playable campaign slice with supporting characters, locations, trackers,
  lore unlocks, and journal state.
$craft:
  referenceId: 01a06897-2c82-723a-b60c-ac8303fc1af5
---

# Brief: First Three Missions

**Status:** Done locally; Game Start metadata pending after push
**Opened:** 2026-09-03
**Reviewer:** ChatGPT (Codex)

## Goal

Turn the first three mission drafts into a coherent playable opening that exercises the existing Mission, Lore Card, Bargain, Grace, Condition, and Mission Tracker systems. Preserve the scripted homecoming and the first genuine player choice at Méabh's arrival while ensuring that checks and refusals cannot block the campaign.

## Relevant canon

- The Encounter begins in August 151 AD. Méabh is a Human Takeling and trained Leannán Sidhe secretly assigned by Morrigan; Niamh is her Changeling replacement; Grannie recognises the connection but remains silent.
- Coinín is a Pooka field researcher using rabbit glamour. Buaic is a Brownie candle maker. Neither can knowingly lie, and neither can safely consume Earth food.
- Coinín's supplies serve hidden Fey logistics, but the player should still think he wants the food for himself during these Missions.
- Fey technology must be presented through Celtic perception. Failure changes position or cost and never blocks a Story or Side Mission.

## Files touched

- /Missions/The Encounter.mission.json — new
- /Missions/Milk and Cream.mission.json — new
- /Missions/Iron and Wax.mission.json — new
- /Mission Journals/Campaign Mission Journal.mission-journal.json
- /Characters/Non-Player Characters/Méabh.non-player-character.json
- /Characters/Non-Player Characters/Niamh.non-player-character.json — new
- /Characters/Non-Player Characters/Grannie.non-player-character.json — new
- /Characters/Non-Player Characters/Coinín.non-player-character.json — new
- /Characters/Non-Player Characters/Buaic.non-player-character.json — new
- /Archetypes/Pooka.archetype.json — new
- /Locations/Granny's Hut.location.json
- /Locations/Tri Mhuilinn.location.json
- /Locations/Fairy Ring.location.json
- /Locations/Beekeper's Workshop.location.json
- /GM Instructions/System Rules.gm-instructions.md

## Changes

- [x] Make The Encounter the Story starting Mission (`ST-01`) at provisional date August 🌒.
- [x] Make Milk and Cream (`SI-01`) and Iron and Wax (`SI-02`) Side tutorials that unlock together after The Encounter and may be played in either order.
- [x] Give all three `durationSteps: 0`, keeping the immediate tutorial sequence within one moon phase.
- [x] Author ordered, hidden GM beats with player objectives, fail-forward instructions, protected secrets, and deliberate Lore Card unlocks.
- [x] Use Mission Trackers for cream, milk, workshop clearance, honey, and the wax candle; do not create inventory items.
- [x] Specify runtime creation and partial settlement of Coinín's Silence rather than pre-creating an active bargain before anyone accepts it.
- [x] Add the four required NPCs and the Pooka Archetype; enrich Méabh's existing NPC record without replacing her image.
- [x] Remove the IA Agent Archetype from Méabh at campaign start; she is Morrigan's covert Leannán asset, not yet an Internal Affairs agent.
- [x] Add scene-ready content to the four existing opening Locations without altering their maps or images.
- [x] Initialise the Mission Journal with all three Missions, The Encounter as `startingMission`, and August 🌒 as the campaign date.
- [x] Do not create a Pooka Lore Card yet; no complete Common/Deep/Fey tier text was supplied.

## Acceptance criteria

- [x] Every Mission satisfies the existing schema and references real or same-batch records.
- [x] The two tutorial Missions are unavailable before The Encounter completes, then become available together.
- [x] The three Missions can be played without permanent failure or campaign blockage.
- [x] Trackers display counted quantities at zero and hide unique achievements until acquired.
- [x] Brownie, Hob, and Boggart Common Knowledge unlock during the opening; Brownie Deep Lore unlocks through Buaic's exchange.
- [x] No shared author-time map is modified; Side Mission coordinates describe runtime scene pins only.
- [ ] After push, replace the obsolete Tavern Start metadata with a start at Granny's Hut using The Encounter and The Blacksmith, and verify the playable flag/context visibility in the app.
- [ ] Before a clean playtest, reset or replace The Blacksmith's existing UI-demo state (test Grace entries, Reputation Markers, depleted Conditions, and spoiler Bargain references). This batch deliberately preserves that pre-existing user data.

---

## Done — implementation notes

The local content implementation is complete. The provisional starting moon phase is 🌒 because the source specified August but not a phase; changing it later requires only the Journal and The Encounter fixed date. All three Missions take zero steps because the opening, night collection, and Brownie errand form one immediate tutorial cluster.

The Side Missions are intentionally siblings rather than a fixed chain. Their completion logic records partial delivery against the runtime Coinín's Silence Bargain and resolves the goods obligation only after both Side Missions have supplied all three goods. Coinín's continuing promise remains active.

The Game Start designation metadata is app-managed and cannot be safely rewritten as ordinary JSON. After these files are pushed, use Oracle or Craft's game-start controls to rename/replace Tavern Start, point it at The Blacksmith and Granny's Hut, and launch The Encounter. Mission Journal and System Rules should be pinned; Mission files should remain partial or searchable.

The existing Player Character was not rewritten. It currently contains obvious interface-test state, including large test Grace balances, sample Reputation Markers and Condition depletion, and knowledge of late Bargains. Preserve it for UI work if useful, but reset it or create a clean starting character before evaluating spoiler flow and tutorial balance.

## Review

<Awaiting post-push review.>
