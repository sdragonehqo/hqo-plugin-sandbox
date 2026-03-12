# Cowork Memory — Schema Reference

Complete reference for how all 4 plugins write to the memory system.
Used by the recall skill to interpret and query stored data.

---

## Storage Layout

```
~/Documents/ppt-index/
├── INDEX              — one line per entry, all sources, Grep-optimized
├── config.md          — Slack channel configuration (written by onboard)
└── s/
    ├── YYYYMMDD.md    — daily detail files, one per calendar day
    └── ...
```

---

## INDEX Format

One pipe-delimited line per entry. Appended only — never modified in place.

```
DATE|TIME|USER|SOURCE|TAGS|SUMMARY
```

| Field | Format | Example |
|---|---|---|
| DATE | YYYYMMDD | `20260312` |
| TIME | HHMM 24hr | `0930` |
| USER | system username | `sal.dragone` |
| SOURCE | see sources table below | `email` |
| TAGS | 3–5 lowercase, comma-separated | `joel,hubspot,renewal` |
| SUMMARY | max 120 chars, plain text | `4 emails — 2 need attention` |

---

## Daily File Format

File: `~/.cowork/memory/s/YYYYMMDD.md`

```markdown
# YYYYMMDD

## TIME|USER|SOURCE|TAGS
SUMMARY
+ completed item or decision made
+ another completed or informational item
- open action item or reply needed
- Open: unresolved question
```

Multiple entry blocks per file, separated by a blank line. Each block maps to one INDEX line.

---

## Sources

| SOURCE | Written by | Trigger | Grouping |
|---|---|---|---|
| `cowork` | cowork-memory (`save-session`) | Cowork global instruction — fires after every completed task list | One entry per session |
| `email` | cowork-email-digest | Scheduled daily | All emails grouped into one entry per run |
| `slack` | cowork-slack-digest | Scheduled daily | All conversations grouped into one entry per run |
| `meeting` | cowork-meeting-digest | Scheduled daily | One entry per individual meeting |

---

## Item Prefix Conventions

| Prefix | Meaning |
|---|---|
| `+` | Completed, informational, decision confirmed |
| `-` | Open item, action needed, reply needed |
| `+ Decision:` | Explicit decision (used by meeting-digest) |
| `- Action:` | Assigned action item with owner (used by meeting-digest) |
| `- Open:` | Unresolved question flagged for follow-up (used by meeting-digest) |

---

## Config File

`~/Documents/ppt-index/config.md` — written by onboard skill, read by slack-digest.

```markdown
# Cowork Memory — Configuration

## Slack Channels

Monitored in addition to DMs:

| Channel Name | Channel ID |
|---|---|
| #channel-name | CXXXXXXXX |

## Setup Date
YYYY-MM-DD
```
