---
name: plugin-setup
description: >
  Use when the user wants to set up, onboard, or check the status of their
  installed HqO plugins. Triggers on "run plugin setup", "onboard my plugins",
  "check my plugins", "set up my tools", or any request to initialize or
  verify HqO plugin connections. Runs automatically and only asks the user
  to confirm actions — never asks them to request each step.
---

# HqO Plugin Setup

Automatically detect which HqO plugins are installed, determine which ones
haven't been set up yet, and run onboarding for each — the user only confirms,
never drives.

This skill is NOT fully conversational — it figures out what needs to be done
and presents it. The user confirms or skips, that's it.

The goal is to ensure the user's installed plugins are connected and ready to
use — NOT to install plugins for them.

---

## Step 1: Access Check

Verify read/write access to `~/Documents/`.

If accessible, ensure the following exist (create if missing):
- `~/Documents/plugin-maintenance/` — scratch pad directory
- `~/Documents/plugin-maintenance/maintenance-list.md` — setup log

If `maintenance-list.md` is new, initialize it:

```markdown
# Plugin Maintenance Log

| Plugin | Setup Date | Status | Notes |
|--------|------------|--------|-------|
```

---

## Step 2: Discover Installed HqO Plugins

Run the following to list all non-local installed plugins:

```bash
claude plugins list
```

Filter the output: keep only entries that appear in
`references/hqo-plugins.md`. This prevents the skill from touching
pre-installed remote or built-in Claude Code plugins.

### If no HqO plugins are found — STOP HERE

Tell the user:

```
No HqO plugins are currently installed.

To get started:
  1. Open Claude Code settings
  2. Navigate to Customize → Browse Plugins → Organization Plugins
  3. Install the plugins you want to use
  4. Re-run plugin setup when you're ready

Nothing to do yet.
```

Do not proceed past this point.

---

## Step 3: Diff Against Maintenance Log

Read `~/Documents/plugin-maintenance/maintenance-list.md`.

Extract plugins already marked `✓ complete`.

**Needs setup** = (installed HqO plugins) − (already set up) − (`hqo-plugin-scaffold`, developer-only) − (`cowork-maintenance`, this plugin itself)

If everything is already set up, tell the user and offer to re-run for a
specific plugin if they want. Stop here unless they confirm.

---

## Step 4: Present the Plan

Show the user what's queued before doing anything:

```
Found X installed HqO plugin(s) that need setup:

  • <plugin-name-1>
  • <plugin-name-2>
  ...

I'll work through each one now. You'll only need to confirm connections
or credential prompts — I'll handle the rest.

Ready? (yes / skip <plugin-name>)
```

Wait for one confirmation. If the user skips a plugin, note it as
`⏭ skipped` in the log and remove it from the queue.

---

## Step 5: For Each Plugin (run in order)

### 5a. Read Plugin Docs

Clone or pull the marketplace:

```bash
gh repo clone HqOapp/hqo-plugin-marketplace /tmp/hqo-plugin-marketplace
# or if it exists:
cd /tmp/hqo-plugin-marketplace && git pull
```

Read in parallel:
- `plugins/<plugin-name>/CHANGELOG.md`
- `plugins/<plugin-name>/setup.md` (if it exists — not all plugins have one)

### 5b. Universal Tool Check

**This step runs for every plugin, regardless of whether it has a setup.md.**

Check if the plugin requires any MCP tools or external connections
(documented in its `setup.md` under Requirements, or inferrable from its
skills). If it does:

1. Attempt a lightweight call to verify each required tool is accessible
2. If a tool is NOT connected, tell the user:
   ```
   <plugin-name> requires <tool-name> to be connected.
   Go to: Settings → Connections → <tool-name> and connect it, then confirm.
   ```
3. Wait for confirmation before proceeding with that plugin
4. If a tool cannot be verified and the user wants to skip, log it as
   `⚠ incomplete — <tool-name> not connected`

If the plugin has no tool requirements, move straight to 5c.

### 5c. Run Onboarding (if setup.md exists)

If `setup.md` exists, follow its Setup Steps. Some plugins have minimal
setup (just a tool check), others have more (global instructions, schedules,
intake questionnaires). Follow what's documented — don't add steps that
aren't there.

If `setup.md` does not exist, the tool check in 5b is the complete setup
for that plugin. Log it as complete.

Use `~/Documents/plugin-maintenance/<plugin-name>/` as scratch pad for
any notes or debug output during setup.

### 5d. Log the Result

```markdown
| <plugin-name> | <YYYY-MM-DD> | ✓ complete | <notes> |
```

Or if incomplete:
```markdown
| <plugin-name> | <YYYY-MM-DD> | ⚠ incomplete | <what failed> |
```

---

## Step 6: Summary

```
Setup complete.

  ✓ <plugin-name-1> — ready
  ✓ <plugin-name-2> — ready
  ⚠ <plugin-name-3> — incomplete: <reason>
  ⏭ <plugin-name-4> — skipped

Log saved to ~/Documents/plugin-maintenance/maintenance-list.md
```

---

## Rules

- If no HqO plugins are installed, stop at Step 2 — do not proceed
- Skip `hqo-plugin-scaffold` always — it's for developers only
- Not every plugin has a setup.md — the universal tool check (5b) is always
  the minimum for any plugin that has tool requirements
- Never run setup for `cowork-maintenance` or `hqo-plugin-scaffold` — always excluded
- Never re-run setup for a plugin already marked `✓ complete` unless the
  user explicitly requests it
- Never attempt setup for plugins not in `references/hqo-plugins.md`
- The maintenance log and scratch pad are local only — never committed
- If a plugin has no tool requirements and no setup.md, log it as complete
  after confirming it loads — that's all it needs
