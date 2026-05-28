---
name: obsidian-think-leverage
description: Find the breakthroughs hiding in plain sight. Use my Obsidian vault plus the Obsidian CLI to identify specific skills, knowledge domains, investigations, and mental models that, if developed, would produce disproportionate breakthroughs across multiple areas of my life or work.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Leverage — Find the Breakthroughs Hiding in Plain Sight

Purpose:
Use my Obsidian vault plus the Obsidian CLI to identify specific skills, knowledge domains, investigations, and mental models that, if developed, would produce disproportionate breakthroughs across multiple areas of my life or work.

Usage:

- /obsidian-think-leverage
- /obsidian-think-leverage [domain]

If a [domain] is provided, focus constraint mapping primarily on that area while still considering cross-domain effects.

Definition:
A true leverage point is something where 50–100 hours of focused learning or practice would unlock meaningful progress in 3+ areas simultaneously. If it only helps one project, it’s useful but not leverage.

Phase 1: Constraint Mapping (Vault-Driven)

Step 1: Structural Scan (CLI-only)

Run:

- Obsidian tags counts sort=count
- Obsidian orphans
- Obsidian deadends
- Obsidian unresolved

Use these to identify:

- where a lot of notes exist but remain under-linked or unresolved
- themes that dominate the vault
- structurally weak areas (orphans, deadends) around active topics

Step 2: Context Files (Goals and Blockers)

Read core context and active MoCs:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"
- Obsidian read file="<relevant MoCs in _MoC/>"
- Obsidian read file="<active project notes>"

From these, extract:

- goals with no clear path forward
- items marked as questioning/evolving/uncertain
- stated blockers, recurring frictions, or “stuck” language
- things described as important but not progressing

Step 3: Daily Notes (Past ~30 Days)

Use CLI:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # for roughly the past 30 days

From these, extract:

- repeated frustrations or friction (3+ appearances)
- abandoned attempts or projects quietly dropped
- admiration/aspiration signals (“I wish I could…”, “I need to learn…”)
- “need to learn”, “don’t know how”, similar phrases
- energy patterns: what produces flow vs. drain
- requests for help or dependence on others
- decisions repeatedly deferred

Step 4: Behavioral Archaeology via Search

Run focused searches in daily notes:

- Obsidian search query="stuck" path="\_Daily"
- Obsidian search query="frustrated" path="\_Daily"
- Obsidian search query="can't" path="\_Daily"
- Obsidian search query="don't know how" path="\_Daily"
- Obsidian search query="need to learn" path="\_Daily"
- Obsidian search query="wish I" path="\_Daily"
- Obsidian search query="asked" path="\_Daily"
- Obsidian search query="delegated" path="\_Daily"
- Obsidian search query="waiting on" path="\_Daily"
- Obsidian search query="blocked" path="\_Daily"

Admiration/aspiration:

- Obsidian search query="impressive" path="\_Daily"
- Obsidian search query="genius" path="\_Daily"
- Obsidian search query="how did" path="\_Daily"
- Obsidian search:context query="want to be"
- Obsidian search:context query="get better at"

Use these results to refine:

- recurring frictions
- capability gaps
- dependencies on others
- unresolved “I wish I could…” themes

Step 5: Project and Domain Audit

From my-world.md and linked project notes in \_MoC/:

For each active project:

- identify its current binding constraint (skill, knowledge, decision, relationship, time, etc.)
- check how many other projects share the same constraint

Use CLI as needed:

- Obsidian search:context query="<constraint>" # see where else it appears
- Obsidian search query="<constraint>" path="\_Daily"

Constraints that appear across 3+ projects/domains are high-leverage candidates.

Phase 1 Output: Constraint Map

Summarize:

- Recurring Frictions
- Capability Gaps
- Dependency Points
- Decision Bottlenecks

with vault evidence for each.

Phase 2: Leverage Detection (Vault-Based)

Method 1: Bottleneck Convergence (Highest Reliability)

From the constraint map:

- list each constraint and all projects/domains it touches
- rank by number of domains affected

Use CLI to confirm cross-domain presence:

- Obsidian search:context query="<constraint>"
- Obsidian search query="<constraint>" path="\_Daily"

Constraints that block 3+ areas become top leverage-point candidates.

Method 2: Adjacent Possible

Search for partially developed skills/knowledge:

- Obsidian search:context query="started learning"
- Obsidian search:context query="basics of"
- Obsidian search:context query="should go deeper"
- Obsidian search query="tutorial" path="\_Daily"
- Obsidian search query="course" path="\_Daily"

Look for:

- skills you already use but haven’t formalized or deepened
- knowledge referenced but never systematically studied
- tools used shallowly despite deep capabilities

Method 3: Multiplier Models

Search for repeated decision/strategy struggles and hints of missing frameworks:

- Obsidian search query="framework"
- Obsidian search query="model"
- Obsidian search:context query="thinking about"
- Obsidian search query="book" path="\_Daily"
- Obsidian search query="read" path="\_Daily"

Look for:

- recurring mistakes
- slow, painful decisions
- admired thinkers or books not yet internalized

Method 4: Removal Leverage

Search for repetitive manual work:

- Obsidian search query="every day" path="\_Daily"
- Obsidian search query="routine" path="\_Daily"
- Obsidian search query="always have to" path="\_Daily"
- Obsidian search query="manual" path="\_Daily"
- Obsidian search query="repetitive" path="\_Daily"

Identify:

- tasks that consume significant time/energy
- whether automation/delegation/removal would free up high-value time

Method 5: Relationship Leverage

Search for relationship-related notes:

- Obsidian search query="meeting with" path="\_Daily"
- Obsidian search query="conversation with" path="\_Daily"
- Obsidian search:context query="network"
- Obsidian search:context query="mentor"
- Obsidian search:context query="advisor"

Look for:

- relationships underutilized due to capability gaps
- domains where more knowledge would sharply increase relationship value

Method 6: Identity Leverage (Use Sparingly)

Search for self-description vs goals:

- Obsidian search:context query="I am"
- Obsidian search:context query="I'm not"
- Obsidian search:context query="not my strength"
- Obsidian search:context query="someone who"

Compare to stated goals and trajectories from my-world.md and project notes.
Only treat identity gaps as leverage points if multiple other methods point the same way.

Phase 3: Beyond the Vault (Imported Leverage)

Using the constraints from Phase 1 and goals from my-world.md:

- identify fields/domains the vault never mentions but which clearly address those constraints
- consider adjacent fields, foundational disciplines, counter-intuitive domains, historical parallels

This phase is not limited to vault content.
The vault is the diagnostic; solutions may come from any external field.

Phase 4: Verification Filters

For each candidate leverage point, apply:

1. 3+ Domain Test:
   Does it unlock 3+ projects or domains? If not, downgrade to “useful skill” rather than leverage.

2. Evidence Test:

- vault-sourced: is there clear vault evidence of the constraint?
- beyond-vault: is the constraint clearly shown in the vault, even if the solution isn’t?

3. Feasibility Test:
   Can this realistically be developed in ~50–100 focused hours? If not, mark as long-term.

4. Counterfactual Test:
   If I had this 6 months ago, what specifically would have gone differently? If you cannot name real differences, treat it as weaker leverage.

5. Already-Tried Test:

- Obsidian search query="<leverage point name>" path="\_Daily"
- Obsidian search query="<related skill/topic>" path="\_Daily"

Was it attempted and dropped? If so, consider whether the real leverage is fixing whatever blocked development.

6. Compound Test:
   Does this capability compound over time (gets more powerful as it’s used)?

Phase 5: Output

Goal: 3–7 leverage points, not more.

For each leverage point:

Leverage Point [#]: [Name]

- What it is:
  [Specific skill, domain, model, or capability]

- Source:
  [Vault-sourced / Beyond-vault / Blind spot]

- Constraint it breaks:
  [Which friction/bottleneck, with concrete vault evidence]

- Domains it unlocks:
  [List projects/domains and how each would benefit]

- Counterfactual:
  [What would have gone differently in the last 6 months]

- How to develop it:
  [Concrete path: specific book/course/person/project sequence for ~50–100 hours]

- Compound potential:
  [How it becomes more valuable over time]

- Feasibility:
  [Realistic hours and tradeoffs]

- Confidence:
  [High / Medium / Low, with a short rationale]

Then provide:

## Ranking

Order leverage points by:
(domains unlocked × constraint severity × compound potential) / effort

## #1 Leverage Point

The single best candidate for the next 60–90 days of focused effort.

## The Surprising One

A leverage point that comes from outside the vault’s current frame, clearly tied back to vault constraints.

## Anti-Leverage List

List skills/domains that feel attractive but have weak or no connection to current constraints, with a short “why not” for each.

## Temporal Tracking

If prior leverage runs exist (look for `/leverage` mentions in \_Daily/), briefly compare:

- whether earlier leverage points were developed
- whether they performed as expected
- whether any old unresolved leverage looks overdue now

Guidelines:

- Always tie leverage suggestions back to concrete vault constraints.
- Do not limit solutions to what the vault already mentions.
- Prefer fewer, stronger leverage points over long lists.
- Be specific enough that I could start tomorrow.
- Be honest if the obvious leverage point is something I’ve been avoiding.
