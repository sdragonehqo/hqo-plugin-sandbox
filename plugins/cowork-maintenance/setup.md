# Setup: cowork-maintenance

## Requirements

- Read/write access to `~/Documents/` — creates and manages `~/Documents/plugin-maintenance/` as a maintenance log and scratch pad
- **`gh` CLI authenticated** — used to clone the HqO plugin marketplace repo (`gh repo clone HqOapp/hqo-plugin-marketplace /tmp/hqo-plugin-marketplace`) when reading plugin `setup.md` files during onboarding
- **`claude` CLI available** — used to run `claude plugins list` to discover installed plugins
- No MCP connectors required

## First-Run Steps

1. Grant Claude Code access to your Documents folder. In the Cowork toolbar, click the folder icon, select your Documents folder, and confirm.
2. Ensure the `gh` CLI is installed and authenticated:
   ```
   gh auth status
   ```
   If not authenticated, run `gh auth login` and follow the prompts.
3. Ensure the `claude` CLI is available in your shell PATH (it should be present if Claude Code is installed).
4. Run `/cowork-maintenance:plugin-setup`. The skill will:
   - Create `~/Documents/plugin-maintenance/maintenance-list.md` if it doesn't exist
   - Run `claude plugins list` to discover installed HqO plugins
   - Clone the marketplace repo to `/tmp/hqo-plugin-marketplace` if not already present
   - Read each plugin's `setup.md` and run setup steps, asking for confirmation where needed
   - Log results to `~/Documents/plugin-maintenance/maintenance-list.md`

## Verification

Run `/cowork-maintenance:plugin-setup`. If setup is working correctly:
- The skill presents a list of installed HqO plugins that need setup
- It works through each one sequentially, reading the plugin's `setup.md` from the marketplace clone
- After completion, `~/Documents/plugin-maintenance/maintenance-list.md` contains a row for each processed plugin with status `✓ complete` or `⚠ incomplete`

To verify access, confirm that `~/Documents/plugin-maintenance/maintenance-list.md` was created and is readable.

## Notes

- This plugin only processes plugins listed in its internal `references/hqo-plugins.md` known-plugins list — it will not touch built-in or third-party Claude Code plugins.
- Plugins already marked `✓ complete` in the maintenance log are skipped on subsequent runs unless the user explicitly requests a re-run.
- The maintenance log and scratch pad at `~/Documents/plugin-maintenance/` are local only and are never committed to any repo.
- If a plugin has no `setup.md` in the marketplace, the skill logs a note and continues — it does not block the queue.
- The marketplace repo is cloned to `/tmp/hqo-plugin-marketplace/`. If it already exists there, the skill runs `git pull` instead of a fresh clone.
- This plugin reads `setup.md` files as read-only — it never modifies files in the marketplace clone.
