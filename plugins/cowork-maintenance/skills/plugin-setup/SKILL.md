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

---

## Step 1: Access Check

Verify read/write access to `~/Documents/`.

If accessible, ensure the following exist (create if missing):
- `~/Documents/plugin-maintenance/` — scratch pad directory
- `~/Documents/plugin-maintenance/maintenance-list.md` — setup log

If `maintenance-list.md` is new, initialize it with this header:

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

Filter the output: keep only entries that appear in the
`references/hqo-plugins.md` known-plugins list. This prevents the skill from
touching pre-installed remote or built-in Claude Code plugins.

---

## Step 3: Diff Against Maintenance Log

Read `~/Documents/plugin-maintenance/maintenance-list.md`.

Extract the list of plugins already marked with status `✓ complete`.

**Needs setup** = (installed HqO plugins) − (already set up)

If everything is already set up, tell the user and offer to re-run setup for
a specific plugin if needed. Stop here unless they want to proceed.

---

## Step 4: Present the Plan

Before doing anything, show the user a summary:

```
Found X HqO plugin(s) that need setup:

  • <plugin-name-1>
  • <plugin-name-2>
  ...

I'll work through each one now. You'll only need to confirm connections
or credentials — I'll handle the rest.

Ready to start? (yes / skip a specific one)
```

Wait for confirmation. If the user skips a specific plugin, remove it from
the queue and note it as `⏭ skipped` in `maintenance-list.md`.

---

## Step 5: For Each Plugin (run in order)

For each plugin in the needs-setup queue:

### 5a. Read Plugin Docs

Navigate to the marketplace clone at `/tmp/hqo-plugin-marketplace/plugins/<plugin-name>/`.

If the marketplace hasn't been cloned:
```bash
gh repo clone HqOapp/hqo-plugin-marketplace /tmp/hqo-plugin-marketplace
```

Read in parallel:
- `CHANGELOG.md` — understand what the plugin does and its current version
- `setup.md` — step-by-step setup and connection requirements for this plugin
  (if `setup.md` doesn't exist, skip the setup.md step and note it in the log)

### 5b. Run Setup

Execute the setup steps from `setup.md`. For each step that requires user
action (e.g., confirming a connection, entering a credential, installing an
MCP server), present it clearly and wait for confirmation. Never skip a
required connection silently.

Use `~/Documents/plugin-maintenance/<plugin-name>/` as a scratch pad for
any notes, debug output, or config snapshots generated during setup.

### 5c. Log the Result

After setup completes (or fails), append a row to `maintenance-list.md`:

```markdown
| <plugin-name> | <YYYY-MM-DD> | ✓ complete | <any notes> |
```

If setup failed or was incomplete:
```markdown
| <plugin-name> | <YYYY-MM-DD> | ⚠ incomplete | <what failed> |
```

---

## Step 6: Summary

After all plugins have been processed, show:

```
Setup complete.

  ✓ <plugin-name-1> — set up successfully
  ✓ <plugin-name-2> — set up successfully
  ⚠ <plugin-name-3> — incomplete: <reason>
  ⏭ <plugin-name-4> — skipped

Log saved to ~/Documents/plugin-maintenance/maintenance-list.md
```

---

## Rules

- Never re-run setup for a plugin already marked `✓ complete` unless the user
  explicitly requests it
- Never attempt setup for plugins not in `references/hqo-plugins.md`
- The maintenance log and scratch pad are local only — never committed to any repo
- `setup.md` files live in the plugin's marketplace folder — they are read-only
  from the maintenance skill's perspective
- If a plugin has no `setup.md`, log a note and move on — don't block the queue
