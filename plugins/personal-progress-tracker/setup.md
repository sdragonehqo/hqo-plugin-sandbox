# Setup: personal-progress-tracker

## Requirements

- No MCP tools required for this plugin itself — it orchestrates setup for the whole cowork memory system
- Read/write access to `~/Documents/` — creates and manages `~/Documents/ppt-index/`
- Write access to `~/.claude/CLAUDE.md` — the `onboard` skill writes the auto-save global instruction here
- Note: the `onboard` skill tests Gmail, Slack, and Google Drive connectors on behalf of the digest plugins during setup, but those connectors are not required by `personal-progress-tracker` itself

## Setup Steps

1. Grant Documents folder access: click the **folder icon** in the Cowork toolbar, select your **Documents** folder, and confirm. The `onboard` skill verifies access by attempting to read `~/Documents/` — if access is denied, it will stop and prompt you to grant it before continuing.

2. Run `/personal-progress-tracker:onboard`. This is the recommended first-run for the entire cowork memory system. It is conversational — it asks questions and waits for answers at each step. It walks through:

   - **Step 0 — Memory storage:** Creates `~/Documents/ppt-index/INDEX` (empty) and `~/Documents/ppt-index/s/.keep` (placeholder for the sessions subdirectory)
   - **Step 1 — Connector check:** Tests Gmail (`gmail_get_profile`), Slack (`slack_read_user_profile`), and Google Drive (`google_drive_search`) and reports pass/fail for each. Missing connectors are noted but do not block setup — you can reconnect and re-run later.
   - **Step 2 — Global instruction:** Writes the automatic session save rule to `~/.claude/CLAUDE.md` (or confirms it is already present). The rule text is: `After completing ALL items in a task list, you MUST immediately invoke the skill cowork-memory:save-session…`
   - **Step 3 — Slack channel config:** Asks which channels to monitor in addition to DMs, looks up channel IDs, and writes `~/Documents/ppt-index/config.md`
   - **Step 4 — Scheduled tasks:** Confirms digest times (defaults: email at 8:00am weekdays, Slack at 8:00am weekdays, Gemini Notes at 9:00am weekdays) and creates all three scheduled tasks

3. After onboarding completes, open **Cowork Settings → Scheduled Tasks** and set the **Working Folder** to your **Documents** folder for each of the three tasks (`cowork-email-digest`, `cowork-slack-digest`, `cowork-gemini-notes`). This manual step is required for the scheduled tasks to have write access to `~/Documents/ppt-index/`.

## Verification

- Run `/personal-progress-tracker:recall` and ask "what did I work on today?" — if storage is initialized, it will grep `~/Documents/ppt-index/INDEX` and return results (or confirm the index is empty if no sessions have been saved yet)
- Confirm `~/Documents/ppt-index/INDEX` exists
- Confirm `~/Documents/ppt-index/config.md` contains your Slack channel configuration
- Check `~/.claude/CLAUDE.md` for the auto-save block referencing `cowork-memory:save-session`

## Notes

- This plugin is the central setup and recall hub for the entire cowork memory system. Set it up before using any of the digest plugins (`cowork-email-digest`, `cowork-slack-digest`, `cowork-gemini-notes`).
- The `onboard` skill is conversational by design — it asks and waits. Do not skip steps; if a connector is missing, the skill notes it and continues.
- The `recall` skill searches across all sources (sessions tagged `cowork`, `email`, `slack`, `meeting`) by grepping `~/Documents/ppt-index/INDEX` first, then reading only the specific daily files at `~/Documents/ppt-index/s/YYYYMMDD.md` whose dates appear in the index results. It never reads the full INDEX directly.
- If a connector was missing during onboarding, connect it in Cowork Settings → Connectors and re-run `/personal-progress-tracker:onboard`. The skill checks for an existing `cowork-memory:save-session` rule before writing and will skip that step if already done.
- To change a digest schedule after onboarding, use `update_scheduled_task` with the relevant task ID (`cowork-email-digest`, `cowork-slack-digest`, or `cowork-gemini-notes`).
