---
name: Hollow Tree Escape Routes
description: Establishes Coinín's Hollow Tree, service tunnel, escape hatch, and
  woodland Fairy Door as fixed geography in the opening Missions.
$craft:
  referenceId: 01a07329-5cd0-70ff-9d11-653aef0efe3d
---

# Brief: Hollow Tree Escape Routes

**Status:** Done
**Opened:** 2026-09-05
**Reviewer:** ChatGPT (Codex)

## Goal

Turn a successful playtest improvisation into authored geography. Failed patrol-avoidance checks in The Encounter may lead to a narrow escape through Coinín's service tunnel without blocking the Mission, while the separate Fairy Door provides an optional route for the two tutorial Side Missions.

## Relevant canon

- The Hollow Tree stands at the edge of the Fairy Ring glade and serves as Coinín's concealed safehouse.
- Its service tunnel is a physical maintenance passage ending at a hidden hatch near the far side of the glade.
- Its Fairy Door is a permanent portal to a concealed endpoint in the woods west of Trí Mhuilinn. It is not the service tunnel, a Fairy Ring, an emergency teleporter, or literal magic.
- Failure changes cost or position and must not block Story or Side Missions.

## Files touched

- /Locations/Hollow Tree.location.json
- /Locations/Hollow tree - servide tunnel.location.json
- /Locations/Hidden hatch.location.json
- /Locations/Hidden Fairy Door.location.json
- /Locations/Fairy Ring.location.json
- /Locations/Tri Mhuilinn.location.json
- /Missions/The Encounter.mission.json
- /Missions/Milk and Cream.mission.json
- /Missions/Iron and Wax.mission.json

## Changes

- [x] Give all four new Location records enough player-facing and GM-facing prose to make their connections unambiguous.
- [x] Make the Hollow Tree the hiding location in The Encounter.
- [x] On a failed patrol-avoidance check, use the service tunnel and Hidden hatch as the established fail-forward escape; do not invent a replacement route.
- [x] Move Coinín's bargain scene and later briefings and deliveries to his Hollow Tree.
- [x] Offer the Fairy Door as an optional route to the western woods in Milk and Cream and Iron and Wax without bypassing either Mission's real obstacles.
- [x] Preserve successful stealth as success: the escape tunnel is not forced when the patrol moves on.

## Acceptance criteria

- [x] A failed stealth check can reduce Cover and trigger pursuit without capture or Mission failure.
- [x] The service tunnel, hatch, and Fairy Door remain three distinct pieces of geography and technology.
- [x] All new Mission location references point to the existing Hollow Tree record.
- [x] No map metadata, Mission status, tracker progress, or playthrough state is changed.

---

## Done — implementation notes

The routes are encoded both in their Location records and at the exact Mission beats where the GM needs them. The physical escape is reserved for a patrol setback; the portal is optional travel utility in the tutorial Side Missions.

The existing file name `Hollow tree - servide tunnel` retains its spelling because it is already a synced Craft identity. It can be renamed separately in the app or with `craft mv` without mixing a rename into these content edits.

## Review

<Awaiting post-push playtest.>
