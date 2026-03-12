# cowork-gemini-notes Changelog

## [3.0.0] — 2026-03-12

### Breaking
- Moved storage target from `~/.cowork/memory/` to `~/Documents/ppt-index/`. Storage must be initialized via `personal-progress-tracker:onboard` before first use.

### Changed
- Renamed from `cowork-meeting-digest` / `meeting-digest` skill to `cowork-gemini-notes` / `gemini-notes` for platform-specific clarity.

### Fixed
- Gmail MCP only returns `text/plain` MIME part, stripping the Google Drive link embedded as an HTML hyperlink. Step 3 now tries link extraction first, then falls back to `google_drive_search` with `name contains '<meeting title>'` + `modifiedTime desc` ordering.
- Replaced `fullText contains` Drive query with `name contains` to allow `modifiedTime desc` ordering (previously caused a Drive API validation error).

## [2.1.0] — 2025-12-01

### Added
- Initial standalone release as `cowork-meeting-digest`.
- ONE entry per meeting (not grouped like email/slack).
- `+ Decision:`, `- Action:`, `- Open:` item prefix conventions.
