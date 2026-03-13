# Setup: hqo-plugin-scaffold

## Requirements

- `gh` CLI authenticated (`gh auth status`) — required for the `merge-plugin` skill to push to GitHub
- No MCP connectors required
- No dependencies on other plugins

## Setup Steps

1. Verify `gh` CLI is authenticated:
   ```bash
   gh auth status
   ```
   If not authenticated, run `gh auth login` and follow the prompts.

2. No other setup required. `scaffold` and `build-agent` skills work immediately with no configuration.

## Verification

Run `/hqo-plugin-scaffold:scaffold` — it should ask you whether you're creating a new plugin or adding a skill.

## Notes

- Developer-only plugin — excluded from `cowork-maintenance:plugin-setup` automatically
- `merge-plugin` skill requires `gh` CLI and push access to the target marketplace repo
