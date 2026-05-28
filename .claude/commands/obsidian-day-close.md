---
name: obsidian-day-close
description: Process today's work and help me close the day using the Obsidian CLI.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Process today's work and help me close the day using the Obsidian CLI.

Goal:
Use targeted Obsidian CLI queries to extract what matters from today's daily note, surface relevant vault connections, suggest filing and links, and identify what should carry forward into tomorrow.

Related commands: `/obsidian-day-today` (start of day) | `/obsidian-mine-graduate` (promote standout ideas from today)

Step 1: Read Today's Daily Note

1. Start with today's note:
   - Obsidian daily:read

2. If needed, also read today's daily note by path:
   - Obsidian read path="\_Daily/YYYY-MM-DD.md"

Parse everything captured in today's note, including:

- free-form writing
- work notes
- tasks mentioned
- commitments made
- people referenced
- ideas, realizations, and open questions

Step 1.5: Structured Vault Connection Discovery

After reading today's note, identify the 2-4 major themes or topics from today.

Then run these every time:

1. Theme distribution:
   - Obsidian tags counts sort=count

2. Topic context searches:
   - Obsidian search:context query="<theme 1 from today>"
   - Obsidian search:context query="<theme 2 from today>"
   - Obsidian search:context query="<theme 3 from today>" # only if relevant

3. If today's note references specific notes directly:
   - Obsidian links file="YYYY-MM-DD" # outgoing links from today's note
   - Obsidian backlinks file="YYYY-MM-DD" # what links back to today's note, if useful

4. For the most important referenced notes or topics:
   - Obsidian backlinks file="<referenced note>"
   - Obsidian links file="<referenced note>"

5. Forgotten or structurally interesting notes:
   - Obsidian orphans
   - Obsidian unresolved

Use these results to surface:

- recurring themes,
- related notes worth revisiting,
- emerging patterns,
- topics that appeared again today,
- unresolved or neglected ideas that match today's themes.

Do not explore the whole vault indiscriminately.
Prefer the smallest set of CLI calls that gives strong signal.

Step 2: Extract and Categorize

From today's daily note, extract and group:

- Action items
  - tasks I need to do
  - follow-ups
  - commitments or promises
  - deadlines if mentioned

- Ideas and insights
  - realizations worth preserving
  - project or process ideas
  - shifts in perspective

- People and commitments
  - anyone I owe a message, answer, follow-up, or decision

- Open questions
  - things to investigate
  - unresolved decisions
  - uncertainties needing clarification

Step 3: Suggest Filing Locations

For each extracted item, recommend where it should live.

Use only these filing targets unless there is a strong reason otherwise:

- keep in today's daily note
- add to my-world.md
- add to an existing MoC in \_MoC/
- add to an existing project/person/concept note
- create a new note only if the topic is likely to recur

Prefer restraint.
Do not assume every thought deserves a permanent note.

Step 4: Suggest Backlinks and Links to Add

Use CLI to inspect today's note and related notes:

- Obsidian links file="YYYY-MM-DD"
- Obsidian backlinks file="YYYY-MM-DD"
- Obsidian search query="<person name>"
- Obsidian search query="<project name>"
- Obsidian search query="<concept name>"

Identify terms from today's note that should likely link to existing notes, especially:

- people
- projects
- recurring concepts
- decisions already discussed elsewhere

Present these as:

- suggested wikilinks to add in today's note
- suggested backlinks worth reviewing in related notes

Step 5: Suggest Context Updates

Check whether anything from today should update:

- my-world.md
- an active MoC in \_MoC/
- an existing project note
- an existing person note

Use CLI selectively to confirm the target note before suggesting the update:

- Obsidian search query="<topic>"
- Obsidian read file="<relevant note>"

Focus on:

- new priorities
- changed assumptions
- resolved questions
- new constraints
- meaningful shifts in direction or confidence

Do not update notes automatically.
Only propose specific changes.

Step 6: Carry Forward

Based on today's note and the supporting CLI lookups, identify:

- what should carry into tomorrow
- unfinished but important threads
- any commitments due soon
- the single most important thing to keep top of mind tomorrow

If today's note does not already include a closing reflection, draft short answers for:

- What did I actually move forward today?
- What remains unresolved?
- What matters most tomorrow?

Output format:

## Today's Extraction

Concise categorized bullets

## Vault Connections

Important links, repeats, contradictions, unresolved items, or emerging themes surfaced by the CLI queries

## Filing Suggestions

Short practical suggestions for where items should go

## Links to Add

Suggested wikilinks or backlinks to consider

## Context Updates

Specific note updates worth making

## Carry Forward

What matters tomorrow

Guidelines:

- Keep it concise; this is a 5-minute ritual.
- Use the Obsidian CLI as the primary retrieval method.
- Prefer targ
