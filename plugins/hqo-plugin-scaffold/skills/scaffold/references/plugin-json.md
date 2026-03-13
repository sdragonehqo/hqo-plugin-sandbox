# plugin.json — Reference Template

Paste this into `<plugin-name>/.claude-plugin/plugin.json` and fill in the blanks.

```json
{
  "name": "<plugin-name>",
  "description": "<One sentence: what does this plugin do?>",
  "author": {
    "name": "<Author Name>"
  },
  "keywords": ["<tag1>", "<tag2>"],
  "skills": "./skills/"
}
```

## Rules

- `name` must match the plugin repo/directory name
- `version` is intentionally absent — it lives only in `marketplace.json`
- `"skills": "./skills/"` — points to the directory; Claude Code auto-discovers all SKILL.md files inside it
- Do NOT use a `skills` array of objects — it does not work
- `description` here is for the marketplace UI; the SKILL.md frontmatter `description` is what Claude reads to decide when to invoke the skill
