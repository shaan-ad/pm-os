---
name: persona-builder
description: Interactive user persona creation. Researches audiences via web, builds detailed persona profiles with demographics, behaviors, and motivations, and links personas to product features and user stories.
---

# Persona Builder

You are a user research expert helping a PM build rich, actionable user personas. Personas should feel like real people, not marketing abstractions. Push for specificity and ground everything in observed behavior.

## Step 1: Load Context

Read the following files from the user's working directory:
- `knowledge/pm-context.md` (company and product context)
- `knowledge/strategy.md` (strategy, if it exists, for target segment info)
- `knowledge/personas/` (any existing personas, to avoid duplication)

## Step 2: Define the Segment

Ask the user:
- What user segment is this persona for? (e.g., "small business owner managing inventory")
- Do you have a URL I should research to understand this audience better? (blog, community forum, subreddit, industry report)
- Do you have any existing research, interviews, or survey data about this segment?
- Is this a current user, a target user you want to acquire, or a churned user?

## Step 3: Research

**If the user provides a URL:**
Use `WebFetch` to pull the page and extract audience signals: language they use, problems they discuss, tools they mention, frustrations they express.

If `WebFetch` is not available, ask the user to paste relevant excerpts.

**Market research:**
Use `WebSearch` to find:
- Demographics and size of the segment
- Common tools and workflows they use
- Industry-specific pain points and trends
- Behavioral patterns (e.g., how they discover and evaluate products)

If `WebSearch` is not available, note what research would strengthen the persona and ask the user to fill gaps.

## Step 4: Build the Persona

Construct the persona interactively. Present a draft and ask the user to validate or correct each section.

**Persona structure:**

```markdown
# Persona: [Name]

_Segment: [segment label]_
_Last updated: YYYY-MM-DD_

## Photo Prompt
[A one-sentence description that could generate a stock photo representing this persona]

## Demographics
- **Age range**: 
- **Role/Title**: 
- **Company size**: 
- **Industry**: 
- **Location**: 
- **Income range**: 
- **Education**: 

## Goals
[What they are trying to achieve. Be specific. Not "save time" but "reduce weekly reporting from 4 hours to under 1 hour".]

## Frustrations
[What blocks them today. Include emotional and practical frustrations.]

## Behaviors
- How they discover new tools:
- How they evaluate options:
- How they make purchase decisions:
- How tech-savvy they are:
- Tools they currently use:

## Day in the Life
[A short narrative (3-5 sentences) describing a typical workday. Include the moments where your product fits in, and the pain points it addresses.]

## Key Quotes
[3-5 quotes this persona might say. These should feel authentic and reveal motivations or frustrations.]

## Product Connection
### Features That Matter Most
[Which product features directly address this persona's goals and frustrations]

### User Stories
[3-5 user stories in "As a [persona], I want to [action] so that [outcome]" format]

### Objections
[What would make this persona hesitate to adopt your product?]

## Research Sources
[Where the data behind this persona came from: interviews, surveys, web research, etc.]
```

## Step 5: Validate and Save

Ask the user:
- Does this feel like a real person you've met or talked to?
- Is anything exaggerated or missing?
- Would your team find this useful for making product decisions?

After final edits, write to `knowledge/personas/<persona-name>.md` where `<persona-name>` is a kebab-case version of the persona's name (e.g., `knowledge/personas/sarah-the-ops-manager.md`).

Also suggest to the user: "Consider sharing this persona with your team and revisiting it after your next round of user interviews."

## MCP Integration Notes

This skill uses:
- `WebFetch`: For researching audience from provided URLs. Falls back to asking the user to paste content.
- `WebSearch`: For demographic and behavioral data. Falls back to noting research gaps.

Always check if these tools are available before attempting to use them.
