# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A collection of personal Claude Code slash commands (custom prompts). Each `.md` file is a prompt that can be invoked as a `/obsidian-<category>-<name>` command inside Claude Code sessions. These prompts are designed to work with an Obsidian vault via the Obsidian CLI.

Commands are grouped by category prefix: `day-` (daily rhythm), `mine-` (idea mining), `think-` (analysis), `vault-` (vault structure), `write-` (output generation), `grow-` (strategy).

## File Format

Every prompt file follows this structure:

```markdown
---
name: obsidian-<category>-<name>
description: <one-line description used for command discovery>
disable-model-invocation: false
allowed-tools: Read, Bash
aliases:
---

<prompt body>
```

The frontmatter `name` field must match the filename (e.g., `obsidian-day-today.md` → `name: obsidian-day-today`). The `description` is what surfaces in Claude Code's command picker. `allowed-tools` restricts which tools the prompt can invoke — current convention is `Read, Bash` for all vault-access prompts.

## Obsidian CLI Convention

All prompts assume the Obsidian CLI is available for vault access. The standard pattern is:

- Use `Obsidian <command>` first (faster, gives backlinks/tags/search natively)
- Fall back to direct filesystem reads if the CLI is unavailable
- Start exploration from `My World.md` as the root context note, then follow links

Common CLI commands referenced across prompts:
- `Obsidian daily:read` — today's daily note
- `Obsidian tags counts sort=count` — tag frequency
- `Obsidian orphans` / `Obsidian deadends` / `Obsidian unresolved` — structural gaps
- `Obsidian search query="<term>"` — full-text search
- `Obsidian backlinks file="<note>"` / `Obsidian links file="<note>"` — graph traversal

## Prompt Design Patterns

Each prompt follows a phased structure: gather vault evidence → synthesize → output actionable artifacts. Key conventions shared across prompts:

- **Vault as foundation, not ceiling**: prompts explicitly instruct the model to reason beyond what's in the vault
- **No external integrations**: prompts are text-output only — no dashboards, no HTML, no calendar writes
- **Evidence-cited output**: recommendations reference specific vault findings, not generic advice
- **Anti-pattern sections**: most prompts include explicit "do not do X" guardrails to prevent generic AI responses

## Environment Variables

Prompts that write directly to the vault using the `Write` or `Edit` tools (currently `obsidian-write-interview` and `obsidian-write-profile`) require `$OBSIDIAN_VAULT_PATH` to be set in the shell environment:

```sh
export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
```

Prompts that write via the Obsidian CLI (`obsidian-mine-graduate`, `obsidian-vault-backlinks`) use the CLI's own vault configuration and do not require this variable. Any new prompt that uses `Write file_path=` or `Edit file_path=` pointing into the vault must use `$OBSIDIAN_VAULT_PATH` as the root — never hardcode a path.

## Adding a New Prompt

1. Create `obsidian-<category>-<name>.md` with the frontmatter block above (pick a category from the list at the top)
2. Set `name` to match the filename exactly
3. Write a specific `description` — this is what the user sees in the command picker
4. Keep `allowed-tools: Read, Bash` unless the prompt needs additional tool access
5. Structure the body with numbered phases and explicit output format sections
