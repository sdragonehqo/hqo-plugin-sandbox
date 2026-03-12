---
name: gemini-notes
description: >
  This skill should be used when the user asks to "digest my meetings",
  "review my transcripts", "meeting notes", "process gemini notes",
  "what meetings did I have", "summarize my meetings", or any request
  involving pulling Google Gemini Notes emails and extracting structured
  meeting summaries from linked Google Drive transcripts.
---

# Gemini Notes Digest → Cowork Memory

Process Google Gemini Notes emails from the past 24 hours, retrieve linked Google Drive transcripts, and write structured entries to the cowork memory system. This skill is fully automated — do NOT ask the user for any input.

## Step 0: Mount and Resolve Memory Path

Each Cowork session runs in a fresh VM — your local memory directory must be explicitly mounted before any reads or writes, otherwise data is written to the VM's ephemeral storage and lost when the session ends.

**First, use the Read tool to trigger the mount:**

```
Read ~/.cowork/memory/INDEX
```

This causes Cowork to mount your local `~/.cowork/memory` into the session. If the file doesn't exist yet, continue anyway.

**Then create the directory structure and resolve the absolute path:**

```bash
mkdir -p ~/.cowork/memory/s && touch ~/.cowork/memory/INDEX && MEMORY_DIR=$(eval echo ~/.cowork/memory) && echo "$MEMORY_DIR"
```

Use the printed absolute path in all subsequent Grep, Read, and Write tool calls. Never use `~` in tool call paths.

## Step 1: Search Gmail for Gemini Notes

Use `gmail_search_messages` with query `from:gemini-notes@google.com newer_than:1d` to find all Gemini Notes emails from the past 24 hours.

If no emails found, output `No new Gemini Notes in the past 24 hours.` and stop.

## Step 2: Read Each Email

For each email, use `gmail_read_message` to get the full body. Record the email subject, sender, and `internalDate` timestamp.

Note: the Gmail MCP only returns the plain-text MIME part. Gemini Notes embed the Google Doc link as an HTML hyperlink ("Open meeting notes") which is stripped from plain text — Step 3 handles this with a Drive search fallback.

## Step 3: Get the Transcript Document

Follow this sequence for each email:

**3a. Try extracting a link from the email body:**
Look for `https://docs.google.com/document/d/DOCUMENT_ID/...` in the email body. If found, skip to 3c.

**3b. Fallback — Search Google Drive:**
If no link is found (expected in most cases due to the plain-text limitation):

1. Extract the meeting title from the email subject. Subject format is typically:
   `Notes: "Meeting Title" Mon DD, YYYY` → extract the quoted portion as the search term
2. Call `google_drive_search` with:
   - query: `name contains '<meeting title>' and mimeType = 'application/vnd.google-apps.document'`
   - order_by: `modifiedTime desc`
3. From the results, pick the document whose `created_time` is closest to the email's `internalDate`
4. Extract the document ID from the returned `uri` field (strip the `drive:///` prefix)

If Drive search returns no results, note `[Transcript unavailable]` and continue to the next email.

**3c. Fetch the transcript:**
Call `google_drive_fetch` with the document ID to retrieve the full transcript text.

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

For each meeting, append to `RESOLVED_MEMORY_PATH/s/DATE.md`:
```
## TIME|USER|meeting|TAGS
SUMMARY
+ Decision: approved Q2 budget for marketing campaign
+ Decision: moving to weekly standups instead of biweekly
- Action: Joel to send updated HubSpot workflow by Friday
- Action: Sal to review Clari renewal terms
- Open: need to decide on vendor for event platform
```

For each meeting, append ONE line to `RESOLVED_MEMORY_PATH/INDEX`:
```
DATE|TIME|USER|meeting|TAGS|SUMMARY
```

## Step 7: Confirm

Output:
```
Gemini Notes digest saved.
X meeting(s) processed
tags: tag1, tag2, tag3
```

Then list each meeting title with its action item count.

## Rules

- NEVER prompt the user for input
- Write ONE entry per meeting (not grouped)
- Prefix decisions with `+ Decision:` and actions with `- Action:` and open questions with `- Open:`
- Always attempt to fetch the full transcript via Drive search — do not summarize from email preview alone
- Use `name contains` (not `fullText contains`) in Drive search — it supports `modifiedTime desc` ordering
- Do not read or modify existing INDEX/session entries — append only
