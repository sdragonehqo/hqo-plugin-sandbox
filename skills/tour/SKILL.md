---
name: tour
description: >
  Use when the user says "start the demo", "show me what Cowork can do",
  "run the tour", "demo cowork", or any request to get an interactive
  walkthrough of Cowork's capabilities.
---

# Cowork Interactive Demo Tour

Run all 4 demo chapters back-to-back with minimal narration. Don't explain what you're about to do — just do it. One-line intros, then execute.

---

## Step 1: Chapter 1 — File Chaos

Print one line, then immediately run the skill:
```
📁 Chapter 1: File Chaos — let's go.
```

Invoke the `file-chaos` skill inline (run it directly).

After completion, print one short line and move on immediately:
```
That's file intelligence. On to Chapter 2.
```

---

## Step 2: Chapter 2 — Dashboard + Connected Tools

Print one line, then immediately run the skill:
```
📊 Chapter 2: Dashboard — connecting to everything and building it live.
```

Invoke `/cowork-demo:dashboard` inline.

After completion, print one short line and move on:
```
Dashboard built, tools connected. On to Chapter 3.
```

---

## Step 3: Chapter 3 — Agents

Print one line, then immediately run the skill:
```
🤖 Chapter 3: Agents — spinning them up.
```

Invoke `/cowork-demo:agent-demo` inline.

After completion, print one short line and move on:
```
Parallel agents done. Final chapter.
```

---

## Step 4: Chapter 4 — Scheduled Tasks

Print one line, then immediately run the skill:
```
⏰ Chapter 4: Scheduled Tasks — the one you'll actually keep.
```

Invoke `/cowork-demo:slack-chaos` inline.

---

## Step 5: Close

After all chapters complete, print:

```
That's Cowork. Files, dashboards, agents, scheduled tasks — all in one conversation, on your actual computer.

Want to try it on something real?
```

---

## Rules

- Do NOT explain what you're about to do — just do it
- One-line chapter intros max, then execute the skill immediately
- Do NOT pause between chapters or ask if the user is ready — keep moving
- The tone is confident and fast — this is a live show, not a walkthrough
- If a skill fails, skip it and move on without dwelling on it
