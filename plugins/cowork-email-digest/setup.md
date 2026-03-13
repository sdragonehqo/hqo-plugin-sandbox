# Setup: cowork-email-digest

## Requirements

- **Gmail MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `gmail_search_messages` and `gmail_read_message`.
- `cowork-memory` already set up — `~/Documents/ppt-index/` must exist before the digest can write to it
- Read/write access to `~/Documents/` — digests are written to `~/Documents/ppt-index/`

## Setup Steps

1. Ensure `cowork-memory` is set up first. Run `/personal-progress-tracker:onboard` if you have not already — this creates `~/Documents/ppt-index/` and initializes the index.

2. Connect Gmail in Cowork Settings → Connectors. Verify the connection by invoking `gmail_get_profile` — it should return your Gmail address without error.

3. Schedule the email digest as a recurring task. The recommended way is via `/personal-progress-tracker:onboard`, which creates the scheduled task automatically with these parameters:

   ```
   taskId: "cowork-email-digest"
   description: "Daily email digest to cowork memory"
   prompt: "Run /cowork-email-digest:email-digest"
   cronExpression: "0 8 * * 1-5"   ← weekdays at 8:00am
   ```

   If you want a different time, confirm your preferred time during onboarding or update the task afterward with `update_scheduled_task`.

4. After the task is created, open **Cowork Settings → Scheduled Tasks**, click **Edit** on the `cowork-email-digest` task, set the **Working Folder** to your **Documents** folder, and save. This step is required — scheduled tasks run in fresh sessions and need explicit folder access to write to `~/Documents/ppt-index/`.

## Verification

Run `/cowork-email-digest:email-digest` manually. It should:
1. Search Gmail for messages matching `newer_than:1d -in:draft -in:sent`
2. Categorize each email as Needs Attention, Informational, or Background Noise
3. Write one grouped entry (all emails in one block) to `~/Documents/ppt-index/s/YYYYMMDD.md`
4. Append one line to `~/Documents/ppt-index/INDEX`
5. Output a confirmation: `Email digest saved. X emails reviewed — Y need attention, Z informational, W noise (skipped)`

If no emails are found, it outputs `No new emails in the past 24 hours.` and stops — that is expected behavior.

## Notes

- All emails in a single digest run are grouped into ONE memory entry, not one entry per email
- Background noise is filtered out entirely and not written to memory. Noise patterns include: `noreply@`, `updates@`, `news@`, `billing@`, `calendar-notification@google.com`, `drive-shares-dm-noreply@google.com`, and similar automated senders
- Automated emails are promoted to Needs Attention if they contain "urgent", "action required", "expiring", "suspended", or "security alert" — or if a payment failed or a calendar invite came from a new external contact
- The Working Folder on the scheduled task must be set manually after onboarding — the skill cannot write to `~/Documents/ppt-index/` without it
- This plugin depends on `personal-progress-tracker` for scheduled task setup and on `cowork-memory` for storage
