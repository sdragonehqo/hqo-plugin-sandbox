---
name: task-master
description: >
  This skill should be used when the user asks to "review my digests",
  "what happened today", "show me everything", "task master", "daily briefing",
  "what needs my attention", "unified summary", "what action items do I have",
  "what decisions were made today", or any request involving reviewing,
  querying, or acting on information from multiple sources.
version: 2.0.0
---

# Task Master — Cross-Source Intelligence

Read the cowork memory INDEX, build a unified briefing across all sources (cowork, email, slack, meetings), and support interactive follow-up.

## Step 0: Verify Storage Exists

```bash
test -f ~/.cowork/memory/INDEX || { echo "No memory found."; exit 1; }
```

If it fails, tell the user to run `/setup` first.

## Phase 1: Load Today's Context

### 1a. Get today's date

```bash
date +%Y%m%d
```

### 1b. Grep the INDEX for today

```
Grep pattern="DATE" path="~/.cowork/memory/INDEX"
```

If no results for today, try the last 3 days by adjusting the date pattern.

### 1c. Read today's session file

```
Read ~/.cowork/memory/s/DATE.md
```

This gives you all entries across all sources for the day — cowork sessions, emails, slack, meetings — in one file.

## Phase 2: Build Unified Briefing

Parse all entries from the session file. Group and present in this order:

### Needs Your Attention
Items with `-` prefix across ALL sources. These are actions, replies needed, and open questions. Tag each with its source.

Format:
```
- **[email]** Reply to Joel re: HubSpot workflow approval
- **[slack]** Respond to Liz Thomson re Ozzy status
- **[meeting]** Action: Send updated Clari terms to finance by Friday
- **[cowork]** Build dashboard UI (in progress)
```

### Decisions Made
Items with `+ Decision:` prefix (from meetings) or significant `+` items that represent outcomes.

### Key Activity
Brief summary of what happened across all sources, grouped by source:
- **Cowork:** X sessions — [themes]
- **Email:** X emails — Y need attention
- **Slack:** X conversations — Y need response
- **Meetings:** X meetings — [key topics]

### Open Questions
Items with `- Open:` prefix from meetings, plus any `-` items that are unresolved questions.

### Quick Stats
```
Sources loaded: cowork, email, slack, meeting
Total entries: X
Action items: Y
Items needing response: Z
```

## Phase 3: Interactive Mode

After presenting the briefing, support follow-up:

**Questions about the data:**
- "What did [person] say?" → Grep INDEX for person name, read relevant session files
- "Were there action items about [topic]?" → Grep session files
- "What happened with [project]?" → Grep INDEX for project tag

**Take actions:**
- "Draft a reply to [person]" → Use Gmail or Slack tools
- "Send a message to [person] about [topic]" → Use Slack tools
- "Compile a report on [topic]" → Search memory and create document

**Historical queries:**
- "What happened this week?" → Grep INDEX for date range
- "Has [topic] come up before?" → Grep INDEX for topic across all dates
- "Show me action items from the past 3 days" → Grep session files for `^- `

**Filter by source:**
- "Show me just Slack items" → Grep INDEX for `|slack|`
- "Meeting notes only" → Grep INDEX for `|meeting|`

## Rules

- Always cite the source when presenting items: **[cowork]**, **[email]**, **[slack]**, **[meeting]**
- Prioritize "Needs Your Attention" — these are time-sensitive
- Keep briefing concise — save detail for when the user asks
- Use Grep on INDEX for historical queries, never Read the INDEX
- When the user asks to take action, use the appropriate tools directly
