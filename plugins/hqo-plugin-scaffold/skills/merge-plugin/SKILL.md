---
name: merge-plugin
description: >
  Use when the user wants to publish or merge a plugin into a plugin marketplace
  repo. Handles "merge this plugin", "publish to marketplace", "push to the
  marketplace", or any request to add a local plugin to a marketplace repo.
---

# Plugin Merge

Merge a locally developed plugin into a plugin marketplace repo.
Always push to a sandbox or staging repo first and confirm it loads before
pushing to the main repo.

This skill IS conversational — ask questions and wait for answers.

---

## Step 1: Identify the Plugin and Repos

Ask the user:

> "What is the path to the plugin folder you want to merge?"
> "What is the marketplace repo? (e.g., `org/plugin-marketplace`)"
> "Do you have a sandbox or staging repo to test in first?"

---

## Step 2: Pre-flight Checks

Before copying anything, verify:

1. **plugin.json has NO `skills` array** — this causes install failures in the
   marketplace. The file should only contain `name`, `description`, `author`,
   `keywords`, and optionally `"skills": "./skills/"`. Remove any `skills` array
   of objects if present.

2. **CHANGELOG.md exists** — create one if missing. Version must be `1.0.0`
   for a new plugin (never `0.x`).

3. **Plugin name matches folder name** — `name` in plugin.json must match
   the directory name.

Fix any issues before proceeding.

---

## Step 3: Clone the Marketplace

```bash
gh repo clone <org/marketplace-repo> /tmp/plugin-marketplace
```

If it already exists at `/tmp/plugin-marketplace`, run `git pull` instead.

If the user has a sandbox repo, add it as a remote:

```bash
cd /tmp/plugin-marketplace
git remote add sandbox https://github.com/<user>/<sandbox-repo>.git
```

---

## Step 4: Copy the Plugin

```bash
cp -r <plugin-path> /tmp/plugin-marketplace/plugins/<plugin-name>
```

---

## Step 5: Update marketplace.json

Add an entry to `/tmp/plugin-marketplace/.claude-plugin/marketplace.json`:

```json
{
  "name": "<plugin-name>",
  "source": "./plugins/<plugin-name>",
  "version": "1.0.0",
  "description": "<one sentence from plugin.json>",
  "category": "productivity",
  "tags": ["<keywords from plugin.json>"],
  "author": {
    "name": "<author from plugin.json>"
  }
}
```

---

## Step 6: Commit

```bash
cd /tmp/plugin-marketplace
git add .claude-plugin/marketplace.json plugins/<plugin-name>/
git commit -m "Add <plugin-name> plugin"
```

---

## Step 7: Push to Sandbox First (if available)

If the user has a sandbox repo:

```bash
git push sandbox main
```

Then ask the user:

> "Pushed to sandbox. Please try installing the plugin and confirm it loads
> correctly before I push to main."

**Wait for confirmation before proceeding.**

If no sandbox, skip to Step 8 and let the user know they're pushing direct to main.

---

## Step 8: Push to Main

Once confirmed (or if no sandbox):

```bash
git push origin main
```

Confirm to the user:

```
Merged: <plugin-name> v1.0.0

Published to:
  sandbox → <sandbox-repo> ✓ (confirmed working)
  main    → <marketplace-repo> ✓
```

---

## Rules

- **NEVER push to `origin main` before the user confirms the sandbox install works** (if a sandbox exists)
- plugin.json must NOT have a `skills` array of objects — this breaks marketplace installs
- Version lives only in `marketplace.json`, never in `plugin.json`
- CHANGELOG always starts at `1.0.0` for new plugins, never `0.x`
- If the sandbox install fails, diagnose and fix before pushing to main
