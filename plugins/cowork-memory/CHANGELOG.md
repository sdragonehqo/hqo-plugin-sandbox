# cowork-memory Changelog

## [4.0.1] — 2026-03-13

### Fixed
- setup.md: corrected CLAUDE.md write path to use session mount (`<session-path>/.claude/CLAUDE.md`) instead of `~/.claude/CLAUDE.md` which resolves to the VM scratch dir

## [4.0.0] — 2026-03-12

### Breaking
- Moved storage from `~/.cowork/memory/` to `~/Documents/ppt-index/` to avoid per-session permission prompts in Cowork VM environments. Existing data will not be migrated automatically.

## [3.1.0] — 2025-12-01

### Changed
- Trimmed plugin to `save-session` skill only; digest skills moved to standalone plugins.
