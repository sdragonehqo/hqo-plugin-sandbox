# Setup: cowork-slack-digest

## Requirements

- **Slack MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `slack_search_public_and_private`, `slack_read_channel`, `slack_read_thread`, and `slack_search_channels`.
- Read/write access to `~/Documents/` — writes digests to `~/Documents/ppt-index/` and reads channel config from `~/Documents/ppt-index/config.md`
- `cowork-memory` and `personal-progress-tracker` must be set up first — the `~/Documents/ppt-index/` storage and `config.md` must already exist before the digest runs

## First-Run Steps

1. Ensure `cowork-memory` is set up and `personal-progress-tracker:onboard` has been run so that `~/Documents/ppt-index/` exists.
2. Connect Slack in Cowork Settings → Connectors. Verify the connection by calling `slack_read_user_profile` with no arguments — it should return your Slack profile without error.
3. Run `/personal-progress-tracker:onboard` (if not already done). During onboarding, you will be asked which Slack channels to monitor in addition to DMs. The onboard skill will look up channel IDs and write them to `~/Documents/ppt-index/config.md`.
   - For private channels not found by search: right-click the channel name in Slack's sidebar → hover over Copy → click Copy link. The channel ID is the segment starting with `C` at the end of the URL (e.g., `CXXXXXXXX`).
4. After scheduling, open Cowork Settings → Scheduled Tasks, click Edit on the `cowork-slack-digest` task, set the Working Folder to your Documents folder, and save. The default schedule is weekdays at 8:00am.

## Verification

Run `/cowork-slack-digest:slack-digest` manually. It should:
1. Read `~/Documents/ppt-index/config.md` to load monitored channels
2. Search DMs and read configured channels for messages from the past 24 hours
3. Categorize conversations as Needs Response, Action Items, or Informational
4. Write one grouped entry to `~/Documents/ppt-index/s/YYYYMMDD.md`
5. Append one line to `~/Documents/ppt-index/INDEX`
6. Output a summary: `Slack digest saved. X conversations — Y need response, Z informational`

## Notes

- Storage path and config path changed in v2.0.0 from `~/.cowork/memory/` to `~/Documents/ppt-index/`. Existing data at the old path will not be migrated.
- If `config.md` does not exist, the skill will process DMs only and note that no channels are configured.
- All Slack activity in a single digest run is grouped into ONE memory entry.
- Full threads are always fetched before summarizing — the skill does not summarize from preview text alone.
- The scheduled task prompt is built dynamically during `personal-progress-tracker:onboard` to include the configured channel IDs. If you add channels later, re-run onboard or manually update the scheduled task prompt.
- This plugin depends on `personal-progress-tracker` for scheduled task setup and channel config, and on `cowork-memory` for storage.
