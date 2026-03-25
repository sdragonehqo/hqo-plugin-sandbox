---
name: demo
description: >
  Use when the user says "run the demo", "start the demo", "demo everything",
  "show me everything", "full demo", "cowork demo", or any request to run all
  demo chapters end-to-end without stopping.
---

# Cowork Demo — Full Run

Run all 4 demo chapters back-to-back. No preamble, no pausing, no asking if the user is ready. Just execute.

Print one line to kick it off:
```
Let's go. 4 chapters, no stopping.
```

Then invoke the `tour` skill inline. It will chain through all chapters automatically.

---

## Rules

- Do NOT add any introduction or explanation before starting
- Do NOT pause between chapters or ask for confirmation
- Just run the tour skill and let it handle everything
- If the user says "demo" with no other context, this is the skill to use
