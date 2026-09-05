---
name: Scripted Opening Transition
description: Replaces the unreliable dynamic opening with a static Grannie's Hut
  sequence and moves the goat chase and Fairy Ring reveal into The Encounter.
$craft:
  referenceId: 01a0735b-d091-7186-9baa-ea396be99aec
---

# Brief: Scripted Opening Transition

**Status:** Done
**Opened:** 2026-09-05
**Reviewer:** Hitchcock and ChatGPT (Codex)

## Goal

Make the opening deterministic without displaying the forest sequence over Grannie's Hut. The static opening establishes Grannie, Niamh, local folklore, the cream, and the horseshoe, then ends with one apparent player choice at the yard gate. The Encounter absorbs that response and carries play without another prompt through the goat chase, Fairy Ring activation, Méabh's plea, and Coinín's offer.

## Relevant canon

- The opening begins at Grannie's Hut with The Blacksmith as the current pre-generated Player Character.
- Grannie uses human folkloric language. Narration must not confirm literal magic, and Fey/Fey-educated speech uses technological or folkloric-technical terms such as glamour, signal, device, ring, door, field, or working.
- Méabh's assignment, her relationship to Niamh, and Coinín's supply-chain motives remain protected secrets.
- The first genuine player choice occurs only after Coinín offers the Hollow Tree.

## Files touched

- /Game Starts/Start at Granny's Hut.game-start.json — opening metadata updated in the Craft app
- /Missions/The Encounter.mission.json

## Changes

- [x] Change the Game Start opening from Dynamic to Scripted/Static.
- [x] Keep the scripted sequence entirely at Grannie's Hut and end on the goat waiting at the yard gate with “What do you do?”
- [x] Add `goat-chase` as the first beat of The Encounter.
- [x] Treat the player's response to the goat as tone and detail while ensuring the goat bolts and pursuit begins.
- [x] Continue without another prompt through the chase, scene transition, Fairy Ring activation, Méabh's plea, patrol signs, and Coinín's offer.
- [x] Make Coinín's offer the boundary before the first genuine Mission choice.

## Static opening script

Use one static beat per narration or dialogue card. Speakers should be the GM, Grannie, Niamh, and The Blacksmith as indicated.

1. **GM:** When you come back home with the firewood, Grannie is sitting by the hearth with a mug of cider. You are surprised to see Niamh placing jars and wrapped provisions in the pantry.
2. **Grannie:** “You're back. Did you find enough firewood? And you didn't go near the hawthorn tree, did you? We don't want the Little People put out with us.”
3. **The Blacksmith:** “Don't worry, Grannie.”
4. **GM:** Niamh comes out of the pantry, red-haired and pretty as ever; even the freckles scattered across her cheeks seem oddly neat.
5. **Niamh:** “Oh, haigh! I brought you some groceries and cider from the inn. Can't stay, though. I left Finn with the baby; they're probably both hungry by now. See you!”
6. **GM:** Niamh slips out into the village before Grannie can press another cup on her.
7. **Grannie:** “Fierce sweet girl, that one. A shame she married the innkeeper. I know well you fancied her.”
8. **The Blacksmith:** “Half of Trí Mhuilinn fancies her, Grannie. The other half thinks she's a witch.”
9. **Grannie:** “I don't think she's a witch at all. She's a Changeling. Look at the freckles on her—matched left and right, every one of them. Two new ones now, one on either side of her nose. Who ever heard of freckles behaving themselves like that?”
10. **The Blacksmith:** “You're blind, Grannie.”
11. **Grannie:** “I'm not that blind! But she's one of the good ones. Not all the Fey are vicious, sweetheart. Some will help you, like Hobs and Brownies. Some are only after mischief, like Pookas and Boggarts. Some mean death is near, like the Banshee. But if ever you meet the Dullahan…”
12. **The Blacksmith:** “You and your age-old folktales…”
13. **Grannie:** “They're not that old! The Little People only came when I was twenty-three. Midsummer night it was, and lights all across the sky…”
14. **The Blacksmith:** “Grannie, you told me the story a thousand times. The first townsfolk who went missing, the music you heard in the woods, the fox that wasn't a fox…”
15. **Grannie:** “It was a Pooka! The woods haven't been right since. Anyway, that goat is after acting strange again. Would you have a look at him? And put a pot of cream by the door while you're at it. Check the horseshoe as well—it was loose again yesterday.”
16. **GM:** You set a pot of cream beside the threshold and press the loose horseshoe back into place above the door. Then you step into the yard.
17. **GM:** The goat is waiting by the yard gate. He is standing much too still, with one hoof hooked over the lower bar and a strip of red cloth hanging from his mouth. His yellow eyes meet yours with the calm of a creature who has already sinned and is considering whether to make it worse.
18. **Grannie:** “That goat is after doing something clever. Stop him before he teaches it to the rest of the village.”
19. **GM:** The goat chews once. What do you do?

## Opening instructions

Immediately start The Encounter in response to the player's answer. Set it active, record its start date, and begin with `goat-chase`. Treat the answer as colour and tactical detail; do not allow it to avoid the inciting incident. Continue without another prompt through `goat-chase` and `ring-arrival`. Hand control back only after Coinín says, “Come, quickly! You can hide inside my hollow tree!”

## Acceptance criteria

- [x] The Game Start displays deterministic static cards at Grannie's Hut rather than asking the GM to reproduce a script dynamically.
- [x] Its only prompt is the final goat-at-the-gate question.
- [x] Any answer leads immediately into the goat chase without a roll or additional prompt.
- [x] The GM changes the scene to Trí Mhuilinn and then the Fairy Ring during the Mission.
- [x] Méabh and Coinín appear before control returns to the player.
- [x] The player's first genuine decision remains open rather than prescribing acceptance of Coinín's hiding place.

---

## Done — implementation notes

The Encounter now contains the deterministic transition and explicit prompt boundary. The Game Start was changed directly in the Craft app from Dynamic to Scripted and populated with the nineteen authored cards above, including Grannie, Niamh, The Blacksmith, and GM narration. Its opening instruction now starts The Encounter at `goat-chase` and withholds the next prompt until Coinín's offer.

Because Game Start metadata is app-managed, the local checkout still shows the previous Dynamic metadata until the next `craft pull`. Pull before pushing the local Mission change so the checkout records the newly saved Scripted opening.

## Review

<Awaiting Game Start application and playtest.>
