---
name: email-digest
description: >
  This skill should be used when the user asks to "digest my emails",
  "review my inbox", "email daily digest", "summarize my emails",
  "what emails came in", "check my inbox", or any request involving
  reviewing recent emails and producing a structured summary.
version: 2.0.0
---

# Email Daily Digest → Cowork Memory

Review emails from the past 24 hours, categorize by importance, and write structured entries to the cowork memory system. This skill is fully automated — do NOT ask the user for any input.

## Step 0: Ensure Storage Exists (ALWAYS run first)

```bash
mkdir -p ~/.cowork/memory/s && touch ~/.cowork/memory/INDEX
```

## Step 1: Search Gmail

Use `gmail_search_messages` with query `newer_than:1d -in:draft -in:sent` to find all received emails from the past 24 hours.

If no emails are found, output `No new emails in the past 24 hours.` and stop.

## Step 2: Read and Categorize Each Email

For each email, use `gmail_read_message` to get the full content. Categorize into:

**Needs Attention** — requires reply, decision, or action. Direct questions, meeting requests from humans, client/colleague messages, approval requests, escalations, time-sensitive asks.

**Informational** — worth noting but no immediate action. Announcements, FYI threads, shipping confirmations, reference material.

**Background Noise** — skip these entirely, do NOT write to memory. Google Calendar notifications, Drive notifications, product updates, marketing newsletters, billing receipts, social media notifications. Sender patterns: `noreply@`, `updates@`, `news@`, `billing@`, `calendar-notification@google.com`, `drive-shares-dm-noreply@google.com`.

**Edge case — promote to Needs Attention:** Even if automated, escalate if payment failed/declined, calendar event from new external contact, or contains "urgent", "action required", "expiring", "suspended", "security alert".

## Step 3: Generate Metadata

Run `date +%Y%m%d` for DATE and `date +%H%M` for TIME. Run `whoami` once for USER.

For each email that is **Needs Attention** or **Informational**, generate:

| Field | Format |
|-------|--------|
| DATE | `YYYYMMDD` — date the digest ran |
| TIME | `HHMM` — time the digest ran |
| USER | System username |
| SOURCE | Always `email` |
| TAGS | 3–5 lowercase keywords from email content. Include sender name as tag if relevant. |
| SUMMARY | Max 120 chars. What the email is about + why it matters. |

## Step 4: Format Items

For each email entry, format action items:
- `+` = no action needed (informational)
- `-` = needs reply/action (attention items)

Example:
```
- Reply to Joel re: HubSpot workflow approval
- Review Clari renewal terms by Friday
+ FYI: Q2 planning doc shared by marketing team
```

## Step 5: Write to Memory

**Group all emails into ONE entry per digest run** (not one entry per email).

Append to `~/.cowork/memory/s/DATE.md`:
```
## TIME|USER|email|TAGS
SUMMARY
- action item from email 1
- action item from email 2
+ informational item
```

Append ONE line to `~/.cowork/memory/INDEX`:
```
DATE|TIME|USER|email|TAGS|SUMMARY
```

The SUMMARY should be an overview like: `4 emails reviewed — 2 need attention (Joel re HubSpot, Clari renewal)`

The TAGS should cover all emails in the batch: `joel,hubspot,clari,renewal,marketing`

## Step 6: Confirm

Output:
```
Email digest saved.
X emails reviewed — Y need attention, Z informational, W noise (skipped)
tags: tag1, tag2, tag3
```

Then list the attention items with one-line descriptions.

## Rules

- NEVER prompt the user for input
- Skip background noise entirely — do not write it to memory
- Group all emails into ONE memory entry per digest run
- Do not read or modify existing INDEX/session entries — append only
