# SKILL.md — Reference Template

Paste this into `plugins/<plugin-name>/skills/<skill-name>/SKILL.md`.

```markdown
---
name: <skill-name>
description: >
  Use when the user asks to "<trigger phrase 1>", "<trigger phrase 2>",
  "<trigger phrase 3>", or any request involving <broad description of use case>.
---

# <Human-Readable Skill Title>

<One paragraph: what this skill does and what the user gets out of it.>
This skill is [fully automated — do NOT ask the user for any input] OR [conversational — ask questions and wait for answers].

---

## Step 0: Verify Storage

<If the skill reads/writes cowork memory:>
Memory lives at `~/Documents/ppt-index/`. Read `~/Documents/ppt-index/INDEX`
to confirm storage exists. If not found, tell the user to run
`/personal-progress-tracker:onboard` first.

---

## Step 1: <First Step Title>

<Instructions for the first step.>

---

## Step 2: <Next Step Title>

<Instructions.>

---

## Step N: Confirm

Output a brief summary of what was done.

---

## Rules

- <Rule 1 — things the skill must always or never do>
- <Rule 2>
- Do not read or modify existing INDEX entries — append only
```

## Conventions

| Pattern | Convention |
|---|---|
| Automated vs conversational | State clearly at the top |
| Storage path | Always `~/Documents/ppt-index/` |
| INDEX append | One line: `DATE\|TIME\|USER\|SOURCE\|TAGS\|SUMMARY` |
| Daily file append | `~/Documents/ppt-index/s/YYYYMMDD.md` |
| Item prefixes | `+` completed/informational, `-` open/action needed |
| Date generation | `date +%Y%m%d` for DATE, `date +%H%M` for TIME |
| User generation | `whoami` once per run |
| Source field | `cowork`, `email`, `slack`, or `meeting` |
