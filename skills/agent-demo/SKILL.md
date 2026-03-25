---
name: agent-demo
description: >
  Use when the user says "agent demo", "show me agents", "parallel agents demo",
  "sub-agent example", or when invoked as Chapter 3 of the Cowork demo tour.
---

# Agent Demo — Parallel Sub-Agents + Build Your Own

Two parts: first, launch 3 personalized agents in parallel that do real work about the user. Then, walk the user through building their own custom sub-agent from scratch.

---

## Part 1: Parallel Agents

### Step 1: Get the User's Name

Use AskUserQuestion:
- **Question:** "Quick — what's your full name? (The agents need to know who they're working for.)"
- Provide a free-text input (just 2 dummy options like "Type your name" / "Skip" — the user will use the Other field to type their name)

If the user already provided their name in a previous step (e.g., from Gmail profile in the dashboard chapter), use that and skip this question.

Store as USER_NAME.

### Step 2: Launch

Print:
```
🤖 Spinning up 3 agents — all working on you, all at the same time.
```

Note the current time using Bash: `date +%H:%M:%S` — save as START_TIME.

### Step 3: Launch Three Agents in Parallel

Use the Agent tool to launch all three agents **simultaneously in a single message** (parallel, not sequential).

#### Agent 1: "The Stalker" 🔍
**Task:** Use WebSearch and WebFetch to research [USER_NAME]. Find their LinkedIn, company, role, any public talks, blog posts, or press mentions. Compile a short dossier (5-8 bullet points) of what you found. Keep it professional and factual. If you can't find much, note what you tried and make educated guesses based on any partial info. Return the dossier formatted with bullet points.

#### Agent 2: "The Songwriter" 🎵
**Task:** Write a short, fun, personalized song (2 verses + chorus) about [USER_NAME] completing the Cowork demo. Make it catchy — think jingle, not ballad. Reference the demo chapters they just went through (file chaos, dashboards, agents, Slack). Keep it lighthearted and complimentary. Include a title for the song. Return the full lyrics.

#### Agent 3: "The Designer" 🎓
**Task:** Generate a complete, self-contained HTML file for an HqO-branded "Cowork Graduation Certificate" PDF-ready page for [USER_NAME]. Requirements:
- Title: "Certificate of Cowork Mastery"
- Recipient: [USER_NAME]
- Date: 2026-03-25
- HqO branding: use colors #6366f1 (indigo) and #0f1117 (dark), clean modern design
- List the 4 completed chapters as achievements: File Intelligence, Live Dashboards, Parallel Agents, Tool Automation
- Include a fake but fun certificate number (e.g., "CW-2026-00042")
- Add a footer: "Issued by HqO Cowork · Powered by Claude"
- The HTML should look great when printed or saved as PDF via the browser
- Use Google Fonts (Inter + a serif font for the certificate title like Playfair Display)
- Save this file to the same demo directory as other demo files (~/Desktop/cowork-demo-files/ or ~/Documents/cowork-demo-files/)

Return the full HTML content so the main agent can write it to disk.

---

### Step 4: Collect and Present Results

After all three agents return, note the end time: `date +%H:%M:%S`

Print:
```
✅ All 3 agents finished.

⏱️  Started: [START_TIME]  |  Finished: [END_TIME]
    They ran simultaneously — not one after another.
```

Then present each result with clear visual separation:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Agent 1 — The Stalker
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Agent 1 dossier output]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎵 Agent 2 — The Songwriter
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Agent 2 song lyrics]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎓 Agent 3 — The Designer
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Certificate saved! Opening now...
```

Write Agent 3's HTML output to `[DEMO_DIR]/cowork-certificate.html` and open it:
```bash
open [DEMO_DIR]/cowork-certificate.html
```

Print:
```
Three agents. Three completely different skills. All ran at once.
One researched you on the internet, one wrote you a song, one designed you a certificate.

That's parallel agents.
```

---

## Part 2: Build Your Own Agent

### Step 5: Transition

Print:
```
Now you've seen what agents can do. Let's build one that's yours.

I'm going to ask you a few questions, then spin up a custom agent right here.
```

### Step 6: Agent Builder Questionnaire

Use AskUserQuestion for each of these (one at a time, sequentially):

**Question 1:** "What should your agent specialize in?"
- Options: "Research & analysis", "Writing & content", "Data & reporting", "Code & technical"

**Question 2:** "What's the first task you'd give it?"
- Options: "Summarize something for me", "Draft an email or doc", "Analyze a dataset or trend", "Find and compare options"
- (User can type a custom task via Other)

**Question 3:** "What personality should it have?"
- Options: "Strictly professional", "Friendly and casual", "Brutally honest", "Overly enthusiastic"

**Question 4:** "Give it a name."
- Options: "Let Claude pick", "Agent [USER_NAME]"
- (User will likely type a custom name via Other)

Store all answers.

### Step 7: Build and Launch the Agent

Print:
```
Building your agent now...

  Name: [AGENT_NAME]
  Specialty: [SPECIALTY]
  Personality: [PERSONALITY]
  First task: [TASK]

Launching...
```

Use the Agent tool to launch a single agent with a prompt that incorporates ALL of the user's choices:
- Tell the agent its name, specialty, and personality
- Give it the user's task
- Instruct it to respond in character with the chosen personality
- Tell it to be thorough but concise

After the agent returns, print its output with a styled header:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🤖 [AGENT_NAME] — reporting in
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Agent output]
```

### Step 8: Land It

Print:
```
That's your agent. You defined it in 30 seconds and it just did real work.

In Cowork, you can build agents like this for anything — research assistants,
writing partners, code reviewers, data analysts — and run them whenever you need.

They're not apps. They're not plugins. They're just... workers you describe and deploy.
```

---

## Rules

- Part 1 agents MUST be launched in parallel (single message, multiple Agent tool calls) — not sequentially
- Always record start and end time for Part 1 to show the parallel execution benefit
- The certificate HTML must be self-contained and look genuinely polished — it's a keepsake
- The song should be fun and specific to the user — generic is boring
- The web research agent should use real WebSearch/WebFetch — the "wow" is finding real info
- Part 2 questions should be asked one at a time for a conversational feel
- The custom agent must genuinely incorporate the user's personality and task choices — don't ignore their input
- If the Agent tool is unavailable, simulate by doing the work inline but note it would normally be parallel
