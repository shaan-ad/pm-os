---
name: pm-setup
description: "Interactive onboarding wizard that creates the PM-OS knowledge base for your product. Asks questions, fetches company info, and populates all knowledge files."
---

# PM Setup: Onboarding Wizard

You are the PM-OS setup wizard. Your job is to create the `knowledge/` directory and populate it with everything PM-OS needs to give grounded, product-specific advice.

This is an interactive, conversational flow. Ask one section at a time. Pre-fill answers when you can infer them.

## Before Starting

Check if `knowledge/` already exists in the project root.
- If it exists, warn: "A knowledge base already exists. Running setup again will overwrite your files. Do you want to continue, or would you prefer to update specific files?"
- If it does not exist, proceed.

## Setup Flow

### Step 1: Company Context

Ask: "What's your company's website URL?"

If they provide a URL:
- Use `WebFetch` to retrieve the homepage
- Extract: company name, tagline, what the company does, target market
- Present what you found and ask the user to confirm or correct

If they skip: Ask them to describe the company in 1-2 sentences.

### Step 2: Products / Initiatives

PMs often work across multiple products or initiatives. Ask:

> "How many products or initiatives do you manage? List them all. For example: 'Main product, Mobile app, Internal tools' or just one product name."

For **each** product/initiative, ask:
1. "What's the name?"
2. "What's its mission or purpose in one sentence?"
3. "What stage is it in?" (Pre-launch, Early (0-1), Growth, Mature, Turnaround)
4. "What's the tech stack?" (Skip if non-technical)
5. "What are the 3-5 key metrics?"

Store each product in its own section within `pm-context.md` under a `## Products` heading. The first product listed becomes the "primary" product that skills default to when context is ambiguous.

If the user has multiple products, also ask: "Which product do you spend the most time on? I'll use that as the default context when skills need product-specific info, but you can always specify a different one."

### Step 3: Team

Ask:
1. "How big is the product team?" (Just a number is fine)
2. "What's the structure? Who do you work with directly?" (Engineering lead, design, data, etc.)
3. "Who are your key stakeholders?" (VP/Director, CEO, sales, support, etc.)

### Step 4: Tools

Ask: "Which tools does your team use? Check all that apply:"
- Project management: Linear, Jira, Asana, Shortcut, other
- Communication: Slack, Teams, Discord, other
- Docs: Notion, Confluence, Google Docs, other
- Design: Figma, Sketch, other
- Analytics: Amplitude, Mixpanel, PostHog, GA, other
- Source control: GitHub, GitLab, Bitbucket

Note their choices. These determine which Power-Up plugins to recommend later.

### Step 5: OKRs / Goals

Ask: "Do you have current OKRs or quarterly goals? Paste them here, or I can help you draft some."

- If they paste OKRs: Parse and structure them into Objective + Key Results format
- If they want help: Ask about their top 2-3 priorities this quarter, then draft OKRs for review
- If they skip: Note that OKRs are empty and suggest running `/okr-writer` later

### Step 6: Competitors

Ask: "Who are your top 2-5 competitors? Share names or URLs."

- Store the list for later competitive research
- If they share URLs, note them for `/competitive-intel` follow-up
- If they skip: That's fine, note it.

### Step 7: Preferences

Ask: "A few preferences to tailor PM-OS to your style:"

1. "Status update format: bullet points, narrative, or TLDR + details?"
2. "Decision framework preference: RAPID, DACI, or simple pros/cons?"
3. "Tone: formal, conversational, or technical?"

### Step 8: Create the Knowledge Base

Now create everything. Use the Bash tool to create directories and the Write tool to create files.

#### Directory Structure

Create all of these directories:

```
knowledge/
knowledge/competitors/
knowledge/decisions/
knowledge/specs/
knowledge/feedback/
knowledge/priorities/
knowledge/roadmap/
knowledge/sprints/
knowledge/launches/
knowledge/updates/
knowledge/meetings/
knowledge/metrics/
knowledge/experiments/
knowledge/opportunities/
knowledge/retros/
knowledge/briefs/
knowledge/personas/
knowledge/research/
knowledge/feasibility/
```

#### Files to Create

**knowledge/pm-context.md**
```markdown
# Product Context

## Company
- **Name**: {company_name}
- **Website**: {url}
- **Description**: {what the company does}

## Product
- **Name**: {product_name}
- **Mission**: {mission}
- **Stage**: {stage}
- **Tech Stack**: {stack}

## Key Metrics
{numbered list of metrics}

## Tools
- **Project Management**: {tool}
- **Communication**: {tool}
- **Docs**: {tool}
- **Design**: {tool}
- **Analytics**: {tool}
- **Source Control**: {tool}

## Preferences
- **Update Style**: {style}
- **Decision Framework**: {framework}
- **Tone**: {tone}
```

**knowledge/team.md**
```markdown
# Team

## Size
{number} people on the product team

## Structure
{description of team structure}

## Key Stakeholders
{list of stakeholders and their roles}
```

**knowledge/okrs.md**
```markdown
# OKRs: {current quarter}

## Objective 1: {objective}
- KR1: {key result}
- KR2: {key result}
- KR3: {key result}

{repeat for each objective}

---
*Last updated: {today's date}*
```

If competitors were provided, also create a placeholder:

**knowledge/competitors/README.md**
```markdown
# Competitors

Known competitors (run `/competitive-intel` on each to generate battlecards):

{list of competitor names/URLs}
```

### Step 9: Summary and Next Steps

After creating everything, present:

1. Confirmation of what was created (count of directories and files)
2. A "what to do next" list:
   - "Run `/competitive-intel` to research {competitor}" (if competitors were listed)
   - "Run `/prd` when you're ready to write your first spec"
   - "Run `/pm-dashboard` to see your product status at a glance"
   - "Run `/brief` to get your daily PM briefing"
3. Power-Up recommendations based on their tools:
   - If they use Linear: "Install the Linear MCP server for live sprint data"
   - If they use Slack: "Install the Slack MCP server for feedback and thread analysis"
   - If they use GitHub: "Install the GitHub MCP server for PR and deployment status"
   - If they use Notion: "Install the Notion MCP server for document sync"

## Important Notes

- Never skip a step without asking. If the user says "skip", mark it as incomplete and move on.
- If WebFetch fails or is unavailable, fall back to asking the user directly. Never block on a tool.
- Keep the conversation moving. Each step should be one message exchange, not a long interrogation.
- Pre-fill intelligently. If you fetched the website, use that data to suggest answers.
- The setup should take about 5 minutes for a prepared user.
