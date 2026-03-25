---
name: slack-chaos
description: >
  Use when the user says "scheduled tasks", "schedule something", "show me scheduling",
  "daily affirmation", or when invoked as Chapter 4 of the Cowork demo tour.
---

# Scheduled Tasks — Daily Affirmation

Demonstrates Cowork's ability to schedule recurring tasks. Instead of a gimmick, this one leaves the user with something real: a daily positive affirmation scheduled to run every morning.

---

## Step 1: Explain It

Print:
```
⏰ Chapter 4: Scheduled Tasks

Cowork doesn't just do things when you ask — it can do things on a schedule.
Every morning, every Friday, every hour — whatever you set up.

Let me show you by scheduling something you'll actually want to keep.
```

---

## Step 2: Personalize the Affirmation

Use AskUserQuestion:

**Question 1:** "What time do you usually start your day?"
- Options: "7:00 AM", "8:00 AM", "9:00 AM", "10:00 AM"

**Question 2:** "What kind of affirmation do you want?"
- Options: "Motivational (let's crush it)", "Calm & grounding (you're doing great)", "Funny (chaotic encouragement)"

Store the time as AFFIRMATION_HOUR and the style as AFFIRMATION_STYLE.

---

## Step 3: Schedule It

Use `CronCreate` to schedule a recurring task:

- **cron**: Set based on AFFIRMATION_HOUR — e.g., if 9:00 AM, use something like `"3 9 * * *"` (offset the minute slightly per CronCreate best practices — avoid :00 and :30)
- **recurring**: true
- **prompt**: Craft a prompt based on AFFIRMATION_STYLE:

  For **Motivational**: "Print a short, powerful daily affirmation for the user. 2-3 sentences max. Be bold, direct, and energizing. Start with a sun emoji. End with 'Now go.' No fluff."

  For **Calm & grounding**: "Print a short, calming daily affirmation for the user. 2-3 sentences max. Be warm, gentle, and reassuring. Start with a leaf emoji. Remind them they're enough. No pressure."

  For **Funny**: "Print a short, funny daily affirmation for the user. 2-3 sentences max. Be absurd, chaotic, and weirdly supportive. Start with a random emoji. Make them laugh before their first meeting."

After scheduling, print:
```
✅ Scheduled.

Every morning at [TIME], Cowork will drop a fresh affirmation right here in your terminal.

That's a recurring scheduled task — it fires automatically, no prompt needed.
It'll run for 7 days, then auto-expire. You can cancel it anytime.
```

---

## Step 4: Preview

Print:
```
Here's a preview of what tomorrow morning looks like:
```

Then generate one example affirmation in the chosen style right now, so the user sees what they're getting.

---

## Step 5: Land It

Print:
```
Scheduled tasks are how Cowork goes from assistant to autopilot.

Some real examples:
  — "Every Monday at 8am, summarize my unread emails and Slack messages"
  — "Every Friday at 5pm, draft a weekly status update from my git commits"
  — "Every morning, check my calendar and prep me for my first meeting"

You just tell it what and when. It handles the rest.
```

---

## Rules

- ALWAYS actually schedule the task using CronCreate — this is real, not a mock
- Offset the cron minute away from :00 and :30 per CronCreate guidelines
- The preview affirmation should match the chosen style and feel genuine
- Keep affirmation prompts tight — the scheduled output should be 2-3 sentences, not a paragraph
- Mention the 7-day auto-expiry so the user knows it won't run forever
- Tone is warm and practical — this chapter is the "useful takeaway" of the demo
