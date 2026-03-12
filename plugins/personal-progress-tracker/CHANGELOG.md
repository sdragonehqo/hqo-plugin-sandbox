# Changelog — personal-progress-tracker

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

---

## [Unreleased]

## [1.3.0]
### Changed
- Slack channel lookup now includes instructions for grabbing the ID manually via right-click → Copy → Copy link in the Slack sidebar
- Updated meeting digest scheduled task reference from `cowork-meeting-digest` to `cowork-gemini-notes`

## [1.2.0]
### Changed
- Global instruction setup now writes directly to `~/.claude/CLAUDE.md` instead of prompting the user to paste it manually
- Slack channel lookup now falls back to searching private channels if not found in public search

## [1.1.0]
### Changed
- Slack scheduled task prompt now embeds channel IDs and DM preference collected during onboarding, so the task is self-documenting in the Cowork UI

## [1.0.0]
### Added
- `onboard` skill — connector verification, global instruction setup, Slack channel config, scheduled digest creation
- `recall` skill — open-ended cross-source memory search across cowork, email, Slack, and meeting sources
- `memory-schema.md` reference doc for recall skill
