# Changelog

## 1.1.0 — 2026-03-13

### Changed
- plugin-setup: use `request_cowork_directory ~/Documents` (not subdirectory) for access
- plugin-setup: replace `claude plugins list` with direct read of `installed_plugins.json` via glob (VM-aware)
- plugin-setup: replace `gh repo clone` with local cache read at `<session-path>/.local-plugins/cache/`
- plugin-setup: exclude `cowork-maintenance` itself from setup queue
- plugin-setup: CLAUDE.md write path now uses `<session-path>/.claude/CLAUDE.md`

## 1.0.0 — 2026-03-13

- Initial release
- `plugin-setup` skill: automatically detects installed HqO plugins, diffs against maintenance log, and runs onboarding for any that haven't been set up yet
