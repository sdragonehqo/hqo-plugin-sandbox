# Setup: cowork-gemini-notes

## Requirements

- **Gmail MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `gmail_search_messages` and `gmail_read_message` to find Gemini Notes emails from `gemini-notes@google.com`.
- **Google Drive MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `google_drive_search` and `google_drive_fetch` to retrieve meeting transcript documents from Drive.
- `cowork-memory` already set up — `~/Documents/ppt-index/` must exist before the digest can write to it
- Read/write access to `~/Documents/` — digests are written to `~/Documents/ppt-index/`

## Setup Steps

1. Ensure `cowork-memory` is set up first. Run `/personal-progress-tracker:onboard` if you have not already — this creates `~/Documents/ppt-index/` and initializes the index.

2. Connect **Gmail** in Cowork Settings → Connectors. Verify by invoking `gmail_get_profile` — it should return your Gmail address without error.

3. Connect **Google Drive** in Cowork Settings → Connectors. Verify by invoking `google_drive_search` with a minimal query — it should return results without error.

4. Ensure Google Gemini is enabled in your Google Workspace and configured to send meeting notes emails. After meetings, Gemini sends summary emails from `gemini-notes@google.com` containing a link to the Google Docs transcript. Both the Gmail and Google Drive connectors must be active for the full transcript to be fetched.

5. Schedule the Gemini Notes digest as a recurring task. The recommended way is via `/personal-progress-tracker:onboard`, which creates the task automatically:

   ```
   taskId: "cowork-gemini-notes"
   description: "Daily Gemini Notes digest to cowork memory"
   prompt: "Run /cowork-gemini-notes:gemini-notes"
   cronExpression: "0 9 * * 1-5"   ← weekdays at 9:00am
   ```

   If you want a different time, confirm your preferred time during onboarding or update the task afterward with `update_scheduled_task`.

6. After the task is created, open **Cowork Settings → Scheduled Tasks**, click **Edit** on the `cowork-gemini-notes` task, set the **Working Folder** to your **Documents** folder, and save.

## Verification

Run `/cowork-gemini-notes:gemini-notes` manually on a day when you have had meetings. It should:
1. Search Gmail for messages from `gemini-notes@google.com` in the past 24 hours
2. For each email, attempt to extract a Google Docs link from the body; if not found (expected — the Gmail MCP strips HTML hyperlinks from plain-text output), fall back to searching Drive with `name contains '<meeting title>'` ordered by `modifiedTime desc`
3. Fetch the full transcript document with `google_drive_fetch`
4. Extract attendees, decisions (`+` items), action items (`-` items), key topics, and open questions (`-` items)
5. Write one entry per meeting (not grouped) to `~/Documents/ppt-index/s/YYYYMMDD.md`
6. Append one line per meeting to `~/Documents/ppt-index/INDEX`
7. Output: `Gemini Notes digest saved. X meeting(s) processed` followed by each meeting title and its action item count

If no Gemini Notes emails are found, it outputs `No new Gemini Notes in the past 24 hours.` and stops — that is expected behavior.

## Notes

- Unlike the email and Slack digests, each meeting gets its own separate memory entry — they are not grouped into one
- Decisions are prefixed `+ Decision:`, action items `- Action:`, and open questions `- Open:` in the session file
- The Gmail MCP returns only the plain-text MIME part, which strips the embedded Google Doc hyperlink from Gemini Notes emails. The Drive search fallback uses `name contains` (not `fullText contains`) because `fullText contains` does not support `modifiedTime desc` ordering — this is by design
- If a transcript document cannot be found on Drive, the skill notes `[Transcript unavailable]` for that meeting and continues processing the rest
- The Working Folder on the scheduled task must be set manually after onboarding — the skill cannot write to `~/Documents/ppt-index/` without it
- This plugin depends on `personal-progress-tracker` for scheduled task setup and on `cowork-memory` for storage
