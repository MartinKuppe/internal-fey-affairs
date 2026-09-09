---
name: Lore and Knowledge
description: Discovery and knowledge-tier flags. Consult when revealing or unlocking lore.
$craft:
  referenceId: 01a0879b-2fa5-731b-b003-20090882f7f1
---

# Lore and Knowledge

Consult when a subject is encountered, discussed or investigated, or when a Mission grants lore. Card flags record earned player knowledge; opening a link does not unlock anything.

- Common Knowledge: set `discovered` true when encountering the subject. A meaningful conversation can qualify; a passing mention does not.
- Deep Lore: set `discovered` and `deepLoreKnown` true after consulting a suitable human expert in an `expertRegions` region. If `feyLoreUnlocksDeepLore` is true and the PC has Fey Lore, this can unlock on first discovery.
- Fey Knowledge: set `discovered` and `feyKnowledgeKnown` true after an appropriate Mission, revelation or Fey source. Asking may involve Grace or a Bargain; consult **Grace, Language and Bargains**.

Apply authored `startLoreUnlocks` at mission start, beat `loreUnlocks` when reached/resolved as authored, `completionLoreUnlocks` on completion and `failureLoreUnlocks` on permitted failure. Use the same flag rules above; a deeper tier always includes discovery.

The GM controls these flags. Expose only the earned tier, never hidden notes or future reveals. When creating a Lore Card, add it to the Lore Journal's master `cards` list; discovered cards display automatically. See **Mission Procedures** for mission transitions.

