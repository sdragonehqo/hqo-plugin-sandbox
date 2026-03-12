# cowork-slack-digest Changelog

## [2.0.0] — 2026-03-12

### Breaking
- Moved storage target from `~/.cowork/memory/` to `~/Documents/ppt-index/`. Storage must be initialized via `personal-progress-tracker:onboard` before first use.
- Channel config now read from `~/Documents/ppt-index/config.md` (previously `~/.cowork/memory/config.md`).

## [1.1.0] — 2025-12-01

### Added
- Scheduled task prompt now dynamically includes configured channel IDs and DM preference from onboarding.
- Instructions for finding private channel IDs via Slack copy-link method.

## [1.0.0] — 2025-11-01

### Added
- Initial standalone release, split from cowork-memory.
- DM and configured channel digest with thread expansion.
