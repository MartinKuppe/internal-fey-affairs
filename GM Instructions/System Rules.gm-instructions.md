---
name: System Rules
description: The light RPG engine for Internal Fey Affairs — attributes,
  abilities, origins, species and archetypes, checks, Grace, bargains,
  reputation markers, and lore cards.
$craft:
  referenceId: 019fde04-45a3-7a04-ae62-38704ecce2e1
  settings:
    agentEditable: true
---

# System Rules

## The Shape of Play

This is an action adventure with a strong story: an almost linear campaign where story missions are played in order, and secondary missions can be played between them in any order. Play leans on investigation, stealth, and conversation. Combat is rare, costly, and usually a failure state — not a sport. Always offer a stealthy, social, or investigative alternative.

## Outfits

- A Character wears one current Outfit, stored as the `outfit` reference on their Character file. Changing that reference changes what they are wearing.
- All five initial Outfits — Common Clothes, Dark Outfit, Formal Court Attire, Fabulous Glamour, Vest Inside-Out — are available to every character in v1. No outfit bonuses, penalties, unlocks, or Grace costs exist yet.

## Attributes

Six attributes, each rated 1–5 dice:

- **Body** — strength
- **Hands** — agility
- **Head** — intelligence
- **Heart** — intuition
- **Legs** — speed
- **Shadow** — deception

At character creation, the attribute linked to the character's origin starts at 3 dice; the other five start at 2.

## Abilities

Nineteen abilities, each trained or untrained: Fey Lore, Learnedness, Charisma, Art & Beauty, Music, Milk, Repairs, Iron, Animals, Bargains, Lying, Court life, Stealth, Nature, Wayfinding, Riding, Dancing, Food, Weapons.

An origin grants exactly three trained abilities. The GM may award additional abilities during play.

## Checks

1. The GM chooses the attribute for the task, and an ability when one clearly applies.
2. The player rolls that many d6s — the attribute rating, plus one extra die if trained in the ability.
3. Every die showing 4 or more is a success.
4. The GM sets the difficulty as the number of successes needed: 1 easy, 2 standard, 3 hard, 4+ very hard.
5. Resolve the outcome from the number of successes before asking follow-ups or continuing.

## Origins

When a player picks an origin, it defines who their character was before the story began: their attribute, their three abilities, and their place in the village. See the Origins folder for the eighteen choices.

- A newly created playable protagonist is expected to have an Origin.
- NPCs do not require an Origin. An NPC may carry an Origin only when it genuinely represents their mechanical starting background.
- Minor NPCs do not need full attributes or trained abilities unless play requires them.

## Species & Archetypes

- Species is **Human** or **Fey** in ordinary project metadata.
- **Elgafar** / **Elgafari** terminology is reserved for rare, deep technical lore; do not use it in ordinary play or character data.
- Clones and Takelings are Human. A Takeling is a Human taken in infancy and raised by the Fey; a Clone is grown from a pattern. A Changeling is a particular clone pattern or role — not a separate Species, and not Fey.
- Fey kinds such as Lepracaun, Banshee, Pooka, Brownie, and Leannán Sidhe are roles, functions, training, patterns, or social identities — Archetypes, not Species.
- Characters explicitly store every applicable Archetype. Never infer an undisclosed archetype from an indirect reference chain; classification is explicit and flat.
- Archetypes are descriptive and grant no automatic mechanics until future rules explicitly assign mechanical effects.
- Player-facing narration must respect spoiler boundaries: never expose a character's Secrets or the GM/canon notes on Species or Archetype definitions.

## Grace

Grace is the currency of favors among the Fey — and the measure of whether one pays what courtesy demands.

- Grace is issued as separate currencies, one per queen or court. Currency definitions live in the Grace folder — one file per queen-issued currency.
- A character's Grace is a ledger, not a single number. Every entry records the currency, a signed amount (positive credits the character, negative debits the character), the day, and the exact reason. Balances are always derived from the ledger — never maintain a separate running total that could contradict it.
- A plea ("please…") proposes an implied bargain. It transfers no Grace until it is accepted and performed.
- A rejected plea creates no cost.
- Once a plea is accepted and performed, thanks are expected.
- When thanks are given, the Grace transferred is the greater of the amount implied by the plea and the amount implied by the thanks.
- If a conversation ends without thanks, the accepted plea becomes unthanked. Additional consequences begin the following day.
- Treat the unpaid Grace and the social breach separately:
  - Delayed thanks settles the Grace debt.
  - An excuse, apology, or forgiveness repairs the ingratitude.
- Grace debt is a negative amount in the ledger.
- Lepracaun exchanges move value between two currencies: record two ledger entries — a negative amount in the currency given, a positive amount in the currency received, on the same day, with the same reason naming the rate.
- Record every Grace gain or loss in the ledger with its exact reason. Make significant changes immediately clear in narration.

## Bargains & Contracts

- One Bargain file per contract. Every bargain has at least two parties; each party carries its exact obligation wording, its due condition, and its resolved state.
- Parties may be characters, informal groups, or legal entities — a party's label names them; link a character file when one exists.
- Create a Bargain file whenever an obligation forms: a grace-trade, a settlement for wrongdoing, an employment or mission contract, an indenture, or any contract the player asks to see.
- A party's obligation may link to the specific contract it falls under via its Reference contract field (a Bargain reference, when that contract is a different document). Related contracts are also linked on the bargain file itself.
- A bargain is player-visible when it is the player's own or was witnessed, overheard, read, disclosed, or discovered during investigation. Set "Known to the player" and narrate it.
- When a new bargain is created, announce it clearly and immediately — a distinct, consistent system message naming the parties and the key terms.

## Reputation Markers

- Markers are universal and visible to everyone. They are simply present or absent — no scores, no scopes, no severity.
- Initial markers: Ungrateful, Disrespectful, Unsightly, Violent, Cruel, Untrustworthy, Oath-Breaker, Generous, Respectful, Handsome, Elegant, Merciful, Trustworthy, Oath-Keeper.
- Each active marker carries a short reason explaining how it was acquired.
- Markers influence NPC reactions and fictional position. Never add automatic hidden numerical modifiers.
- Narrate when a marker is acquired or removed.

## Lore Cards

- A Lore Card holds three knowledge tiers — Common Knowledge, Deep Lore, and Fey Knowledge. The card's flags (`discovered`, `deepLoreKnown`, `feyKnowledgeKnown`) are GM-controlled; players never unlock lore directly.
- Encountering a card's subject unlocks Common Knowledge: set `discovered` to true.
- A meaningful conversation about the subject may unlock the card; a merely passing mention does not.
- Starting a relevant mission may unlock Lore Cards explicitly configured by that mission — once the future Mission system supports it.
- Following a hyperlink or reference only opens a file; it never changes discovery state automatically.
- Consulting a suitable human expert in one of the card's `expertRegions` may unlock Deep Lore: set `deepLoreKnown` to true.
- When `feyLoreUnlocksDeepLore` is true and the player character has the Fey Lore ability, Deep Lore may unlock at the same time the card is first discovered.
- Fey Knowledge normally comes from a mission, a revelation, or a suitable Fey source: set `feyKnowledgeKnown` to true.
- Asking a Fey for knowledge may require a Bargain or incur Grace consequences — use the existing Bargain and Grace Ledger rules.
- Setting `deepLoreKnown` or `feyKnowledgeKnown` must also set `discovered` to true.
- Keep the Lore Journal's master `cards` list updated whenever a Lore Card is created; the journal shows discovered cards automatically.

## Guiding the Game Master

- When the user says they are using a skill or performing an action, ask them to roll an appropriate check before resolving the outcome.
- Keep combat quick and rare; always offer a stealthy, social, or investigative alternative.
- Let side missions be tackled in any order between story missions.
- The emoji shorthand in design notes is internal only — never present it to players.
