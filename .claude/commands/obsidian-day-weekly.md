---
name: obsidian-day-weekly
description: Prepare ideas for a weekly learnings email based on my Obsidian vault.
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

Help me prepare ideas for a weekly learnings email based on my Obsidian vault.

Goal:
Use the Obsidian CLI to surface patterns, insights, and candidate topics from the past week. Output everything to the terminal. Do NOT create any files.

Step 1: Read Previous Weekly Learnings (Continuity)

Use the Obsidian CLI to find and read recent weekly learnings notes.

1. Find weekly learnings notes:
   - Obsidian search query="Weekly Learnings"

2. Read the two most recent:
   - Obsidian read file="<most recent weekly learnings>"
   - Obsidian read file="<second most recent weekly learnings>" # if it exists

From those notes extract:

- what was written about last time (to avoid repeating without development)
- threads that were opened but not resolved
- things promised ("I'll write more about this soon")
- tone and style: first person, reflective, connecting specific events to broader ideas

Step 2: Read Daily Notes (Past 7 Days)

Use the Obsidian CLI to read daily notes from the past 7 days.

1. Start with today:
   - Obsidian daily:read

2. Read the previous 6 days explicitly:
   - Obsidian read path="\_Daily/YYYY-MM-DD.md" # for each of the past 6 days

From these notes extract:

- what actually happened this week (not just planned)
- conversations or meetings that shifted thinking
- ideas that emerged with noticeable energy
- decisions made or direction changes
- problems encountered and how they were handled
- people mentioned and what the interactions were about
- frustrations, breakthroughs, and realizations
- anything surprising

If daily notes are sparse, expand the window to 10–14 days.

Step 3: Load Context Files

Use the Obsidian CLI to load core context so you can connect the week’s events to bigger directions.

1. Read my core context:
   - Obsidian read file="My World.md"

2. Inspect its connections:
   - Obsidian links file="My World.md"

3. Read the most relevant linked notes:
   - active MoCs in \_MoC/ that are linked from my-world.md
   - current project notes linked from my-world.md

Use these to see which events from the week connect to larger strategic questions or long‑running themes.

Step 3.5: Trace Idea Evolution for the Week

Use the Obsidian CLI to see how this week’s thinking connects across notes and to the broader vault.

1. Identify 2–4 recurring themes from this week’s daily notes.

2. For those themes:
   - Obsidian search:context query="<recurring theme>"
   - Obsidian search query="<recurring theme>"

3. For key notes that keep coming up:
   - Obsidian backlinks file="<key note>"
   - Obsidian links file="<key note>"

4. Check weekly theme distribution:
   - Obsidian tags counts sort=count

5. For each daily note this week, if useful:
   - Obsidian links file="YYYY-MM-DD" # see what each day connects to

Look for:

- ideas that appeared on multiple days (good weekly‑learning candidates)
- backlink chains where a conversation or meeting rippled into later thinking
- themes from daily notes that connect to open questions or priorities in context files
- notes from previous weeks that this week’s thinking builds on or contradicts

Step 4: Ignore External Systems for Now

Do not integrate calendar, email, or messaging for this version.
If relevant, rely on whatever summaries are already captured in daily notes.

Step 5: Synthesize — Output to Terminal

Print everything below directly to the terminal. Do NOT create any files.

Output Format:

WEEKLY LEARNINGS PREP — Week [number], [year]

FROM LAST EMAIL (threads to continue or resolve):

    [Thread from previous weekly learnings that developed further this week]
    [Promise made that can now be addressed]

CANDIDATE TOPICS (ranked by depth of thinking + relevance):

For each candidate:

[#]. [Topic name] — [Project or area]

    What happened:
        [Specific events, conversations, or decisions this week]
    The insight:
        [The non‑obvious thing worth sharing]
    Source:
        [Which daily notes or other notes contain the raw thinking; cite note names and dates]

List 5–8 candidate topics if possible.

CONNECTING THREAD:

If there is a theme that ties multiple candidates together this week, name it and describe it briefly. The best weekly learnings connect specific events to a bigger idea.

OPERATIONAL UPDATES (not learnings, but still useful):

    [Decisions, schedule changes, milestones, etc.]

SUGGESTED STRUCTURE:

Recommend 3–4 sections for the weekly learnings email based on what has the most depth, so that writing it should take no more than an hour.

Output Guidelines:

- Print everything to the terminal; do NOT create or modify files.
- Be specific: reference which daily note, which conversation, or which note the insight comes from.
- Prioritize insights over status updates.
- Match the existing tone: first person, reflective, connecting specifics to broader ideas.
