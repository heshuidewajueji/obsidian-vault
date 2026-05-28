---
name: obsidian-vault-world
description: Establishes context for Claude Code within my Obsidian vault. Use when starting an Obsidian session.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---
# Context Loading

Your job is to build comprehensive context about the user before beginning any work. Read thoroughly and follow backlinks.

## Step 1: Core Context Files

Use the Obsidian CLI to read all context files:

```bash
Obsidian read file="README"                       # Vault overview and structure
Obsidian read file="[[My World]]"             # Entry point
Obsidian read file="[[My Workflow Context]]"     # Scheduling, workflow, preferences
```

## Step 2: Explore Directories

Use the Obsidian CLI to list and understand the contents of key folders:

```bash
Obsidian files folder="_Daily" # Daily notes
Obsidian files folder="_MoC" # Root MoCs 
```

**Note:** All atomic notes live in the vault root. Do not search subdirectories
other than _Daily/, _MoC/, and _Templates/.
## Step 3: Follow Backlinks

As you read each file, use the Obsidian CLI to follow backlinks and discover connections:

```bash
Obsidian backlinks file="<note name>"  # Find what links TO a note
Obsidian links file="<note name>"      # Find outgoing links FROM a note
Obsidian read file="<linked note>"     # Read linked notes
```

Continue following backlinks recursively until you have read all connected documents.

## Step 4: Recent Daily Notes

Use the Obsidian CLI to read the most recent daily notes (last 5-7 days):

```bash
Obsidian daily:read
Obsidian read path="_Daily/YYYY-MM-DD.md"  # for each past day
```

Understand:

- What the user has been working on
- What they've been thinking about
- Current priorities and blockers
- Recent decisions and shifts

## Step 4b: Recent Weekly Learnings

Use the Obsidian CLI to find and read the most recent 2-3 weekly learnings:

```bash
Obsidian search query="Weekly Learnings"
Obsidian read file="<most recent learnings>"
```

These capture how thinking is evolving week to week.

## Step 4c: Vault Structure & Hidden Connections

Use the Obsidian CLI to explore the vault's structure and surface things that aren't visible from reading individual files:

```bash
Obsidian orphans                    # Notes nothing links to (potentially forgotten or neglected)
Obsidian deadends                   # Notes with no outgoing links (isolated thinking)
Obsidian unresolved                 # Things referenced in [[brackets]] but never created (gaps)
Obsidian tags counts sort=count     # Theme distribution across the vault
```

Use this to understand:

- Which areas of thinking are well-connected vs. isolated
- What ideas have been started but not developed (orphans)
- What the user keeps referencing but hasn't formalized (unresolved links)
- Where attention is concentrated vs. sparse (tag distribution)

Include notable findings in the synthesis.

## Step 5: Synthesis

Once you have read everything, provide a brief synthesis:

1. **Current priorities** - What matters most right now
2. **Active projects** - What's in motion
3. **Open questions** - What's unresolved
4. **Recent shifts** - What's changed in thinking or approach

Then say: "Context loaded. What would you like to work on?"

## Notes

- If a specific domain is passed as an argument (e.g., `/context podcast`), prioritize that domain's files but still read the core context
- Pay attention to confidence markers: `[solid]`, `[evolving]`, `[hypothesis]`, `[questioning]`
- The goal is maximum context so the agent can work effectively without asking basic questions