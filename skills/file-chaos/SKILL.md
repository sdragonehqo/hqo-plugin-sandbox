---
name: file-chaos
description: >
  Use when the user says "file chaos", "organize files demo", "show me file organization",
  "make a mess and clean it up", or when invoked as Chapter 1 of the Cowork demo tour.
---

# File Chaos — Live File Organization Demo

Creates a folder of realistically messy, badly-named files — the kind every computer has — then reads their contents and organizes them into a clean, logical structure. This is a live demonstration of Cowork's ability to understand files by what's *in* them, not just what they're called.

---

## Step 1: Request Folder Access

**MANDATORY — DO NOT SKIP THIS STEP. You MUST request folder access before creating any files.**

Call the `request_cowork_directory` MCP tool **without a path** so the native folder picker opens and the user can choose where to put the demo files. Do NOT use AskUserQuestion — let the OS folder picker handle it.

Print this before calling the tool:
```
Pick a folder — I'll create a "cowork-demo-files" subfolder inside it.
Open the folder on your computer so you can watch the files appear live.
```

Once the user selects a folder, the tool returns a `VM path` (e.g. `/sessions/.../mnt/some-folder`). Create a `cowork-demo-files/` subfolder inside that mounted path and store the full path as DEMO_DIR.

---

## Step 2: Create First File

**CRITICAL: You MUST use the Write tool (NOT Bash) to create files. The Bash tool is sandboxed and will write to a temp directory — the user will NOT see those files on their actual computer. The Write tool writes to the real filesystem.**

Use the Write tool to create the first file inside DEMO_DIR:

Write file: `[DEMO_DIR]/report_FINAL_v3_USE_THIS_ONE.txt`
Content: `Q4 2025 Revenue Summary\nTotal Revenue: $4.2M\nGrowth: 18% YoY\nKey driver: Enterprise segment up 34%`

After the first file is created, print:
```
📁 Creating the mess now — watch your folder.
```

---

## Step 3: Create the Remaining Files

Use the **Write tool** for every file. You may call multiple Write tool calls in parallel to create files faster — but NEVER use Bash echo/heredoc to create files (those go to a sandbox, not the real filesystem).

Create these 19 remaining files in DEMO_DIR using the Write tool:

**Messy financial files:**
- `q3 numbers revised (2).txt` → "Q3 2025 Financial Results\nRevenue: $3.6M\nEBITDA margin: 22%\nHeadcount: 142"
- `ACTUAL_budget_final_FINAL.txt` → "FY2026 Budget Proposal\nTotal: $18.5M\nEngineering: $7.2M\nSales: $5.1M\nG&A: $6.2M"
- `untitled document 3.txt` → "Invoice #4421\nClient: Brookfield Properties\nAmount: $24,500\nDue: 2026-02-01"

**Messy meeting notes:**
- `notes.txt` → "Meeting with Sarah re: product roadmap\nAction items: finalize Q2 milestones, share deck with board\nNext sync: Thursday 2pm"
- `notes (copy).txt` → "1:1 with Marcus\nDiscussed pipeline concerns, he's worried about Q1 close rate\nFollow up: send competitive analysis"
- `mtg_notes_jan_idk.txt` → "All-hands recap\nCEO shared 2026 vision\nKey theme: customer retention over new logo growth\nQ&A: 14 questions submitted"
- `IMPORTANT read this.txt` → "Reminder: board deck due EOD Friday\nNeed sign-off from CFO before sending\nLegal review still pending on slide 12"

**Messy HR/people files:**
- `New hire stuff.txt` → "Onboarding checklist for Jamie Rodriguez\nStart date: March 3\nRole: Senior AE, West Coast\nEquipment: MacBook Pro, requested standing desk"
- `offer_letter_template_v2_updated.txt` → "Employment Offer Letter Template\nPosition: [ROLE]\nSalary: [AMOUNT]\nStart Date: [DATE]\nReports to: [MANAGER]"
- `performance stuff 2025.txt` → "Mid-year performance review cycle\nReview window: June 15 - July 1\nManagers: complete self-assessments first\nHR contact: people@hqo.co"

**Messy research files:**
- `research.txt` → "Competitor Analysis: Envoy vs HqO\nEnvoy: strong SMB presence, weaker enterprise\nHqO advantage: Leesman integration, CRE focus\nWin rate head-to-head: 67%"
- `stuff from the internet.txt` → "CRE market trends 2026\nHybrid work stabilizing at 3.2 days/week average\nFlight to quality accelerating\nLEED certified buildings commanding 18% rent premium"
- `draft_email_to_send_maybe.txt` → "Hi [Name],\nFollowing up on our conversation at CREtech last week.\nWould love to show you what HqO is doing with workplace analytics.\nAre you free for 20 minutes next week?"

**Misc clutter:**
- `asdfgh.txt` → "password reminder: check 1password\nalso dentist appt April 3rd\npick up dry cleaning"
- `Copy of Copy of presentation notes.txt` → "Slide 1: Opening — lead with the workplace data story\nSlide 5: Live demo of tenant app\nSlide 9: ROI case study (use Hines example)\nClosing: call to action — pilot offer"
- `TODO.txt` → "- Send Q4 deck to board\n- Review Marcus's pipeline\n- Approve Jamie's offer letter\n- Schedule competitor deep-dive\n- Renew Salesforce contract (deadline: March 31)"
- `random_backup_2024.txt` → "Old contract draft — Manulife\nInitial term: 3 years\nACV: $180,000\nStatus: SUPERSEDED — see Salesforce for current version"
- `new doc.txt` → "Ideas for Q2 campaign\n- Workplace intelligence webinar series\n- CRETech sponsorship activation\n- Leesman benchmark report as lead gen"
- `zz_archive_old_do_not_use.txt` → "ARCHIVED — 2024 pricing model\nDo not share externally\nReference only"

Launch as many parallel Write calls as possible to create files quickly so the user sees them appearing in Finder.

After creating all files, print:
```
✅ Mess created. 20 files dumped into one folder.
   Names like "ACTUAL_budget_final_FINAL.txt" and "asdfgh.txt" — classic.

Reading every file to understand what's actually in them...
```

---

## Step 4: Read and Categorize

Use the Read tool to read the contents of each file. Based on the content (not the filename), assign each file to one of these categories:

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

Create subdirectories and move files using the **Write tool** — read each file's content with the Read tool, then Write it to the new categorized path inside DEMO_DIR (e.g., `[DEMO_DIR]/📊 Finance/report_FINAL_v3_USE_THIS_ONE.txt`). Then delete the original flat files using Bash `rm`.

This ensures the organized files also appear on the real filesystem, not in a sandbox.

After organizing, use Bash `ls -R [DEMO_DIR]` to show the final structure.

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
and I'll remove the demo folder entirely.
```

---

## Rules

- **NEVER use Bash to create or move files** — Bash is sandboxed and writes to a temp directory. The user will NOT see those files. ALWAYS use the Write tool so files go to the real filesystem.
- The first Write call to the chosen directory will trigger the directory access permission prompt — this is intentional and required.
- Always use **absolute paths** (e.g., `/Users/sal/Desktop/cowork-demo-files/`) — never use `~` in Write tool paths.
- Use parallel Write tool calls where possible to create files fast so the user sees them popping into Finder.
- Categorization must be based on file *content*, not filename — say this explicitly in the output.
- Print the categorization table BEFORE moving files so the user sees the reasoning.
- Keep the tone light — this is a demo, have fun with the file names.
- If the demo folder already exists, remove its contents first before recreating.
