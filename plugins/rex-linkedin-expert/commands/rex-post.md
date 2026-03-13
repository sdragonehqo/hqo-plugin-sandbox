---
description: Draft a LinkedIn post in your REX Expert voice
allowed-tools: Read, WebSearch, WebFetch
argument-hint: [topic, data point, or article URL]
---

Draft a LinkedIn post for the user in their established REX Expert voice. Use weekly.

## Step 1: Load Knowledge

Read the skill and reference files:
- `${CLAUDE_PLUGIN_ROOT}/skills/rex-linkedin-expert/SKILL.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/rex-linkedin-expert/references/post-templates.md`
- `${CLAUDE_PLUGIN_ROOT}/skills/rex-linkedin-expert/references/hashtag-library.md`

## Step 2: Understand the Input

The user will provide one or more of: a topic they want to post about, a data point or stat, an article URL, a piece of industry news, or a personal observation.

If the user provides a URL, fetch the content using WebFetch to understand what they want to reference.

If the input is vague, ask one focused clarifying question (not more than one).

## Step 3: Determine Post Type

Based on the input, determine the best post type from the 5:3:2 framework:
- **Curated**: If they're sharing someone else's content or a data point from an external source
- **Original**: If they're sharing their own observation, pattern, or perspective
- **Personal/humanizing**: If they're reflecting on a personal experience or lesson

## Step 4: Draft the Post

Write the post following the appropriate template from `references/post-templates.md`.

Apply all style rules from the skill:
- Match the user's established voice (sentence length, tone, formality level, recurring patterns)
- No em dashes. Use parentheses, commas, or periods instead.
- Positive and curious, not critical.
- Conversational, not corporate.
- Concise and thought-provoking, not lengthy essays.
- Frame insights for the user's primary audience.
- Include CTA to https://www.hqo.com/ on roughly every other post.

Add 3-5 hashtags using the formula from `references/hashtag-library.md`.

## Step 5: Verify and Present

- Verify any stats or data points cited for accuracy. If a stat cannot be confirmed, flag it and suggest an alternative. Attribute all data clearly.
- Check for grammatical errors, typos, and awkward phrasing.
- Present the final post.
- Offer to adjust tone, length, audience framing, or hashtags.

If the user has been posting for several weeks, their prior career references should be fading per the Prior Career Fade Strategy. Adjust accordingly.
