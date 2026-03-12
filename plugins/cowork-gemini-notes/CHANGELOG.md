# Changelog — cowork-gemini-notes

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

---

## [Unreleased]

## [2.0.0]
### Changed
- Renamed plugin from `cowork-meeting-digest` to `cowork-gemini-notes` — platform-specific naming to allow other note-taking integrations in the future
- Renamed skill from `meeting-digest` to `gemini-notes`
- Fixed Step 3: Gmail MCP returns plain text only, stripping the Google Doc hyperlink. Now falls back to `google_drive_search` using `name contains '<meeting title>'` with `modifiedTime desc` ordering to find the transcript. Avoids `fullText contains` which requires `relevance` ordering.
- Step 6 now uses `RESOLVED_MEMORY_PATH` consistently

## [1.0.0]
### Added
- Initial standalone release, extracted from cowork-memory
- Fixed path resolution for Grep/Read tool calls
