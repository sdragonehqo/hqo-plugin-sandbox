# Setup: cowork-slack-digest

## Requirements

- **Slack MCP connector** — must be connected in Cowork Settings → Connectors
- `cowork-memory` already set up — `~/Documents/ppt-index/` must exist before the digest can write to it
- Read/write access to `~/Documents/` via `request_cowork_directory`

## Setup Steps

1. Verify `cowork-memory` is set up first (`~/Documents/ppt-index/INDEX` must exist).

2. Verify Slack is connected by calling `slack_read_user_profile` with no arguments — it should return the user's Slack profile without error.

3. Ask the user:
   > "Which Slack channels should the digest monitor, in addition to your DMs?"

   DMs are always included. If the user says "just DMs", proceed with an empty channels list.

   For each channel named, look up its ID:
   - Search public channels first with `slack_search_channels`
   - If not found, search again with `types: "private_channel"`
   - If still not found, ask the user to grab the ID manually:
     > "Right-click the channel in Slack → Copy → Copy link. The URL ends in `/CXXXXXXXX` — paste that ID here."

4. Ask the user:
   > "Are there any DMs or group messages you'd like to exclude from the digest?"

   This is optional. Collect names/handles if any. Skip if none.

5. Write `~/Documents/ppt-index/config.md` with the collected config:

   ```markdown
   # Cowork Memory — Configuration

   ## Slack Channels

   Monitored in addition to DMs:

   | Channel Name | Channel ID |
   |---|---|
   | #channel-name | CXXXXXXXX |

   ## Excluded DMs

   People or groups whose DMs should be skipped:

   | Name / Handle |
   |---|
   | @username |

   ## Setup Date
   YYYY-MM-DD
   ```

   Omit the Excluded DMs table if there are no exclusions.

6. Create the scheduled task. Build the prompt dynamically from the config:

   ```
   taskId: "cowork-slack-digest"
   description: "Daily Slack digest to cowork memory"
   prompt: |
     Run /cowork-slack-digest:slack-digest

     Configuration:
     Include DMs: yes
     Include channels: #channel-name (CXXXXXXXX)
     Exclude DMs: @username1
   cronExpression: "0 8 * * 1-5"
   ```

   Set `Include channels: none` if DMs only. Omit `Exclude DMs` line if no exclusions.

7. Tell the user:
   > "One manual step: open Cowork Settings → Scheduled Tasks → Edit `cowork-slack-digest` → set Working Folder to your Documents folder → Save."

## Verification

Run `/cowork-slack-digest:slack-digest` manually. It should read `config.md`, search DMs and configured channels for the past 24 hours, and write one entry to `~/Documents/ppt-index/s/YYYYMMDD.md`.

## Notes

- If `config.md` does not exist, the skill processes DMs only — it does not fail
- To change channels or exclusions later, re-run setup or manually update the scheduled task prompt in Cowork Settings → Scheduled Tasks
- The Working Folder on the scheduled task must be set manually — the skill cannot write to memory without it
