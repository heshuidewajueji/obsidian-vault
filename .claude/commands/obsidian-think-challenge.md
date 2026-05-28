---
name: obsidian-think-challenge
description: Use my Obsidian vault’s own history to challenge current beliefs, surface contradictions, and pressure-test evolving ideas. This is not about being contrarian. It is about using accumulated thinking to find where the cracks are.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Challenge — Push Back on My Thinking

Purpose:
Use my Obsidian vault’s own history to challenge current beliefs, surface contradictions, and pressure-test evolving ideas. This is not about being contrarian. It is about using accumulated thinking to find where the cracks are.

Usage:

- /obsidian-think-challenge
- /obsidian-think-challenge [topic]

If a [topic] is provided, focus the analysis on that topic while still checking for cross-domain tensions.

Step 1: Identify What to Challenge

Case A: No topic provided

Read current context:

- Obsidian read file="My World.md"
- Obsidian links file="My World.md"
- Obsidian read file="<most relevant linked context/project notes>"

Also read recent daily notes:

- Obsidian daily:read
- Obsidian read path="\_Daily/YYYY-MM-DD.md" # for roughly the last 7–14 days

From these, extract explicit beliefs and positions.

Structured belief extraction:
For each daily note and context note, look for statements like:

- "I believe...", "I think...", "the issue is...", "what matters is..."
- "The way to...", "The problem with...", "What works is..."
- strong assertions, confident predictions, absolute statements
- advice that implies a belief
- repeated “this is how things are” statements

List these beliefs explicitly before evaluating them.
This avoids vague “the vault feels contradictory” impressions.

Case B: Topic provided

Use:

- Obsidian search query="<topic>"
- Obsidian search:context query="<topic>"

From the most relevant notes and recent daily entries found, extract the current belief(s) or position(s) about that topic in the same structured way.

Step 2: Find the Contradictions and Pressure Points

For each current belief or position identified in Step 1:

Use vault evidence to challenge it.

Useful CLI operations:

- Obsidian backlinks file="<note containing the belief>"
- Obsidian search:context query="<related or opposite concept>"
- Obsidian search query="<earlier version of this thinking>" path="\_Daily"

Look for:

- Self-contradictions:
  places where earlier notes say something that conflicts with the current belief.

- Abandoned positions:
  strong prior positions that quietly disappear, with no explicit update.

- Unearned confidence:
  beliefs effectively marked [solid] in tone but never tested against reality or data.

- Wishful thinking:
  hypotheses treated as conclusions without supporting evidence.

- Blind spots:
  important adjacent questions never asked,
  related topics that never appear in searches.

- Stale thinking:
  positions that have not been revisited despite new experience or information.

Temporal weighting:

- Recent contradictions (past ~3 months):
  active confusion; give them the most weight.

- Medium-term contradictions (3–12 months):
  may signal genuine evolution; check if the old position was consciously abandoned or just forgotten.

- Old contradictions (12+ months):
  usually represent growth; only flag if the old position is still implicitly controlling decisions or was never explicitly updated.

Cross-context comparison:
Check whether different context areas stake contradictory positions on the same topic.

Use:

- Obsidian search:context query="<belief statement>"

across context notes linked from my-world.md.

Step 3: Build the Challenges

For each significant tension found, construct a challenge:

Challenge [#]: [Short title]

Current position:
[What I currently believe, citing specific note and date]

The problem:
[What contradicts, complicates, or undermines this belief]

Evidence from the vault:

- [note name, date, quote] → why this conflicts
- [another note or daily entry] → how it complicates the story
- [temporal context] → how long this tension has existed

The question this raises:
[A real, non-rhetorical question that would need an answer to resolve the tension]

Severity:

- Crack:
  minor inconsistency, worth noticing but not urgent.
- Tension:
  real contradiction or unresolved conflict that affects decisions.
- Foundation risk:
  if this belief is wrong, a lot of other thinking built on it is also wrong.

Step 4: Synthesize

Patterns in the challenges:
Describe any meta-patterns, such as:

- evolving ideas not being tested in the real world,
- repeated theorizing without direct experience or feedback,
- differences between how similar topics are treated in different domains,
- recurring gaps between stated beliefs and observed behavior in daily notes.

The hardest question:
Identify the single most uncomfortable question the vault raises about current thinking — the one that is easiest to avoid and most valuable to face.

Recommended actions:
For each challenge, propose one concrete action that would help resolve or clarify it, such as:

- a conversation with a specific kind of person,
- an experiment to run,
- a note to write that explicitly updates or replaces an older belief,
- a decision to re-open and think through again,
- a question to hold intentionally for the next week.

Output Format

CHALLENGE REPORT
Scope: [General / focused on topic]
Beliefs examined: [number]
Challenges found: [number]

[Challenges, ordered by severity]

[Patterns]

[The hardest question]

[Recommended actions]

Guidelines:

- be honest, not harsh; aim for clarity, not criticism
- always cite specific notes and dates
- the best challenges feel like “I hadn’t noticed that” rather than “that’s unfair”
- do not challenge beliefs that feel [solid] unless the vault genuinely contains contradicting evidence
- this should feel like talking to the most honest, well-informed version of myself, grounded in my own notes
