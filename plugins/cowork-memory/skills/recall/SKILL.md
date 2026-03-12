---
name: recall
description: >
  This skill should be used when the user asks "what did I do on...",
  "find my work on...", "recall", "search sessions", "what's still open",
  "show me last week", "what emails came in", "what happened in slack",
  "meeting notes from...", or any request to search or retrieve past
  context from the cowork memory system — across all sources (cowork
  sessions, email, slack, meetings).
version: 2.0.0
---

# Recall from Cowork Memory

Search and retrieve past context from the unified cowork memory system. This searches across ALL sources: cowork sessions, emails, Slack messages, and meetings.

## Step 0: Verify Storage Exists (ALWAYS run first)

Run this command before doing anything else:

```bash
test -f ~/.cowork/memory/INDEX || { echo "No memory found."; exit 1; }
```

If it fails, tell the user: "No cowork memory found. Run `/setup` to initialize the memory system."

**Storage layout:**
- `~/.cowork/memory/INDEX` — one line per entry, pipe-delimited
- `~/.cowork/memory/s/YYYYMMDD.md` — daily detail files
- Index line format: `DATE|TIME|USER|SOURCE|TAGS|SUMMARY`
- SOURCE values: `cowork`, `email`, `slack`, `meeting`

## Search Strategy

**ALWAYS start with the INDEX file using Grep. NEVER Read the INDEX directly — it may be very large.**

### Step 1: Interpret the Query

| User asks | Search type | Grep pattern on INDEX |
|-----------|------------|----------------------|
| "What did I do March 5th?" | Date | `20260305` |
| "Last week's work" | Date range | `2026030[3-7]` |
| "Everything in March" | Month | `202603` |
| "HubSpot work" | Topic | `hubspot` |
| "What did John do?" | User | `\|john` |
| "Show me emails" | Source | `\|email\|` |
| "Slack activity" | Source | `\|slack\|` |
| "Meeting notes" | Source | `\|meeting\|` |
| "Cowork sessions only" | Source | `\|cowork\|` |
| "What's still open?" | Open tasks | (see Step 3) |
| "Recent sessions" | Last N days | Compute date range, Grep |

Combine filters when needed. Example: "HubSpot emails" → Grep for lines matching both `email` and `hubspot`.

### Step 2: Grep the Index

```
Grep pattern="<pattern>" path="~/.cowork/memory/INDEX"
```

This returns matching lines with date, time, user, source, tags, and summary. Often this alone answers the question.

### Step 3: Read Session Details (only if needed)

If the user wants task-level detail or full context, read the specific day file:
```
Read ~/.cowork/memory/s/YYYYMMDD.md
```

Only read files whose dates appeared in the index matches.

### Step 4: Find Open Tasks (special case)

To find all open/incomplete tasks across all sources:
```
Grep pattern="^- " path="~/.cowork/memory/s/"
```

This returns every open item with its file path (which contains the date).

### Step 5: Present Results

- Group by date if spanning multiple days
- Tag each result with its source: **[cowork]**, **[email]**, **[slack]**, **[meeting]**
- Show summary and task/item list for each entry
- Highlight open items
- If the user asked a specific question, answer it directly — don't dump raw data
- Keep output concise unless the user asks for full detail

## Rules

- ALWAYS Grep the INDEX first — never Read it
- Only Read session files that matched in the INDEX
- If no results found, suggest alternative search terms or date ranges
- If `~/.cowork/memory/` doesn't exist, tell the user to run `/setup` first
