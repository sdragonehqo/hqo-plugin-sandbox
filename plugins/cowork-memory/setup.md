# Setup: cowork-memory

## Requirements

- No MCP tools required
- Read/write access to `~/Documents/` — all memory is stored at `~/Documents/ppt-index/`
- `personal-progress-tracker` plugin installed (its `onboard` skill initializes the storage and writes the global instruction)

## Setup Steps

1. Grant Documents folder access: click the **folder icon** in the Cowork toolbar, select your **Documents** folder, and confirm. This is required before any memory read or write will work.

2. Run `/personal-progress-tracker:onboard` — this is the recommended first-run path. It creates the memory storage directory structure automatically:
   - `~/Documents/ppt-index/INDEX` — empty file; the append-only index of all sessions
   - `~/Documents/ppt-index/s/.keep` — placeholder that creates the daily sessions subdirectory

3. The `onboard` skill also writes the automatic session save rule to `~/.claude/CLAUDE.md`. If you need to add it manually, append this block exactly:

   ```
   ## Automatic Session Save

   IMPORTANT — AUTOMATIC SESSION SAVE:
   After completing ALL items in a task list, you MUST immediately invoke the skill `cowork-memory:save-session` to persist the session to local memory. Do not wait for the user to ask. Do not skip this step. This is mandatory after every completed task list.
   ```

   To verify the rule is in place, check `~/.claude/CLAUDE.md` for the string `cowork-memory:save-session`. If it is already present, no action is needed.

## Verification

Complete a task list in any Cowork session. The agent should automatically invoke `cowork-memory:save-session` at the end without being prompted. Confirm by checking:
- A session file exists at `~/Documents/ppt-index/s/YYYYMMDD.md` for today's date
- A new line was appended to `~/Documents/ppt-index/INDEX`

You can also invoke the skill manually by saying "save session" — it should respond with `Session saved.` followed by tags, without asking for any input.

## Notes

- Memory is stored locally at `~/Documents/ppt-index/`; daily session files live at `~/Documents/ppt-index/s/YYYYMMDD.md`
- The `INDEX` file is append-only — one pipe-delimited line per session in the format `DATE|TIME|USER|SOURCE|TAGS|SUMMARY`
- The auto-save rule in `~/.claude/CLAUDE.md` applies globally to every new Cowork session automatically
- If Documents folder access is lost (e.g., after a system restart), re-grant it via the Cowork toolbar folder icon
- This plugin has no digest functionality of its own — email, Slack, and meeting digests are handled by `cowork-email-digest`, `cowork-slack-digest`, and `cowork-gemini-notes` respectively
