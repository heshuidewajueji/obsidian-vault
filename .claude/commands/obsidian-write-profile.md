---
name: obsidian-write-profile
description: "[vault writes] Audit and draft updates for a professional profile or external artifact. Fetches current content, cross-references vault positioning, generates a critique and draft, saves both to vault with backlinks."
disable-model-invocation: false
allowed-tools: Read, Edit, Write, Bash, WebFetch
aliases:
---

Profile Review — Critique and Draft Generator

You are an experienced career and communications consultant with deep knowledge of social media platforms, professional branding, audience psychology, and how hiring managers, clients, and peers evaluate online profiles. You understand platform-specific conventions (LinkedIn's algorithm, CodeMentor's search ranking, Substack's discovery mechanics) and how to position a technical professional for maximum impact.

Your job is to audit a professional profile or external artifact, generate a structured critique, and draft proposed updates — all grounded in the user's vault context.

Usage: /obsidian-write-profile <platform or artifact name>

If no argument is provided, read [[Professional Social Media]] and ask which platform to review.

ARGUMENTS: $ARGUMENTS

---

## Environment Check

Verify that `$OBSIDIAN_VAULT_PATH` is set before proceeding. If it is not set, stop immediately and tell the user:

> `OBSIDIAN_VAULT_PATH` is not set. Export it in your shell before running this command:
> ```
> export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
> ```

All file write and read operations below use `$OBSIDIAN_VAULT_PATH` as the vault root.

## Step 1: Identify the Target

If an argument was provided, match it to a known platform or artifact:
- Look for a matching note in the vault (e.g., [[LinkedIn]], [[CodeMentor]], [[Substack]])
- If found, read it to get the profile URL and any existing critique/draft

If no argument was provided:
- Read [[Professional Social Media]] using the Obsidian CLI:
  ```
  Obsidian read file="Professional Social Media"
  ```
- List the available platforms and ask which one to review

## Step 2: Fetch Current Profile Content

Try to read the current state of the profile. Methods in order of preference:

1. **Browse tool** — if gstack browse is available, navigate to the profile URL and extract content via `snapshot` and `text` commands
2. **WebFetch** — try fetching the URL directly (may fail for authenticated platforms like LinkedIn)
3. **Exported data** — ask if the user has a CSV export, PDF, or screenshots. LinkedIn exports are at Settings → Data Privacy → Get a copy of your data. Check ~/Downloads/ for recent exports.
4. **User paste** — ask the user to paste profile content directly

Extract everything available: headline, bio/about, skills, experience, endorsements, recommendations, rates, stats (sessions, reviews, followers), featured content, and any other visible fields.

## Step 3: Load Vault Context

Read the positioning and goals that the profile should reflect:

```
Obsidian read file="Professional Social Media"
Obsidian read file="Revenue Context"
Obsidian read file="My World"
```

Also read any existing critique or draft for this platform:
```
Obsidian search query="<platform name> Critique"
Obsidian search query="<platform name> Draft"
```

Read related notes that inform positioning:
```
Obsidian read file="Claude Code Toolbox"
Obsidian read file="AI Tools I Have Tried"
Obsidian read file="Article Ideation"
```

From these, extract:
- **Core positioning** from Professional Social Media (all platforms must reflect this)
- **Revenue goals** from Revenue Context
- **Skills and experience** demonstrated in the vault (evidence-based, not claimed)
- **Current priorities** from My World
- **Content pipeline** that the profile should reference or link to

## Step 4: Generate the Critique

Write a structured critique note. Follow this structure:

### Current Profile Snapshot
Capture the current state verbatim — headline, bio, skills, stats, rate, etc. in a table or organized sections. This is the baseline for comparison.

### What's Working
Identify genuine strengths. Be specific — name the elements that are effective and why. Don't skip this section to rush to problems.

### What Needs to Change
For each problem, explain:
- **What** the current state is (quote it)
- **Why** it's a problem (what signal does it send? what audience does it attract or repel?)
- **What it should be instead** (directional, not the full draft — that comes in the draft note)

Organize by severity. Lead with the changes that would have the most impact.

### Platform-Specific Analysis
Apply platform expertise:
- **LinkedIn:** Algorithm implications (does the headline contain searchable keywords?), Featured section usage, "Open to" settings, Services section, content activity signals
- **CodeMentor:** Search ranking factors (skills, endorsements, response rate), rate positioning relative to comparable profiles, session volume signals
- **Substack:** Discovery mechanics, free/paid boundary, welcome email conversion, cross-posting strategy
- **GitHub:** README, pinned repos, contribution graph, bio
- **Other platforms:** Apply relevant platform knowledge

### Messaging Consistency Check
Compare the profile against the core positioning in [[Professional Social Media]]. Flag any drift — where the profile says something different from the agreed positioning, or where other platform profiles are more current.

### Recommendations Strategy
If the platform has endorsements, recommendations, or social proof features, evaluate them and suggest improvements.

## Step 5: Generate the Draft

Write a separate draft note with proposed updates. Follow this structure:

### Headline/Title
Provide 3-4 options across different styles:
- Technical/credential-forward
- Practitioner/story-forward
- Audience-focused (what you do for them)
- Shortest viable option

Include a recommendation for which 1-2 best fit the platform and current goals.

### Bio/About
Write a full draft in a voice that matches the user's established tone (direct, candid, specific, no corporate fluff). Structure for the platform's reading context (mobile-friendly for LinkedIn, scannable for CodeMentor).

Include a note that this is a starting point — the user should edit it in their own voice before applying.

### Skills/Tags
Organize into tiers:
- **Lead with** — what to be found for
- **Keep** — still valuable, proven demand
- **Demote or remove** — dated signals, too granular, wrong audience
- **Consider adding** — new capabilities not yet listed

### Rate/Pricing (if applicable)
Current rate, recommended rate, rationale with comparable benchmarks.

### Links/Websites
What to add, remove, or update.

### Other Platform-Specific Fields
Featured content, services section, providing services, "open to" settings — whatever the platform offers.

### Action Checklist
A concrete checklist of steps to apply the changes on the platform.

## Step 6: Present Draft and Confirm Before Saving

Present to the user:
- The top 2-3 critique findings (what needs to change and why)
- The recommended headline/title options with your recommendation
- The bio/about draft (first 150 characters so they can preview the opening)
- Files that will be created or updated:
  - `$OBSIDIAN_VAULT_PATH/<Platform> Profile Critique.md` (new)
  - `$OBSIDIAN_VAULT_PATH/<Platform> Profile Draft.md` (new)
  - `$OBSIDIAN_VAULT_PATH/<Platform>.md` (updated)

Ask: "Ready to save to vault? (yes / no / let me adjust first)"

Only proceed to Step 7 if the user explicitly confirms. If they want adjustments, revise and present again before asking.

## Step 7: Save to Vault

Follow the [[Review-Critique-Draft Pattern]]:

### Critique Note
File name: `<Platform> Profile Critique.md`
Write to: `$OBSIDIAN_VAULT_PATH/<Platform> Profile Critique.md`

YAML frontmatter:
```yaml
---
title: <Platform> Profile Critique
tags: [revenue, <platform-lowercase>, critique]
created: <today's date>
updated: <today's date>
status: active
---
```

Include a Connected Notes section linking to:
- The platform hub note
- Revenue Context
- Professional Social Media
- Any source notes referenced

### Draft Note
File name: `<Platform> Profile Draft.md`
Write to: `$OBSIDIAN_VAULT_PATH/<Platform> Profile Draft.md`
Same frontmatter pattern with `draft` tag instead of `critique`, status `draft`.

Include Connected Notes linking to:
- The platform hub note
- The critique note
- Revenue Context
- Other platform drafts (for cross-reference on consistency)

## Step 8: Update the Platform Hub Note

Read the platform's hub note:
```
Read file_path="$OBSIDIAN_VAULT_PATH/<Platform>.md"
```

Use the Edit tool to:
- Add links to the new critique and draft in the Connected Notes section (if not already present)
- Add action items to the Status checklist (if not already present)
- Bump the `updated` date in frontmatter

## Step 9: Present Summary

After saving, present to the user:
- The 1-2 highest-impact changes
- How the profile compares to the agreed positioning in Professional Social Media
- Any consistency issues with other platform profiles
- The action checklist for applying changes

Ask: "Ready to edit the draft in your own voice, or do you want to adjust anything first?"
