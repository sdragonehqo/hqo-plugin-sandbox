---
name: recall
description: >
  Use when the user wants to search or retrieve past work from memory.
  Handles open-ended queries like "any work with the sales team this week",
  "what did I do last Thursday", "find anything about HubSpot", "show me
  open items", "what meetings did I have about renewals", "what happened in
  Slack this week", or any request to look back at past sessions, emails,
  Slack, or meetings.
---

# Recall — Cross-Source Memory Search

Search and synthesize past context across all cowork memory sources: sessions, email, Slack, and meetings. Interpret the user's natural language query and return the right answer — don't dump raw data.

See `./references/memory-schema.md` for the complete schema.

---

## Step 0: Verify Storage

Read `~/Documents/ppt-index/INDEX` to confirm the memory system is initialized:

```
Read ~/Documents/ppt-index/INDEX
```

If the file is not found, tell the user: "Memory storage not found. Run `/personal-progress-tracker:onboard` to set up."

---

## Step 1: Interpret the Query

Translate the user's request into search parameters before touching any files.

**Date interpretation:**

| User says | Grep pattern |
|---|---|
| "today" | today's date `YYYYMMDD` |
| "yesterday" | yesterday's date |
| "this week" | date range `202603(10\|11\|12\|13\|14)` (compute actual range) |
| "last week" | prior Monday–Friday range |
| "March" / "this month" | `202603` |
| specific date "March 5th" | `20260305` |

**Source filtering:**

| User says | Add to pattern |
|---|---|
| "emails" / "inbox" | `\|email\|` |
| "Slack" / "DMs" | `\|slack\|` |
| "meetings" / "meeting notes" | `\|meeting\|` |
| "sessions" / "work I did" | `\|cowork\|` |
| no source specified | search all sources |

**Topic/person filtering:**

Extract keywords from the query — names, project names, tools, topics — and use them as grep patterns against TAGS and SUMMARY fields.

Combine filters: "emails about HubSpot this week" → date range + `\|email\|` + `hubspot`

---

## Step 2: Grep the INDEX

Always start here. Never Read the INDEX directly — it may be very large.

```
Grep pattern="PATTERN" path="~/Documents/ppt-index/INDEX" output_mode="content"
```

If no results on first try:
- Broaden the date range
- Try alternative keywords (e.g., "hubspot" → "crm", "deal")
- Try without source filter
- Try adjacent days

---

## Step 3: Read Day Files (only if needed)

For full task-level detail, read the specific daily file(s) whose dates appeared in the INDEX matches:

```
Read ~/Documents/ppt-index/s/YYYYMMDD.md
```

Only read files with matching dates from Step 2. Don't read files speculatively.

---

## Step 4: Find Open Items (special case)

For queries about open/unfinished work ("what's still open", "any pending actions", "what do I still need to do"):

```
Grep pattern="^- " path="~/Documents/ppt-index/s/" output_mode="content"
```

This returns every open item across all days with its file path (containing the date). Filter further by keyword if the query is scoped (e.g., "open items about Joel").

---

## Step 5: Synthesize and Respond

Answer the user's actual question. Structure the response based on what they asked:

- **Specific question** ("did I talk to Joel about HubSpot?") → direct answer with supporting evidence
- **Time range recap** ("what did I do this week?") → grouped by day, summarized by source
- **Open items** ("what's still unfinished?") → list of `-` items with dates and source tags
- **Topic search** ("anything about the Clari renewal?") → all matching entries across sources and dates, newest first

Always tag each result with its source: **[cowork]** **[email]** **[slack]** **[meeting]**

Lead with a direct answer or summary. Offer to drill deeper if the user wants more detail.

---

## Rules

- Always Grep the INDEX first — never Read it
- Use `~/Documents/ppt-index/` in all tool calls
- Only read daily files whose dates appeared in INDEX results
- If nothing found, suggest alternative search terms or confirm the date range
- Cite source on every result
- Summarize — don't paste raw memory entries at the user
