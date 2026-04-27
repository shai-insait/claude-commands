# INSAIT Claude Commands

Shared Claude Code slash commands for the INSAIT team.

## Installation

Copy any command file into your local `~/.claude/commands/` folder:

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/insait-test-csv.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-test-csv.md
```

Then restart Claude Code — the command will appear as `/insait-test-csv`.

## Available Commands

| Command | Description |
|---------|-------------|
| `/insait-test-csv` | Build or validate a Strict Replay test CSV for the INSAIT platform |

## Adding new commands

Drop `.md` files into `.claude/commands/` and open a PR.
