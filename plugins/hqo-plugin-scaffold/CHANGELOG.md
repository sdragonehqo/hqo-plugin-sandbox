# hqo-plugin-scaffold Changelog

## [1.0.0] — 2026-03-13

### Added
- Initial release.
- `scaffold` skill: generates plugin.json, SKILL.md, CHANGELOG.md for a new plugin or skill following HqO conventions.
- `merge-plugin` skill: merges a local plugin into the HqO marketplace — pushes to sandbox first, confirms install, then pushes to main.
- `build-agent` skill: adds a subagent to an existing plugin, creating the agents/ directory and updating plugin.json.
- Reference templates: plugin-json.md, skill-md.md, marketplace-entry.md, changelog.md.
