---
name: dashboard
description: >
  Use when the user says "build the dashboard", "show me the dashboard demo",
  "create a dashboard", "connected tools", or when invoked as Chapter 2 of the Cowork demo tour.
---

# Dashboard — Custom Live Dashboard + Connected Tools

Builds a personalized, interactive HTML dashboard by actively connecting to every available tool, pulling real data, and assembling it into a custom view. This chapter does double duty: it shows the dashboard-building capability AND demonstrates the breadth of Cowork's tool connections live.

---

## Step 1: Personalize

Use AskUserQuestion to ask the user two quick questions:

**Question 1:** "What's your role?"
- Options: "Sales / Revenue", "Engineering / Product", "Operations / HR", "Executive / Leadership"

**Question 2:** "What do you want the dashboard to focus on?"
- Options: "My day today (emails, meetings, priorities)", "Team overview (activity, comms, workload)", "Exec snapshot (metrics, highlights, action items)"

Store the answers — they determine the dashboard's title, sections, and tone.

---

## Step 2: Connect to Everything

Print:
```
Connecting to your tools now — let's see what's live.
```

Run ALL of these tool calls in parallel. For each one, track whether it succeeded or failed — this determines what shows up in the dashboard's "Connected Tools" section.

### Gmail (3 calls)
1. `mcp__claude_ai_Gmail__gmail_get_profile` — get user's name and email
2. `mcp__claude_ai_Gmail__gmail_search_messages` with `q: "is:unread"`, `maxResults: 5` — unread emails with senders/subjects
3. `mcp__claude_ai_Gmail__gmail_search_messages` with `q: "is:starred newer_than:7d"`, `maxResults: 5` — starred items

### Gmail → Calendar extraction (1 call)
4. `mcp__claude_ai_Gmail__gmail_search_messages` with `q: "invite.ics OR subject:(accepted OR invitation OR invite) newer_than:1d"`, `maxResults: 10` — find calendar invites from today. For each result, read the message to extract meeting title, time, and attendees.

### Slack (2 calls)
5. `mcp__claude_ai_Slack__slack_search_users` with query matching the user's name from Gmail — find the user's Slack profile
6. `mcp__claude_ai_Slack__slack_search_channels` with query `"general"` — get workspace channels to show Slack is live

### HubSpot (1 call)
7. `mcp__claude_ai_HubSpot__get_user_details` — get the user's HubSpot identity. If this succeeds, also run:
8. `mcp__claude_ai_HubSpot__search_crm_objects` with `objectType: "deals"`, `limit: 5`, sorted by `closedate ASCENDING` — pull upcoming deals or pipeline data

### Atlassian (1 call)
9. `mcp__claude_ai_Atlassian_2__getAccessibleAtlassianResources` — check if Jira/Confluence are connected

After all calls complete, print a live connection report:
```
✅ Gmail — connected ([X] unread, [Y] starred)
✅ Slack — connected ([workspace name])
✅ HubSpot — connected ([user name])
✅ Atlassian — connected ([site name])
⚠️  Google Calendar — no direct connection (pulling meetings from email invites)
[or ❌ for any that failed]

Pulling data into your dashboard...
```

---

## Step 3: Generate the Dashboard

Write a single self-contained HTML file. Save it to the same DEMO_DIR as file-chaos if that ran first (check if `~/Desktop/cowork-demo-files/` exists, then `~/Documents/cowork-demo-files/`, otherwise create `~/Desktop/cowork-demo-files/`). Name it `dashboard.html`.

The dashboard should be visually impressive and **personalized with real data from every tool that connected**.

**Header:**
- Title: personalized based on role + focus choice (e.g., "Sal's Morning Briefing", "[Name]'s Team Dashboard", "[Name]'s Exec Snapshot")
- Subtitle: "Built live by Cowork — pulling from [N] connected tools"
- Show the user's email from Gmail profile
- Dark, modern design (dark navy/charcoal background, white text, accent colors)

**Section 1 — Connected Tools (the showpiece):**
A prominent grid showing every tool that was tested, with real status:
- Each tool gets a card with: icon/name, connection status (green dot = Live, red = Failed, yellow = Partial), and a one-line data summary
- Gmail: "Live — [X] unread emails"
- Slack: "Live — connected to [workspace]" or "Not connected"
- HubSpot: "Live — [X] deals in pipeline" or "Not connected"
- Atlassian: "Live — [site name]" or "Not connected"
- Google Calendar: "Partial — pulling from email invites (connect for full access)"
- Supabase, Linear: "Available — not yet connected" (show as gray/available)

**Section 2 — Today's Inbox Snapshot (Gmail data):**
- Unread email count (real number)
- List of up to 5 unread emails showing sender + subject line (real data)
- Starred/priority item count

**Section 3 — Today's Schedule (from email invites):**
- Parse meeting invites and calendar-related emails found in Step 2
- Show them as a timeline/schedule card with times and meeting names
- If meetings found, show them as a visual timeline
- Always show a note: "Via Gmail invite detection — connect Google Calendar for complete schedule"

**Section 4 — CRM Snapshot (HubSpot data, if connected):**
- If HubSpot connected: show deal count, upcoming close dates, pipeline summary
- If not connected: show a "Connect HubSpot to see your pipeline here" placeholder card

**Section 5 — Slack Activity (if connected):**
- If Slack connected: show workspace name, channels found, user profile
- If not connected: placeholder card

**Section 6 — Personalized Metrics (animated counters):**
Based on the user's role/focus choice, show 4 relevant metrics using real data where available:

For "My day today":
- Unread emails (real), meetings today (from invites), starred items (real), connected tools count

For "Team overview":
- Unread emails (real), Slack channels (real if connected), HubSpot deals (real if connected), connected tools count

For "Exec snapshot":
- Unread emails (real), meetings today (from invites), deals closing soon (HubSpot if connected), connected tools count

**Section 7 — Quick Actions (role-appropriate, using real data):**
Show 3-4 suggested actions referencing actual data pulled:
- "Draft a reply to [most recent unread sender]" (real sender name)
- "Prep for [next meeting name from calendar]" (real meeting if found)
- "Review [deal name] closing [date]" (real HubSpot deal if connected)
- "Summarize #general from today" (if Slack connected)

**Footer:**
- "Built by Cowork · Powered by Claude · [N] tools connected · Custom-generated [date]"

**Design requirements:**
- Single HTML file, no external dependencies except Google Fonts (Inter)
- Dark mode: background `#0f1117`, cards `#1a1d27`, accent `#6366f1` (indigo)
- Smooth CSS animations on load (fade in, counter animation, staggered card reveals)
- Fully responsive
- Animated counters: count up from 0 over 1.5 seconds on page load (vanilla JS)
- Connected tools status dots: green (#22c55e) for live, red (#ef4444) for failed, yellow (#eab308) for partial, gray (#6b7280) for available

---

## Step 4: Open and Present

After writing the file, open it in the user's default browser:
```bash
open [path to dashboard.html]
```

Print:
```
Dashboard built and opened in your browser.

That pulled live data from [N] tools — Gmail, Slack, HubSpot, Atlassian —
read your actual emails, found your meetings, checked your pipeline,
and assembled it into a custom dashboard in seconds.

No templates. No configuration. Just "build me a dashboard" and it connects to everything.
```

---

## Rules

- The HTML file must be completely self-contained — no external JS libraries, no separate CSS files
- Google Fonts CDN link is allowed (Inter font)
- The design must look polished and modern — this is a showpiece
- ACTUALLY call every tool listed in Step 2 — the whole point is demonstrating real connections, not faking them
- Track success/failure for each tool and reflect it honestly in the dashboard — showing a failed connection is BETTER than faking a successful one (it proves we really tried)
- If a tool call fails, gracefully degrade — show "Not connected" in the dashboard, don't crash
- The Connected Tools section should be the most visually prominent section — it's the hero of this chapter
- The animated counters MUST work using vanilla JavaScript only
- Do NOT skip the personalization questions — the custom feel is the point
- For Google Calendar: there is no direct MCP tool. Always pull calendar data from Gmail invite emails. Note this in the dashboard as "Partial — via email invites"
