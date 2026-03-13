# hqo-plugin-scaffold Changelog

## [1.1.0] — 2026-03-13

### Fixed
- scaffold skill: removed incorrect instruction to update skills array when adding a new skill (skills are auto-discovered)
- scaffold skill: setup.md now included in Files written confirmation
- references/skill-md.md: fixed path prefix, updated Step 0 to reference cowork-maintenance:plugin-setup
- references/marketplace-entry.md: author field is now a placeholder instead of hardcoded name
- references/changelog.md: fixed path prefix

## [1.0.0] — 2026-03-13

### Added
- Initial release.
- `scaffold` skill: generates plugin.json, SKILL.md, CHANGELOG.md for a new plugin or skill following HqO conventions.
- `merge-plugin` skill: merges a local plugin into the HqO marketplace — pushes to sandbox first, confirms install, then pushes to main.
- `build-agent` skill: adds a subagent to an existing plugin, creating the agents/ directory and updating plugin.json.
- Reference templates: plugin-json.md, skill-md.md, marketplace-entry.md, changelog.md.
