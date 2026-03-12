---
name: meeting-digest
description: >
  This skill should be used when the user asks to "digest my meetings",
  "review my transcripts", "meeting notes", "process gemini notes",
  "what meetings did I have", "summarize my meetings", or any request
  involving pulling Gemini Notes emails and extracting structured
  meeting summaries from linked Google Drive transcripts.
version: 2.0.0
---

# Meeting Digest → Cowork Memory

Process Gemini Notes emails from the past 24 hours, retrieve linked Google Drive transcripts, and write structured entries to the cowork memory system. This skill is fully automated — do NOT ask the user for any input.

## Step 0: Ensure Storage Exists (ALWAYS run first)

```bash
mkdir -p ~/.cowork/memory/s && touch ~/.cowork/memory/INDEX
```

## Step 1: Search Gmail for Gemini Notes

Use `gmail_search_messages` with query `from:gemini-notes@google.com newer_than:1d` to find all Gemini Notes emails from the past 24 hours.

If no emails found, output `No new meeting notes in the past 24 hours.` and stop.

## Step 2: Read Each Email

For each email, use `gmail_read_message` to get the full body. Extract all Google Drive links:
- `https://docs.google.com/document/d/DOCUMENT_ID/...`

Record the email subject, sender, and timestamp alongside each link.

## Step 3: Fetch Transcript Content

For each Google Drive document link:
1. Extract the document ID (segment after `/d/` and before the next `/`)
2. Use `google_drive_fetch` with the document ID to get the full transcript

If a document can't be fetched, note `[Transcript unavailable]` and continue.

## Step 4: Analyze Each Transcript

For each transcript, extract:

**Attendees** — all participants mentioned

**Decisions** — explicit decisions, commitments, agreements → format as `+` items

**Action Items** — tasks assigned, with owner if mentioned → format as `-` items

**Key Topics** — main subjects discussed, each with 1-2 sentences of context

**Open Questions** — unresolved items flagged for follow-up → format as `-` items

## Step 5: Generate Metadata

Run `date +%Y%m%d` for DATE and `date +%H%M` for TIME. Run `whoami` once for USER.

**Write ONE entry per meeting** (not grouped like email/slack — each meeting gets its own entry because meetings are distinct events with distinct attendees and topics).

For each meeting:

| Field | Format |
|-------|--------|
| DATE | `YYYYMMDD` |
| TIME | `HHMM` of the meeting (from email timestamp), or digest run time if unavailable |
| USER | System username |
| SOURCE | Always `meeting` |
| TAGS | 3–5 lowercase keywords. Include meeting type, key topics, attendee names. |
| SUMMARY | Max 120 chars. Meeting title + key outcome or decision. |

## Step 6: Write to Memory

For each meeting, append to `~/.cowork/memory/s/DATE.md`:
```
## TIME|USER|meeting|TAGS
SUMMARY
+ Decision: approved Q2 budget for marketing campaign
+ Decision: moving to weekly standups instead of biweekly
- Action: Joel to send updated HubSpot workflow by Friday
- Action: Sal to review Clari renewal terms
- Open: need to decide on vendor for event platform
```

For each meeting, append ONE line to `~/.cowork/memory/INDEX`:
```
DATE|TIME|USER|meeting|TAGS|SUMMARY
```

## Step 7: Confirm

Output:
```
Meeting digest saved.
X meeting(s) processed
tags: tag1, tag2, tag3
```

Then list each meeting title with its action item count.

## Rules

- NEVER prompt the user for input
- Write ONE entry per meeting (not grouped)
- Prefix decisions with `+ Decision:` and actions with `- Action:` and open questions with `- Open:`
- Always fetch the full transcript — don't summarize from email preview alone
- If an email has no Drive links, note it and continue
- Do not read or modify existing INDEX/session entries — append only
