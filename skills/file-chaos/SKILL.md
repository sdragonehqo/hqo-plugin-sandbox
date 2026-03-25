---
name: file-chaos
description: >
  Use when the user says "file chaos", "organize files demo", "show me file organization",
  "make a mess and clean it up", or when invoked as Chapter 1 of the Cowork demo tour.
---

# File Chaos — Live File Organization Demo

Creates a folder of realistically messy, badly-named files — the kind every computer has — then reads their contents and organizes them into a clean, logical structure. This is a live demonstration of Cowork's ability to understand files by what's *in* them, not just what they're called.

---

## Step 1: Ask Where to Create the Files

**MANDATORY — DO NOT SKIP THIS STEP. You MUST use AskUserQuestion before creating any files.**

Call the AskUserQuestion tool with this exact question before doing anything else:

- **question**: "Where should I create the demo files? Pick a spot you can see — open that folder and watch the files appear live."
- **header**: "File location"
- **options**:
  - label: "Desktop (Recommended)", description: "~/Desktop/cowork-demo-files/ — easiest to watch files appear in real time"
  - label: "Documents", description: "~/Documents/cowork-demo-files/"
  - label: "Downloads", description: "~/Downloads/cowork-demo-files/"

Wait for the user's answer. Map their choice to a path:
- Desktop → `~/Desktop/cowork-demo-files/`
- Documents → `~/Documents/cowork-demo-files/`
- Downloads → `~/Downloads/cowork-demo-files/`
- Other (custom) → use whatever path they provide

Store the chosen path as DEMO_DIR.

---

## Step 2: Create the Folder and Open It in Finder

First, create the empty folder and immediately open it in Finder so the user can watch:

```bash
mkdir -p [DEMO_DIR] && open [DEMO_DIR]
```

Then print:
```
📁 Folder created and opened in Finder. Watch it — files are about to start appearing.
```

**Pause 2 seconds** (use `sleep 2` in Bash) to give the user time to see the Finder window.

---

## Step 3: Create the Chaos

Now create the files. Use the Bash tool to populate the folder with 20 realistically-named messy files.

Then create the following files with realistic dummy content:

**Messy financial files:**
- `report_FINAL_v3_USE_THIS_ONE.txt` → content: "Q4 2025 Revenue Summary\nTotal Revenue: $4.2M\nGrowth: 18% YoY\nKey driver: Enterprise segment up 34%"
- `q3 numbers revised (2).txt` → content: "Q3 2025 Financial Results\nRevenue: $3.6M\nEBITDA margin: 22%\nHeadcount: 142"
- `ACTUAL_budget_final_FINAL.txt` → content: "FY2026 Budget Proposal\nTotal: $18.5M\nEngineering: $7.2M\nSales: $5.1M\nG&A: $6.2M"
- `untitled document 3.txt` → content: "Invoice #4421\nClient: Brookfield Properties\nAmount: $24,500\nDue: 2026-02-01"

**Messy meeting notes:**
- `notes.txt` → content: "Meeting with Sarah re: product roadmap\nAction items: finalize Q2 milestones, share deck with board\nNext sync: Thursday 2pm"
- `notes (copy).txt` → content: "1:1 with Marcus\nDiscussed pipeline concerns, he's worried about Q1 close rate\nFollow up: send competitive analysis"
- `mtg_notes_jan_idk.txt` → content: "All-hands recap\nCEO shared 2026 vision\nKey theme: customer retention over new logo growth\nQ&A: 14 questions submitted"
- `IMPORTANT read this.txt` → content: "Reminder: board deck due EOD Friday\nNeed sign-off from CFO before sending\nLegal review still pending on slide 12"

**Messy HR/people files:**
- `New hire stuff.txt` → content: "Onboarding checklist for Jamie Rodriguez\nStart date: March 3\nRole: Senior AE, West Coast\nEquipment: MacBook Pro, requested standing desk"
- `offer_letter_template_v2_updated.txt` → content: "Employment Offer Letter Template\nPosition: [ROLE]\nSalary: [AMOUNT]\nStart Date: [DATE]\nReports to: [MANAGER]"
- `performance stuff 2025.txt` → content: "Mid-year performance review cycle\nReview window: June 15 - July 1\nManagers: complete self-assessments first\nHR contact: people@hqo.co"

**Messy research files:**
- `research.txt` → content: "Competitor Analysis: Envoy vs HqO\nEnvoy: strong SMB presence, weaker enterprise\nHqO advantage: Leesman integration, CRE focus\nWin rate head-to-head: 67%"
- `stuff from the internet.txt` → content: "CRE market trends 2026\nHybrid work stabilizing at 3.2 days/week average\nFlight to quality accelerating\nLEED certified buildings commanding 18% rent premium"
- `draft_email_to_send_maybe.txt` → content: "Hi [Name],\nFollowing up on our conversation at CREtech last week.\nWould love to show you what HqO is doing with workplace analytics.\nAre you free for 20 minutes next week?"

**Misc clutter:**
- `asdfgh.txt` → content: "password reminder: check 1password\nalso dentist appt April 3rd\npick up dry cleaning"
- `Copy of Copy of presentation notes.txt` → content: "Slide 1: Opening — lead with the workplace data story\nSlide 5: Live demo of tenant app\nSlide 9: ROI case study (use Hines example)\nClosing: call to action — pilot offer"
- `TODO.txt` → content: "- Send Q4 deck to board\n- Review Marcus's pipeline\n- Approve Jamie's offer letter\n- Schedule competitor deep-dive\n- Renew Salesforce contract (deadline: March 31)"
- `random_backup_2024.txt` → content: "Old contract draft — Manulife\nInitial term: 3 years\nACV: $180,000\nStatus: SUPERSEDED — see Salesforce for current version"
- `new doc.txt` → content: "Ideas for Q2 campaign\n- Workplace intelligence webinar series\n- CRETech sponsorship activation\n- Leesman benchmark report as lead gen"
- `zz_archive_old_do_not_use.txt` → content: "ARCHIVED — 2024 pricing model\nDo not share externally\nReference only"

Use a single Bash heredoc block to create all 20 files efficiently.

After creating them, print:
```
✅ Mess created. 20 files dumped into one folder.
   Names like "ACTUAL_budget_final_FINAL.txt" and "asdfgh.txt" — classic.

Step 2 of 3: Reading every file to understand what's actually in them...
```

---

## Step 4: Read and Categorize

Use the Read tool or Bash `cat` to read the contents of each file. Based on the content (not the filename), assign each file to one of these categories:

| Category | Folder Name | What belongs here |
|---|---|---|
| Finance | `📊 Finance/` | Revenue reports, budgets, invoices, financial summaries |
| Meeting Notes | `📝 Meeting Notes/` | Meeting recaps, 1:1 notes, action items |
| People & HR | `👥 People & HR/` | Hiring, onboarding, performance reviews, offer letters |
| Research | `🔍 Research/` | Competitor analysis, market research, trend reports |
| Drafts & Comms | `✉️ Drafts & Comms/` | Email drafts, presentation notes, campaign ideas |
| Archive | `🗄️ Archive/` | Old versions, superseded docs, backups |
| Personal | `🙈 Personal/` | Personal reminders, non-work notes |

Print a categorization table before moving files:
```
Here's what I found inside each file:

  ACTUAL_budget_final_FINAL.txt  →  📊 Finance  (FY2026 budget proposal)
  report_FINAL_v3_USE_THIS_ONE.txt  →  📊 Finance  (Q4 revenue summary)
  [... continue for all 20 files ...]

Moving files now...
```

---

## Step 5: Organize the Files

Use Bash to create the subdirectories and move files into them based on the categorization above.

After moving, run `find [DEMO_DIR] -type f | sort` to show the final structure.

Print:
```
✅ Done. Here's what your folder looks like now:

  📊 Finance/           (4 files)
  📝 Meeting Notes/     (4 files)
  👥 People & HR/       (3 files)
  🔍 Research/          (3 files)
  ✉️ Drafts & Comms/    (3 files)
  🗄️ Archive/           (2 files)
  🙈 Personal/          (1 file)

20 files. 7 categories. Zero decisions made by you.

The key thing: it didn't sort by filename or extension.
It read what was *inside* each file and made a judgment call.
That's the difference.
```

---

## Step 6: Offer Cleanup

Print:
```
Want me to delete the demo folder when you're done? Just say "clean up file chaos"
and I'll remove [DEMO_DIR] entirely.
```

---

## Rules

- Always create files with `echo` or heredoc via Bash — never manually write 20 separate Write tool calls
- Categorization must be based on file *content*, not filename — say this explicitly in the output
- Print the categorization table BEFORE moving files so the user sees the reasoning
- Keep the tone light — this is a demo, have fun with the file names
- If the demo folder already exists, wipe it first with `rm -rf [DEMO_DIR]` before recreating
