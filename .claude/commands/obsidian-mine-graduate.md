---
name: obsidian-mine-graduate
description: "[vault writes] Graduate ideas from recent daily notes into standalone notes or existing notes using the Obsidian CLI."
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Goal:
Scan recent daily notes, surface the strongest ideas worth promoting, and help me decide what should graduate into the main vault. Do not create or modify files until I explicitly approve.

Usage:

- /obsidian-mine-graduate
- /obsidian-mine-graduate [optional topic or cluster]

If a topic or cluster is provided, prioritize candidates related to that area.

Related commands: `/obsidian-mine-make` (identify which graduated notes are ready to publish) → `/obsidian-write-learned` (draft writing from identified candidates)

Phase 1: Scan Recent Daily Notes

Read the past 14 days of daily notes using the Obsidian CLI.

1. Start with today:

- Obsidian daily:read

2. Then read the previous 13 daily notes individually:

- Obsidian read file="YYYY-MM-DD"

Use my \_Daily/ daily-note structure when resolving these notes.

From each daily note, extract candidates for graduation.

Look for explicit signals (inline tags):

- #idea — a product concept, article topic, or experiment worth capturing
- #expand — an existing note/concept that deserves more depth
- #surprise — something unexpected (a result, behavior, or realization)
- #friction — a recurring pain point or workflow blocker
- #decision — a choice made today that future-you should know about
- #question — an open question worth tracking
- language like:
  - "I should write about"
  - "worth investigating"
  - "need to explore"
  - "this is important"
- named concepts or phrase-like ideas
- unresolved [[links]] that point to real concepts not yet formalized

Look for implicit signals:

- passages with unusually high energy
- original claims, frameworks, or theories
- recurring themes appearing across multiple days
- repeated questions that never get their own note
- ideas that seem bigger than the moment they were captured in

Do not extract:

- tasks or to-dos
- meeting logistics
- schedule notes
- venting without an underlying idea
- items that clearly already have a substantial standalone note unless the daily note adds something genuinely new

Phase 2: Cross-reference with Existing Vault

For each candidate, check whether it already exists or already has strong coverage.

Use:

- Obsidian search query="<candidate concept>"
- Obsidian search:context query="<candidate concept>"
- Obsidian backlinks file="<candidate note>" # if a likely existing note exists
- Obsidian links file="<candidate note>" # if a likely existing note exists

Categorize each candidate as one of:

- New concept
  - no note exists, or no meaningful coverage exists
- Underdeveloped
  - a note exists, but it is thin or obviously incomplete
- Already covered
  - a substantial note already exists
- Recurring unresolved
  - appears multiple times as an unresolved [[link]] or repeated concept

Also identify:

- related MoCs in \_MoC/
- related project, concept, or person notes
- whether the candidate already belongs to an existing cluster

Phase 3: Present Candidates

Present candidates ordered by priority.

Prioritize:

- recurring unresolved concepts
- ideas appearing on multiple days
- high-energy original thinking
- concepts relevant to active priorities from my-world.md or recent notes

For each candidate include:

- idea / concept name
- source daily notes
- number of days mentioned
- status category
- recommendation:
  - create standalone note
  - enrich existing note
  - connect to existing note only
  - skip for now

Also include:

- a 1-2 sentence summary of the idea as expressed in the daily notes
- short exact quote(s) from the daily notes
- what it connects to in the vault

Use this output structure:

## Graduation Candidates

| #   | Idea / Concept | Source | Days Mentioned | Status | Recommendation |
| --- | -------------- | ------ | -------------- | ------ | -------------- |
| 1   | ...            | ...    | ...            | ...    | ...            |

Then for each candidate:

### [#] [Idea / Concept]

- Summary:
- Quotes:
- Related notes:
- Why it may deserve graduation:

After presenting candidates, ask which ones I want to graduate.

Do not create or modify anything yet.

Phase 4: Graduate Selected Ideas Only After Approval

Only execute this phase if I explicitly approve one or more candidate numbers.

For each approved candidate:

A. If creating a new standalone note:

- create the note in the vault root unless I specify otherwise
- write a concise working note or mini-essay capturing:
  - the core claim, question, or idea
  - context from the daily notes where it originated
  - links to relevant existing notes using [[wikilinks]]
  - open questions or next steps
- preserve the original voice and energy from the daily notes
- avoid generic summary language

B. If enriching an existing note:

- Obsidian read file="<existing note>"
- add the new material from the daily notes clearly, ideally with a dated subsection if appropriate
- add newly discovered links if they are real and useful

C. If connecting to an MoC:

- Obsidian read file="<relevant MoC>"
- add the note to the most relevant section
- preserve the MoC’s existing structure

D. Update source daily notes only if approved:

- replace bare references with [[links]] where it genuinely improves retrieval
- do not overlink every mention

Phase 5: Summary

After execution, output:

## Graduated Today

- notes created
- notes enriched
- MoCs updated
- daily notes updated

## Still in the Queue

- surfaced but skipped ideas
- ideas worth reconsidering next run

## Vault Health

- total ideas found in the scan window
- number graduated vs skipped
- number of explicit signal tags found (#idea, #expand, #surprise, #friction, #decision, #question)
- number of untagged but strong candidates found
- recurring themes still not graduated

Guidelines:

- surface first, act second
- always ask before creating or modifying files
- keep graduated notes concise
- preserve original voice and energy
- do not sanitize or flatten strong thinking
- prefer a few strong candidates over a long noisy list
- the goal is a fast review and selective promotion process
