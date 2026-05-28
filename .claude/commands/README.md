# internet-vin-prompts

A collection of Claude Code custom slash commands for Obsidian vault management and personal knowledge work.

Each `.md` file in this repo is an invokable `/obsidian-<category>-<name>` command inside Claude Code sessions. The commands are designed around the [Obsidian CLI](https://github.com/obsidian-cli/obsidian-cli) and assume a specific vault structure.

---

## Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated
- Obsidian with the Obsidian CLI available in your shell path (`obsidian` should work as a command)
- A vault with:
  - `My World.md` — root context note used as the traversal anchor
  - `_Daily/YYYY-MM-DD.md` — daily notes folder
  - `_MoC/` — optional, for Maps of Content referenced by several commands
- `OBSIDIAN_VAULT_PATH` set in your shell for commands that write directly to the vault (`obsidian-write-interview`, `obsidian-write-profile`):
  ```sh
  export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
  ```

---

## Installation

Clone this repo into (or symlink it from) your Claude Code global commands directory:

```sh
# Option A: clone directly
git clone <repo-url> ~/.claude/commands/internet-vin-prompts

# Option B: symlink an existing clone
ln -s /path/to/internet-vin-prompts ~/.claude/commands/internet-vin-prompts
```

After that, all `/obsidian-*` commands appear in the Claude Code command picker in any session.

---

## Command Reference

### Daily Workflow

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-day-today` | Plan today using recent daily notes and active vault context | No | — |
| `/obsidian-day-close` | Process today's work, surface connections, and close the day | No | — |
| `/obsidian-day-7plan` | Shape the next 7 days around active intellectual threads | No | — |
| `/obsidian-day-weekly` | Surface vault material for a weekly learnings email | No | — |

### Idea Mining

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-mine-graduate` | Find ideas in daily notes ready to become standalone vault notes | Yes (with approval) | — |
| `/obsidian-mine-emerge` | Surface conclusions implied by the vault but never explicitly written | No | `[domain]` |
| `/obsidian-mine-ideas` | Generate ideas across multiple domains from vault + context | No | `[domain]` |
| `/obsidian-mine-make` | Identify what in your vault has enough depth to publish | No | — |

### Analysis

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-think-challenge` | Pressure-test current beliefs using your vault's own history | No | `[topic]` |
| `/obsidian-think-contradict` | Find logically incompatible beliefs held simultaneously | No | — |
| `/obsidian-think-trace` | Trace how a specific idea or belief has evolved over time | No | `[concept]` |
| `/obsidian-think-drift` | Compare stated intentions against actual behavior over 30–60 days | No | — |
| `/obsidian-think-leverage` | Find skills or knowledge that would unlock breakthroughs in 3+ areas | No | `[domain]` |
| `/obsidian-think-connect` | Map unexpected bridges between two separate domains | No | `[domain1] [domain2]` |

### Vault Structure

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-vault-map` | Analyze the topology of thinking across the entire vault | No | — |
| `/obsidian-vault-backlinks` | Identify high-value missing links and fix graph dead-ends | No | — |
| `/obsidian-vault-world` | Load full vault context as the foundation for a session | No | — |

### Output Generation

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-write-learned` | Draft writing on a topic at three depth levels (post, personal essay, universal essay) | No | `[topic]` |
| `/obsidian-write-interview` | Generate interview questions from vault article ideas | Yes (with approval) | — |
| `/obsidian-write-profile` | Audit and draft updates for a professional profile | Yes (with approval) | `[profile URL]` |

### Strategy

| Command | Description | Writes to Vault | Args |
|---|---|---|---|
| `/obsidian-grow-money` | Revenue advisor — diagnoses revenue system and surfaces opportunities beyond the vault | No | `[domain]` |

---

## Choosing a Command

**"What should I work on?"**
→ Start with `/obsidian-day-today` (today) or `/obsidian-day-7plan` (this week)

**"What ideas are ready to develop?"**
→ `/obsidian-mine-graduate` to promote daily note fragments → `/obsidian-mine-make` to find what's ready to publish → `/obsidian-write-learned` to draft it

**"Am I thinking clearly?"**
→ `/obsidian-think-challenge` to stress-test beliefs → `/obsidian-think-contradict` for logical inconsistencies → `/obsidian-think-drift` for intention vs. behavior gaps

**"What hidden connections exist?"**
→ `/obsidian-mine-emerge` for implied conclusions → `/obsidian-think-connect` for bridges across two domains → `/obsidian-think-leverage` for high-leverage learning bets

**"What does my vault look like structurally?"**
→ `/obsidian-vault-map` for topology → `/obsidian-vault-backlinks` to improve graph density → `/obsidian-vault-world` to load full context

**"What should I publish?"**
→ `/obsidian-mine-make` → `/obsidian-write-learned` → `/obsidian-write-interview`

**"How do I make more money?"**
→ `/obsidian-grow-money`

**Workflow patterns that work well together:**

- Daily close loop: `/obsidian-day-today` → work → `/obsidian-day-close`
- Weekly synthesis: `/obsidian-mine-graduate` → `/obsidian-mine-emerge` → `/obsidian-mine-make` → `/obsidian-write-learned`
- Thinking audit: `/obsidian-think-contradict` → `/obsidian-think-challenge` → `/obsidian-think-drift`

---

## Adding New Commands

See [CLAUDE.md](./CLAUDE.md) for the file format, frontmatter schema, Obsidian CLI conventions, and the design patterns shared across all prompts.
