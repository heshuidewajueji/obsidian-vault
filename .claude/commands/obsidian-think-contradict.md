---
name: obsidian-think-contradict
description: Find incompatible beliefs held simultaneously in my Obsidian vault.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Contradict — Find Incompatible Beliefs Held Simultaneously

Purpose:
Surface beliefs in my Obsidian vault that cannot both be true at the same time. This is not idea evolution and not pressure-testing. It is about identifying beliefs that appear to coexist right now but logically cannot cohere.

Usage:

- /obsidian-think-contradict
- /obsidian-think-contradict [domain]

If a domain is provided, focus primarily on that area while still checking for cross-domain contradictions where relevant.

How This Differs

- /obsidian-think-challenge asks: "Is this belief wrong?"
- /obsidian-think-trace asks: "How did this idea evolve?"
- /obsidian-think-contradict asks: "You appear to believe X and also Y right now. Both cannot be true. Which is it?"

Step 0: Scope

If a [domain] is provided:

- Obsidian search query="<domain>"
- Obsidian search:context query="<domain>"
- Obsidian read file="My World.md"
- Obsidian links file="My World.md"

Use those results to identify the most relevant current notes, MoCs in \_MoC/, and daily-note material for that domain.

If no domain is provided, run generally across current context plus recent daily notes.

Step 1: Extract Beliefs

Read current context notes first.

Start with:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"

Then read the most relevant 4-6 linked notes, especially:

- active MoCs in \_MoC/
- active project notes
- active concept notes
- active person notes if relevant

Then read recent daily notes:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # past 14-21 days

From these sources, extract explicit and implied beliefs.

Look for:

- "I believe..."
- "I think..."
- "The way to..."
- "What matters is..."
- strong assertions
- strong advice
- repeated decisions
- stated priorities
- repeated criticism or rejection of something

For each extracted belief, represent it as:

- Belief: "[statement]"
- Source: [note name, date, context]
- Implied inverse: "[what this belief denies, excludes, or deprioritizes]"

Minimum target:

- extract at least 15-20 beliefs before attempting contradiction detection, unless the scoped domain is very small

Step 2: Run Contradiction Detection

Run all three methods independently.

Method 1: Explicit Contradictions

Within-domain:
Compare beliefs drawn from the same note set, same MoC, or same current area.
Use:

- Obsidian search:context query="<belief A statement>"
- Obsidian search:context query="<belief A inverse>"

Cross-domain:
Compare beliefs across different areas of the vault.
Use:

- Obsidian search query="<belief from domain A>" path="\_MoC"
- Obsidian search query="<belief from domain A>" path="\_Daily"

Look for:

- direct conflicts
- same question answered in opposite ways
- values that exclude each other

Method 2: Implicit Contradictions

Stated values vs revealed behavior:
Use daily notes to check whether behavior matches stated values.

Examples of useful searches:

- Obsidian search query="<stated value>" path="\_Daily"
- Obsidian search query="should" path="\_Daily"
- Obsidian search query="need to" path="\_Daily"
- Obsidian search query="decided" path="\_Daily"
- Obsidian search query="chose" path="\_Daily"

Look for:

- what the vault says matters vs what the daily notes show being chosen repeatedly
- advice given vs actual behavior
- priorities stated vs time/attention patterns implied by notes

Method 3: Priority Contradictions

Look for multiple things treated as "the most important" at the same time.

Search for priority language across current context:

- Obsidian search:context query="most important"
- Obsidian search:context query="priority"
- Obsidian search:context query="what matters most"

Look for:

- multiple top priorities that cannot all be top priority simultaneously
- incompatible operating principles across projects or domains
- central commitments that compete in a logically incoherent way

Step 3: Evolution Filter

Before reporting any contradiction, run this mandatory 4-test filter.

Test 1: Temporal Currency
Are both beliefs current?

Use:

- Obsidian search query="<older belief>" path="\_Daily"

If one belief appears only in old notes and not recently, it may be evolution rather than contradiction.
If so, mark it as:

- possibly resolved through evolution
  rather than an active contradiction.

Test 2: Domain Compartmentalization
Could both beliefs be true in different domains?
Do not report a contradiction if the beliefs are domain-appropriate and can reasonably coexist.

Test 3: Confidence Marker Check
If notes explicitly contain confidence markers or clearly differing certainty levels, weigh that.
A firm belief contradicting a tentative hypothesis is weaker than two equally firm beliefs colliding.

Test 4: Cannot-Both-Be-True Test
State both beliefs plainly.
Ask:

- Is there any reasonable interpretation where both can be true at the same time?

If yes:

- this is tension, not contradiction
  Do not report it as a contradiction.

Step 4: Classify Each Contradiction

For each contradiction that survives the evolution filter:

Category:

- Resolve
  - confused thinking or incoherent strategy; one belief likely needs revision
- Hold
  - a productive tension that should be named and held consciously
- Celebrate
  - evidence of growth; an old belief is still present in the vault but the newer one reflects real learning

Weight:

- Trivial
- Moderate
- Significant
- Foundational

Interpret these as:

- Trivial: interesting but low consequence
- Moderate: affects decisions in one area
- Significant: affects strategy or identity
- Foundational: undermines coherence in a major area if unresolved

Step 5: Output

Use this format:

CONTRADICT REPORT
Scope: [General / Domain-focused]
Beliefs examined: [number]
Contradictions found: [number]
Survived evolution filter: [number]

For each contradiction:

Contradiction [#]: [Title]

Belief A: "[Statement]"

- Source: [note, date]
- Confidence: [if available]

Belief B: "[Statement]"

- Source: [note, date]
- Confidence: [if available]

Why these cannot both be true:
[Explicit logical statement]

Category:
[Resolve / Hold / Celebrate]

Weight:
[Trivial / Moderate / Significant / Foundational]

What to do:
[Specific recommendation]

Then provide:

## Synthesis

- contradiction count
- by category
- by weight

## Deepest Contradiction

The contradiction that, if clarified, would simplify the most elsewhere.

## Pattern Analysis

Do the contradictions cluster?
Examples:

- stated values vs lived behavior
- ambitions vs constraints
- conflicting operating principles across domains

Anti-patterns to avoid:

- treating changed beliefs as contradictions when they are just evolution
- flattening domain nuance into fake contradiction
- manufacturing contradictions through loose paraphrase
- treating every contradiction as a problem rather than sometimes a productive tension
- doing therapy instead of structural logic analysis

Guidelines:

- be precise and specific
- always cite actual notes and dates
- the evolution filter is mandatory
- only report contradictions that genuinely fail the cannot-both-be-true test
- if the vault is currently coherent, say so clearly
