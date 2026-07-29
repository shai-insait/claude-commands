---
description: End-of-day wrap for a project: update memory, post a Linear status update, then draft the daily update in the project's Slack channel. Usage: /eod <project>
argument-hint: <project> (e.g. hyp, wobi, psagot, harel, ilending)
---

You are wrapping today's work session on the project named in $ARGUMENTS. Do the three steps below IN ORDER (memory → Linear → Slack).

First, from THIS conversation, gather the raw material: what was actually done this session, what's next, and what's blocked. That feeds all three outputs. If little happened, say so rather than padding.

## Resolve the project (same as /session-start)
- Memory file: `find ~/.claude/projects -name "*.md" | grep -i <project>`.
- Linear project ID: `reference_linear.md` (or `list_projects` by name, CX team).
- Slack channel ID: project memory, else `slack_search_channels`.

## Step 1 — Memory
Search first so you UPDATE, don't duplicate (per the memory protocol). Append a dated entry to the project memory capturing today: what got done, decisions made, new IDs/assets, what's next, and open items. Use absolute dates. Be specific enough that a future session can resume cold. Update the parent `MEMORY.md` index line if the headline changed.

## Step 2 — Linear
Post a dated project status update (`save_status_update`) with a health flag: `onTrack` | `atRisk` | `offTrack`.
- Read the previous status update first, for continuity and an honest diff.
- Manager-facing: what changed since the last update, current state, what's next, and any risks or decisions needed. (The project Description is the source-of-truth doc; the status update is the dated pulse.)

## Step 3 — Slack daily update — DRAFT ONLY, never send
Read the channel's last 1–2 daily updates first for continuity, then create a DRAFT (`slack_send_message_draft`) in the project channel using this exact template:

_Daily Update: <Project>_ 🤖

✅ *Accomplished*
• <high-level bullets of what got done today>

🔜 *Next*
• <what we'll work on next / in the upcoming days>

⛔ *Stuck / need help*
• <blockers: where we're stuck, who we need to resolve it. Mostly bugs/features that block progress. If nothing is blocking, say so plainly.>

Rules:
- High level, short, to the point, with a clear sense of where we need help.
- **No em dashes anywhere** — use commas, colons, or a new sentence. (Standing rule for all of Shai's writing.)
- It is a DRAFT: never auto-send. Leave it for me to review and send.
- If the update should answer a manager question raised in the channel, address it.

## Finish
Give a one-line confirmation of what was written where: memory entry, Linear update + health flag, and the Slack draft link. Flag anything you were unsure about or left for me to decide.
