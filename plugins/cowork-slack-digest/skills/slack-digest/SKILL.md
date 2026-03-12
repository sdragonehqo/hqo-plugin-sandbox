---
name: slack-digest
description: >
  This skill should be used when the user asks to "digest my slack",
  "review my DMs", "slack daily digest", "what happened in slack",
  "summarize my slack", "check my slack messages", or any request
  involving reviewing Slack DMs and channels and producing a structured
  summary.
---

# Slack Daily Digest → Cowork Memory

Review Slack DMs and configured channels from the past 24 hours, and write structured entries to the cowork memory system. This skill is fully automated — do NOT ask the user for any input.

## Step 0: Ensure Storage Exists (ALWAYS run first)

```bash
COWORK_DIR=$(eval echo ~/.cowork) && echo "$COWORK_DIR" && mkdir -p "$COWORK_DIR/memory/s" && touch "$COWORK_DIR/memory/INDEX"
```

Use the resolved path printed by this command (e.g. `/Users/yourname/.cowork`) in all subsequent Grep/Read/Write tool calls instead of `~/.cowork/`. Never use `~` in Grep/Read/Write tool calls — always use the fully expanded absolute path.

## Step 1: Load Channel Configuration

Read `~/.cowork/memory/config.md` for the list of monitored Slack channels. If the file doesn't exist, only process DMs and note that no channels are configured.

## Step 2: Determine Time Window

```bash
date -v-24H +%s
```

Store this Unix timestamp for filtering.

## Step 3: Search DMs

Use `slack_search_public_and_private` to find DM messages from the last 24 hours:
- Set `channel_types` to `"im,mpim"` for DMs and group DMs
- Set `after` to the Unix timestamp from Step 2
- Run searches with `to:me` and `from:me` to ensure full coverage

For messages with threads, use `slack_read_thread` to get full conversation context.

If no DMs found, note "No DM activity" and continue.

## Step 4: Read Configured Channels

For each channel in the config:
- Use `slack_read_channel` with the channel ID
- Set `oldest` to the Unix timestamp from Step 2
- For threaded messages (reply_count > 0), use `slack_read_thread`

If no messages in a channel, skip it.

## Step 5: Categorize Conversations

For each conversation/thread, determine:

**Needs Response** — someone directly asked the user a question or is waiting on a reply. These become `-` items.

**Action Items** — tasks assigned or implied, with owner if mentioned. These become `-` items.

**Informational** — discussions, updates, decisions that are good to know. These become `+` items.

## Step 6: Generate Metadata

Run `date +%Y%m%d` for DATE and `date +%H%M` for TIME. Run `whoami` once for USER.

| Field | Format |
|-------|--------|
| DATE | `YYYYMMDD` |
| TIME | `HHMM` |
| USER | System username |
| SOURCE | Always `slack` |
| TAGS | 3–5 lowercase keywords. Include people names and channel names as tags. |
| SUMMARY | Max 120 chars. Overview of Slack activity + what needs attention. |

## Step 7: Write to Memory

**Group all Slack activity into ONE entry per digest run.**

Append to `~/.cowork/memory/s/DATE.md`:
```
## TIME|USER|slack|TAGS
SUMMARY
- Reply to Liz Thomson re Ozzy status
- Follow up with Joel on HubSpot workflow
+ Marketing team discussed Q2 campaign timeline in #marketing-hub
+ Greg shared Dream 100 list update
```

Append ONE line to `~/.cowork/memory/INDEX`:
```
DATE|TIME|USER|slack|TAGS|SUMMARY
```

The SUMMARY should be an overview like: `6 conversations — 2 need response (Liz re Ozzy, Joel re HubSpot)`

## Step 8: Confirm

Output:
```
Slack digest saved.
X conversations — Y need response, Z informational
tags: tag1, tag2, tag3
```

Then list the items needing response.

## Rules

- NEVER prompt the user for input
- Group all Slack activity into ONE memory entry per digest run
- Always read full threads before summarizing — don't summarize from preview text alone
- Do not read or modify existing INDEX/session entries — append only
