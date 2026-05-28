---
name: obsidian-think-drift
description: Compare stated intentions against actual behavior across the vault over the past 30–60 days. Surface gaps between what I say matters and where my attention, energy, and note activity actually go. Find what is being avoided.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Drift — What Am I Avoiding?

Purpose:
Compare stated intentions against actual behavior across the vault over the past 30–60 days. Surface gaps between what I say matters and where my attention, energy, and note activity actually go. Find what is being avoided.

Usage:

- /obsidian-think-drift

Scope:
This version is vault-first only.
Do not use calendar, email, or message integrations.
Use:

- my-world.md
- linked context/project notes
- \_Daily notes
- task references if they exist in the vault

Step 1: Gather Stated Intentions

Read core context:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"
- Obsidian read file="<most relevant linked active notes>"

Extract:

- stated priorities and goals
- commitments made to self
- things marked important or urgent
- open questions flagged for investigation
- experiments proposed but not yet run
- projects in active/current status

Then inspect daily notes for stated intentions:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # past 30–60 days

Look for:

- "I need to..."
- "I should..."
- "I want to..."
- tomorrow priorities
- carry-forward items
- recurring intentions that keep reappearing

Build a list of explicit intentions before comparing against behavior.

Step 2: Gather Actual Behavior

Use daily notes and note activity as the primary behavior record.

Across the past 30–60 days of `_Daily` notes, look for:

- what topics come up repeatedly
- what topics appear once and vanish
- what gets energy (long entries, follow-through, multiple linked notes, visible momentum)
- what gets avoidance treatment (brief mentions, repeated “should,” no follow-through)
- what projects actually generate note activity and backlinks

Use CLI checks such as:

- Obsidian search query="<stated priority A>" path="\_Daily"
- Obsidian search query="<stated priority B>" path="\_Daily"
- Obsidian backlinks file="<priority project note>"
- Obsidian links file="<priority project note>"

If task syntax is used in the vault, also inspect tasks:

- Obsidian tasks daily
- Obsidian search query="- [ ]" path="\_Daily"

For each stated priority, estimate:

- frequency of mention
- quality of engagement
- evidence of follow-through
- whether it generated new notes, links, or decisions

Step 3: Detect Drift

For each stated priority or intention, compare:

- stated importance
- actual mention frequency
- actual follow-through
- recentness of engagement
- whether related work advanced or stalled

Classify the alignment roughly:

- Strong alignment:
  said it mattered and actually worked on it

- Partial alignment:
  active in thought, weak in execution

- Drift:
  repeatedly named as important but largely untouched

- Dropped:
  once important, now clearly deprioritized

Also look for:

- things getting attention that are not named priorities
- recurring “side threads” consuming energy
- intentions that keep reappearing without movement

Step 4: What’s Being Avoided

Identify the uncomfortable list:
things that still appear to matter but receive little or no action.

For each avoided item, report:

- What:
  the thing being avoided

- Evidence:
  how often it was mentioned vs what actual movement happened

- Possible why:
  one or more plausible causes, such as
  - fear
  - unclear next step
  - not actually important
  - too big / too vague
  - relational friction
  - missing skill or information

- Cost:
  what continued avoidance is likely costing

Apply this decision tree:

(a) Mentioned in the past 7 days?

- Yes = active avoidance
- No = passive drift / possible deprioritization

(b) Are related items getting done?

- If yes, this may be specific avoidance of this exact item
- If no, it may be a broader capacity or energy issue

(c) Does it have a clear next step?

- If no, ambiguity may be the blocker

(d) Does it require someone else?

- If yes, the block may be relational, not motivational

Step 5: Recurring Push Patterns

Look for loops:
the same intention appearing, getting postponed, then resurfacing again.

Use searches like:

- Obsidian search query="should" path="\_Daily"
- Obsidian search query="tomorrow" path="\_Daily"
- Obsidian search query="next" path="\_Daily"
- Obsidian search query="need to" path="\_Daily"

For each recurring push pattern, note:

- how many times it cycled
- what kind of item it is
- what likely keeps the loop going
- what would break the loop

Step 6: Honest Assessment

Write a direct but non-judgmental summary:

- what I said mattered
- what I actually touched
- what clearly drifted
- what appears intentionally dropped
- what appears avoided but still alive

The goal is clarity, not criticism.

Step 7: Recommended Corrections

For each significant drift, suggest one of:

- Drop it:
  if repeated avoidance suggests it is no longer a real priority

- Do it now:
  if it matters and the next step is already clear

- Define the next step:
  if the block is ambiguity, not resistance

- Delegate it:
  if it matters but I am unlikely to do it personally

- Reclassify it:
  move it from active priority to background / someday / not now

Output Format

DRIFT REPORT -- [Date range]

## Alignment Table

For each stated priority:

- Priority
- Stated importance
- Actual attention / vault activity
- Alignment score (1–10)
- Drift level (low / medium / significant)

## Unplanned Attention

What got disproportionate attention despite not being a named priority.

## What’s Being Avoided

For each item:

- what it is
- evidence
- likely reason
- cost
- decision-tree classification

## Recurring Push Patterns

What keeps looping without being resolved.

## Honest Assessment

A short, clear summary of the mismatch between intentions and behavior.

## Recommended Corrections

Specific recommended action for each significant drift item.

Guidelines:

- be honest but not harsh
- always cite specific dates, notes, and evidence
- the most valuable insight is often simple: “you said X mattered; you haven’t touched it in 3 weeks”
- do not confuse legitimate reprioritization with avoidance
- distinguish between:
  - dropped and should stay dropped
  - dropped but still costly
  - alive but avoided
