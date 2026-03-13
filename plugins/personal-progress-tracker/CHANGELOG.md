# personal-progress-tracker Changelog

## [2.1.0] — 2026-03-13

### Changed
- `onboard` skill disabled (renamed to SKILL.md.disabled) — onboarding is now handled per-plugin by cowork-maintenance
- `recall` skill remains active and unchanged

## [2.0.0] — 2026-03-12

### Breaking
- Storage moved from `~/.cowork/memory/` to `~/Documents/ppt-index/`. Onboard now creates this directory structure directly using the Write tool (no bash mkdir required).

### Changed
- Onboard Step 0 now uses Write tool to initialize `~/Documents/ppt-index/INDEX` directly, avoiding VM ephemeral path issues.
- CLAUDE.md global instruction is written to `~/.claude/CLAUDE.md` via Read + Edit/Write (no bash).
- Config stored at `~/Documents/ppt-index/config.md`.
- Recall Step 0 verifies storage by reading `~/Documents/ppt-index/INDEX`.

### Added
- Private Slack channel ID instructions: right-click channel → Copy → Copy link → extract ID from end of URL.
- Scheduled task prompts dynamically include configured channel IDs and DM preference.

## [1.4.0] — 2025-12-01

### Added
- Recall skill with open-ended natural language query against INDEX and daily session files.
- Memory schema reference document at `skills/recall/references/memory-schema.md`.
