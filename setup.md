# cowork-demo Setup

This plugin requires no credentials or API keys. It uses connectors already available in Cowork.

## Required Connectors

| Connector | Used By | Required? |
|---|---|---|
| Slack MCP | `slack-chaos` | Recommended (falls back to mock if unavailable) |
| Scheduled Tasks MCP | `slack-chaos` | Recommended (falls back to mock if unavailable) |
| File System (Cowork) | `file-chaos`, `dashboard` | Required |

## First-Run Notes

- `file-chaos` will create a folder at `~/Documents/cowork-demo-files/` — safe to delete after the demo
- `dashboard` saves an HTML file to `~/Documents/cowork-demo-files/dashboard.html`
- `slack-chaos` schedules and immediately cancels a task — no messages are ever sent
- All demo artifacts are self-contained and can be cleaned up by deleting `~/Documents/cowork-demo-files/`

## No Setup Required

This plugin is designed to work out of the box. If Slack or scheduled task connectors are not connected, the relevant skills will fall back to narrated mock demos automatically.
