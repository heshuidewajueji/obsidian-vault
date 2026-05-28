---
name: obsidian-write-interview
description: "[vault writes] Generate interview questions for an article from Article Ideation. Mines the vault for raw material, identifies the non-obvious angle, proposes titles, drafts structured interview questions, and saves to the vault."
disable-model-invocation: false
allowed-tools: Read, Edit, Write, Bash
aliases:
---

Article Interview Question Generator

Generate structured interview questions for an article idea. Takes an article identifier from [[Article Ideation]] (e.g., "Article E", "Article G"), mines the vault for raw material, identifies the non-obvious angle, proposes titles, drafts interview questions organized by theme, and saves everything to the Obsidian vault.

Usage: /obsidian-write-interview <article identifier or topic>

If no argument is provided, read [[Article Ideation]] and ask which article to work on.

ARGUMENTS: $ARGUMENTS

---

## Environment Check

Verify that `$OBSIDIAN_VAULT_PATH` is set before proceeding. If it is not set, stop immediately and tell the user:

> `OBSIDIAN_VAULT_PATH` is not set. Export it in your shell before running this command:
> ```
> export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
> ```

All file write and read operations below use `$OBSIDIAN_VAULT_PATH` as the vault root.

## Step 1: Load the Article Idea

Read [[Article Ideation]] using the Obsidian CLI:

```
Obsidian read file="Article Ideation"
```

Find the matching article entry. Extract:
- The working title
- The description / angle
- The listed source notes
- Any author commentary (check the "My Thoughts" section for caveats or concerns about this article)

If the article identifier doesn't match anything in Article Ideation, list the available articles and ask which one to use.

## Step 2: Mine the Vault for Raw Material

Read every source note listed in the article's entry:

```
Obsidian read file="<source note>"
```

Then search for related material the article description doesn't reference:

```
Obsidian search query="<key concept from article>"
Obsidian backlinks file="<each source note>"
```

Also read any existing interview question files to understand what material has already been covered by prior articles:

```
Obsidian read file="Writing Context"
```

Read each interview question file listed there. This prevents duplicate questions and enables cross-article references.

Look for:
- **Concrete stories** with specific details, numbers, or timelines
- **Failures and frustrations** — these are often the most compelling material
- **Surprises** — moments where reality diverged from expectations
- **Patterns** — recurring themes across multiple vault notes
- **Gaps** — things the article will need that don't exist in the vault yet (these become interview questions)

## Step 3: Identify the Non-Obvious Angle

Before writing questions, force this analysis:

1. **What does the reader expect from this title?** (the obvious take)
2. **What does the vault actually contain?** (the real story)
3. **Where's the gap between 1 and 2?** That gap is where the article lives.
4. **What would surprise someone?** What part of this is counterintuitive?
5. **Does the author have caveats about this article?** (from "My Thoughts" in Article Ideation) — if so, the caveat itself might be the real angle.

Also check: how does this article relate to the others in the series? What does the reader need to have read first? What does this article set up for later?

## Step 4: Generate Title Candidates

Produce 10-12 title candidates organized into groups:

- **Direct practitioner voice** (3-4) — honest, first-person, matches the author's established tone
- **Contrarian / challenges the narrative** (3-4) — pushes back on conventional wisdom
- **Punchy / shareable** (2-3) — optimized for LinkedIn sharing and click-through
- **Outcome-focused** (2-3) — leads with what the reader will get

Follow LinkedIn conventions from [[LinkedIn Post Best Practices]]:
- Hook must work in ~210 characters (LinkedIn truncation point)
- No markdown rendering — what you type is what appears
- Titles should be specific enough to promise value but open enough to invite curiosity

Include a recommendation for which 1-2 titles best fit the author's voice and the article's position in the series.

## Step 5: Inventory Existing Material

Create a table of material already in the vault that's relevant to this article:

| Use Case / Story / Pattern | Source | Concreteness (High/Medium/Low) |
|---|---|---|

This helps the author see what's ready to use and what needs to be generated via interview.

## Step 6: Draft Interview Questions

Write 20-24 interview questions organized into themed sections. The section structure should emerge naturally from the article's topic, but always include:

- **An origin/discovery section** (3-4 questions) — how did this start? What triggered it?
- **The core content sections** (10-14 questions) — the meat of the article, organized by subtopic
- **An honest assessment section** (3-4 questions) — failures, costs, things that didn't work
- **A through-line question** (1) — "if this article has a single thesis, what is it?"

Question design principles:
- **Ask for stories, not opinions.** "Walk me through a specific time when..." beats "What do you think about..."
- **Ask for the version they'd tell a friend, not a conference audience.** Casual voice produces better material.
- **Reference specific vault material.** "You mentioned X in [note] — tell me more about that" shows the questions are grounded in real context, not generic.
- **Include the uncomfortable questions.** Failures, costs, things that didn't work. These produce the most distinctive content.
- **End sections with open-ended prompts** that let unexpected material emerge.

## Step 7: Present Draft and Confirm Before Saving

Present to the user:
- The recommended title(s) and the non-obvious angle in 2-3 sentences
- The full list of interview questions by section (count and section names)
- The editorial notes summary
- Files that will be created or updated:
  - `$OBSIDIAN_VAULT_PATH/<filename>.md` (new interview questions file)
  - `$OBSIDIAN_VAULT_PATH/_MoC/Writing Context.md` (updated)

Ask: "Ready to save to vault? (yes / no / let me adjust first)"

Only proceed to Step 8 if the user explicitly confirms. If they want adjustments, revise and present again before asking.

## Step 8: Write Editorial Notes

Add a "Claude's Pre-Interview Editorial Notes" section at the end with:

1. **Strongest material before the interview** — what's already in the vault that's ready to use
2. **Key tension to exploit** — the central conflict or surprise that gives the article its edge
3. **Gaps to fill during interview** — what the vault doesn't have that the article needs
4. **Relationship to other articles** — how this fits in the series arc
5. **Structural suggestion** — a proposed article structure (acts, sections) based on the available material

## Step 9: Save to Vault

Determine the file name. Convention: `<Topic> Article Interview Questions.md`
Match existing patterns from the vault (e.g., "Skeptic Article Interview Questions", "Migration Article Interview Questions").

Write the file to the vault root using the Write tool:

```
Write file_path="$OBSIDIAN_VAULT_PATH/<filename>.md"
```

The file MUST include YAML frontmatter:

```yaml
---
title: <Topic> Article Interview Questions
tags: [writing, ideation, interview]
created: <today's date>
updated: <today's date>
status: active
---
```

The file MUST include a Connected Notes section at the bottom linking back to:
- [[Article Ideation]]
- All prior interview question files
- All source notes referenced
- [[Writing Context]]

## Step 10: Update Writing Context

Read the Writing Context MoC file directly:

```
Read file_path="$OBSIDIAN_VAULT_PATH/_MoC/Writing Context.md"
```

Use the Edit tool to add the new interview questions file to the Core Notes section, maintaining alphabetical order among the interview question entries. Also bump the `updated` date in the frontmatter.

## Step 11: Present Summary

After saving, present to the user:
- The recommended title(s)
- The non-obvious angle in 2-3 sentences
- Count of questions generated
- Key gaps the interview needs to fill
- How this article fits in the series arc

Ask: "Ready to start answering the interview questions, or do you want to adjust anything first?"
