# Upgrading to GM Harness 2

GM Harness 2 is Craft's new Game Master (the app calls it GM Harness V2;
`.craft/project.json` says `"gmEngine": "v2"`). It keeps its own
memory, the scene, the map, turn order, clocks, objectives and character
sheets. Most of what made a world playable under the original GM (memory
files, tracker files, pinned rule files, triggered instruction files, long
procedural rules) is now built in, and leftovers make the new GM worse,
not safer. It also reads its instructions from a different place.

This topic overrides the parts of making-it-playable that describe the
original GM: building memory, quest and tracker systems out of files, GM
instruction files and their triggers, and context visibility.

## Which GM runs this world

Look at `settings.gmEngine` in `.craft/project.json`:

- **No `gmEngine` key**: the original GM. It reads the world's
  `.gm-instructions.md` files. `settings.gameOptions` is stored and
  pushed but does nothing in play until the world is upgraded.
- **`"v2"`**: GM Harness 2. It reads `settings.gameOptions` and ignores
  instruction files. The upgrade is done; see "Already on GM Harness 2".

## Before you change anything

- **You do the translation.** The upgrade is an edit to the workspace:
  you read the old instructions, write `settings.gameOptions`, and set
  `"gmEngine": "v2"` in the same edit, so one push switches the world
  with its new rules. Creators can also upgrade from the app (a banner on
  the world, or Game Master settings), which translates for them; you
  don't run that.
- **Players get GM Harness 2 only from a published version.** New games
  start from the latest published version and take its engine, so until
  a v2 version is published, players keep the original GM, and so does a
  game the user starts to playtest. A world that has never been published
  is the exception: its games start from the live world, so they switch
  as soon as you push.
- **Games in progress stay on the original GM** for life, and it still
  reads the world's old instruction, tracker and memory files. When a
  game's owner syncs a newer version into it, a deletion they accept
  removes that file from the game. So hide the old tracker and memory
  files instead ("Hide the old bookkeeping files" below), and delete one
  only when the user asks.
- **Old `.gm-instructions.md` files are ignored by GM Harness 2.** The app
  marks them Deprecated. Leave them in place unless the user asks you to
  remove them. A rule you carry over goes into `settings.gameOptions`,
  never into a new instruction file. `craft meta set-gm-trigger` only
  affects the original GM.

## Upgrading a world

1. **Check `settings.gmEngine`.** No key: carry on. `"v2"`: see
   "Already on GM Harness 2".
2. **Ask about a rollback point.** Offer to publish the world as it is
   first (`craft publish -m "Last version on the original GM"`).
   Restoring that version from the app's version history brings the
   original GM back with that version's files and settings. Ask before
   this and every other publish; publish only on a yes.
3. **Translate.** Read every `.gm-instructions.md` file, `settings.gm`,
   and the rule files the instructions name (the core roll, combat,
   conditions). Write `settings.gameOptions` by "Where each old
   instruction file goes", "Remove" and "Rewrite" below. Where an
   instruction disagrees with a rule file, the rule file wins: write it
   that way, and tell the user each disagreement.
4. **Switch the engine in the same edit:** add `"gmEngine": "v2"` to
   `settings`, next to `gameOptions`. The push that carries your
   translation switches the world. A push never goes back: removing the
   key or writing `"v1"` on a v2 world is refused.
5. **Fix game starts, then file types and files** ("Game starts" and
   "Files and file types" below). Memory fields go with `craft push
   --include-types`; tracker and memory files are hidden, not deleted.
6. **`craft check`, then `craft push`.** `craft check` warns about
   anything over the limits, and about `"gmEngine": "v2"` with no
   instructions of the world's own.
7. **The user reviews in the app's GM Instructions tab.** Its **System
   prompt** page shows what the GM is given every turn for a set of player
   picks (after Craft's own text on who the GM is, which it doesn't show).
   Its **Instruction tester** takes a player's move and shows which
   instructions, trackers and player options it would bring in. Until a v2
   version is published, these are the place to try the new rules. When a
   test game misses an instruction, try it in a fresh game (the GM copies
   its own recent replies) and retry the turn a couple of times, then on a
   stronger model: if a stronger model follows it, the instruction is
   fine; if it misses too, rewrite the instruction.
8. **Publish a v2 version once the user agrees** (`craft publish -m
   "…"`). Only then do new games start on GM Harness 2.

### Already on GM Harness 2

Nothing to switch. Run `craft pull` first, so you have what the app's
upgrade translated, then refine `settings.gameOptions` by the rules
below, fix game starts and files as above, and ask whether a v2 version
has been published. A new world made from scratch (`craft new` without a
system) starts on GM Harness 2; one built on a system, and a clone, keeps
its source's engine.

## Where the instructions live

`settings.gameOptions` in `.craft/project.json`. The file is pushed whole,
so edit it in place and leave every other key as pulled (see
configuring-your-project). `null` means the world has only Craft's
defaults; to start your own, write an object with empty `instructions` and
`options` arrays. A complete example:

```json
{
  "sections": ["World", "Rules"],
  "instructions": [
    {
      "name": "Premise",
      "when": "always",
      "section": "World",
      "body": "The Hollow Coast, forty years after the sea rose. Salvage crews pick through drowned towns for what still works, and the Harbor Compact rules what is left of the shore. The tone is grim and practical; hope is earned in small things."
    },
    {
      "name": "The core roll",
      "when": "always",
      "section": "Rules",
      "body": "Roll 2d6 and add the stat that fits. 10 or more: it works. 7 to 9: it works at a cost the GM names. 6 or less: it fails, and things get worse."
    },
    {
      "when": "situational",
      "trigger": "When a character drops to 0 HP",
      "section": "Rules",
      "body": "They fall unconscious and make a death save at the start of each of their turns: roll 2d6, and 7 or higher is a success. Three successes and they stabilize; three failures and they die."
    }
  ],
  "options": [
    {
      "key": "salvage",
      "label": "Salvage",
      "description": "How hard it is to find what the crew needs.",
      "kind": "one",
      "default": ["scarce"],
      "choices": [
        {
          "key": "scarce",
          "label": "Scarce",
          "description": "Every find is a fight.",
          "instructions": [
            {
              "when": "always",
              "body": "Useful salvage is rare. A good find is the point of a whole scene, and someone else always wants it."
            }
          ]
        },
        {
          "key": "plentiful",
          "label": "Plentiful",
          "description": "More treasure, less hunger.",
          "instructions": [
            {
              "when": "always",
              "body": "Salvage turns up often. Keep the danger in the places and the people, not in going without."
            }
          ]
        }
      ]
    }
  ],
  "platform": {
    "instructions": {
      "filling-gaps": { "off": true }
    }
  }
}
```

- **`instructions`** are the world's own, in every game.
  - `when`: `"always"` puts it in the GM's prompt every turn;
    `"situational"` brings it in only when its moment comes up.
  - `name`: the heading. Required on an always-on instruction outside a
    choice.
  - `trigger`: required on a situational one. Write the moment, starting
    with "When…".
  - `section`: optional; `sections` lists the headings, in order.
    Craft's defaults sit in "Narration", "Rules", "World": use those where they fit.
- **`options`** are player options, asked when a game starts.
  - `kind`: `"one"` (pick one; `default` names exactly one choice),
    `"several"` (pick any) or `"toggle"` (on or off; exactly one choice).
  - Keys are lowercase letters, digits and hyphens. Each choice brings its
    own `instructions`, in the same shape (`name` is optional there:
    an untitled one takes its option's or choice's label).
  - Never change a `key` once games exist: a game's picks point at keys.
    Rename labels instead.
- **`platform`** records what the world changed of Craft's defaults.
  `{ "off": true }` under an instruction id or option key removes it. To
  reword one of Craft's, ask the user to edit it in the app's GM
  Instructions tab: a reworded default records which Craft revision it was
  based on. Entries such as `{ "platform": "combat-tracker" }` in
  `instructions`, or `{ "key": "platform-response-length", "platform":
  true }` in `options`, only place Craft's items in the list. Leave them
  as pulled.

Use exactly these field names: a key the format doesn't know is dropped,
and a write that holds one is flagged with its path.

### Trackers

A tracker is something the GM keeps in the current state for the player to watch
change: a Heat clock, a chase, the party's supplies, standing with each
faction. When the user asks for one, or the world keeps count of something
that is no one sheet's, write a tracker: never a file, and never a plain
rule ("Track Heat from 0 to 6"), which puts nothing in the current state.

A tracker is an instruction with a `tracker`:

- `name` is the tracker's name, the same as `tracker.start.name`, and
  `when` is `"always"`. No `section`: trackers are listed apart.
- `body` is its rules: how it moves, what each value or threshold brings,
  when it ends. The GM reads them beside the tracker, only while it is in
  the current state. The prompt keeps one line for it every turn (its name,
  when it starts and its `when`). None of it counts toward the always-on
  limit.
- In a player option's choice, it is used only in games that pick that
  choice; with the key of one of the world's trackers, it replaces that one
  in those games.

```json
{
  "instructions": [
    {
      "name": "Heat",
      "when": "always",
      "body": "Heat +1 for each loud job or witness left alive, +2 for a killing in the open. At 6 the Watch raids the hideout, then Heat drops to 2.",
      "tracker": {
        "key": "heat",
        "start": {
          "name": "Heat",
          "lasts": "always",
          "fields": [
            { "kind": "clock", "value": 0, "min": 0, "max": 6, "tone": "bad", "at": { "6": "The Watch raids the hideout" } }
          ]
        },
        "starts": "game-start",
        "when": "the crew draws attention: a loud job, a witness left alive"
      }
    },
    {
      "name": "Chase",
      "when": "always",
      "body": "Each exchange, whoever wins the roll moves the chase one step their way. At Away the quarry is gone; at Caught the pursuers have them. End it when either end is reached.",
      "tracker": {
        "key": "chase",
        "start": {
          "name": "Chase",
          "fields": [
            { "kind": "scale", "value": 0, "min": -3, "max": 3, "ends": ["Caught", "Away"] }
          ]
        },
        "starts": "when-started",
        "trigger": "someone flees or gives chase",
        "when": "someone in the chase gains or loses ground"
      }
    },
    {
      "name": "Factions",
      "when": "always",
      "body": "A faction's Standing goes up when the party helps it and down when they cross it. At -3 it sends people after them; at +3 it offers a safehouse.",
      "tracker": {
        "key": "factions",
        "start": {
          "name": "Factions",
          "lasts": "game",
          "fields": [
            {
              "kind": "records",
              "entries": [],
              "each": [
                { "kind": "scale", "label": "Standing", "value": 0, "min": -3, "max": 3, "ends": ["Enemies", "Allies"] }
              ]
            }
          ]
        },
        "starts": "first-change",
        "when": "the party helps or crosses a faction"
      }
    }
  ],
  "options": []
}
```

The `tracker`:

- `key`: lowercase letters, digits and hyphens, unique among the
  world's trackers. Never change it once games exist: a tracker in a
  game's current state points at it.
- `starts`: `"game-start"` (in the current state when a game begins),
  `"first-change"` (it appears the first time the GM changes it: a list
  of clues, standing with factions met along the way) or
  `"when-started"` (the GM starts one when its moment comes: a chase, a
  ritual).
- `trigger`: required for a `"when-started"` tracker, and only there:
  what starts it, as the end of "Starts when …" (`"someone flees or gives
  chase"`), at most 200 characters. Craft checks for it before each
  player turn while the tracker is out of the current state, and tells the GM how to
  start it.
- `when`: required, unless `updates` is `"every-turn"`. What
  updates it, as the end of "Update it when …"
  (`"the party is seen while sneaking"`), with no "When" before it, at
  most 300 characters. The GM reads it every turn, with an example
  change Craft writes from the fields, and Craft checks for it after each
  reply.
- `updates`: leave it out for a tracker updated on its `when`.
  `"every-turn"` is for one the GM updates in every reply, whatever
  happens (breaking news, market prices): it has no `when` (a save with
  one is refused), Craft tells the GM each turn to update it, and reminds
  it after a reply that didn't. Never write "every turn" as a `when`:
  Craft checks a trigger against what happened in the story, and "every
  turn" never happens. Write no example change here, and no JSON anywhere: the GM says
  changes in words and its update tool keeps the tracker's data. In
  `body`, say how the tracker moves in plain words ("+1 when the crew
  is seen"); when a rule needs a change written out, write it as the GM
  sends it, the tracker's name then the change ("Heat": "End").
- `start.name`: at most 60 characters.
- `start.lasts`, inside `start`: leave it out and the tracker ends when
  the party leaves the place (a chase, a fight); `"game"` keeps it until
  the GM ends it; `"always"` keeps it in the current state from the game's start
  to its end, and nothing ends it (a ship's hull, the season), so its
  `starts` is `"game-start"`. A `"game-start"` tracker with no
  `lasts` is gone for good after the first place: give it `"game"` or
  `"always"`.
- `start.hidden`: `true` keeps it from the players until the GM reveals
  it.
- `start.fields`, 1 to 8; with more than one, each needs a different
  `label` (at most 40 characters):
  - `count`: a number (`value`, optional `min`, `max`, and `display`:
    `"number"`, `"bar"` or `"pips"`): supplies, days left, morale.
  - `clock`: trouble filling a segment at a time (`value`, `min` 0,
    `max` of 2 to 12 segments).
  - `scale`: a tug of war (`value`, `min` below `max`, at most
    21 steps, and `ends` naming the low and high ends).
  - `text`: a line of words that changes (`text`): the weather, what
    lies ahead.
  - `list`: steps or goals (`items`, each `{ "text", "state": "open" }`,
    up to 16); usually empty, since the GM adds them as the story gives
    them.
  - `order`: who acts in turn (`entries` empty).
  - `records`: a row per person, faction or place (`entries` empty, up to
    12 once the GM adds them), each starting with the fields in `each`
    (count, clock, scale, list or text, up to 4, each with a
    `label`).
  - `links`: a pair of people, factions or places with fields between
    them, like bonds or how two factions stand (`entries` empty, `each`
    as for `records`). A pair has no direction: say one in a field's label.
  - Numbers take `tone` (`"good"` or `"bad"` to fill) and `at` (what
    reaching a value brings: `{ "6": "The Watch raids the hideout" }`). An
    `order` or `records` field's `show` names stats from the sheets to
    show beside each person (`["HP"]`), or `"where"`: never copy a sheet's
    values into a tracker.
  - Anything a tracker starts with (an item, a row, an entry, each end of
    a pair) needs a name.
- `perPerson`: `true` for one per person, like death saves.
- Leave out `watch`: Craft makes the check after each reply from
  `when` and `trigger`.

Every save checks a tracker the way the app's tracker form does: a name,
what updates it (`when`), what starts one on a trigger, its rules
(`body`), its fields' numbers and labels, a name for everything it
starts with, and a key no other tracker of the list has. A rejected write
says which.

Craft's own trackers are Combat tracker (`combat-tracker`, key `fight`), Objectives (`objectives-tracker`, key `objectives`), Party (`party-tracker`, key `party`), Death saves (`death-saves-tracker`, key `death-saves`) and Time (`time-tracker`, key `time`). A world tracker with the key of one of
Craft's replaces it (`fight` for a world's own Combat); to drop one
without a replacement, turn it off in `platform`. Code moves the time
(`time`), so a world rewords its rules or turns it off, and never defines
its own `time`. A world's tracker files from the original GM (Heat,
faction standing, supplies) become trackers, not files.

In the app, the GM Instructions tab's Trackers tab edits the same thing,
with a preview of each tracker as it looks in play and templates to start
from (Alarm, Chase, Supplies, Wanted and others).

### Craft's defaults

Every world gets these unless it turns them off. They are the stylistic
part of the GM's system prompt (how it narrates, when it rolls), and the
creator can reword any of them in the GM Instructions tab. Don't write
your own copies.

| Id | Name | When |
|---|---|---|
| `player-character` | Player agency | always |
| `a-good-turn` | A good turn | always |
| `places-not-squares` | Places, not squares | always |
| `travel-times` | Journeys, not timetables | always |
| `when-to-roll` | When to roll | always |
| `combat-tracker` | Combat tracker | always |
| `objectives-tracker` | Objectives | always |
| `party-tracker` | Party | always |
| `death-saves-tracker` | Death saves | always |
| `time-tracker` | Time | always |
| `player-moves` | Player moves | situational |
| `filling-gaps` | Filling gaps | always |
| `platform-response-length` | Response length | player option |
| `platform-story-pacing` | Story pacing | player option |
| `platform-long-distance-travel` | Long-distance travel | player option |

### Limits

The push checks only the structure of `gameOptions`. The limits below are
enforced when the app saves GM Instructions: a world pushed over one still
plays, but the creator can't save any edit in the GM Instructions tab until
it's trimmed. `craft check` warns about each. Stay inside them.

- **Always-on text:** at most 10,000 characters (names and bodies),
  counted for the largest game a player can pick: Craft's defaults (about
  4,300), the world's own, the longest choice of each pick-one option and
  every choice of the others. Trackers don't count.
- **Situational instructions:** at most 100 in that same largest game.
- **Instructions:** 200 in all.
- **Bodies:** up to 2,000 characters for an always-on instruction and
  1,000 for a situational one.
- **Names and triggers:** 120 characters each.
- **Player options:** 9, including Craft's 3, with up to 8 choices each.
- **Options and choices:** labels up to 60 characters, descriptions up
  to 200, keys up to 60.
- **Sections:** up to 30, each name up to 40 characters.

## Where each old instruction file goes

- **Default files** (always in context, often pinned): always-on only if
  the GM needs them on almost every turn; otherwise a situational
  instruction for each moment they cover.
- **Narrative-triggered files:** situational, with a trigger that names
  the moment the old one fired on.
- **Tool-triggered files** (fired when the GM read, created or updated a
  kind of file): a situational instruction about that moment in the story
  ("When a new character joins the party", "When someone levels up"), or
  drop it when it is only about the file or the tool.
- **Several files about one moment:** one situational instruction.

## Remove: GM Harness 2 does these itself

Anything about how Craft itself works goes. Keep what is specific to this
world and its game mechanics.

- **Memory systems:** memory files, archives, chapter summaries,
  "re-read X before every reply", and memory fields on file types. Memory
  is built in: the GM keeps a running summary of the game and looks up
  anything earlier. Don't store GM memory in files.
- **Tracker files:** scene trackers, initiative and encounter trackers,
  quest logs, clocks kept in files. The GM keeps the scene, turn order,
  clocks, countdowns, death saves and the party's objectives in the current state.
  A leftover tracker file is worse than none: the GM has written changes
  into tracker files instead of the sheets they belonged on.
- **Bookkeeping:** "subtract the gold after every purchase", "write the
  new HP to the sheet", lists of fields to update. The GM records changes
  as it narrates them, and a recorder catches anything it missed.
- **Map and tokens:** moving tokens, pinning new places, removing the
  dead. The map follows the story.
- **Roll procedure:** "never resolve a roll yourself", "wait for the
  player to roll". Asking for a roll ends the GM's turn on its own.
- **Output formats:** beats, `spec` blocks, JSON, headings, scene titles,
  lists of choices, dialogue formatting. The GM writes plain prose with
  `**Name** (mood):` speaker lines, and the play screen and narrator
  voices depend on that format.
- **Tool procedures:** bash, shell commands, file paths, which tool to call
  and when. There is no shell, and the GM knows its own tools: looking
  something up before narrating it, creating a file for something new,
  updating the one that exists. Describe the rule, not the bookkeeping.
- **Hidden-note formats:** out-of-character reasoning blocks, "think step
  by step" scaffolding. The GM keeps its own notes.
- **Context tricks:** pinning, partial visibility and "always keep file X
  in mind". On GM Harness 2 a file is visible or hidden (pinned and
  partial count as visible), and the GM looks files up as it needs them.
- **Read-everything rules:** "every turn, read every lore page", "audit
  every file before you reply", "check the world bible before narrating".
  They cause context rot: the GM spends its attention calling tools
  instead of telling the story, and it already looks up what a turn
  needs.

## Rewrite

- **Translate everything else.** When old instructions come over, cut
  what the Remove list covers and copies of Craft's defaults, and carry
  over every other rule about the world, its mechanics, its tone and how
  scenes play, rewritten for its moment. Don't leave a rule out to keep
  the list short.
- **As few always-on instructions as possible.** Everything always-on is
  in the GM's prompt on every turn, and the more there is, the less well
  it follows any one part. A situational instruction costs nothing on the
  turns it doesn't apply to.
- **Situational unless every turn needs it.** Make an instruction
  always-on only when the GM needs it on almost every turn, whatever
  happens: the premise of the world, the GM's voice and tone, the core game
  system (its core roll and dice), and the few rules no turn may break
  (content limits, what people can know). Anything that applies
  when something happens (a fight, a secret, someone speaking, a purchase,
  a rest, a journey) is situational, even when that moment comes up often.
  When in doubt, situational. Variants are player options, and reference
  rules stay in files the GM can look up.
- **Split mixed instructions.** When an always-on rule carries a part for
  one moment (how NPCs speak, how a secret is written down), keep the
  every-turn sentence always-on and move the rest into that moment's
  situational instruction.
- **Aim for half the room Craft's defaults leave:** they take about
  4,300 of the 10,000-character limit, so keep the world's own
  always-on text under about 2,900 characters.
- **Name your dice.** Put the core roll and its dice in an always-on
  instruction. If the always-on rules name dice and never a d20, the GM's
  dice tools refuse to roll a d20. Writing "never roll a d20" names a d20
  and switches that off, so don't mention a d20 at all. If the system rolls
  under a number or counts successes, say how to read the result.
- **Describe an unusual calendar.** The GM keeps the in-game time in
  Craft's Time tracker (`time-tracker`, key `time`). If the world doesn't
  use an ordinary calendar, describe it in an always-on instruction of the
  world's own named "Calendar": its months, weeks and holidays, and how to
  write the date ("the 3rd of Frostfall, dusk"). For a world that keeps no time, turn the tracker off
  in `platform`.
- **Break rule dumps into situational instructions,** one moment each.
  The rules check decides from the trigger and the first 240 characters of
  the body, and brings in at most 5 instructions a turn, so put the ruling
  first and keep each to one moment. It reads the player's move, the GM's last reply and one line on where
  the scene is and who is in it, not the sheets or the trackers: write the
  trigger as something that happens in the story ("When a fight starts"),
  never something only a sheet or a tracker shows ("While the Heat tracker
  is up"). A tracker's own
  rules go in the tracker. A long "Combat rules" block becomes
  "When a character drops to 0 HP", "When someone grapples", "When a fight
  starts", and so on:

```json fragment
{
  "when": "situational",
  "trigger": "When a character drops to 0 HP",
  "body": "They fall unconscious and make a death save at the start of each of their turns: roll 2d6, and 7 or higher is a success. Three successes and they stabilize; three failures and they die."
}
```

- **Keep triggers specific.** A trigger that matches most turns ("When
  anything happens in combat", "When the player acts") takes one of the 5
  places every turn from an instruction that was needed. If it truly
  applies to every turn, it is always-on.
- **Split moments that can't happen together; merge ones that fire
  together.** Separate melee, ranged and spell attacks rather than one
  "When someone attacks". But when one moment would bring in several
  instructions at once ("A feat is used", "A character uses a Mana Feat",
  "A feat needs a saving throw"), make them one ("When someone uses a
  feat"): apart, they take three places and can disagree.
- **Leave reference rules in files.** A spell list or a gear table stays a
  file. A situational instruction can name the file for the moment it
  matters ("When someone casts a spell: the Spell List file has every
  spell's cost") instead of copying it.
- **Turn variants into player options.** "In hard mode…", optional house
  rules and tone switches become an option whose choices carry the
  instructions, so each game holds only the rules the players picked. The
  default is what the world did before. Don't invent options the world
  never hinted at: none is fine.
- **Merge duplicates and drop restatements** of Craft's defaults. Old
  worlds often have several files for one moment; write one complete
  instruction for it.

## Wording

- **Say what to do, not only what not to.** A "don't" rule leaves the GM
  to guess what to do instead: "Keep replies to two short paragraphs",
  not "Don't be wordy".
- **Write each rule plainly, once.** No capitals or "IMPORTANT" for
  emphasis, and no rule said twice in other words: emphasis on one rule
  weakens the rest, and a rule written in two places drifts into two.
- **Few examples, and varied.** The GM copies examples closely: one
  sample of how an NPC talks and every NPC talks that way. Give two or
  three different ones, or describe the pattern.
- **No contradictions,** between instructions, with the world's rule files
  or with Craft's defaults. When two disagree, the GM guesses which was
  meant.
- **The sheets' exact field names.** If the sheet says Defense, never AC
  in one instruction and Armor in another.
- **Watch for over-correction.** A rule written to fix one bad turn
  ("Don't let the player win easily") makes the GM overdo it everywhere.
  Say what the world wants, and when.

## Files and file types

- **Memory fields:** remove them from the file types in
  `.craft/file-types/` (read refactoring-file-types first, and push with
  `--include-types`).
- **Tracker, memory and archive files:** hide them from the GM (below).
- **Example files matter more.** When the GM creates a character, place or
  item, it models the new file on the world's closest similar file. Keep
  one complete, good example of each type.

### Hide the old bookkeeping files

A v1 world often built files to do the GM's own bookkeeping: memory
systems (Campaign State, World State, GM Session Log, Campaign Log,
Memory), combat, encounter and initiative trackers, quest trackers and
logs, scene trackers and scene state, party trackers and world clocks.
GM Harness 2 keeps its own memory, and the scene, turn order and
objectives in its current state. Left visible, these files confuse it: it has
written changes into tracker files instead of the sheets they belonged
on.

- **Hide, don't delete.** In each file's `$craft` block (the top-level
  `"$craft"` key of a JSON file, or `$craft` in a markdown file's
  frontmatter), set `settings.contextVisibility` to `"hidden"` next to
  its `referenceId`, then `craft push`. The file stays in the world and
  can be shown again. Hide files one by one, not their whole type (a
  type's visibility is set in the app), so other and future files of that
  type stay visible. Delete only when the user asks.
- **Judge by the names a creator gave a whole system:** the file type's
  name, or a folder such as "World Memory", "Archive" or "Sessions".
  Never by a single file's name: "Modify Memory" is a spell.
- **Keep visible:** relationship trackers, bonds and reputations, and
  designated files (characters, locations, game starts, scripts).
- **Check before hiding many.** A tracker-named type with many files is
  usually authored content in a tracker's shape: one world's 195 "Quest
  Tracker" files are real quests with objectives and rewards, which GM
  Harness 2 still needs to read. The app's upgrade preselects a matching
  type only when it has 5 files or fewer. Before hiding a larger type, or
  anything matched only by its folder, open a few of its files and ask
  the user.
- **Move lasting world knowledge** in a file you hide (history, secrets,
  who knows what) into ordinary lore, character or location files;
  per-game state needs no file at all.
- Hiding changes the world's draft only: games in progress aren't
  changed, and new games get it once a version is published.

## Game starts

- **`openingInstructions` is never read by GM Harness 2.** Move what it
  says to the fields that are:
  - who is there and how, the in-game time, what the opening scene is for
    and the first objectives go in `currentState` (the current state
    the game opens with);
  - what the GM needs to know for its first reply (who the people are and
    what they want, what the place holds, what is going on) goes in
    `gmNotes`, which the player never sees;
  - direction for how the first scene plays goes in a dynamic opening's
    `prompt`, or into a static opening's own text.
- **A dynamic opening's `prompt`** goes to the GM as a hidden message on
  the first turn. Write it as direction for the first scene, with nothing
  about trackers, memory files, beats, JSON or tools.
- **Static openings** (`"kind": "static"` with `beats`) work as before.
- Read a game start with `craft meta get <path>`, edit the JSON, and save
  it with `craft meta set-game-start <path> --file <json>`. The shape and
  every field are in making-it-playable.

## Checklist

1. `settings.gmEngine` checked; the user asked whether to publish a
   rollback version first.
2. `settings.gameOptions` has as few always-on instructions as possible
   (the premise of the world, the GM's voice and tone, the core game
   system and the rules no turn may break), under
   about half the always-on budget; every other rule is a one-moment
   situational instruction, and variants are player options, all inside
   the limits. Disagreements with rule files told to the user.
3. Nothing in it covers memory, bookkeeping, tokens, roll procedure,
   output formats, tools, reading every file, or Craft's defaults. Triggers
   are specific, moments that fire together are one instruction, and each
   rule is said once, plainly, with the sheets' field names. What the world
   keeps count of is a tracker (`tracker` on an instruction), never a
   rule about keeping count.
4. `"gmEngine": "v2"` set in the same push as the game options.
5. No game start relies on `openingInstructions`; `currentState` and
   `gmNotes` carry what it said, and dynamic opening prompts are clean.
6. Memory fields are gone from the file types; tracker and memory files
   are hidden, and the user checked any large or folder-only group first.
7. `craft check` clean, pushed, and reviewed by the user in the GM
   Instructions tab (the System prompt page, and the Instruction tester on
   a few player moves).
8. A v2 version published, once the user agreed.
