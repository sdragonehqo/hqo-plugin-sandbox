# Setup: rex-linkedin-expert

## Requirements

- No MCP tools required. The plugin uses only built-in Claude tools (Read, Write, WebSearch, WebFetch).
- No external credentials or API keys.
- No dependencies on other plugins.
- No environment variables or configuration files needed before first use.

## Setup Steps

### 1. Install the plugin

Add the plugin from the HqO marketplace. No configuration is needed before the first run.

### 2. Run the intake (`/rex-expert-setup`)

The onboarding is an interactive intake questionnaire driven by `commands/rex-expert-setup.md`. Invoke it by running `/rex-expert-setup` or by asking about "REX Expert positioning", "LinkedIn thought leadership for HqO", "my REX niche", or "help me build my LinkedIn playbook."

The skill walks through the questions in six conversational groups (it will not dump all questions at once):

**Group 1: About Me**
- Name
- Title and department at HqO
- Primary audience (e.g., property managers and operations leaders at large commercial portfolios, C-suite asset managers at enterprise landlords, mid-market CRE leasing teams)
- Prior background: 2-4 sentences on career before HqO, or specific CRE domain if always in CRE

**Group 2: Vantage Point**
- Unique vantage point: what do you see in your role that others might miss? (1-3 sentences)
- What genuinely energizes you about the CRE/tenant experience space? (1-2 sentences)

**Group 3: Writing Samples (most important)**
- 2-3 examples of writing that sounds like you at your best. Can be old LinkedIn posts, client emails, Slack messages, internal notes — anything unpolished. The goal is natural voice, not polish.

**Group 4: Voice Identity**
- Signature phrase: a phrase or expression you use often that feels like you
- Anti-patterns: what you hate reading on LinkedIn and would never want to sound like
- How people who know you professionally describe the way you communicate

**Group 5: Honest Point of View**
- 1-2 things you actually believe about the CRE or workplace experience industry that most people wouldn't say out loud

**Group 6: Content Capacity**
- Realistic posting frequency (how many times per week on LinkedIn)
- Whether you have time to engage (comment, reshare, react) on non-posting days

If writing samples are provided, the optional communication style questions (style orientation, content approach, post length preference, opinion comfort level) are skipped. If no samples are provided, those four questions are asked to approximate voice.

### 3. Receive the playbook

After all groups are complete, the skill produces a full LinkedIn playbook in the same session. There is no persistent storage: the playbook is delivered as a conversation response. Save it by copying it out of the chat. The playbook contains:

1. **REX Expert Niche Definition** — a niche title, domain, core thesis (1-2 sentences), primary audience, primary REX stage (Activate, Engage, Orchestrate, Understand, or Monetize), signature question, and a 3-5 sentence voice description. The niche is differentiated from Chase Garbarino's macro narrative (Experience Gap, structural demand reset) and from generic CRE voices.

2. **Prior Career Fade Strategy** (for employees from outside CRE) — a phased plan for how prior-career references evolve: strong references in weeks 1-4, lighter references in weeks 5-8, rare references from week 9 onward. Employees who have always been in CRE receive a vantage point analysis instead.

3. **5:3:2 Content Framework** — tailored to stated posting frequency. Maps the ratio of curated posts (5), original posts (3), and personal/humanizing posts (2) per 10 posts into a concrete weekly rhythm.

4. **5 Sample Posts** — one post per type in an arc from early positioning (leaning on prior background) to fully established CRE voice. Each post follows the appropriate template (curated, original, or personal/humanizing) and includes 3-5 hashtags drawn from the hashtag library.

5. **Hashtag Strategy** — core hashtags to rotate from, using the formula: 1 broad reach tag (#CommercialRealEstate, #CRE, #FutureOfWork, #WorkplaceExperience, or #PropTech) + 1-2 industry niche tags + 1-2 topic-specific tags. #HqO and #REX are used sparingly (roughly 1 in 4 posts).

6. **Cross-Linking Guidance** — specific angles for engaging with HqO leadership content: Chase Garbarino (CEO, macro industry transformation), Greg Gomer (CXO, experience design), Shelly Just (COO, operational execution), and Tom Herbert (EMEA, global perspective). No connection to those accounts is required.

### 4. Review and adjust

After the playbook is presented, the skill asks four calibration questions:
- Does the voice feel right, or is it too safe / too bold / not how you actually talk?
- Does the niche feel differentiated enough?
- Is the audience framing correct (speaking to landlords and property managers, not occupiers)?
- Is prior career coming through at the right level?

Provide feedback and the skill will revise. Any stats in sample posts are verified using web search before the final version is presented; unverified stats are flagged explicitly.

## Verification

After completing `/rex-expert-setup`, the following should exist in your conversation:

- A named niche title and domain (e.g., "The Operator's Edge: Making tenant experience programs actually stick")
- A 1-2 sentence core thesis
- A named primary audience and primary REX stage
- A 3-5 sentence voice description confirmed by you
- A concrete weekly posting rhythm (e.g., "2 posts/week: 1 curated Monday, 1 original Thursday")
- 5 sample posts ready to publish or adapt
- A hashtag rotation set
- Cross-linking angles for each HqO leadership voice

The plugin is set up when you can run `/rex-post [topic]` and receive a post draft that matches your confirmed voice and niche without needing to re-explain your background.

## Notes

- The playbook is not saved to disk automatically. Copy it out of the conversation after the session and keep it somewhere accessible (a doc, a note, a Notion page) so you can paste relevant sections when using `/rex-post` in future sessions.
- There are three commands in this plugin. Use them at different cadences:
  - `/rex-expert-setup` — once at onboarding (or to rebuild your playbook if your role or audience changes)
  - `/rex-post [topic or URL]` — weekly, to draft individual LinkedIn posts in your established voice
  - `/rex-review` — monthly, to review what has resonated, evolve your positioning, and get 5 fresh post topic ideas with current CRE industry context
- No scheduled tasks or background automation. This plugin is fully on-demand.
- No em dashes will appear in any post draft. The skill enforces parentheses, commas, or periods instead.
- All statistics in post drafts are verified against original sources before delivery. If a stat cannot be confirmed, the skill will flag it and suggest an alternative with clear attribution (e.g., "per Gallup," "according to Leesman data," "per ULI research").
