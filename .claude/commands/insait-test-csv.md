---
description: Build or validate a Strict Replay test CSV for the Insait platform. Pass a file path to review an existing CSV, or describe the agent to build one from scratch.
argument-hint: [/path/to/file.csv | "describe the agent"]
---

You are an expert in the Insait AI Agent Platform's testing system. Your job is to build or validate a Strict Replay test CSV that can be imported into the Insait testing panel.

Parse $ARGUMENTS:
- If it looks like a file path (starts with `/` or `./` or ends with `.csv`): read the file and validate it, then output a corrected version.
- If it's a description of an agent or flow: build a complete test CSV from scratch covering all scenarios.
- If empty: explain the format and guidelines, then ask what the user needs.

---

## What You Are Building

A **Strict Replay** test CSV for the Insait platform. Each row = one independent multi-turn test conversation. The platform sends the user messages sequentially to the agent and evaluates the agent's final response against the expected outcome using an LLM Judge.

This is NOT a Simulate Replay (those use a persona and cannot be CSV-imported). This is deterministic: you write the exact messages, you define the expected end state.

---

## The Non-Negotiable CSV Rule: Exactly 18 Columns Per Row

```
Column A  = Test Name (unique, descriptive label)
Column B  = Turn 1 (first user message)
Column C  = Turn 2
...
Column Q  = Turn 16
Column R  = Expected Outcome (what the agent should have achieved by the end)
```

**Every row must have exactly 18 fields.** If a test uses fewer than 16 turns, pad the unused Turn columns with empty fields (just commas — no content). The Expected Outcome MUST land in column R (field 18).

**The most common mistake:** Placing the Expected Outcome text directly after the last real turn without padding. This causes the outcome text to land in a Turn column instead of column R — it gets sent to the agent as a user message and evaluation is skipped.

### Correct (15 turns, padded):
```
Test name,Turn1,Turn2,...,Turn15,,Expected Outcome text
```
                               ↑↑
                   Empty Turn16 cell (1 comma = 1 empty field)

### Wrong (15 turns, no padding):
```
Test name,Turn1,Turn2,...,Turn15,Expected Outcome text
```
(Expected Outcome lands in the Turn16 column — broken)

**To verify**: Open in Google Sheets. Check that the Expected Outcome text is in column R for EVERY row. If any row has the text in an earlier column, it needs padding commas.

---

## How to Write the Expected Outcome (Column R)

The LLM Judge receives:
1. All user questions in order (numbered list)
2. Your Expected Outcome text
3. The full conversation transcript

It evaluates:
- Did the agent correctly handle each turn in the flow?
- Did it maintain context across turns?
- Does the final conversation state match the expected outcome?

**Rules for writing expected outcomes:**

| Do | Don't |
|----|-------|
| Describe the END STATE (what was accomplished) | Quote what the agent should say verbatim |
| Name specific fields that were collected | Say "the agent responded helpfully" |
| State which routing path/branch was taken | Use vague terms like "handled correctly" |
| State what the agent did NOT do (for edge/negative cases) | Assume the judge knows your flow logic |
| Be specific about data values and field names | Write a paragraph of prose |

**Good example:**
> "Agent collected all 5 contact fields (first_name, last_name, phone, email, zip_code) and all 10 loan and vehicle fields. Credit score 520 was bucketed as 500-549. Agent routed to the DNQ node and the eCalc API was NOT called. Agent gave a warm non-judgmental message. No lead was submitted."

**Bad example:**
> "Agent handled the low credit score case appropriately and was polite."

Leave Expected Outcome empty only if the test is purely checking whether the agent doesn't crash or freeze — not for any test where correctness matters.

---

## Scenario Categories to Cover

For any Conversation Flow Agent, build tests across these categories:

1. **Happy path** — user provides all required information correctly, flow completes end-to-end
2. **DNQ / disqualification** — user meets a condition that should route them away from the main flow
3. **Borderline values** — values exactly at a threshold (test bucketing/routing logic)
4. **Ambiguous input** — user gives vague or partial answer; agent must ask for clarification
5. **Multi-field in one message** — user volunteers several pieces of info at once; agent must extract without re-asking
6. **Abbreviated / non-standard format** — e.g. "18k" instead of "18000", "CA" instead of "California"
7. **Off-topic digression** — user asks an unrelated question mid-flow; agent must answer and return
8. **Invalid input then correction** — user gives wrong value, then correct value in next turn
9. **Path switching** — user changes intent mid-flow (e.g. switches from one path to another)
10. **Consent/gate edge cases** — decline, then accept; firm refusal; ask what it means
11. **Greeting only / no intent** — user sends a vague opener; agent must present all options
12. **Early exit** — user wants to stop before completing the flow
13. **Language/phrasing variants** — formal vs casual, written-out numbers vs digits

---

## CSV Format Rules

- Standard RFC 4180 CSV, UTF-8 encoding
- Comma delimiter
- Quote any field that contains a comma or newline: `"this, has a comma"`
- Escape internal quotes by doubling: `"she said ""hello"""`
- Include a header row (row 1): `Test Name,Turn 1,Turn 2,...,Turn 16,Expected Outcome`
- Test names must be unique and human-readable (they appear in the UI)
- Empty cells between the last real turn and Expected Outcome = padding (correct)
- Max 10,000 rows before a UI warning appears

---

## How Importing Works (What to Tell the User)

When uploading the CSV in the Insait Testing tab:
1. Upload the CSV file
2. In the import dialog:
   - **Name Column → A**
   - **Message Start → B**
   - **Message End → Q**
   - Find the "Expected Outcome Column" row — **check the "Include" checkbox** (small checkbox next to the label — it is OFF by default and easy to miss. If not checked, all expected outcomes are silently ignored regardless of CSV content)
   - **Expected Outcome Column → R**
3. Row selection: **Skip header row** (row 1)
4. Check the preview — if the Expected Outcome checkbox was checked correctly, a 4th column appears in the preview table showing the outcome text. If that column is missing or empty, the checkbox was not checked.

After import, all tests appear in a folder. Click **Run on the folder** (not individual tests) to execute all scenarios in one batch sequentially.

**Critical UI note:** The "Include" checkbox for Expected Outcome is the most common reason expected outcomes don't appear after import. The CSV can be perfectly formatted and the outcomes will still not import if this checkbox is missed.

---

## Validation Checklist

Before outputting any CSV, verify every row:

- [ ] Exactly 18 fields (count by opening in Google Sheets — Expected Outcome in column R)
- [ ] Rows with fewer than 16 turns have empty padding cells for unused Turn columns
- [ ] Expected Outcome is present for every test where correctness matters
- [ ] Expected Outcome describes END STATE (not agent phrasing)
- [ ] Fields containing commas are properly quoted
- [ ] Test names are unique
- [ ] Header row present as row 1
- [ ] All 13 scenario categories covered (or justified why some don't apply to this agent)

---

## If Reviewing an Existing File

1. Read the file
2. Count fields per row — flag any row where Expected Outcome is NOT in column R
3. Check Expected Outcome quality for each row (end state, specific, names fields/paths)
4. Output: a corrected CSV with all issues fixed, plus a summary of what was changed and why
