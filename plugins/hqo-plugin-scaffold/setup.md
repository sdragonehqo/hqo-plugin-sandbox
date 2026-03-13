# Setup: hqo-plugin-scaffold

## Requirements

- No MCP connectors required
- No external credentials required for the `scaffold` and `build-agent` skills
- **`gh` CLI authenticated** — required for the `merge-plugin` skill, which uses `gh repo clone` and `git push` to copy plugins into the marketplace repo. Run `gh auth login` if not already authenticated.
- Developer-only plugin — intended for HqO plugin authors, not end users

## First-Run Steps

1. Install the plugin from the HqO marketplace.
2. If you intend to use the `merge-plugin` skill, ensure the `gh` CLI is installed and authenticated:
   ```
   gh auth status
   ```
   If not authenticated, run `gh auth login` and follow the prompts.
3. No other configuration is needed. The `scaffold` and `build-agent` skills work immediately after install.

## Verification

- Run `/hqo-plugin-scaffold:scaffold` and answer "new plugin" when prompted. The skill should ask for a plugin name and description, then generate the plugin directory structure including `plugin.json`, `CHANGELOG.md`, `setup.md`, and a `SKILL.md`.
- Run `/hqo-plugin-scaffold:build-agent` and provide a plugin name and agent details. The skill should create `agents/<agent-name>.md` and update `plugin.json` with an `agents` field.
- For `merge-plugin`, provide the path to a local plugin folder and a marketplace repo. The skill should clone the repo, copy the plugin, update `marketplace.json`, commit, and push to sandbox before pushing to main.

## Notes

- The `scaffold` skill generates a `setup.md` stub for new plugins — this is the file format read by `cowork-maintenance:plugin-setup` during automated onboarding.
- The `merge-plugin` skill enforces pre-flight checks before pushing: `plugin.json` must not contain a `skills` array of objects (only `"skills": "./skills/"` is valid), `CHANGELOG.md` must exist and start at `1.0.0`, and the plugin name must match the folder name.
- `merge-plugin` always pushes to a sandbox/staging repo first and waits for the user to confirm the install works before pushing to main. Never bypasses this confirmation if a sandbox repo is available.
- Version numbers live only in `marketplace.json` — never in `plugin.json`.
- After merging a new plugin, add the plugin name to `cowork-maintenance`'s `skills/plugin-setup/references/hqo-plugins.md` so it is recognized during automated maintenance runs.
