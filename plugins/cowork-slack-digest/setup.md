# Setup: cowork-slack-digest

## Requirements

- **Slack MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `slack_search_public_and_private`, `slack_read_channel`, `slack_read_thread`, and `slack_search_channels`.
- `cowork-memory` already set up — `~/Documents/ppt-index/` must exist before the digest can write to it
- Channel configuration at `~/Documents/ppt-index/config.md` — created during `personal-progress-tracker:onboard`
- Read/write access to `~/Documents/` — digests are written to `~/Documents/ppt-index/`

## Setup Steps

1. Ensure `cowork-memory` is set up first. Run `/personal-progress-tracker:onboard` if you have not already — this creates `~/Documents/ppt-index/` and initializes the index.

2. Connect Slack in Cowork Settings → Connectors. Verify the connection by invoking `slack_read_user_profile` with no arguments — it should return your Slack profile without error.

3. Run `/personal-progress-tracker:onboard` (if not already done). During onboarding, you will be asked: **"Which Slack channels should the digest monitor, in addition to your DMs?"** DMs are always included. The onboard skill looks up channel IDs using `slack_search_channels` (searching public channels first, then private) and writes the config to `~/Documents/ppt-index/config.md` in this format:

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

   If you say "just DMs for now", the channels table is left empty and only DMs are monitored.

4. **Finding private channel IDs manually:** If a channel cannot be found by `slack_search_channels`, right-click the channel name in Slack's left sidebar → hover over **Copy** → click **Copy link**. The URL looks like `https://app.slack.com/client/TXXXXXXXX/CXXXXXXXX` — the channel ID is the segment starting with **C** at the end. Paste that ID during onboarding and it will be added to `config.md`.

5. Schedule the Slack digest as a recurring task. The `onboard` skill creates it automatically with a prompt built from your channel config:

   ```
   taskId: "cowork-slack-digest"
   description: "Daily Slack digest to cowork memory"
   prompt: |
     Run /cowork-slack-digest:slack-digest

     Configuration:
     Include DMs: yes
     Include channels: #channel-name (CXXXXXXXX), #channel-name-2 (CXXXXXXXX)
   cronExpression: "0 8 * * 1-5"   ← weekdays at 8:00am
   ```

   If the user chose DMs only, the prompt sets `Include channels: none`.

6. After the task is created, open **Cowork Settings → Scheduled Tasks**, click **Edit** on the `cowork-slack-digest` task, set the **Working Folder** to your **Documents** folder, and save.

## Verification

Run `/cowork-slack-digest:slack-digest` manually. It should:
1. Read `~/Documents/ppt-index/config.md` to load the monitored channel list
2. Search DMs (using `to:me` and `from:me` with `channel_types: "im,mpim"`) and read configured channels for the past 24 hours
3. Fetch full thread content with `slack_read_thread` for any threaded messages
4. Categorize conversations as Needs Response, Action Items, or Informational
5. Write one grouped entry to `~/Documents/ppt-index/s/YYYYMMDD.md`
6. Append one line to `~/Documents/ppt-index/INDEX`
7. Output: `Slack digest saved. X conversations — Y need response, Z informational`

## Notes

- If `config.md` does not exist, the skill processes DMs only and notes that no channels are configured — it does not fail
- All Slack activity in a single digest run is grouped into ONE memory entry
- Full threads are always fetched before summarizing — the skill never summarizes from preview text alone
- The scheduled task prompt is built dynamically during `personal-progress-tracker:onboard` to embed the configured channel IDs. If you add or remove channels later, re-run onboard or manually update the scheduled task prompt in Cowork Settings → Scheduled Tasks
- The Working Folder on the scheduled task must be set manually after onboarding — the skill cannot write to `~/Documents/ppt-index/` without it
- This plugin depends on `personal-progress-tracker` for scheduled task setup and channel config, and on `cowork-memory` for storage
