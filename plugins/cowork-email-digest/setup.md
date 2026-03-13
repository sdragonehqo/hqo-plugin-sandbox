# Setup: cowork-email-digest

## Requirements

- **Gmail MCP connector** — must be connected in Cowork Settings → Connectors. The skill calls `gmail_search_messages` and `gmail_read_message`.
- Read/write access to `~/Documents/` — writes digests to `~/Documents/ppt-index/`
- `cowork-memory` and `personal-progress-tracker` must be set up first — the `~/Documents/ppt-index/` storage must already exist before the digest can write to it

## First-Run Steps

1. Ensure `cowork-memory` is set up and `personal-progress-tracker:onboard` has been run so that `~/Documents/ppt-index/` exists.
2. Connect Gmail in Cowork Settings → Connectors. Verify the connection by running `gmail_get_profile` — it should return your Gmail address without error.
3. Run `/personal-progress-tracker:onboard` (if not already done) to schedule the email digest as a recurring task. The default schedule is weekdays at 8:00am via the `cowork-email-digest` scheduled task.
4. After scheduling, open Cowork Settings → Scheduled Tasks, click Edit on the `cowork-email-digest` task, set the Working Folder to your Documents folder, and save. This is required for the scheduled task to have write access to `~/Documents/ppt-index/`.

## Verification

Run `/cowork-email-digest:email-digest` manually. It should:
1. Search Gmail for messages from the past 24 hours
2. Categorize them as Needs Attention, Informational, or Background Noise
3. Write one grouped entry to `~/Documents/ppt-index/s/YYYYMMDD.md`
4. Append one line to `~/Documents/ppt-index/INDEX`
5. Output a summary: `Email digest saved. X emails reviewed — Y need attention, Z informational, W noise (skipped)`

## Notes

- Storage path changed in v2.0.0 from `~/.cowork/memory/` to `~/Documents/ppt-index/`. If you have entries at the old path, they will not be migrated.
- All emails in a single digest run are grouped into ONE memory entry (not one entry per email).
- Background noise (noreply senders, newsletters, billing receipts, calendar notifications) is filtered out and not written to memory.
- Automated senders are promoted to "Needs Attention" if the email contains keywords like "urgent", "action required", "expiring", "suspended", or "security alert".
- This plugin depends on `personal-progress-tracker` for scheduled task setup and on `cowork-memory` for storage.
