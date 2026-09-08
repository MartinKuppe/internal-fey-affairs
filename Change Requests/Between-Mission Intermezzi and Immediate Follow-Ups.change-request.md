---
name: Between-Mission Intermezzi and Immediate Follow-Ups
description: Adds bounded downtime between Missions, automatic uninterrupted
  Mission chains, explicit Condition recovery rates, and independently tracked
  Bargain obligations.
$craft:
  referenceId: 01a0823e-7d5a-7193-a9fc-028b5e937756
---

# Brief: Between-Mission Intermezzi and Immediate Follow-Ups

**Status:** Done  
**Opened:** 2026-09-08  
**Reviewer:** ChatGPT (Codex), following playtest analysis with Oracle and Hitchcock

## Problem

After the then-combined Milk and Cream Mission (later split into Cream Tax and Milk and Feathers), the player tried to return home and rest before accepting another Mission. The GM repeatedly read the same Mission, Bargain, map, and character files because the standing rules required an immediate Mission choice while the player's natural between-Mission action had no legal representation. Coinín's multi-part payment was also stored as one combined obligation, making partial fulfilment unnecessarily difficult to record.

## Changes

- [x] Add a bounded **intermezzo** choice to the ordinary post-Mission picker. It permits modest free fiction and fiction-supported recovery without becoming a Mission or advancing the campaign calendar.
- [x] Define clear exit conditions for an intermezzo and require the Mission picker to return afterward.
- [x] Add optional Mission `immediateFollowUp`, a direct Mission reference which skips both picker and intermezzo and automatically begins its named successor.
- [x] Keep `unlockAfter` as the normal dependency link on an immediate successor; the direct follow-up reference controls the transition rather than replacing prerequisites.
- [x] Carry the existing assigned team through a continuous chain unless the successor's authored team requirements demand a different valid composition.
- [x] Clarify recovery amounts for Health, Energy, Nerve, Decorum, and Cover while preserving lasting fictional consequences.
- [x] Replace each Bargain party's single obligation with an `obligations` list whose entries resolve independently.
- [x] Migrate all three existing shared Bargain files and update the Bargain layout to show the obligation rows per party.
- [x] Author Coinín's Silence with separate cream, milk, honey, and nondisclosure obligations; update both tutorial Side Missions to resolve only the goods they actually deliver.
- [x] Add terminal-processing discipline so the GM resumes unfinished work idempotently instead of repeatedly reading and reconsidering already-known state.

## Immediate chains planned for future Missions

- Approach Vector → Maiden, Mother and Crone
- Summons → Intruder Alert → Baba Yaga

Those Missions do not yet exist in this checkout. Set their `immediateFollowUp` and matching `unlockAfter` references when they are authored.

## Acceptance

- [x] A player may pause between ordinary Missions without creating an unauthorised Mission.
- [x] Recovery is explicit, fiction-dependent, and cannot erase established consequences.
- [x] An immediate follow-up creates a continuous sequence with no rest, sightseeing, shopping, or alternative Mission selection between its Missions.
- [x] The successor is named explicitly; a bare “no intermezzo” flag cannot leave the GM guessing among several available Missions.
- [x] Partial bargain performance updates only the fulfilled obligation rows.
- [x] Existing shared Bargain content retains all parties and exact terms.
