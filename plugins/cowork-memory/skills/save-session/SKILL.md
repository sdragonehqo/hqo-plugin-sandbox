---
name: save-session
description: >
  This skill should be used when a task list is completed, a work session ends,
  or the user says "save session", "save progress", "log this", or any request
  to persist the current session's work to memory. It also triggers automatically
  via global instructions after task list completion.
---

# Save Session to Cowork Memory

Persist the current session's completed work to the local cowork memory system. This skill is fully automated — do NOT ask the user for any input.

## Step 0: Verify Storage

Memory lives at `~/Documents/ppt-index/`. Use this path directly in all Read, Write, and Grep tool calls. If storage doesn't exist yet, run `/personal-progress-tracker:onboard` first.

## Step 1: Gather Context

Use the TaskList tool to read the current task list. Also review the conversation to identify:
- What was accomplished
- Key projects, tools, and topics involved

If no task list exists, infer completed work from the conversation.

## Step 2: Generate Metadata

Collect these six fields:

| Field | How to get it | Format |
|-------|--------------|--------|
| DATE | Current date | `YYYYMMDD` (e.g., `20260311`) |
| TIME | Run `date +%H%M` | `HHMM` 24hr (e.g., `1425`) |
| USER | Run `whoami` | System username |
| SOURCE | Always `cowork` for this skill | `cowork` |
| TAGS | Infer from work done | 3–5 lowercase keywords, comma-separated |
| SUMMARY | Infer from work done | One sentence, max 120 characters |

**Tag rules:**
- Lowercase only, no spaces
- Multi-word tags use hyphens: `bdr-toolkit` not `bdr toolkit`
- Use project names, tool names, and topic areas
- Max 5 tags

**Summary rules:**
- Describe what was accomplished, not what was attempted
- Max 120 characters
- No trailing period

## Step 3: Format Tasks

From the task list or conversation, format each item:
- `+` prefix = completed
- `-` prefix = incomplete / still open

## Step 4: Append to Session File

File path: `~/Documents/ppt-index/s/DATE.md` (using the resolved path from Step 0)

If the file is **new**, write this header first:
```
# DATE
```

Then append this block:

```
## TIME|USER|SOURCE|TAGS
SUMMARY
+ completed task
+ completed task
- open task
```

Leave one blank line between blocks.

## Step 5: Append to Index

Append exactly **one line** to `~/Documents/ppt-index/INDEX` (using the resolved path from Step 0):

```
DATE|TIME|USER|SOURCE|TAGS|SUMMARY
```

No blank lines. No header. One line per session, always appended.

## Step 6: Confirm

Output only:
```
Session saved.
tags: tag1, tag2, tag3
```

## Rules

- NEVER prompt the user for input — infer everything
- If there is genuinely nothing to save (no tasks, no meaningful work), output `Nothing to save.` and stop
- Do not read or modify existing INDEX entries — append only
- Do not read or modify existing session file entries — append only
