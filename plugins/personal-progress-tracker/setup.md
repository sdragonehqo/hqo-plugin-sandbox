# Setup: personal-progress-tracker

## Requirements

- Read/write access to `~/Documents/` — creates and manages `~/Documents/ppt-index/` and `~/Documents/plugin-maintenance/`
- Write access to `~/.claude/CLAUDE.md` — the onboard skill writes the auto-save global instruction here
- No MCP connectors required for the plugin itself, but the onboard skill tests Gmail, Slack, and Google Drive connectors on behalf of the digest plugins

## First-Run Steps

1. Grant Claude Code access to your Documents folder. In the Cowork toolbar, click the folder icon, select your Documents folder, and confirm.
2. Run `/personal-progress-tracker:onboard`. This conversational skill walks through the following steps automatically:
   - Creates `~/Documents/ppt-index/INDEX` and `~/Documents/ppt-index/s/` (the memory storage used by all cowork plugins)
   - Tests Gmail, Slack, and Google Drive connectors and reports which are connected
   - Writes the automatic session-save rule to `~/.claude/CLAUDE.md`
   - Asks which Slack channels to monitor (in addition to DMs) and writes the config to `~/Documents/ppt-index/config.md`
   - Schedules three recurring digest tasks (email at 8:00am weekdays, Slack at 8:00am weekdays, Gemini Notes at 9:00am weekdays)
3. After onboarding completes, open Cowork Settings → Scheduled Tasks and set the Working Folder to your Documents folder for each of the three scheduled tasks (`cowork-email-digest`, `cowork-slack-digest`, `cowork-gemini-notes`). This step is required for the scheduled tasks to have write access to `~/Documents/ppt-index/`.

## Verification

- Run `/personal-progress-tracker:recall` and ask a question like "what did I work on today?" — if memory storage is initialized correctly, it will search the INDEX and return results (or confirm the index is empty if no sessions have been saved yet).
- Confirm `~/Documents/ppt-index/INDEX` exists and `~/Documents/ppt-index/config.md` contains your Slack channel configuration.
- Check `~/.claude/CLAUDE.md` for the auto-save block referencing `cowork-memory:save-session`.

## Notes

- This plugin is the central setup and recall hub for the entire cowork memory system. It should be set up before any of the digest plugins (`cowork-email-digest`, `cowork-slack-digest`, `cowork-gemini-notes`) are used.
- Storage path changed in v2.0.0 from `~/.cowork/memory/` to `~/Documents/ppt-index/`. The onboard skill creates the new path directly using the Write tool — no shell commands required.
- If a connector is missing during onboarding, the skill notes the failure and continues. You can connect it later in Cowork Settings → Connectors and re-run `/personal-progress-tracker:onboard`.
- The `recall` skill searches across all sources (cowork sessions, email, Slack, meetings) using the INDEX and daily session files at `~/Documents/ppt-index/s/`. It always greps the INDEX first and only reads daily files when needed.
- To change a digest schedule after onboarding, use `update_scheduled_task` with the task ID (`cowork-email-digest`, `cowork-slack-digest`, or `cowork-gemini-notes`).
