---
name: obsidian-vault-map
description: Analyze the structure and shape of thinking across the entire vault using the Obsidian CLI. Show where ideas are concentrated, where the dead zones are, what is central vs peripheral, and what connections are missing.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Map — Vault Topology & Intellectual Landscape

Purpose:
Analyze the structure and shape of thinking across the entire vault using the Obsidian CLI. Show where ideas are concentrated, where the dead zones are, what is central vs peripheral, and what connections are missing.

Usage:

- /obsidian-vault-map

Step 1: Structural Analysis

Use the Obsidian CLI to get the broad structural picture of the vault.

Run:

- Obsidian tags counts sort=count
- Obsidian orphans
- Obsidian deadends
- Obsidian unresolved

Get link density for key notes:

- Obsidian backlinks file="<Hub Note A>"
- Obsidian backlinks file="<Hub Note B>"
- Obsidian links file="<Hub Note A>"
- Obsidian links file="<Hub Note B>"

Count and categorize vault structure:

- Obsidian files folder="\_Daily"
- Obsidian files folder="\_MoC"
- Obsidian files # total vault size

If useful, also inspect:

- Obsidian files folder="\_Templates"

Use this to estimate:

- total note count
- daily note volume
- MoC count
- attachment or non-note noise if visible
- where the graph is dense vs sparse

Step 2: Identify Clusters

Use the most connected notes, dominant tags, and recent active notes to identify the major clusters of thinking.

For the strongest candidate hub notes:

- Obsidian backlinks file="<highly connected note>"
- Obsidian links file="<highly connected note>"

For each cluster, estimate:

- central node / hub note
- approximate cluster size
- internal connection density
- whether the cluster connects well to other clusters or remains mostly isolated

Cluster relationship narrative:
Do not just list clusters. Describe how they relate.

Look for:

- clusters that probably should connect but don’t
- clusters bridged by a single note
- clusters that are appropriately separate
- larger superclusters or meta-themes formed by multiple connected clusters

Step 3: Find the Gaps

A. Missing Connections

Look for notes or clusters that share themes but are not linked.

Use:

- Obsidian search:context query="<theme from cluster A>"
- inspect whether it appears in cluster B’s territory
- repeat in both directions for high-value clusters

Look for:

- similar ideas under different names
- notes addressing the same problem but never linked
- recurring concepts that should probably have a bridge note or MoC link

B. Orphaned Value

Review orphaned notes selectively.

For each meaningful orphan, decide:

- genuinely unrelated and fine to leave isolated
- relevant but not yet connected
- forgotten insight now relevant again
- intentionally isolated vs accidentally abandoned

Use:

- Obsidian read file="<orphan note>" # only when the title suggests value
- Obsidian search:context query="<key concept from orphan>"

C. Unresolved Links

Review unresolved links from Step 1.

Ask:

- which unresolved links are worth creating as notes
- which represent important concepts that clearly want their own space
- which are duplicates, aliases, or weak one-offs that should be ignored

D. Dead Zones

Compare stated priorities against actual note density.

Read:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"
- Obsidian read file="<most relevant active linked notes>"

Look for:

- high-priority areas with surprisingly little note development
- areas that receive lots of attention but are not clearly strategic
- domains that are present as goals but thin as thinking

E. Tag-to-Priority Ratio

For the top tags by count, compare:

- note count
- whether the area is a stated priority in my-world.md or active context notes

Use a simple analysis:

- high tag count + low stated priority = attention sink candidate
- low tag count + high stated priority = dead zone candidate
- high tag count + high priority = aligned investment
- low tag count + low priority = background or dormant area

Step 4: Synthesize

Output format:

VAULT MAP -- [Date]

## Overview

- Total notes
- Daily notes
- MoCs
- Total unresolved links
- Total orphan notes
- Total deadends
- Dominant tags / themes

## Hubs

- most connected notes
- why they appear central

## Cluster Map

For each major cluster:

- Name
- Hub note
- Approximate size
- Density
- Health: Active and growing / Stable / Stagnant / Neglected
- Connections to other clusters

## Relationship Narrative

Describe:

- which clusters are strongly connected
- which are weakly bridged
- which should probably connect but don’t
- which are appropriately separate
- any superclusters or meta-themes

## Shape of Thinking

A narrative description of what the vault reveals about where attention and intellectual energy are going.

Address:

- what dominates
- what is underdeveloped relative to stated importance
- where thinking is accumulating but not synthesizing
- where the graph structure suggests hidden opportunities

## Gaps and Dead Zones

- missing links worth making
- unresolved links worth turning into notes
- orphan notes worth rescuing
- high-priority areas with low development
- attention sinks absorbing energy without strategic value

## Recommended Actions

Prioritized, specific actions:

- notes to create
- links to add
- MoCs to update
- orphan rescues
- clusters to bridge
- areas to intentionally ignore

Guidelines:

- be specific and name actual notes
- focus on actionable structural insights, not just statistics
- the map should change how I see the vault
- the most valuable findings are missing connections and mismatches between priority and development
- do not force everything into one graph; some separation is healthy
