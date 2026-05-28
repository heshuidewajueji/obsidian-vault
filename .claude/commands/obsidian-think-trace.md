---
name: obsidian-think-trace
description: Trace how a specific idea, concept, belief, or theme has evolved across my Obsidian vault over time.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Trace how a specific idea, concept, belief, or theme has evolved across my Obsidian vault over time.

Treat the text I provide after the command as the <topic> to trace.

Goal:
Use the Obsidian CLI to efficiently find and follow the evolution of <topic>, not by brute-force reading the whole vault, but by targeted searches and graph traversal.

Step 0: Topic Input

- Let <topic> be the exact text I typed after /obsidian-think-trace.
- If I provided nothing, ask me to specify a topic and stop.

Step 1: Build a Vocabulary Map (Using CLI Where Helpful)

1. Start with <topic> exactly as given.

2. To find related language already in the vault:
   - Obsidian search query="<topic>"
   - Obsidian search:context query="<topic>"

3. From the most relevant matching notes (especially context notes and MoCs):
   - Extract 3–5 related terms, adjacent concepts, or alternative phrasings.
   - Note jargon, shorthand, or metaphors used for this idea.

4. Construct a vocabulary list:
   - primary: <topic>
   - synonyms: <synonym 1>, <synonym 2>, ...
   - related: <related concept 1>, <related concept 2>, ...

Step 2: Find Explicit Mentions via CLI

Using the vocabulary list, run focused CLI queries:

1. Global searches:
   - Obsidian search query="<topic>"
   - Obsidian search:context query="<topic>"

2. Synonyms and related terms:
   - Obsidian search query="<synonym or related term>"
   - Repeat for each item in the vocabulary list, but stop if results become obviously redundant.

3. Daily notes specifically:
   - Obsidian search query="<topic>" path="\_Daily"
   - Obsidian search query="<synonym or related term>" path="\_Daily" # only for the 1–2 most important synonyms

4. Context and MoC notes:
   - Obsidian search query="<topic>" path="\_MoC"
   - Obsidian read file="My World.md" # if useful for context
   - Obsidian links file="My World.md" # to see related notes

Collect the most relevant hits (do not try to use every single result). Prefer:

- notes that are more recent,
- context/MoC notes,
- daily notes where the topic is central.

Step 2.5: Implicit Pattern Detection (If Explicit Mentions Are Sparse)

If explicit searches return very few results:

1. Use slightly broader contextual searches:
   - Obsidian search:context query="<broader theme for the topic>"

2. Check tag distribution:
   - Obsidian tags counts sort=count

Look for:

- daily notes discussing the same underlying problem the idea addresses, even if they don’t name it,
- decisions or reactions that clearly embody the idea,
- repeated emotional patterns that this idea would explain.

Include these implicit notes as part of the evidence if they clearly relate.

Step 3: Follow the Graph with CLI (Bounded Depth)

For each high-signal note identified so far (especially the earliest and the most recent):

1. Inspect connections:
   - Obsidian backlinks file="<note name>" # what links TO this note?
   - Obsidian links file="<note name>" # what does this note link TO?

2. Follow 1–2 hops out when it clarifies the evolution of the idea:
   - Prioritize notes that:
     - appear in different time periods,
     - represent significant shifts (e.g., from “maybe” to “I’m sure”),
     - connect the topic to projects, people, or decisions.

3. Keep the traversal bounded:
   - Do not follow more than 2 hops from any given note.
   - Prefer quality of connections over quantity.

Step 4: Build a Chronological Timeline

Using all the evidence collected via CLI:

1. Order the key notes chronologically.

2. For each significant moment:
   - Date (from the note or file name, especially in \_Daily/).
   - Note name.
   - A short description of what I believed, questioned, or explored at that time about <topic>.
   - Any visible confidence signal (even if implicit: tentative, conflicted, confident).
   - Any apparent trigger (project, person, event, failure, success) if visible from context.

3. Pay attention to:
   - First appearance.
   - Long gaps (months with no mention).
   - Periods of rapid activity.
   - Reversals or contradictions.

Step 5: Identify the Arc

Synthesize the timeline into an evolution pattern:

- First Appearance:
  - When and where <topic> first shows up.
  - Whether it appears as a question, a hunch, a reaction, or a clear belief.

- Key Inflection Points:
  - Moments where my stance or understanding clearly changes.
  - What seems to have caused each shift (from notes, links, and backlinks).

- Current Position:
  - Where the thinking stands now.
  - How confident it seems.
  - What parts are settled vs. still open.

- Evolution Pattern:
  Choose one or more that best fit:
  - Linear deepening (same direction, more refined over time)
  - Pivot (fundamental change of direction)
  - Convergence (separate ideas merging)
  - Divergence (one idea splitting into several)
  - Circular (returning to the same unresolved question)

- Unresolved Tensions:
  - Notes that disagree with each other.
  - Questions that keep coming up without resolution.

Step 6: Deliver the Trace

Use this structure:

TRACE: <topic>

- First appeared: [date and note]
- Time span: [from first to most recent mention]
- Mentions considered: [approximate count of key notes]
- Velocity: [Accelerating / Steady / Sparse / Dormant] based on recent activity

## Timeline

Chronological bullets with:

- date
- note name
- 1–2 sentence summary of what I thought then

## Arc

A short narrative describing how the idea evolved.

## Tensions

Specific contradictions or unresolved questions across time.

## What’s Next

Based on the arc so far, what would meaningfully advance, clarify, or test this idea next.

Guidelines:

- Be specific and concrete.
- Refer to notes and dates where possible.
- Show changes in wording or stance so the evolution is visible.
- Do not smooth over contradictions; they are part of the value.
