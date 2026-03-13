# CHANGELOG.md — Reference Template

Paste this into `plugins/<plugin-name>/CHANGELOG.md` and fill in the date.

```markdown
# <plugin-name> Changelog

## [1.0.0] — YYYY-MM-DD

### Added
- Initial release.
- <Skill name> skill: <one line on what it does>.
```

## Versioning conventions

| Change type | Bump | Example |
|---|---|---|
| Removed skill, renamed skill, changed storage path | MAJOR | `1.0.0` → `2.0.0` |
| New skill, new major feature | MINOR | `1.0.0` → `1.1.0` |
| Bug fix, wording update, minor fix | PATCH | `1.0.0` → `1.0.1` |

## Section headers

- `### Added` — new skills, new features
- `### Changed` — renamed, restructured, behavior changed
- `### Fixed` — bugs corrected
- `### Breaking` — call out breaking changes explicitly at the top of the entry

## Where to bump the version

Only bump the version in `.claude-plugin/marketplace.json`.
Never add a `version` field to `plugin.json`.
