---
name: onboard
description: >
  Use when the user wants to set up the cowork memory system for the first time,
  configure digests, set up scheduled tasks, verify connectors, or asks to
  "get started", "set up memory", "configure digests", "onboard", or
  "set up scheduled tasks".
---

# Personal Progress Tracker — Onboarding

Guide the user through complete setup of the cowork memory system. This skill IS conversational — ask questions and wait for answers at each step.

## Step 0: Resolve Memory Path

Run this first, every time:

```bash
MEMORY_DIR=$(eval echo ~/.cowork/memory) && echo "$MEMORY_DIR" && mkdir -p "$MEMORY_DIR/s" && touch "$MEMORY_DIR/INDEX"
```

Capture the printed path. Use this resolved absolute path (not `~`) in all subsequent Read, Write, and Grep tool calls.

---

## Step 1: Check Connectors

Test each connector with a minimal call. Report pass/fail for all three before continuing.

| Connector | Test call | Required by |
|---|---|---|
| Gmail | `gmail_get_profile` | email-digest, meeting-digest |
| Slack | `slack_read_user_profile` (no args) | slack-digest |
| Google Drive | `google_drive_search` with minimal query | meeting-digest |

Present results:
```
Connectors:
✓ Gmail — connected
✓ Slack — connected
✗ Google Drive — not connected
```

For any that fail, tell the user exactly where to connect them: **Cowork Settings → Connectors**. Note the failure but continue setup — they can reconnect and re-run this skill later.

---

## Step 2: Global Instruction

Tell the user:

> To enable automatic session saving, go to **Cowork Settings → Global Instructions → Edit** and paste in the following — then come back and confirm.

Show them this exact block:

```
IMPORTANT — AUTOMATIC SESSION SAVE:
After completing ALL items in a task list, you MUST immediately invoke the skill `cowork-memory:save-session` to persist the session to local memory. Do not wait for the user to ask. Do not skip this step. This is mandatory after every completed task list.
```

Ask: **"Have you added this to your global instructions? (yes / not yet)"**

Wait for confirmation before moving on.

---

## Step 3: Configure Slack Channels

Ask: **"Which Slack channels should the digest monitor, in addition to your DMs?"**

DMs are always included. Use `slack_search_channels` to look up channel IDs as the user names them.

Build a list. If the user says "just DMs for now", proceed with an empty channels list.

Write to `RESOLVED_PATH/config.md`:

```markdown
# Cowork Memory — Configuration

## Slack Channels

Monitored in addition to DMs:

| Channel Name | Channel ID |
|---|---|
| #channel-name | CXXXXXXXX |

## Setup Date
YYYY-MM-DD
```

---

## Step 4: Schedule the Digests

Tell the user the three digests will be scheduled as recurring tasks. Show the defaults and ask if they want to adjust any times:

| Digest | Default schedule |
|---|---|
| Email | Weekdays at 8:00am |
| Slack | Weekdays at 8:00am |
| Meetings | Weekdays at 9:00am |

Ask: **"Do these times work, or would you like to change any of them?"**

Once confirmed, create all three scheduled tasks:

**Email digest:**
```
create_scheduled_task(
  taskId: "cowork-email-digest",
  description: "Daily email digest to cowork memory",
  prompt: "Run /cowork-email-digest:email-digest",
  cronExpression: "0 8 * * 1-5"
)
```

**Slack digest:**
```
create_scheduled_task(
  taskId: "cowork-slack-digest",
  description: "Daily Slack digest to cowork memory",
  prompt: "Run /cowork-slack-digest:slack-digest",
  cronExpression: "0 8 * * 1-5"
)
```

**Meeting digest:**
```
create_scheduled_task(
  taskId: "cowork-meeting-digest",
  description: "Daily meeting notes digest to cowork memory",
  prompt: "Run /cowork-meeting-digest:meeting-digest",
  cronExpression: "0 9 * * 1-5"
)
```

Adjust `cronExpression` values if the user requested different times.

---

## Step 5: Confirm

Present a final summary:

```
Setup Complete!

Connectors:     Gmail ✓   Slack ✓   Google Drive ✓
Global instruction: confirmed
Slack monitoring: DMs + #channel1, #channel2
Memory path: /Users/yourname/.cowork/memory/

Scheduled tasks:
  cowork-email-digest    — weekdays 8:00am
  cowork-slack-digest    — weekdays 8:00am
  cowork-meeting-digest  — weekdays 9:00am

You're all set. Try:
  /personal-progress-tracker:recall  — search everything in memory
```

---

## Rules

- This skill is conversational — ask questions and wait for answers
- Always resolve the memory path in Step 0 before any file operations
- Don't skip steps even if a connector is missing — note and continue
- Always confirm schedule times before calling `create_scheduled_task`
- If the user wants to change a digest time later, tell them to use `update_scheduled_task`
