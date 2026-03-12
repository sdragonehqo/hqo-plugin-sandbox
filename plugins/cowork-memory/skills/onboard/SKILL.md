---
name: onboard
description: >
  This skill should be used when the user asks to "set up memory",
  "configure the memory plugin", "onboard", "get started", "set up digests",
  "initialize", "setup", or any request involving first-time setup of the
  cowork memory system including storage, connectors, Slack channels,
  and global instructions.
version: 2.0.0
---

# Cowork Memory — Setup

Walk a new user through complete setup of the cowork memory system. This involves creating the storage directory, verifying tool connections, configuring Slack channels, and setting up the global instruction for automatic session saving.

## Step 1: Create Storage

Run this — no confirmation needed:

```bash
mkdir -p ~/.cowork/memory/s && touch ~/.cowork/memory/INDEX
```

Confirm success to the user.

## Step 2: Verify Tool Connections

Check each required connector with a simple test call:

1. **Gmail** — try `gmail_get_profile`. If it fails, tell the user to connect Gmail in Cowork connector settings.
2. **Slack** — try `slack_read_user_profile` (no arguments). If it fails, tell the user to connect Slack.
3. **Google Drive** — try `google_drive_search` with a minimal query. If it fails, tell the user to connect Google Drive. (Needed for meeting transcripts.)

Report which are connected and which are missing. If any fail, provide specific instructions but continue setup — the user can connect them later.

## Step 3: Configure Slack Channels

DMs are always monitored. Ask which Slack channels to also track.

Use `slack_search_channels` to help find channels by name. Let the user list as many as they want.

Save the configuration to `~/.cowork/memory/config.md`:

```markdown
# Cowork Memory — Configuration

## Slack Channels
The following channels are monitored in addition to DMs:

| Channel Name | Channel ID |
|-------------|------------|
| #channel-name | C0XXXXXXXX |

## Setup Date
YYYY-MM-DD
```

Look up each channel ID using `slack_search_channels`.

## Step 4: Set Global Instruction

Tell the user:

> To automatically save your Cowork sessions, add this to your **Global Instructions** in Cowork Settings:
>
> `When a task list is complete or the session is ending, run /save-session to persist the work to local memory.`

Ask them to confirm when added.

## Step 5: Confirm Setup

Run validation:
1. Check `~/.cowork/memory/s/` exists
2. Check `~/.cowork/memory/INDEX` exists
3. Check `~/.cowork/memory/config.md` exists (if Slack was configured)
4. List connected tools

Present summary:

```
Setup Complete!

Storage: ~/.cowork/memory/
Slack channels: DMs + [list]
Auto-save: [Configured / Needs global instruction]
Connected: Gmail, Slack, Google Drive

Available commands:
  /save-session    — Save current session to memory
  /digest-email    — Pull last 24h of email
  /digest-slack    — Pull last 24h of Slack
  /digest-meeting  — Pull last 24h of meeting notes
  /digest-all      — Run all three digests
  /recall <query>  — Search all memory
  /task-master     — Daily briefing across all sources
```

## Step 6: Optional — Backfill from Gmail Drafts

If the user has been using the Gmail draft system, offer to migrate:

> I see you may have existing session data as Gmail drafts. Want me to backfill your memory from those drafts? This is a one-time read-only migration.

If yes, follow the same logic as the batch backup prompt — read all datetime-subject drafts and write them to memory.

## Rules

- Be conversational during setup — this is one of the few skills where asking the user questions is expected
- If a connector fails, continue setup — don't block on it
- Create storage immediately in Step 1 without asking permission
