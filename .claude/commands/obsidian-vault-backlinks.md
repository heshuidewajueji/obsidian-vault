---
name: obsidian-vault-backlinks
description: "[vault writes] Wire the graph of my Obsidian vault using the Obsidian CLI."
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Wire the graph of my Obsidian vault using the Obsidian CLI.

Goal:
Improve the vault’s traversability by identifying high-value missing links, structurally weak areas, unresolved concepts, and disconnected notes. Focus on wiring, not rewriting. Do not generate substantive content for notes. Only recommend or execute links and minimal stub creation when approved.

Usage:

- /obsidian-vault-backlinks
- /obsidian-vault-backlinks [cluster]

If a cluster name is provided, focus Phases 3-4 primarily on that cluster and its nearest neighbors.

Phase 1: Structural Inventory

Use graph-only CLI commands first. No broad content reading.

Run:

- Obsidian orphans
- Obsidian deadends
- Obsidian unresolved
- Obsidian tags counts sort=count
- Obsidian files

From these results:

- estimate vault size
- identify top structural problem areas
- identify likely hub notes, active themes, and unresolved concepts

Then inspect the graph around top candidate hubs:

- Obsidian backlinks file="<Hub Note A>"
- Obsidian backlinks file="<Hub Note B>"
- Obsidian links file="<Hub Note A>"
- Obsidian links file="<Hub Note B>"

Expand to the top 15-20 likely hub notes only if needed.
Also inspect notes that appear repeatedly as backlink targets.

Build a rough cluster map from:

- backlink concentration
- outgoing link patterns
- repeated unresolved links
- dominant tags

Phase 2: Priority Context

Read a small number of high-value context notes to build a priority filter.

Start with:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"

Then read 4-6 of the most relevant linked notes, especially:

- active MoCs in \_MoC/
- active project notes
- active people or concept notes

Read recent daily notes from the past 7 days:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # past 6 additional days

Extract:

- current priorities and active projects
- open questions and active threads
- people and concepts getting current attention

This becomes the priority filter used later for scoring.

Phase 3: Orphan Rescue Scan

Use the orphan list from Phase 1, but be selective.

Skip non-note files and anything that is not a markdown note.

For remaining orphan candidates:

1. Prioritize reading if:

- the title contains words related to current priorities,
- it appears semantically close to an active cluster,
- it looks like a question or unresolved thought,
- it looks old but potentially rich,
- it may be a meaningful note with no inbound links.

2. For prioritized orphan notes:

- read the title and opening section first
- if still promising:
  - Obsidian read file="<orphan note>"
  - Obsidian search:context query="<key concept from orphan>"

3. For each meaningful orphan:

- identify the nearest cluster, hub, or related note
- determine whether it should connect to:
  - an MoC
  - a project note
  - a person note
  - a concept note
  - an old related thread

Do not read more than 100 notes total across all phases.
Be selective and score signal over coverage.

Phase 4: Cluster Bridge Analysis

Look for high-value missing bridges between clusters.

1. Identify the top 5-6 clusters from:

- hub patterns
- tag distribution
- daily-note themes
- recurring unresolved links

2. For each pair of clusters with weak or missing connections:

- Obsidian search:context query="<theme from Cluster A>"
- search for related vocabulary that may overlap with Cluster B
- try 2-3 synonym or adjacent-concept variants if needed

3. Read intermediary notes only if they appear likely to bridge the clusters:

- Obsidian read file="<intermediary note>"
- Obsidian backlinks file="<intermediary note>"
- Obsidian links file="<intermediary note>"

Look for:

- two clusters solving the same problem under different names
- old notes relevant to current work but never linked
- person notes that should connect to concepts or projects
- semantic twins using different vocabulary

Phase 5: Unresolved Link Triage

Use the unresolved links from Phase 1.

For unresolved links:

- 3+ references: likely worth creating as a stub note
- 2 references: check for near-duplicates, aliases, or name variants first
- 1 reference: usually skip unless tied to current priorities

Before recommending a new stub:

- search for likely existing variants
- avoid duplicate concepts under slightly different names

If a stub is warranted, recommend a minimal note only:

- title
- Related section
- backlinks only
- no synthesized content

Phase 6: Score and Recommend

Score each candidate connection on three dimensions:

1. Conceptual Strength

- 1 = same word, weak real connection
- 3 = related problem, question, or thread
- 5 = same thesis or idea from different angles

2. Structural Impact

- 1 = small improvement to an already well-connected area
- 3 = rescues a valuable orphan or closes an internal gap
- 5 = bridges isolated clusters or formalizes an important unresolved concept

3. Priority Alignment

- 1 = not tied to active work
- 3 = one side touches an active priority
- 5 = both sides are active or strategically important now

Composite score:
Conceptual x Structural x Priority
Maximum = 125

Tiers:

- Critical: 75+
- High: 40-74
- Medium: 15-39
- Low: below 15

Quality controls:

- minimum Conceptual Strength of 2, otherwise skip
- cap at 30 recommendations total
- each recommendation must be explainable in one sentence
- include borderline cases at lower score rather than silently dropping them

Use this connection taxonomy:

- Orphan to Hub
- Cluster Bridge
- Internal Gap
- Empty Hub (flag only)
- Semantic Twin
- Person to Concept
- Temporal Bridge
- Unresolved Worth Creating

Phase 7: Report

Present findings in this format:

BACKLINKS REPORT -- [Date]

Vault size: [N] notes
Orphans assessed: [N] meaningful (of [N] total)
Connections recommended: [N] across [N] tiers

## Critical

[Connection cards]

## High

[Connection cards]

## Medium

[Connection cards]

## Summary

Biggest structural gaps and what these connections would unlock

For each recommendation use:

### [#]. [Type]: [Short description]

**Score:** [X] (Conceptual [N] x Structural [N] x Priority [N])
**What:** [One-sentence description]
**Edit:** Add `[[Target Note]]` to [Source Note] in the most relevant section
**Why:** [One sentence on what this unlocks]

For Empty Hub flags use:

### [#]. Empty Hub: [Note name]

**Referenced by:** [list of notes]
**What this needs:** Your thinking. [N] notes point here and find little or nothing.

Then ask:

- Execute All Critical + High
- Execute Critical only
- Pick specific ones by number
- None

Phase 8: Execute Only If Approved

If I approve execution:

1. Before editing, read the target note structure:

- Obsidian read file="<source note>"

2. Add links where they make semantic sense.
   Do not dump links at the bottom.

3. For approved unresolved stubs:
   Create only:

- title
- Related section
- backlinks list
  No summaries. No synthetic exposition.

4. Never fill Empty Hubs.
   Only flag them.

5. After editing, verify:

- Obsidian links file="<modified note>"

Then report:

- how many connections were made
- how many notes were modified
- how many stub notes were created

Guidelines:

- Focus on wiring, not interpretation.
- Quality over quantity.
- Do not force connections just because words overlap.
- Old but rich notes can be more valuable than recent ones.
- Graph strength and note depth are different things.
- Be explicit about uncertainty and false positives.
