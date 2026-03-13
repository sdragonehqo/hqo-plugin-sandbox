---
name: build-agent
description: >
  Use when the user wants to add a subagent to a plugin. Handles "add an agent",
  "create an agent", "build a subagent", or any request to add an agent to an
  existing plugin. Agents appear in the /agents interface and can be invoked
  automatically by Claude or manually by the user.
---

# Build a Plugin Agent

Add a subagent to an existing HqO plugin. Agents differ from skills — a skill
is a prompt that guides Claude's behavior; an agent is a specialist Claude can
delegate tasks to, appearing in the `/agents` interface.

This skill IS conversational — ask questions and wait for answers.

---

## Step 1: Gather Details

Ask the user:

> "Which plugin are you adding this agent to?"
> "What is the agent's name? Use lowercase with hyphens (e.g., `data-analyst`)."
> "What does this agent specialize in? One sentence — this is what Claude reads
> to decide when to invoke it automatically."
> "Describe the agent's role and behavior in detail. What should it do, how should
> it respond, what expertise should it bring?"

---

## Step 2: Create the Agent File

Create `<plugin-root>/agents/<agent-name>.md`:

```markdown
---
name: <agent-name>
description: <one sentence — when should Claude invoke this agent?>
---

<Detailed system prompt: the agent's role, expertise, behavior, and how it should
approach tasks. Be specific — this is the full persona and instruction set the
agent operates from.>
```

**File location**: `agents/` directory at the plugin root (alongside `skills/`).
No subdirectory needed — one flat `.md` file per agent.

---

## Step 3: Update plugin.json (if needed)

Check `<plugin-root>/.claude-plugin/plugin.json`. If it doesn't already have
an `agents` field, add it:

```json
{
  "name": "<plugin-name>",
  "description": "...",
  "skills": "./skills/",
  "agents": "./agents/"
}
```

If the `agents/` directory is new, Claude Code will auto-discover it — but
being explicit in plugin.json is preferred.

---

## Step 4: Confirm

```
Added agent: <plugin-name>/<agent-name>

Files written:
  <plugin-root>/agents/<agent-name>.md
  <plugin-root>/.claude-plugin/plugin.json  ← agents field added (if new)

The agent will appear in /agents and can be invoked automatically by Claude
based on task context, or manually by the user.

When ready to publish, use /merge-plugin.
```

---

## Rules

- Agent names: lowercase, hyphen-separated
- `description` in frontmatter is what Claude reads to decide when to auto-invoke — make it specific and trigger-oriented
- The body is a full system prompt — be detailed and specific about behavior
- One file per agent, flat in `agents/` — no subdirectories
- Agents and skills can coexist in the same plugin
