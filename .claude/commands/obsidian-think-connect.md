---
name: obsidian-think-connect
description: Find unexpected bridges between domains using the Obsidian CLI.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Connect — Find Unexpected Bridges Between Domains

Purpose:
Take two separate projects, topics, or domains and use the Obsidian CLI plus the vault graph to find non-obvious connections between them.

Usage:

- /obsidian-think-connect [domain A] [domain B]

Examples:

- /obsidian-think-connect design engineering
- /obsidian-think-connect software fashion
- /obsidian-think-connect architecture restaurants

If only one domain is provided, ask for the second and stop.

Step 0: Parse the Two Domains

Let:

- <domain A> = the first domain provided after the command
- <domain B> = the second domain provided after the command

Goal:
Map each domain’s neighborhood in the vault, compare them, and identify the strongest bridges, missing links, and new questions raised by seeing them together.

Step 1: Map Each Domain via CLI

For Domain A:

- Obsidian search query="<domain A>"
- Obsidian search:context query="<domain A>"

From the most relevant matching note(s), identify one or more likely hub notes.

For each hub note in Domain A:

- Obsidian backlinks file="<main note for domain A>"
- Obsidian links file="<main note for domain A>"
- Obsidian tags file="<main note for domain A>" # if supported in this setup

Follow useful backlinks and links 2-3 hops from the strongest hub notes.
Build Domain A’s neighborhood:

- notes
- people
- concepts
- themes
- tags
- unresolved links if relevant

Repeat the same process for Domain B:

- Obsidian search query="<domain B>"
- Obsidian search:context query="<domain B>"
- Obsidian backlinks file="<main note for domain B>"
- Obsidian links file="<main note for domain B>"
- Obsidian tags file="<main note for domain B>" # if supported

Then build Domain B’s neighborhood.

Depth asymmetry handling:

- If one domain is much thinner than the other, go deeper on the thinner one.
- For the smaller domain, follow 3-4 hops instead of 2-3 where useful.
- The smaller or less-developed domain often contains more surprising bridges.

Step 2: Find the Overlaps

Compare the two neighborhoods.

Look for:

Shared references:

- notes that appear in both domains’ link or backlink chains

Shared people:

- people mentioned in both domains
- compare how they appear in each domain

Shared themes:

- values, problems, patterns, or questions appearing in both domains even when note names differ

Use targeted CLI checks:

- Obsidian search:context query="<theme from domain A>"
- Search for this theme inside Domain B’s neighborhood
- Repeat in reverse for themes from Domain B

Shared tags:

- Obsidian tags file="<relevant note in domain A>"
- Obsidian tags file="<relevant note in domain B>"

Shared patterns:

- similar structural problems
- similar tensions
- similar trajectories
- similar unanswered questions

Do not treat simple word overlap as meaningful unless the contexts align.

Step 3: Trace the Bridges

For each promising bridge:

- Obsidian backlinks file="<bridging note>"
- Obsidian links file="<bridging note>"

Ask:

- is this bridge surface-level, structural, or foundational?
- does it connect the same word, or the same underlying pattern?
- does it change how one domain should be understood through the other?

Path-finding:
For the strongest bridges, identify the shortest plausible path between the domain hubs through the vault graph.

If intermediary notes appear important:

- Obsidian backlinks file="<intermediary note>"
- Obsidian links file="<intermediary note>"
- Obsidian read file="<intermediary note>" # only if needed to understand the bridge

Intermediary notes are often where the real insight lives.

Temporal analysis:
When possible, note whether the bridges are:

- Converging: recent links and overlap are increasing
- Diverging: older overlap exists but little recent integration
- Stable: consistent overlap over time

Use note dates or daily-note references when available.
Be cautious about over-claiming time trends if evidence is thin.

Step 4: Synthesize

Output format:

CONNECT: <Domain A> <-> <Domain B>
Notes in A's neighborhood: [number]
Notes in B's neighborhood: [number]
Bridges found: [number]
Trend: [Converging / Diverging / Stable / Unclear]

## Connection Map

For each bridge:

Bridge [#]: [Title]

- In Domain A:
  [How this note / concept / person appears]

- In Domain B:
  [How it appears differently]

- The connection:
  [What links them and why it matters]

- Depth:
  [Surface / Structural / Foundational]

- Implication:
  [What this suggests for one or both domains]

## Strongest Bridge

The single most interesting connection between the two domains.

## Missing Links

Connections that probably should exist but do not yet.
Suggest:

- which notes should link
- whether a bridge note may be warranted
- whether an MoC should reference something it currently misses

## The Question This Raises

What new question becomes visible only after seeing the domains together?

Guidelines:

- Cite specific notes and dates where possible.
- Prefer fewer, deeper bridges over many obvious ones.
- The value is in non-obvious connections, not surface similarity.
- If the two domains genuinely do not connect in the vault, say so clearly.
- Do not fabricate bridges that the graph and note evidence do not support.
