# Making a Project Playable

Content makes a project readable; instructions, state, and playability metadata
make it PLAYABLE.

## How the GM runs the game

The Game Master is an AI agent operating much like a human GM with a binder of
notes: it follows the world's GM instructions, receives relevant project
context, and reads or writes files as play changes the fiction. Craft does NOT
silently provide a universal memory system, quest tracker, combat engine,
relationship model, or campaign state. If the game needs one, the project must
define all four pieces:

1. **Rules** — GM instructions say when and how the mechanic operates.
2. **State model** — a schema defines the fields the mechanic can read/write.
3. **Current state** — files hold the actual quest, memory, encounter, clock,
   relationship, inventory, or other changing data.
4. **Context strategy** — visibility and instruction triggers put the right
   rules/state in front of the GM at the right moment.

For example, a quest system needs more than quest prose: a quest schema might
define status and objectives, quest files hold their current values, and GM
instructions say which events advance them and when to update those files.
Design every custom mechanic through this same rules + schema + files + context
loop. If one piece is missing, the mechanic is only documentation, not a
reliable part of play.

## Which GM runs this world

Where the GM's rules live depends on the engine, `settings.gmEngine` in
`.craft/project.json`:

- **No `gmEngine` key — the original GM (v1).** Its rules are
  `gm_instructions` files, always-on or triggered (below), and pinning
  decides what it holds in context. Everything in this topic applies.
- **`"gmEngine": "v2"` — GM Harness 2.** Its rules are
  `settings.gameOptions`: world instructions, player options, and
  situational triggers. It keeps memory, the scene, turn order, clocks and
  objectives itself, so the rules + schema + files loop above is for the
  world's own mechanics only, never for memory. What a world keeps count
  of for the player to watch (Heat, a chase, standing with factions) is a
  tracker in the GM's current state, defined in `settings.gameOptions` too, not
  a file. It ignores
  `gm_instructions` files (keep them: v1 games still read them) and
  reads any visible file when it needs it. Read the
  upgrading-to-gm-harness-2 topic before writing its rules, and skip "GM
  instructions on v1" below.

Upgrading a world from v1 is done here: translate its instruction files
into `settings.gameOptions` and set `"gmEngine": "v2"` in the same push.
The upgrading-to-gm-harness-2 topic has the steps.

## Starting the experience

When the user presses Play, the game start decides where they are, who they can
be, and what the GM says first. Its app-managed metadata is set with
`craft meta` (files must be pushed first; the CLI advances the sync ledger
automatically):

```
craft meta get "Game Starts/Opening Night.game-start.md"
craft meta set-game-start "Game Starts/Opening Night.game-start.md" --file gs.json
craft meta set-playable "Heroes/Mira.character.json" true
craft meta set-gm-trigger "GM/Toll Rules.gm-instructions.md" --file trigger.json
```

(`set-gm-trigger` is for GM v1's instruction files only; GM v2's triggers
live in `settings.gameOptions`.)

These `craft meta` commands need the CLI. Working without it: complete
the content files (the game-start body, characters, GM instructions), then
list the remaining playability steps — game start config, playable flags,
triggers — for the user to finish in the app.

## Game start configuration

The game start FILE's markdown body is GM context; the metadata is the
machine config. Reference every file by workspace path (preferred; slugs
and plain file names also resolve) — never write ids; the CLI hydrates
links (and derives beat ids and the plain-text `message` projection) on
save:

```json
{
  "startingLocation": "Places/Last Chime.location.json",
  "startingCharacters": ["Heroes/Mira.character.json", "Heroes/Brann.character.json"],
  "opening": {
    "kind": "static",
    "beats": [
      { "speaker": "gm", "text": "The last bell cracked at dusk. Rain hammers the shutters." },
      { "speaker": "Heroes/Mira.character.json", "expression": "wary", "text": "Told you the bell would crack." }
    ]
  },
  "characterOpenings": [
    {
      "character": "Heroes/Brann.character.json",
      "opening": {
        "kind": "static",
        "beats": [
          { "speaker": "gm", "text": "You are last to the chapel, boots soaked through." }
        ]
      }
    }
  ],
  "openingInstructions": "Open quiet and personal; introduce the Hush by absence, not exposition.",
  "currentState": {
    "people": [
      { "character": "People/Sister Vey.character.json", "side": "ally", "area": "Nave" }
    ],
    "purpose": "Learn why the bell cracked and who rang it last.",
    "objectives": ["Find out who rang the last bell"],
    "trackers": [
      { "key": "time", "on": true, "fields": [{ "kind": "text", "text": "Dusk, the night the bell cracked" }] }
    ]
  },
  "gmNotes": "Sister Vey keeps the chapel and rang the bell at dusk; she hides that she heard someone on the tower stair. The chapel: nave, vestry with the bell-rope, a stair to the tower.",
  "displayOrder": 0
}
```

- `startingLocation` — where play begins; should be a location that is
  ON the map.
- `startingCharacters` — the pregens offered to the player. Each must
  ALSO be flagged `craft meta set-playable <path> true` or it cannot be
  chosen in the app.
- `opening` — how the first turn opens. Prefer `"kind": "static"` with
  `beats`: each beat is one line of the scripted opening — `"speaker":
  "gm"` for narration, or a character link for a dialogue card with that
  character's portrait (in chat and the visual-novel story view). It plays
  the moment the player picks a character. The alternative,
  `{ "kind": "dynamic", "prompt": "…" }`, has the GM improvise from your
  prompt instead — fresh every game, but the player waits for the GM's
  first generation. Write openings in the world's voice; the template
  "Welcome to the game…" is the tell of an unfinished project.
- Each beat takes an optional `expression` — a one-word delivery cue from
  the speaker's expression states (the character file type's `states`, or
  the default set: neutral, happy, wary, angry, …). It swaps in the
  matching portrait variation in the story view; an unknown cue falls back
  to the base portrait.
- `characterOpenings` — optional per-character overrides of `opening`,
  played when the player picks that `character`. The top-level
  `opening` stays the required fallback for everyone else. Overrides
  should always be `static` beat openings — the app edits them as
  scripted only.
- `openingInstructions` — private direction for GM Harness V1's first
  turn. GM Harness V2 does not read it: use `gmNotes` and `currentState`.
- `currentState` — what GM Harness V2's current state holds as the game opens.
  Characters with a token on the starting location's map are there already;
  `people` adds others or sets how map characters start: `character`
  (a link), optional `side` (`ally`, `enemy`, `neutral`),
  `area` (a map area's name, else beside the player), `hidden`
  (`{ "how": "unseen" | "sensed" | "disguised", "as": "…" }`) and
  `note` (the GM's alone). `purpose` is what the opening scene is for,
  `objectives` what the party works toward from the first turn.
  `trackers` sets how the world's trackers start in this game, each by
  its key (the `key` of the tracker's definition in
  `settings.gameOptions`): `{ "key": "supplies", "on": true, "fields":
  [{ "kind": "count", "value": 2 }] }` puts one in the current state from
  the start with these values (its fields in the definition's order and
  kinds), `{ "key": "heat", "on": false }` keeps one the world starts
  with every game off in this one. Leave a tracker out to start it as the
  world does. The in-game time is Craft's Time tracker (key `time`),
  always in the current state: its text field is the time the game opens at, in
  the world's own form; leave it out for the GM to say the time. The
  objectives, a fight's turn order and a tracker kept for each person
  can't be set here.
- `gmNotes` — private facts GM Harness V2 reads on its first turn instead
  of looking them up, so the opening narrates sooner: who the people at
  the start are and what they want, what the place holds, what is going
  on. The player never sees them.

A finished project has at least one COMPLETE game start: location +
characters + opening all set, referencing real, pushed files.

## GM instructions on v1: files and triggers

This section is for GM v1 worlds (no `settings.gmEngine` key). On GM v2
these files do nothing: write the rules in `settings.gameOptions`
(upgrading-to-gm-harness-2 topic), whose always-on and situational instructions
replace the two kinds below, and leave existing files in place.

GM instruction files (the "gm_instructions" designation) come in two
kinds:

- **Always-on** (no trigger metadata): world rules the GM should always
  hold — system mechanics, tone, table rules. Body length is unlimited.
  Most projects want ONE of these carrying the core rules — and it is
  only actually standing context if it is PINNED (see context visibility
  below).
- **Triggered** (`set-gm-trigger`): fired at specific moments. LIMITS:
  name ≤ 120 chars, body ≤ 500 chars — write these as tight directives.
  - `{"triggerKind": "narrative", "triggerPhases": ["post_player_message"]}`
    — evaluated after each player message (or `post_gm_message`).
  - `{"triggerKind": "tool", "toolEvents": ["update"], "fileTypeSlugs": ["character"]}`
    — fired when the GM reads/creates/updates/deletes files of the named
    types (e.g. "when a character sheet changes, check the Hush Marks").
  - `--clear` removes the trigger, making the rule always-on again.

## Context visibility — what the GM sees by default

On GM v2 only `hidden` matters: v2 reads any other file whole when it
needs it, and `pinned` and `partial` mean nothing to it (the app gives a
v2 world's files a Visible/Hidden toggle). Everything below about pinning
is GM v1's.

Every file resolves a context visibility — its own setting when the app
configured one, else its file type's default:

- `pinned` — full content always in GM context. A pinned
  `gm_instructions` file is the GM's STANDING instructions; a pinned
  file of any other type (a party tracker, a clock, a quest log) is kept
  fresh in context automatically — the right setting for mutable play
  state.
- `partial` — name + description sit in baseline context; the GM reads
  the body on demand. For things the GM should know exist.
- `searchable` — the default: not in context, found via file search
  when relevant. Right for bulk reference (rules chapters, catalogs).
- `hidden` — invisible to GM context and search.

Keep the pinned set SMALL (progressive disclosure): one short core-rules
instruction plus pointers to searchable reference files beats six pinned
chapters crowding the context window.

A file's own visibility travels in its `$craft` block as
`settings.contextVisibility` (the top-level `"$craft"` key of a JSON
file, or `$craft` in a markdown file's frontmatter): set it there and
`craft push`. A file without one resolves to its type's default
(`searchable` unless the app configured otherwise; a type's default is
set in the app only). Files created from the app's GM instructions panel
arrive pinned; a file pushed from here has no setting until you write
one, so a core-rules file authored here is not standing context until
its block says `"pinned"`.

## Letting players make their OWN character

Pregens cover the quick start; a finished campaign project also carries a
CREATION WIZARD on the character type — `creationFlow` in
`.craft/file-types/character.json`, a flow manifest whose answers feed
AI character generation. See the authoring-flows topic for the format and
what a good wizard asks. Push with `--include-types`.

## The Game Master

Set `settings.gm` in `.craft/project.json` (name, image, voiceId); see
configuring-your-project. The GM's tone is one of its always-on rules
(item 4 below), and it should sound like the world, not like a default
assistant.

## Playability checklist

1. Each important mechanic has GM rules, schema-backed state files, explicit
   update behavior, and an intentional context/trigger strategy. On GM
   Harness 2, no memory or tracker files: it keeps those itself.
2. One complete game start (location + playable characters + opening +
   openingInstructions, plus a currentState (the current state it opens
   with) and gmNotes for GM Harness V2), `displayOrder` set when there are several.
3. Every offered character flagged playable; their sheets are finished
   records, not stubs.
4. GM rules where the engine reads them (`settings.gmEngine`). v1: core
   rules in an always-on GM instruction file, 1–3 triggered rules for the
   moments that define the system (short and imperative). v2: as few
   always-on world instructions in `settings.gameOptions` as possible
   (the premise, the core roll, the tone), every other rule a situational
   instruction for its moment, player options where players would want a
   rule differently (upgrading-to-gm-harness-2 topic).
5. GM name and voice set; opening references the map's starting location.
6. v1: visibility set in each file's `$craft` block — core rules pinned,
   trackers pinned or partial. v2: nothing to pin; only files the GM must
   never read (author notes, out-of-game docs, a v1 world's old trackers)
   need `hidden`.
7. `craft meta get` on each configured file to verify what stuck.
