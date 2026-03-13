# Setup: cowork-gemini-notes

## Requirements

- **Gmail MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `gmail_search_messages` and `gmail_read_message` to find Gemini Notes emails from `gemini-notes@google.com`.
- **Google Drive MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `google_drive_search` and `google_drive_fetch` to retrieve meeting transcript documents.
- Read/write access to `~/Documents/` — writes digests to `~/Documents/ppt-index/`
- `cowork-memory` and `personal-progress-tracker` must be set up first — the `~/Documents/ppt-index/` storage must already exist before the digest can write to it

## First-Run Steps

1. Ensure `cowork-memory` is set up and `personal-progress-tracker:onboard` has been run so that `~/Documents/ppt-index/` exists.
2. Connect Gmail in Cowork Settings → Connectors (required for finding Gemini Notes emails).
3. Connect Google Drive in Cowork Settings → Connectors (required for fetching transcript documents). Verify by running `google_drive_search` with a minimal query — it should return results without error.
4. Ensure Google Gemini is enabled in your Google Workspace and configured to send meeting notes emails. Notes emails arrive from `gemini-notes@google.com` after meetings.
5. Run `/personal-progress-tracker:onboard` (if not already done) to schedule the digest as a recurring task. The default schedule is weekdays at 9:00am via the `cowork-gemini-notes` scheduled task.
6. After scheduling, open Cowork Settings → Scheduled Tasks, click Edit on the `cowork-gemini-notes` task, set the Working Folder to your Documents folder, and save.

## Verification

Run `/cowork-gemini-notes:gemini-notes` manually on a day when you have had meetings. It should:
1. Search Gmail for messages from `gemini-notes@google.com` in the past 24 hours
2. For each email, attempt to extract a Google Docs link or search Google Drive by meeting title using `name contains '<title>'` ordered by `modifiedTime desc`
3. Fetch each transcript document and extract decisions, action items, attendees, and open questions
4. Write one entry per meeting (not grouped) to `~/Documents/ppt-index/s/YYYYMMDD.md`
5. Append one line per meeting to `~/Documents/ppt-index/INDEX`
6. Output: `Gemini Notes digest saved. X meeting(s) processed` followed by each meeting title and its action item count

## Notes

- Storage path changed in v3.0.0 from `~/.cowork/memory/` to `~/Documents/ppt-index/`. Existing data at the old path will not be migrated.
- This plugin was renamed in v3.0.0 from `cowork-meeting-digest` / `meeting-digest` to `cowork-gemini-notes` / `gemini-notes`.
- Unlike email and Slack digests, each meeting gets its own memory entry — they are not grouped.
- The Gmail MCP returns plain-text MIME parts only, stripping the embedded Google Doc hyperlink from Gemini Notes emails. The skill handles this with a Google Drive search fallback using `name contains` (not `fullText contains`) to allow `modifiedTime desc` ordering. This is by design.
- If a transcript document cannot be found on Drive, the entry is noted as `[Transcript unavailable]` and the skill continues.
- This plugin depends on `personal-progress-tracker` for scheduled task setup and on `cowork-memory` for storage.
