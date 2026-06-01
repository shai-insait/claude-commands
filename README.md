# INSAIT Claude Commands & Skills

Shared Claude Code slash commands and skills for the INSAIT team.

> **Commands** are simple slash commands.
> **Skills** are richer — they carry full playbook context and handle multiple modes (build, review, audit).
> Both work the same way: drop a `.md` file into the right folder, restart Claude Code, done.

---

## Quick Install

Run this once to install everything:

```bash
# Commands
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/insait-test-csv.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-test-csv.md
curl -o ~/.claude/commands/insait-agent-json.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-agent-json.md
curl -o ~/.claude/commands/summarize.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/summarize.md
curl -o ~/.claude/commands/feedback-browser.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/feedback-browser.md
curl -o ~/.claude/commands/html-email.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/feedback-browser.md

# Skills
mkdir -p ~/.claude/skills
curl -o ~/.claude/skills/discovery-html.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/discovery-html.md
curl -o ~/.claude/skills/voice-agent.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/voice-agent.md
```

Then restart Claude Code. All commands and skills will be available immediately.

---

## Available Commands

Commands go in `~/.claude/commands/`. Invoke with `/command-name`.

| Command | Invoke | What it does |
|---------|--------|--------------|
| INSAIT Agent JSON | `/insait-agent-json` | Build or validate a Conversation Flow Agent JSON for the INSAIT platform. Pass a file path to fix, or describe an agent to build from scratch. Covers full schema, all node types, exits, variables, and every known import failure mode. |
| INSAIT Test CSV | `/insait-test-csv` | Build or validate a Strict Replay test CSV for the INSAIT platform. Pass a file path to review, or describe the agent to generate test cases. |
| Summarize | `/summarize` | Summarize the last N assistant messages in a session. Usage: `/summarize 5` or `/summarize 3 voice agent rules`. |
| Feedback Browser | `/feedback-browser` | Build a filterable, expandable single-file HTML conversation browser from a pilot/QA feedback file (scores + rep feedback + QA comments + full transcripts). Usage: `/feedback-browser matched.md [analysis.md] [output.html]`. |

### Install individually

```bash
# INSAIT Agent JSON
curl -o ~/.claude/commands/insait-agent-json.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-agent-json.md

# INSAIT Test CSV
curl -o ~/.claude/commands/insait-test-csv.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/insait-test-csv.md

# Summarize
curl -o ~/.claude/commands/summarize.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/summarize.md

# Feedback Browser
curl -o ~/.claude/commands/feedback-browser.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/feedback-browser.md
curl -o ~/.claude/commands/html-email.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/commands/feedback-browser.md
```

---

## Available Skills

Skills go in `~/.claude/skills/`. Invoke with `/skill-name`.

| Skill | Invoke | What it does |
|-------|--------|--------------|
| Discovery HTML | `/discovery-html` | Generate a self-contained interactive HTML discovery/briefing doc — checkboxes, progress bar, localStorage, PDF export. Pass a description or topic. |
| Voice Agent | `/voice-agent` | Build or review voice agent prompts and guardrails using the INSAIT Voice Agent Playbook. Pass a brief to generate all artifacts (system prompt, guardrails, node stubs, config checklist), or pass a file/paste to audit against the full playbook ruleset. |

### Install individually

```bash
# Discovery HTML
curl -o ~/.claude/skills/discovery-html.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/discovery-html.md

# Voice Agent
curl -o ~/.claude/skills/voice-agent.md \
  https://raw.githubusercontent.com/shai-insait/claude-commands/main/.claude/skills/voice-agent.md
```

---

## Updating an existing command or skill

Re-run the same `curl` command — it overwrites the local file. Then restart Claude Code.

---

## Adding a new command or skill

1. Write your `.md` file (see any existing file for format reference)
2. Put it in:
   - `.claude/commands/` for a command
   - `.claude/skills/` for a skill (use this when the instruction set is large or has multiple modes)
3. Add a row to the right table in this README — include the invoke name, and a one-sentence description
4. Add a `curl` install block under "Install individually"
5. Open a PR — once merged, teammates can curl it down
