# INSAIT Claude Commands & Skills

Shared Claude Code slash commands and skills for the INSAIT team.

---

## Installation

### Commands (slash commands)

Copy command files into `~/.claude/commands/`:

```bash
mkdir -p ~/.claude/commands

# insait-test-csv
curl -o ~/.claude/commands/insait-test-csv.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-test-csv.md
```

Restart Claude Code — the command appears as `/insait-test-csv`.

### Skills (richer slash commands with full playbook context)

Copy skill files into `~/.claude/skills/`:

```bash
mkdir -p ~/.claude/skills

# discovery-html
curl -o ~/.claude/skills/discovery-html.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/discovery-html.md

# voice-agent
curl -o ~/.claude/skills/voice-agent.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/voice-agent.md
```

Restart Claude Code — skills appear as `/discovery-html` and `/voice-agent`.

---

## Available Commands

| Command | Description |
|---------|-------------|
| `/insait-test-csv` | Build or validate a Strict Replay test CSV for the INSAIT platform |

## Available Skills

| Skill | Description |
|-------|-------------|
| `/discovery-html` | Generate a fully interactive, self-contained HTML discovery/briefing document |
| `/voice-agent` | Build or review voice agent prompts and guardrails using the INSAIT Voice Agent Playbook |

---

## Adding new commands or skills

- Commands → drop `.md` files into `.claude/commands/` and open a PR
- Skills → drop `.md` files into `.claude/skills/` and open a PR
- Update this README's tables and curl install blocks when you add something
