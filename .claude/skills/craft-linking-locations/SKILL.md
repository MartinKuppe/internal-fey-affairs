---
name: craft-linking-locations
description: "Read this before saying how locations connect: map exits (a door, stairs, a road or a portal drawn on a map to another location), transports (a location that is a ship, caravan or train), and linked locations (a place inside another, the floors of an inn, a way between two places with no spot on a map). Covers the craft map exit field, craft meta set-transport / link / unlink, the stored shapes and the rules."
---

# Linking Locations

The GM moves the party between locations by how they connect. A pin that
`link`s a location only says where it is drawn. To say how you GET there
(the stairs up to the inn's second floor, the gate out of town, a portal
to the Feywild, a tunnel between two districts), a location has three more
things, all app-managed metadata on location files:

- **Exits** — a pin or area on a map that is a way to another location.
- **Linked locations** — how two locations relate with no spot on a map:
  one inside the other, both parts of one place, or a way between them.
- **Transports** — a location that carries people (a ship, a caravan).

What the GM does with them: it finds a place's parts and neighbours
through them, offers their ways as moves ("stairs up to the Top Floor"),
and arrives where a way comes in; `locked`, `hidden` and `oneWay`
limit what it offers. A place linked inside another shows "Within: …" in
the current state even when no map draws it. Worlds with
few maps can link every place this way.

All of these write the server directly (the files must be PUSHED first)
and advance the sync ledger, like `craft map set`. `craft meta get
<location>` shows all three with workspace paths.

## Exits (on a map)

An area or point may carry `exit`. Author it with `craft map get`, edit,
`craft map set` (see authoring-maps.md):

```json
{
  "kind": "pin",
  "label": "Stairs up",
  "position": { "x": 0.82, "y": 0.31 },
  "exit": {
    "kind": "stairs",
    "to": "Places/The Hooked Knucklehead Top Floor.location.json",
    "direction": "up"
  }
}
```

- `kind`: door, gate, stairs, ladder, hatch, path, road, dock, portal,
  bridge, ferry, tunnel, ritual, other (default door).
- `to`: the location it leads to — a workspace path, `slug.type` or
  referenceId. **Leave it out for the way out**: an exit with no `to`
  leads to the location whose map this map sits on (the inn's front door
  out to the street).
- `direction`: which way it runs from here — up, down, in, out, through,
  or a compass point (north … northwest).
- Flags: `main` (the main way in, where arrivals come in), `locked`,
  `hidden` (a secret door), `oneWay`. Words: `when` ("at twilight"),
  `requires` ("an obol"), `label` (the way's own name), `distance`
  ("300 ft"), `time` ("3 days on foot").
- The place an exit leads to is reached THROUGH the map, not drawn on it:
  don't also pin the second floor on the inn's map.

## Linked locations (no spot on a map)

A pair says how `<location>` relates to `<other>`:

```
craft meta link "Places/The Ring Kitchens Cellar.location.json" "Places/The Ring Kitchens.location.json" --file link.json
craft meta unlink "Places/The Ring Kitchens Cellar.location.json" "Places/The Ring Kitchens.location.json"
```

`link.json`, read from the first location's side:

```json
{ "relation": "part_of", "way": { "kind": "stairs", "direction": "up" } }
```

| relation | means | needs |
|---|---|---|
| `part_of` | location is part of other (a cellar of the kitchens) | |
| `contains` | other is part of location | |
| `same_place` | both are parts of one place (two floors of an inn) | `"in": "<path>"` |
| `separate` | two places of their own, joined only by the way | `way` |
| `other` | related some other way, in words | `"words": "is the Shadowfell reflection of"`, `"wordsFirst": "location"` or `"other"` |

`way` is an exit's fields without `to` (and `arriveAt`/`main`): its
`direction` runs from the first location toward the other, and
`oneWay` is `"there"` (first to other only) or `"back"`. `link`
replaces whatever lay between the two. `unlink` removes all of it.

More examples.

Two floors of one inn, stairs up from the first:

```json
{ "relation": "same_place", "in": "Places/The Hooked Knucklehead.location.json", "way": { "kind": "stairs", "direction": "up" } }
```

A road between two towns, in a world with no maps:

```json
{ "relation": "separate", "way": { "kind": "road", "direction": "north", "label": "the Triboar Trail", "distance": "50 mi", "time": "3 days on foot" } }
```

A hidden fey crossing that opens at twilight:

```json
{ "relation": "separate", "way": { "kind": "portal", "when": "at twilight", "hidden": true } }
```

A city's reflection in another realm:

```json
{ "relation": "other", "words": "is the Shadowfell reflection of", "wordsFirst": "location" }
```

**The rules** (the server refuses a save that breaks them):

- A location is inside **at most one** place, never inside itself, and
  never inside a place that is inside it (no loops).
- A pair is stored ONCE, on one of the two places (the part keeps its
  `inside`; a one-way way sits where it starts), and reads from both
  ends: the kitchens list the cellar as their part, stairs down. Never
  write the mirror relation on the other file.
- Saving can MOVE a place: `part_of`/`contains` put the part inside the
  whole (out of where it was), `same_place` moves the other location into
  `in`, and `separate`, `other` and `unlink` take them out of each
  other. Run `craft meta get` on both afterwards.

`craft meta get <location>` prints `linkedLocations` (every pair, read
from both ends, in the shape `link` accepts) and `relations` (only what
this file stores). Stored, a location's `$craft.metadata.relations` is a
list of `{ "id", "type": "inside" | "connects" | "other", "to": "<referenceId>", … }`
— `inside` (with an optional `level`: floor order, 0 the ground floor),
`connects` (a way: the exit fields, no `to` = out to the place it is
inside) and `other` (`label`: the words). Use `craft meta link` rather
than editing it by hand: the command writes the pair to the right place.

## Transports

A ship, airship, caravan or train the party can board is a location with
a transport. Its own map is the deck plan; where it is now stays a pin on
the map of wherever it is, and the GM moves that pin as it travels.

```
craft meta set-transport "Places/The Sea Wyrm.location.json" --file ship.json
craft meta set-transport "Places/The Sea Wyrm.location.json" --clear
```

```json
{ "kind": "ship", "travels": "water", "speed": { "value": 6, "unit": "knots" }, "capacity": 40 }
```

- `kind`: ship, boat, airship, wagon, caravan, carriage, train, car, bus,
  plane, mount, other.
- `travels`: water, land, air, rail, underground, space (a ship can't take
  a road). `speed.unit`: mph, km/h or knots. `capacity`: people aboard.
- Its decks are linked locations: each deck `part_of` the ship, with
  ladders or stairs between them, so they go wherever the ship goes.

## Which to use

- The way sits at a spot on a drawn map (the door in a floor plan, the
  town gate) → an exit.
- No map, or the place isn't on the picture (a cellar, the floors of a
  tower, a realm bridged by the Bifrost) → a linked location.
- Both is fine: an exit on the kitchens' map and the cellar `part_of`
  the kitchens.
