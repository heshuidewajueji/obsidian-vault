---
name: obsidian-mine-emerge
description: Find ideas that are implied by my Obsidian vault but have never been explicitly articulated. Use Obsidian CLI structure and searches, not brute-force reading, to discover conclusions the vault’s own evidence points toward but that are not written down anywhere.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Emerge — Surface Ideas the Vault Implies but Never States

Purpose:
Find ideas that are implied by my Obsidian vault but have never been explicitly articulated. Use Obsidian CLI structure and searches, not brute-force reading, to discover conclusions the vault’s own evidence points toward but that are not written down anywhere.

Usage:

- /obsidian-mine-emerge # general
- /obsidian-mine-emerge [domain] # focused on a specific area

If a [domain] is provided, prioritize notes, MoCs, and context related to that domain.

What Counts as an Emergence

An emergence is NOT:

- a connection between two existing ideas (that’s /obsidian-think-connect)
- a contradiction between beliefs (that’s /obsidian-think-challenge or similar)
- an evolution of existing thinking (that’s /obsidian-think-trace)
- a restatement of something already in the vault

An emergence IS:

- a conclusion that follows from premises scattered across the vault, where the conclusion itself was never drawn
- a pattern that recurs across multiple domains but has never been named
- a belief that behavior reveals but that has never been articulated
- a direction that multiple threads point toward but that has never been identified as a destination

Step 0: Scope

If a [domain] argument was provided, interpret it as a topic or area:

- use Obsidian search query="<domain>"
- use Obsidian search:context query="<domain>"
- use Obsidian links file="My World.md"
- use Obsidian read file="My World.md"
  to find relevant notes, MoCs in \_MoC/, and project/context notes.

Prefer to run all later steps primarily within this scoped subset if domain-focused.

Step 1: Structural Detection (CLI-first)

Use structural CLI commands:

- Obsidian orphans
- Obsidian deadends
- Obsidian unresolved
- Obsidian tags counts sort=count

For interesting orphan or deadend notes:

- Obsidian backlinks file="<orphan note>"
- Obsidian links file="<deadend note>"

Look for:

- orphaned notes that address related problems but have no links between them
- unresolved links that look like meaningful concepts never developed
- clusters of deadend notes in the same area, suggesting thinking that never synthesized

Structural detection should rely on CLI topology first, then minimal reads where needed.

Step 2: Thematic Detection Across Domains

Read a small, focused set of context notes:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"
- Obsidian read file="<relevant MoC in _MoC/>" # a few key MoCs
- Obsidian read file="<current project notes>" # as needed

From these, identify candidate patterns or themes that seem to appear in multiple domains.

Then search with CLI:

- Obsidian search:context query="<pattern candidate>"
- Obsidian search query="<pattern candidate>" path="\_Daily"

Look for:

- the same structural or behavioral pattern in different domains under different names
- repeated tensions or values across work, personal, and creative areas that have never been named

Require at least 3 distinct domains or contexts before treating a pattern as a thematic emergence.

Step 3: Logical Detection (Premise → Unstated Conclusion)

Use recent daily notes and belief statements as the premise pool.

Read recent daily notes (14–21 days):

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # for past 14–21 days

Search for belief statements:

- Obsidian search:context query="I think"
- Obsidian search:context query="I believe"
- Obsidian search:context query="the problem is"
- Obsidian search:context query="what works"

Look for:

- Premise A in note 1 and premise B in note 2 that logically imply conclusion C, where C never appears as a statement.
- principles stated in one domain that, when applied to another domain, would yield a conclusion never written down.
- clusters of observations that clearly support a theory that has never been articulated.

Be strict: the conclusion must actually follow from the premises, not just be compatible.

Step 4: Behavioral Detection (Decisions → Unstated Beliefs)

Analyze decisions recorded in daily notes:

Use CLI:

- Obsidian search query="decided" path="\_Daily"
- Obsidian search query="chose" path="\_Daily"
- Obsidian search query="going to" path="\_Daily"
- Obsidian search query="not going to" path="\_Daily"
- Obsidian search query="cancelled" path="\_Daily"
- Obsidian search query="skipped" path="\_Daily"

Look for:

- consistent patterns where one kind of option is chosen over another
- things consistently avoided, revealing an unstated belief about risk or value
- what gets energy vs what gets procrastinated
- recurring “rules” followed but never written down

Treat a behavioral emergence as “a belief that this pattern of decisions implies but that has never been stated.”

Require several instances (3+), not one-off decisions.

Step 5: Convergence Detection (Multiple Threads → Same Unnamed Destination)

Focus on forward-looking text.

Use CLI:

- Obsidian search:context query="want to"
- Obsidian search:context query="building toward"
- Obsidian search:context query="future"
- Obsidian search:context query="goal"
- Obsidian search:context query="vision"
- Obsidian search query="next" path="\_Daily"

Look for:

- multiple projects or priorities that, if completed, would create the same outcome that has never been named
- separate aspirations converging on a common structure or direction
- incremental decisions that, projected forward, clearly converge on an unarticulated destination

Keep convergence findings clearly marked as lower-confidence and only when multiple independent threads align.

Step 6: Fabrication Check via CLI

For each candidate emergence, run a “already stated?” check:

- Obsidian search query="<emergence stated plainly>"

If any search returns that same idea explicitly articulated, it is not emergent. Discard or downgrade.

Also check for:

- whether the emergence is just a rephrasing of an existing context file sentence
- whether the pattern is already named somewhere

Step 7: Confidence and Reporting

Assign confidence based on:

- number of independent data points
- number of detection methods involved

Confidence:

- High: 5+ clear data points across 2+ methods
- Medium: 3–4 data points from 1–2 methods
- Low: 1–2 data points from a single method (speculative)

Method reliability order:
Structural > Behavioral > Thematic > Logical > Convergence

For each emergence, report:

Emergence [#]: [Title]

The idea:
Plain one-sentence statement of the emergent idea.

Detection method(s):
Which of Structural / Thematic / Logical / Behavioral / Convergence contributed.

Evidence:

- [Note name, date, quote] → why this supports it
- [Another specific note / structural feature / behavior] → why this supports it
- [Optional third data point]

Why it’s emergent:
Why this conclusion, pattern, or belief is not explicitly written anywhere despite the supporting evidence.

Confidence:
High / Medium / Low (with approximate data-point count)

What to do with it:
Should this be:

- a new belief in a context file,
- a question to investigate further,
- a name for an existing pattern,
- or something to leave unformalized for now?

Synthesis:

EMERGE REPORT
Scope: [General / Domain-focused]
Detection methods run: [list]
Potential emergences found: [number]
Survived fabrication check: [number]

[Emergences, ordered by confidence]

Strongest emergence:
The one with the most evidence across the most reliable methods.

Meta-emergence (if any):
Whether the emergences themselves form a pattern.

Anti-patterns to Avoid:

- presenting normal connections as emergences
- manufacturing emergences when the vault doesn’t support them
- rephrasing existing content as if it is new
- vague fortune-cookie statements
- over-interpreting one-off data points
- projecting external frameworks onto the vault

Guidelines:

- every emergence must trace back to specific vault evidence found via Obsidian CLI
- fewer strong emergences are better than many weak ones
- if nothing is truly emergent, that itself is a valid result
