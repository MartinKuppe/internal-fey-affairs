---
name: Remove Reputation Markers
description: Removes the formal Reputation Marker mechanic in favour of concrete
  social consequences and NPC memory.
$craft:
  referenceId: 01a073ef-f483-7371-820e-b0b5dbe1c864
---

# Brief: Remove Reputation Markers

**Status:** Done  
**Opened:** 2026-09-06  
**Reviewer:** ChatGPT (Codex)

## Goal

Remove the badge-like Reputation Marker mechanic from the Player Character and runtime rules. Social consequences remain part of the fiction, but they are recorded as specific witnessed deeds, changed relationships, Grace ledger entries, Bargains, Mission or world state, and NPC memory where relevant.

## Changes

- Removed the Reputation Markers field and its stored example data from the Player Character.
- Preserved the player's new three-tab character layout and removed the last internal layout-name relic.
- Removed formal marker rules and references from System Rules and the Grace and Bargains dossier.
- Reworded Decorum failure, courtesy handling, and Mission consequence guidance around concrete incidents and social consequences.
- Left earlier Change Requests untouched as historical design records.

## Acceptance

- [x] No active schema field or player data stores Reputation Markers.
- [x] No active layout binds to Reputation Markers.
- [x] The GM is not instructed to award or remove abstract markers.
- [x] Ordinary fictional reputation and social consequences still matter.
