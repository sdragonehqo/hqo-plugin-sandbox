---
name: scaffold
description: >
  Use when the user wants to create a new Claude Code plugin or skill following
  HqO conventions. Handles "new plugin", "scaffold a skill", "create a plugin",
  "build a new skill", or any request to start a new plugin or skill from scratch.
---

# HqO Plugin Scaffold

Generate a new Claude Code plugin or add a new skill to an existing plugin,
following HqO marketplace conventions. This skill IS conversational — ask
questions and wait for answers.

---

## Step 1: Determine Scope

Ask the user:

> "Are you creating a **new plugin** (full directory with plugin.json) or adding a **new skill** to an existing plugin?"

Then ask:

> "What is the plugin name? Use lowercase with hyphens (e.g., `cowork-email-digest`)."
> "What does this plugin do? One sentence."

If adding a skill to an existing plugin, also ask:
> "What is the skill name? Use lowercase with hyphens (e.g., `email-digest`)."

---

## Step 2: Scaffold the Files

### If creating a NEW PLUGIN:

Create the following directory structure:

```
<plugin-name>/
├── .claude-plugin/
│   └── plugin.json
├── CHANGELOG.md
├── setup.md
└── skills/
    └── <skill-name>/
        └── SKILL.md
```

**plugin.json** — use the reference template at `./references/plugin-json.md`
**SKILL.md** — use the reference template at `./references/skill-md.md`
**CHANGELOG.md** — use the reference template at `./references/changelog.md`
**setup.md** — document any connections, MCP servers, credentials, or first-run
steps required for this plugin. This is read by `cowork-maintenance` during
onboarding. Individual plugin skills must NOT reference this file.

### If adding a NEW SKILL to an existing plugin:

Create:

```
<plugin-name>/skills/<skill-name>/
└── SKILL.md
```

Skills are auto-discovered from the `skills/` directory — no changes needed
to `plugin.json`. Do NOT touch the version field — version lives only in
`marketplace.json`.

---

## Step 3: Confirm and Remind

After writing all files, confirm to the user:

```
Scaffolded: <plugin-name>/<skill-name>

Files written:
  <plugin-name>/.claude-plugin/plugin.json
  <plugin-name>/CHANGELOG.md
  <plugin-name>/setup.md
  <plugin-name>/skills/<skill-name>/SKILL.md

Next steps:
  1. Fill in the SKILL.md with your skill logic
  2. Fill in setup.md with any connections or first-run requirements
  3. When ready to publish, use /merge-plugin
     — it will push to the sandbox first, confirm it loads, then push to main
  4. After merging, add the plugin name to cowork-maintenance's
     skills/plugin-setup/references/hqo-plugins.md
```

---

## Rules

- Plugin names: lowercase, hyphen-separated, descriptive (e.g., `cowork-email-digest`)
- Skill names: lowercase, hyphen-separated, match the plugin purpose
- Version ONLY lives in `marketplace.json` — never in `plugin.json`
- Skills appear in Claude Desktop as `plugin-name:skill-name`
- Every skill needs a `description` in frontmatter for Claude to know when to invoke it
- CHANGELOG.md starts at `1.0.0` for new plugins
- category is always `"productivity"` unless the user specifies otherwise
