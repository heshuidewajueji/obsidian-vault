---
name: obsidian-mine-make
description: Scan the vault for ideas, perspectives, experiences, and tensions that have enough depth and originality to become real work. Surface what’s ready, what’s almost ready, and what natural forms each could take.

disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Make — What in Your Vault Should Become Something the World Sees?

Purpose:
Scan the vault for ideas, perspectives, experiences, and tensions that have enough depth and originality to become real work. Surface what’s ready, what’s almost ready, and what natural forms each could take.

Usage:

- /obsidian-mine-make
- /obsidian-mine-make [domain] # focus on a specific area

Related commands: `/obsidian-mine-graduate` (promote daily note fragments first) → `/obsidian-write-learned` (draft writing from identified candidates) → `/obsidian-write-interview` (generate structured interview questions)

Vault Access:
Use the Obsidian CLI for all reads and searches. Assume:

- daily notes live in `_Daily/`
- priorities and context start from `my-world.md` and its links

What Counts as a Candidate

Not a candidate:

- an idea that appears once
- a simple connection between two notes (that’s /obsidian-think-connect)
- an unnamed pattern implied by the vault (that’s /obsidian-mine-emerge)
- a brainstorm of possible projects (that’s /obsidian-mine-ideas)
- a daily-note insight that just needs promotion (that’s /obsidian-mine-graduate)
- an idea’s evolution over time (that’s /obsidian-think-trace)

Is a candidate:

- a topic with enough accumulated material to sustain a real work
- a perspective that diverges from conventional wisdom
- an experience documented over time with a clear arc
- an unresolved tension with substance on both sides
- something with evidence that others care (conversations, responses, resonance)

Test:
Could someone sit down today and draft something from what the vault already contains (plus focused shaping)? If yes or “almost,” it’s a candidate. If they’d start from scratch, it’s not.

Step 1: Density Detection

Goal:
Find topics where sustained attention has created depth.

Commands:

- Obsidian tags counts sort=count
- Obsidian search query="<top tag or theme>" path="\_Daily"
- Obsidian search:context query="<theme>"
- Obsidian backlinks file="<theme note if it exists>"

Look for:

- topics mentioned ~10+ times across 5+ days
- notes that keep getting longer or revisited
- themes appearing in `_Daily` and in context notes linked from `my-world.md`
- topics with multiple backlinks forming a cluster

Track per dense topic:

- approximate combined word count
- number of distinct days it appears
- how many domains (work/personal/projects) it spans

Step 2: Originality Detection

Goal:
Find perspectives that would add something new in public.

Commands:

- Obsidian search:context query="most people think"
- Obsidian search:context query="the problem with"
- Obsidian search:context query="what people miss"
- Obsidian search:context query="actually"
- Obsidian search:context query="wrong about"
- Obsidian search:context query="nobody talks about"
- Obsidian search:context query="counterintuitive"
- Obsidian search:context query="I believe"
- Obsidian search:context query="my position"
- Obsidian tags counts sort=count # then read the highest-signal notes

Look for:

- claims that push against mainstream consensus
- frameworks or models built from direct experience
- observations from unusual vantage points
- beliefs you hold strongly that most people wouldn’t
- contrarian positions backed by actual evidence

Track per perspective:

- what the conventional view likely is
- how this vault’s view differs

Step 3: Narrative Detection

Goal:
Find story arcs and turning points.

Commands:

- Obsidian search query="realized" path="\_Daily"
- Obsidian search query="changed my mind" path="\_Daily"
- Obsidian search query="used to think" path="\_Daily"
- Obsidian search query="turning point" path="\_Daily"
- Obsidian search query="before and after" path="\_Daily"
- Obsidian search query="learned" path="\_Daily"
- Obsidian search query="shifted" path="\_Daily"

For a promising topic:

- Obsidian search query="<topic>" path="\_Daily"
- Obsidian read file="\_Daily/YYYY-MM-DD" # key nodes in the arc

Look for:

- beliefs/approaches that changed, with before and after
- project/relationship arcs (struggle → resolution/ongoing tension)
- journeys captured in real time, not just summaries
- genuine surprise/recognition moments

Track:

- rough arc (beginning → change → current state)
- whether emotional texture survives in the notes

Step 4: Tension Detection

Goal:
Find unresolved questions with substance on both sides.

Commands:

- Obsidian search:context query="on one hand"
- Obsidian search:context query="not sure"
- Obsidian search:context query="torn between"
- Obsidian search:context query="the tension"
- Obsidian search:context query="both true"
- Obsidian search:context query="paradox"
- Obsidian search:context query="tradeoff"
- Obsidian search:context query="questioning"
- Obsidian search:context query="evolving"

Look for:

- recurring questions that resist simple answers
- positions held in productive tension
- tradeoffs explored from multiple angles
- documented experience of sitting with ambiguity

Track:

- both sides of each tension
- evidence/experience supporting each side

Step 5: Resonance Detection

Goal:
Find topics with evidence that others care.

Commands:

- Obsidian search:context query="people keep asking"
- Obsidian search:context query="conversation about"
- Obsidian search:context query="someone said"
- Obsidian search:context query="response to"
- Obsidian search:context query="resonated"
- Obsidian search:context query="they asked"
- Obsidian search:context query="DM"
- Obsidian search query="podcast" path="\_Daily"
- Obsidian search query="thread" path="\_Daily"
- Obsidian search query="posted" path="\_Daily"
- Obsidian search query="feedback" path="\_Daily"

Look for:

- conversations that generated energy or follow-up
- ideas with strong reactions (agreement or disagreement)
- questions others repeatedly ask you
- posts/threads that got traction and triggered more thinking

Track:

- what external signal showed up
- what kind of people reacted (roles, not necessarily names)

Step 6: Cross-Reference and Deduplicate

Merge across methods:

- density + originality → strong material + reason to exist
- narrative + tension → story + depth
- resonance + any other signal → audience plus substance

Optional already-published check (only if you maintain such a folder):

- Obsidian files folder="\_Published"
- Obsidian search query="<candidate topic>" path="\_Published"

Drop or downgrade candidates that are already fully expressed publicly unless the vault shows substantial new thinking since.

Previous runs:

- Obsidian search query="/obsidian-mine-make" path="\_Daily"

Note:

- previously surfaced and acted-on candidates
- previously surfaced and ignored candidates
- new candidates since the last run

Step 7: Score and Assess

For each candidate, score 1–5 on:

Depth:
1 = a few scattered mentions  
3 = several entries, some developed  
5 = extensive material across multiple notes/domains

Originality:
1 = common observation  
3 = somewhat distinctive  
5 = genuinely novel, backed by experience

Clarity:
1 = vague; hard to articulate  
3 = core idea findable but fuzzy  
5 = core idea expressible in 1–2 sentences

Urgency:
1 = mentions fading or dormant  
3 = steady  
5 = mentions increasing in frequency/intensity recently

Total: /20

Tiers:

- Ready (16–20): draftable now.
- Almost Ready (11–15): promising; needs specific work.
- Developing (6–10): worth tracking; not ready.

Step 8: Natural Form Assessment

For each candidate with score ≥ 11, propose 2–3 plausible forms.

Ask:

- Essay / long-form:
  can it sustain 1,500–3,000 words with existing evidence?
- Short post / provocation:
  is the idea sharper as a short hit?
- Audio / conversation:
  would dialogue unlock more than monologue?
- Visual / walkthrough:
  is there a strong visual or demo angle?
- Series:
  is it too big or too in-flux for a single piece?

For each form:

- why it fits the existing material
- what would be different about exploring it that way

Output Format

MAKE REPORT
Scope: [General / Domain-focused]
Detection methods run: [list]
Candidates found: [number]
New since last run: [number or "First run"]

Ready (16–20)

Candidate [#]: [Title]

Core idea:
[1–2 sentences]

Score:
Depth: X | Originality: X | Clarity: X | Urgency: X | Total: X/20

Detection methods:
[Density / Originality / Narrative / Tension / Resonance]

Evidence:

- [note, date, quote] — [why it matters]
- [note, date, quote] — [why it matters]
- [pattern/cluster] — [why it matters]

Natural forms:

- [Form 1]: [why this form fits]
- [Form 2]: [why this form fits; how it differs]
- [Form 3]: [if relevant]

What is still missing:
[very concrete gaps]

Almost Ready (11–15)

Same structure, with emphasis on:

- what exactly would move it into Ready

Developing (6–10)

Abbreviated:

- core idea
- total score
- what would need to accumulate or change

Synthesis

Strongest candidate:
[which one and why]

The surprise:
[a candidate you probably wouldn’t have picked for yourself and why it matters]

The thread:
[any meta-theme that connects multiple candidates; what it says about what this vault is really about]

Conspicuous absences:
[domains with lots of vault activity but no viable candidates yet, and why]

Anti-Patterns

Avoid:

- turning this into a content calendar
- generating titles/hooks/marketing copy
- prescribing a single “best” format
- inflating thin topics into candidates
- vague, generic candidate names
- over-optimistic readiness scoring
- duplicating what other commands handle better

Guidelines:

- every candidate must tie back to specific notes; no evidence, no candidate
- cite notes and dates so everything is auditable
- be ruthlessly honest about readiness
- “what is still missing” must be actionable, not vague
- treat this as a scouting report from someone who actually read the vault
- always include at least one genuine surprise candidate
