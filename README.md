# HqO Plugin Marketplace

Official Claude Code plugin marketplace for HqO.

## Add this marketplace

In Claude Desktop (Cowork mode):

1. Click **Plugins** (bottom-left)
2. Click the marketplace dropdown → **"Add marketplace from GitHub"**
3. Enter: `hqo-gtm/hqo-plugin-marketplace`

## Adding a new plugin

1. Create a plugin folder in `plugins/` (see structure below)
2. Add an entry to `.claude-plugin/marketplace.json`
3. Bump `version` in the marketplace entry and add a `CHANGELOG.md` entry
4. Push to `main`

## Plugin structure

```
plugins/
└── your-plugin-name/
    ├── .claude-plugin/
    │   └── plugin.json         # metadata only — no version field
    ├── skills/                 # agent-invoked skills (SKILL.md per subfolder)
    ├── agents/                 # subagent .md files
    ├── hooks/
    │   └── hooks.json
    ├── scripts/                # shell/python scripts for hooks/MCP
    ├── .mcp.json               # MCP server definitions
    └── CHANGELOG.md
```

## Versioning rules

- Version lives **only** in `marketplace.json` (plugin entry `version` field) — never in `plugin.json`
- Bump version in `marketplace.json` on every release
- Add an entry to the plugin's `CHANGELOG.md` for every version bump
- Follow semver: `MAJOR.MINOR.PATCH`
