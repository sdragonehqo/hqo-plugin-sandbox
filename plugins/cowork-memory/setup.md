# Setup: cowork-memory

## Requirements

- Read/write access to `~/Documents/` — the plugin stores all memory at `~/Documents/ppt-index/`
- No MCP connectors required
- No external credentials or API keys required

## First-Run Steps

1. Grant Claude Code access to your Documents folder. In the Cowork toolbar, click the folder icon, select your Documents folder, and confirm.
2. Run `/personal-progress-tracker:onboard` — this creates the `~/Documents/ppt-index/` directory structure (the `INDEX` file and `s/` sessions subdirectory) and writes the auto-save global instruction to `~/.claude/CLAUDE.md`. The `cowork-memory` plugin depends on this storage being initialized before any session can be saved.
3. Once onboarding is complete, the `save-session` skill runs automatically after every completed task list — no manual invocation needed.

## Verification

After completing a task list in any session, confirm that:
- A session file exists at `~/Documents/ppt-index/s/YYYYMMDD.md` (today's date)
- A new line was appended to `~/Documents/ppt-index/INDEX`

You can also trigger it manually by saying "save session" and checking that Claude responds with `Session saved.` followed by the tags.

## Notes

- Storage path changed in v4.0.0 from `~/.cowork/memory/` to `~/Documents/ppt-index/`. If you have existing data at the old path, it will not be migrated automatically.
- The global auto-save instruction is written to `~/.claude/CLAUDE.md` by `personal-progress-tracker:onboard` — do not run `cowork-memory` setup in isolation; always use the onboard skill.
- This plugin has no digest functionality of its own. Email, Slack, and meeting digests are handled by separate plugins (`cowork-email-digest`, `cowork-slack-digest`, `cowork-gemini-notes`).
