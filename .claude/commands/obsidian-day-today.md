---
name: obsidian-day-today
description: Review my current state and help me plan today using the Obsidian CLI.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Review my current state and help me plan today using the Obsidian CLI.

Goal:
Use targeted Obsidian CLI queries to generate a practical daily plan grounded in recent notes, active context, and relevant vault structure.

Related commands: `/obsidian-day-close` (end of day) | `/obsidian-day-7plan` (weekly view)

Step 1: Read Recent Daily Notes

1. Start with today's daily note:
   - Obsidian daily:read

2. Also read recent daily notes from the last 7 days:
   - Obsidian read path="\_Daily/YYYY-MM-DD.md" # for each of the previous 6 days

3. If the recent daily notes are sparse, expand the window to the last 10-14 days.

Look for:

- recurring themes
- unfinished threads
- commitments I made to myself
- blockers
- energy patterns
- informal tasks such as "TODO", "I need to", "follow up", "remember to"

Step 2: Load Active Context via CLI

1. Read my core context note:
   - Obsidian read file="My World.md"

2. Inspect the connections from that note:
   - Obsidian links file="My World.md"

3. Read only the most relevant active notes linked from my-world.md, especially:
   - active MoCs in \_MoC/
   - active project notes
   - active people or concept notes

Do not read the entire vault indiscriminately.
Start from my-world.md and follow only the most relevant active links.

Look for:

- active projects
- current priorities
- open questions
- work already in motion

Step 3: Form Preliminary Priorities

Based on recent daily notes and active context, identify:

- the 2-3 most important things to move today
- the dominant themes from the last week
- any tensions, tradeoffs, or uncertainty

These preliminary priorities should guide the next CLI lookups.

Step 4: Deep Vault Exploration with CLI

Use Obsidian CLI to enrich and challenge the preliminary priorities.

1. Theme distribution:
   - Obsidian tags counts sort=count

Compare the most active themes against today's preliminary priorities.

2. For each top priority:
   - Obsidian search:context query="<priority topic>"
   - Obsidian search query="<priority topic>"
   - Obsidian backlinks file="<related note>"
   - Obsidian links file="<related note>"

3. Follow useful connections 1-2 hops deep only when they sharpen the priority.

4. Broader structural checks:
   - Obsidian orphans
   - Obsidian unresolved

Look for:

- related thinking I may have forgotten
- contradictions with what I said recently
- lower-effort actions that unlock bigger work
- signs that one priority is avoidance while another is the real work
- unresolved or neglected notes relevant to today's priorities

Prefer the smallest set of CLI calls that gives strong signal.
Do not wander the whole vault.

Step 5: Deliver a Daily Plan

Give me:

## Core Theme

One sentence capturing what today should be about.

## Top Priorities

The 2 most important things to progress today.

For each priority, include:

- why it matters today
- the most relevant vault context
- any important connection, contradiction, or open question surfaced by the CLI lookups

## Quick Wins

Up to 5 items that can be done in under 15 minutes and that:

- reduce mental overhead
- unblock something else
- prevent small problems from growing
- are genuinely worth doing today

## Risks / Blockers

Anything likely to derail the day.

## Suggested First Move

The single best thing to do first.

Guidelines:

- Be direct, practical, and concise.
- Use the Obsidian CLI as the primary retrieval method.
- Prefer targeted search, links, and backlinks over broad reading.
- Keep the plan usable in a few minutes, not a long essay.
