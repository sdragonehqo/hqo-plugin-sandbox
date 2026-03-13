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

## Step 1: Request Directory Access

Before any file I/O, explicitly request access to the maintenance directory:

```
Use request_cowork_directory with path: ~/Documents
```

Once access is granted, ensure the following exist (create if missing):
- `~/Documents/plugin-maintenance/maintenance-list.md` — setup log

If `maintenance-list.md` is new, initialize it:

```markdown
# Plugin Maintenance Log

| Plugin | Setup Date | Status | Notes |
|--------|------------|--------|-------|
```

---

## Step 2: Discover Installed HqO Plugins

Do NOT use `claude plugins list` — it does not reflect the user's actual
installed plugins inside the Cowork VM.

Instead, locate and read the installed plugins manifest:

```
Glob: /sessions/*/mnt/.local-plugins/installed_plugins.json
```

Read the file at the matched path. The keys in the JSON object are the
installed plugins, in the format `<plugin-name>@<marketplace>` (e.g.
`cowork-memory@hqo-plugin-marketplace`). Each entry also includes a
`"version"` field — save this for Step 5.

**Save the resolved session path** (e.g.
`/sessions/tender-affectionate-cerf/mnt`) — you'll need it to read plugin
docs in Step 5.

Filter: keep only entries whose plugin name appears in
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

### 5a. Read Plugin Docs from Local Cache

Do NOT clone or pull from GitHub. All installed plugin files are already
cached locally. Use the session path and version resolved in Step 2 to
construct the cache path:

```
<session-path>/.local-plugins/cache/hqo-plugin-marketplace/<plugin-name>/<version>/
```

Read in parallel:
- `CHANGELOG.md` — understand what the plugin does and its current version
- `setup.md` — step-by-step setup requirements (may not exist for all plugins)

### 5b. Universal Tool Check

**This step runs for every plugin, regardless of whether it has a setup.md.**

Check if the plugin requires any MCP tools or external connections
(documented in its `setup.md` under Requirements). If it does:

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
- Never run `claude plugins list` — use `installed_plugins.json` instead
- Never clone the marketplace repo — read plugin files from local cache
- Never run setup for `cowork-maintenance` or `hqo-plugin-scaffold` — always excluded
- Not every plugin has a setup.md — the universal tool check (5b) is the
  minimum for any plugin with tool requirements
- Never re-run setup for a plugin already marked `✓ complete` unless the
  user explicitly requests it
- Never attempt setup for plugins not in `references/hqo-plugins.md`
- The maintenance log and scratch pad are local only — never committed
- If a plugin has no tool requirements and no setup.md, log it as complete
  after confirming it loads — that's all it needs
