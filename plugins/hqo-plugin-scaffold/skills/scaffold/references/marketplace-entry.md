# marketplace.json — Entry Reference

Add this object to the `plugins` array in `.claude-plugin/marketplace.json`.

```json
{
  "name": "<plugin-name>",
  "source": "./plugins/<plugin-name>",
  "version": "1.0.0",
  "description": "<One sentence visible in Claude Desktop marketplace UI>",
  "category": "productivity",
  "tags": ["<tag1>", "<tag2>", "<tag3>"],
  "author": {
    "name": "<Author Name>"
  }
}
```

## Rules

- `version` lives HERE and ONLY here for relative-path plugins — never in `plugin.json`
- Start new plugins at `1.0.0`
- Bump MAJOR for breaking changes (storage path, removed skills, renamed skills)
- Bump MINOR for new skills or significant new features
- Bump PATCH for bug fixes
- `tags` — 3–5 lowercase keywords; helps users find the plugin in the marketplace
- `category` options: `productivity`, `developer-tools`, `communication`
- `source` is always `./plugins/<plugin-name>` for local plugins in this repo

## Full marketplace.json structure (for reference)

```json
{
  "name": "hqo-plugin-marketplace",
  "owner": {
    "name": "HqO",
    "email": ""
  },
  "metadata": {
    "description": "HqO Plugin Marketplace — official Claude Cowork & Code plugins.",
    "version": "1.0.0"
  },
  "plugins": [
    { ... }
  ]
}
```
