---
name: obsidian-day-7plan
description: Reshape the next 7 days around what’s alive in the vault.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

7plan — Reshape the Next 7 Days Around What’s Alive in the Vault

Purpose:
Use my Obsidian vault and the Obsidian CLI to understand what intellectual threads, rabbit holes, open questions, and near-breakthrough ideas are alive right now, then shape the next 7 days around them.

This version is vault-first only.
Do not use calendar, tasks, or email integrations.
Treat this as a 7-day planning and focus-allocation command, not a scheduling automation command.

Usage:

- /obsidian-day-7plan

Phase 1: Understand the Thinking (Vault-First)

The next 7 days should serve what is actually alive in the vault, not a generic priority list.

Step 1: Structural Inventory

Run:

- Obsidian tags counts sort=count
- Obsidian orphans
- Obsidian deadends
- Obsidian unresolved

Look for:

- most active themes
- ideas created but never connected
- referenced-but-never-developed concepts
- domains where thinking is accumulating but not synthesizing

Step 2: Read Daily Notes (Past 14 Days)

Use:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # for each of the past 14 days

If recent daily notes are sparse, expand to 21 days.

Look for:

- energy patterns: what produced flow vs what drained
- recurring questions or investigations
- intentions such as "I want to explore", "I need to think about", "I should write"
- threads that started and then went silent
- decisions made, rejected, or deferred
- tasks or concerns that are masking deeper thinking work

Step 3: Read Core Context

Read:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"

Then read the most relevant linked notes, especially:

- active MoCs in \_MoC/
- active project notes
- active concept notes
- active person notes if relevant

Extract:

- stated priorities
- open questions
- areas marked by uncertainty, drift, or unresolved thinking
- threads that appear alive but not yet clarified

Anything that looks evolving, uncertain, unresolved, or close to articulation is a candidate for protected thinking time.

Step 4: Deep Vault Exploration (Core Step)

For each active thread identified from daily notes, tags, and context:

Use:

- Obsidian search:context query="<thread>"
- Obsidian search query="<thread>"
- Obsidian backlinks file="<related note>"
- Obsidian links file="<related note>"

Follow useful links and backlinks 2-3 hops deep.

Build a map of:

- Active investigations
  - what rabbit holes are being explored
  - what state they are in
  - what would move them forward

- Near-breakthrough ideas
  - threads with enough accumulated thought to crystallize into
    - writing
    - a decision
    - a framework
    - a design
    - a note worth graduating

- Stalled threads
  - ideas that had energy but stopped
  - likely reasons: missing conversation, missing research, ambiguity, lack of protected time

- Convergence points
  - separate threads pointing toward the same conclusion
  - places where one focused session could unlock multiple areas

- Latent patterns
  - recurring themes across domains that have not yet been named clearly

Step 5: Behavioral Patterns

Use:

- Obsidian search query="decided" path="\_Daily"
- Obsidian search query="want to" path="\_Daily"
- Obsidian search query="need to write" path="\_Daily"
- Obsidian search query="cancelled" path="\_Daily"
- Obsidian search query="skipped" path="\_Daily"

Ask:

- what does behavior reveal about what actually matters?
- what keeps getting energy?
- what keeps getting postponed?
- what sounds important but is not truly alive?
- what hidden thread is driving multiple recent choices?

Output of Phase 1: The Intellectual Map

Before planning the week, present:

## Top Active Threads

For each:

- current state
- what would advance it
- estimated time needed
- type of work: writing, research, conversation, building, synthesis, decision

## Near-Breakthrough Candidates

Threads that deserve protected blocks this week because they are close to crystallizing.

## Stalled Threads Worth Reviving

Threads that may be worth restarting if time opens up, including what likely stalled them.

## Meta-Pattern

What the recent thinking appears to be converging toward.

Phase 2: Shape the Next 7 Days

Do not use calendar integrations.
Instead, create a vault-informed 7-day plan that allocates focus, depth, and intentionality.

For each of the next 7 days, provide:

### Day [N]

- Day theme:
  what this day should mainly serve, tied to one active thread

- Protected block:
  a 2-3 hour minimum deep-work block
  include:
  - the thread name
  - what to do in the block
  - what a successful session would produce

- Secondary support work:
  lighter tasks, follow-up, reading, linking, drafting, or admin that support the main thread without replacing it

- Avoid / protect against:
  what is likely to derail the day based on recent behavior patterns

- If energy is low:
  the lowest-friction useful version of the day

Week-level summary:

## Week Summary

- Which threads get protected time this week
- Which threads are intentionally deferred
- Where the highest-leverage deep work sits
- What the single most important creative or thinking session of the week should be
- What this 7-day shape is trying to produce by the end of the week

Phase 3: Optional Execution Suggestions

Do not edit files or external systems automatically.

Instead, suggest optional next actions such as:

- create a daily note prompt for each day theme
- create a short checklist for each protected block
- identify which note should be updated before each session
- suggest which stalled thread should be ignored on purpose this week

Guidelines:

- all output should be text in the session
- no dashboards, no HTML, no external integrations
- every planning recommendation should reference the intellectual map built from the vault
- protected blocks must be specific, not generic “focus time”
- be honest about what does not fit in the next 7 days
- optimize for depth and movement, not completeness
