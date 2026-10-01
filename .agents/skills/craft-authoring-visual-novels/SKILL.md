---
name: craft-authoring-visual-novels
description: "Read this before creating or editing a visual novel with `craft vn` — the story's JSON document (pages, speakers and the cast, expressions, backdrops, music, chapters, where cutouts stand on desktop and phones, pictures on pages), the create/pull/push/publish loop, and what makes a visual novel read well."
---

# Authoring Visual Novels

A visual novel is a story readers click through one page at a time: a
backdrop fills the stage, the speaker's cutout stands on it, and the line
sits in a text box below. In Craft a visual novel is ONE JSON document
owned by its author, separate from the project's files, so it has its own
commands (`craft vn`) and its own file that you edit whole.

## The loop

```
craft vn create "Lanterns Over Kestrel Bay"
craft vn assets .craft/visual-novels/lanterns-over-kestrel-bay.vn.json
# write the story into the file's "document"
craft vn check .craft/visual-novels/lanterns-over-kestrel-bay.vn.json
craft vn push .craft/visual-novels/lanterns-over-kestrel-bay.vn.json
craft vn publish .craft/visual-novels/lanterns-over-kestrel-bay.vn.json --unlisted
```

- `create` starts the story in this workspace's project, where the whole
  team sees it in the project's Visual Novels settings (`--personal`
  keeps it off the project), and writes its file into
  `.craft/visual-novels/`. For an existing story: `craft vn list`, then
  `craft vn pull <id>`.
- `check` validates the file locally; run it after every batch of edits.
  `push` runs the same checks, saves the file as the story's draft, and
  rewrites the file with what the server stored. Nothing readers see
  changes until you publish.
- `publish` freezes the draft as the version readers get. A new story is
  private; `--unlisted` shares it by link. `--public` also lists it on
  Discover: ask the user before making anything public.
- Push refuses when the story changed on the server since the file was
  pulled (the user edited it in the app, or another copy was pushed).
  Take theirs with `craft vn pull <file> --force` and redo your edit, or
  keep yours with `craft vn push <file> --force`; ask the user before
  overwriting their app edits.
- The app's editor saves its whole copy of a story. If the user has the
  story open there, ask them to close the tab (or reload it after your
  push): its next autosave would overwrite your work.
- Pull refuses to overwrite a file with edits that were never pushed
  (`--force` discards them).

Visual novels need the CLI or the app. Without the CLI, draft the story
somewhere the user can read it and tell them to build it in the app (the
project's Visual Novels settings, or Visual Novels in the sidebar); never
hand-write a `.vn.json`, since only a pulled file can be pushed.

## The file

```json fragment
{
  "$craft": {
    "kind": "visual-novel",
    "id": "01a0cc71-0341-75c4-8126-a17c294b2325",
    "remote": "https://api.craftrpgs.com",
    "baseHash": "4dd92df4-cce5-7ae3-95a6-d1457729ff9c",
    "fileHash": "4dd92df4-cce5-7ae3-95a6-d1457729ff9c"
  },
  "title": "Lanterns Over Kestrel Bay",
  "description": "A harbor oracle reads the lanterns before the storm.",
  "document": { "version": 2, "pages": [], "chapters": [], "music": [] }
}
```

Edit `title` (up to 160 characters), `description` (up to 1,000, shown
on Discover; `null` for none) and `document`. Leave `$craft` exactly
as it is: it names the story and holds the hashes that keep pull and push
from overwriting anyone's work.

## A complete document

```json
{
  "version": 2,
  "pages": [
    {
      "id": "p-001",
      "kind": "sceneHeading",
      "title": "Kestrel Bay",
      "subtitle": "The night the lanterns went out",
      "background": {
        "imageUrl": "https://media.craftrpgs.com/example/harbor-night.webp",
        "mobileImageUrl": "https://media.craftrpgs.com/example/harbor-night-tall.webp",
        "label": "The harbor at night"
      }
    },
    {
      "id": "p-002",
      "kind": "narration",
      "text": "Rain needles the harbor. Past the breakwater, the last lantern gutters."
    },
    {
      "id": "p-003",
      "kind": "dialogue",
      "text": "You came back. I told the tide you would.",
      "speaker": {
        "type": "cast",
        "castId": "oracle",
        "expressionId": "oracle-thoughtful"
      }
    },
    {
      "id": "p-004",
      "kind": "dialogue",
      "text": "Counting what?",
      "speaker": { "type": "cast", "castId": "wren" }
    },
    {
      "id": "p-005",
      "kind": "dialogue",
      "text": "*Listen.* The water is counting something.",
      "speaker": {
        "type": "cast",
        "castId": "oracle",
        "expressionId": "oracle-wary",
        "x": 62,
        "mobile": { "x": 60 }
      },
      "images": [
        {
          "id": "drowned-lantern",
          "url": "https://media.craftrpgs.com/example/lantern.webp",
          "alt": "A drowned lantern, still glowing",
          "x": 20,
          "y": 42,
          "width": 18,
          "aspectRatio": 0.75,
          "rotation": -8,
          "mobile": { "x": 22, "y": 30, "width": 30 },
          "stay": { "throughPageId": "p-006" }
        }
      ]
    },
    {
      "id": "p-006",
      "kind": "handout",
      "style": "letter",
      "title": "Harbormaster's note",
      "text": "Close the bay at dusk.\n\nDo **not** answer the bells.",
      "imageChanges": [{ "imageId": "drowned-lantern", "opacity": 0.6 }]
    },
    {
      "id": "p-007",
      "kind": "sceneHeading",
      "title": "The Bell Tower",
      "background": {
        "imageUrl": "https://media.craftrpgs.com/example/bell-tower.webp",
        "transition": "cut"
      }
    },
    {
      "id": "p-008",
      "kind": "readAloud",
      "text": "The lanterns do not go out. They are put out."
    },
    {
      "id": "p-009",
      "kind": "dialogue",
      "text": "Up here the bells never stop. Down there, they never start.",
      "speaker": { "type": "custom", "name": "Bell-ringer", "role": "character" }
    }
  ],
  "chapters": [
    { "id": "ch-harbor", "title": "The Harbor", "startPageId": "p-001" },
    { "id": "ch-tower", "title": "The Bell Tower", "startPageId": "p-007" }
  ],
  "music": [
    {
      "id": "cue-rain",
      "title": "Rain on Slate",
      "url": "https://media.craftrpgs.com/example/rain-on-slate.mp3",
      "startPageId": "p-001",
      "endPageId": "p-006",
      "volume": 0.5
    }
  ],
  "assets": {
    "speakers": [
      {
        "id": "oracle",
        "name": "The Oracle",
        "role": "gm",
        "imageUrl": "https://media.craftrpgs.com/example/oracle.webp",
        "cutoutUrl": "https://media.craftrpgs.com/example/oracle-cutout.webp",
        "placement": { "position": "right", "mobile": { "x": 62, "scale": 0.9 } },
        "expressions": [
          {
            "id": "oracle-thoughtful",
            "name": "thoughtful",
            "imageUrl": "https://media.craftrpgs.com/example/oracle-thoughtful.webp",
            "cutoutUrl": "https://media.craftrpgs.com/example/oracle-thoughtful-cutout.webp"
          },
          {
            "id": "oracle-wary",
            "name": "wary",
            "imageUrl": "https://media.craftrpgs.com/example/oracle-wary.webp",
            "cutoutUrl": "https://media.craftrpgs.com/example/oracle-wary-cutout.webp"
          }
        ]
      },
      {
        "id": "wren",
        "name": "Wren",
        "role": "player",
        "imageUrl": "https://media.craftrpgs.com/example/wren.webp",
        "cutoutUrl": "https://media.craftrpgs.com/example/wren-cutout.webp",
        "placement": { "position": "left", "mobile": { "x": 38, "scale": 0.9 } },
        "expressions": []
      }
    ],
    "backgrounds": [
      {
        "id": "bg-harbor",
        "url": "https://media.craftrpgs.com/example/harbor-night.webp",
        "label": "The harbor at night"
      },
      {
        "id": "bg-tower",
        "url": "https://media.craftrpgs.com/example/bell-tower.webp",
        "label": "The bell tower"
      }
    ],
    "music": [
      {
        "id": "track-rain",
        "title": "Rain on Slate",
        "url": "https://media.craftrpgs.com/example/rain-on-slate.mp3"
      }
    ]
  },
  "settings": { "textReveal": "word" }
}
```

## Pages

`pages` is the reading order. Every page has an `id`, a `kind` and
`text`.

| kind | what readers see | fields that matter |
|---|---|---|
| `dialogue` | someone speaks; their name and portrait head the text box | `speaker`, `text` |
| `narration` | the narrator's prose | `text`; a `speaker` puts their name on it |
| `readAloud` | boxed text for a moment that should land | `text` |
| `handout` | a document the reader holds | `title`, `text`, `style`: `letter`, `note`, `inscription` or `book` |
| `sceneHeading` | a centred title card that opens a scene | `title`, `subtitle` |
| `scene` | the stage itself, captioned along the bottom | `text` (the caption, may be empty); a `speaker` stands someone there silently |
| `video` | a clip playing over the stage | `videoUrl` (mp4 or webm), optional `mobileVideoUrl` (a clip made for a phone held upright), `text` as its caption |

- `text` is light markdown: `**bold**`, `*italics*`, and blank lines
  (`\n\n`) between paragraphs. Keep one beat per page; `check` warns
  past 500 characters.
- `background` is `{ "imageUrl": url or null, "label": ..., "transition":
  "fade" | "cut" }`. Set it ONLY on the page where the backdrop changes:
  every later page keeps it. `null` clears to a plain stage. (A hidden
  page's backdrop doesn't carry forward.) Optional `mobileImageUrl` is a
  portrait version phones held upright show instead of cropping the wide
  one (9:16 art); without it they show `imageUrl`.
- `textPosition`: `bottom` (default) or `center`. `transition` is how
  the page enters: `fade` (default), `slide` or `none`.
- `hidden: true` keeps a page in the document but out of the story.
- `audio` (generated narration) and `source` (the game message a page
  came from) are written by the app. Leave them alone and never invent
  them. Changing a page's words makes its narration stale; `check` warns,
  and the user regenerates it in the app.
- The last page is the ending: auto play stops there, and the reader
  chooses when to finish.

## Speakers and the cast

The cast is `document.assets.speakers`, and a line names its speaker by
reference:

```json fragment
{ "type": "cast", "castId": "oracle", "expressionId": "oracle-wary" }
```

The member supplies the name, portrait, cutout and voice, so changing a
member changes every line they say. A member is `{ id, name, role,
imageUrl, cutoutUrl, voiceId, placement, expressions }`:

- `imageUrl` is the portrait in the text box; `cutoutUrl` is a
  transparent image that stands on the stage.
- `role` is `gm` for the narrator (the Game Master), `player` or
  `character` for everyone else; the editor labels members by it.
- `voiceId` is the voice their lines are narrated in (ids from
  `craft voices`).
- `expressions` are `{ id, name, imageUrl, cutoutUrl }` (up to 32). A
  line wears one with `expressionId`; without it, the member's base art.
  An expression swaps both the portrait and the cutout.
- On any line: `showStandee: false` hides the cutout, `showPortrait:
  false` hides the portrait.

Someone who speaks once can be a custom speaker instead:
`{ "type": "custom", "name": "Bell-ringer", "role": "character" }`, with
optional `imageUrl` and `cutoutUrl`. Anyone who speaks twice belongs in
the cast.

## Where cutouts stand

Stories are read in two layouts: `desktop`, a wide stage (a computer, a
tablet, a phone held sideways), and `mobile`, a phone held upright, where
the stage is narrow. A spot is any of:

- `position`: `left`, `center` or `right`;
- `x`: the cutout's centre as a percentage of the stage width (0–100);
  it wins over `position`;
- `y`: a lift above where the cutout rests, as a percentage of the stage
  height (−40–40);
- `scale`: 0.4–1.8, where 1 is the normal size.

A member's `placement` is their default spot, used on every page that
doesn't place them; its `mobile` object holds the phone's own spot. A
page places its speaker with the same fields on `page.speaker` (the
phone's under `speaker.mobile`) only when that beat moves them. Fields
stack one at a time: on desktop the page, then the member's default, then
centre, lift 0 and size 1; on a phone the page's `mobile`, then the
member's `mobile`, then whatever desktop settled on.

Give every recurring character a default so each keeps their side of a
conversation. On a phone the `left` and `right` presets hug the edges
of a narrow stage: a `mobile.x` around 35 or 65 and a scale near 0.9
keep faces on screen.

## Pictures on a page

`page.images` (up to 12, back to front) are props, notes and signs. A
picture belongs to its page alone unless it says to stay.

- `x` and `y` place the picture's centre, as percentages of the stage's
  width and height (−50–150, so a picture can hang off an edge).
- `width` is a percentage of the stage WIDTH, and the height follows from
  `aspectRatio` (the image's width divided by its height: 1.5 for 3:2).
  Use the image's real ratio unless you mean to squash it.
- `rotation` is degrees clockwise (−180–180).
- `layer` is `front` of the speaker's cutout (the default) or `back`.
- `flipX` / `flipY` (true) mirror it left to right or upside down, and
  `opacity` (0.1–1, unset is solid) lets the backdrop show through. Both
  apply in every layout.
- `mobile` changes any of `x`, `y`, `width`, `aspectRatio` and
  `rotation` on phones; the rest follow desktop.
- `alt` describes the picture for readers who can't see it.
- `stay` keeps it on the pages after, exactly where it is: `"scene"`
  until the backdrop changes, or `{ "throughPageId": "p-014" }` through
  that page. Place a prop that stays put through a conversation ONCE, with
  a stay, rather than copying it onto every page. What stays is drawn
  behind the later pages' own pictures and sits out video pages. A later
  page placing a picture with the same `id` takes it over from there.
- A later page changes how a staying picture looks, never where it is,
  with `imageChanges`: `[{ "imageId": "drowned-lantern", "opacity":
  0.6 }]`. Any of `width`, `aspectRatio`, `rotation`, `opacity`,
  `flipX`, `flipY` and `layer`, plus `mobile` for the phone's
  `width` / `aspectRatio` / `rotation`. A change lasts from its page
  until another page changes the same thing; add `"onlyHere": true` for a
  beat that passes, and the next page looks as the pages before left it. A
  different image is a new picture, not a change.

## Chapters, music and the rest

- `chapters` are `{ id, title, startPageId }`; each runs until the next
  begins. The first must start on the first page (push adds "Chapter 1"
  when it doesn't). Open chapters on a `sceneHeading` page.
- `music` cues are `{ id, title, url, startPageId, endPageId, volume }`:
  a cue plays from its start page through its end page (`null`: until the
  next cue or the end), at `volume` 0–1 (default 0.6). Where cues
  overlap, the one that started latest plays.
- `assets.backgrounds` and `assets.music` are the pick lists the app's
  editor offers; pages don't reference them. Keep the backdrops and tracks
  you use there so the user can reuse them.
- `settings.textReveal`: `word` (words fade in, the default) or
  `instant`. `settings.narration` is managed in the app.
- Every page, chapter, cue, member, expression and picture has an `id`:
  any unique string up to 64 characters. Readable ids (`p-012`,
  `oracle`, `oracle-wary`) keep the file easy to edit. Keep the ids a
  story already has: lines point at members by id, and chapters and cues
  point at pages.

## Art and music

Every url must be on one of Craft's media hosts (in production,
`https://media.craftrpgs.com/...`; other environments and older art use
others, which `check` knows). A url from anywhere else fails `check`.
Never invent urls.

- `craft vn assets <file>` prints the art the story can borrow: its
  project's (or game's) GM and characters as ready cast members with their
  portraits, cutouts and expressions, its locations as backdrops, and the
  user's finished studio music. Every entry is shaped for
  `document.assets`; copy what the story uses and keep the ids.
- New art: `craft image generate` or `craft image upload` (read
  generating-images.md first: generation spends the user's energy), then
  paste the returned `url`.
- Cutouts and expressions come best from character files: give a
  character a portrait, run `craft image expressions` and
  `craft image cutouts` on its file, and `craft vn assets` lists the
  results. In the app, the story's cast panel can also draw expressions
  and cut out art for a member.
- Music comes from the user's studio, where tracks are made in the app.

## What reads well

- One beat per page: a line of dialogue, a short paragraph of narration.
  Readers click after every page, so long pages stall them.
- Open each scene by changing the backdrop, and each chapter with a
  `sceneHeading`.
- Let characters carry the scene in their own lines; keep narration
  between them brief.
- Match expressions to the emotion of each line, and give every speaking
  character a portrait and a cutout.
- Save `readAloud` and handouts for the moments a reader should stop on.
- Stage conversations: defaults on opposite sides, and per-page spots
  only when someone moves.
- End on a page worth ending on.
- Before publishing: `check` passes and its warnings are dealt with, the
  story has a `description`, and the user has seen it.

## When check or push complains

- `unknown field`: a misspelled or misplaced key; the path says where.
- `no cast member has the id`: a line's `castId` isn't in
  `assets.speakers`; the message lists the ids that are.
- `has no expression with the id`: use one of the ids the message lists,
  or leave `expressionId` out for the member's base art.
- `no page has the id`: a chapter or music cue points at a missing page.
- `is already the id of`: two entries share an id; give one a new id.
- `Media URL must use an approved media host`: the url isn't on a
  Craft media host.
- `Push refused ... changed on the server`: see the loop above.
