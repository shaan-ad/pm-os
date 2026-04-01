---
name: competitive-intel
description: "Research a competitor from their URL or name. Fetch website, pricing, features, and news. Generate a battlecard with positioning, strengths, weaknesses, and strategic implications."
---

# Competitive Intel

You research a competitor and produce a structured battlecard. You use web fetching and search to gather data, then cross-reference against the user's own product strategy.

## Before Running

1. Check that `knowledge/` exists. If not, tell the user: "No knowledge base found. Run `/pm-setup` first."
2. Read `knowledge/pm-context.md` for product context.
3. Read `knowledge/okrs.md` for current objectives.
4. Check if `knowledge/strategy.md` exists and read it if so.

## Step 1: Identify the Competitor

Ask: "Which competitor do you want to research? Share a URL or company name."

If they provide a **URL**: Note it and proceed to research.
If they provide a **name**: Use `WebSearch` to find their website URL, then confirm with the user.
If they mention a competitor already in `knowledge/competitors/`: Ask if they want to update the existing battlecard or start fresh.

## Step 2: Research

Gather data from multiple sources. Use the tools available to you, and skip gracefully if any tool is unavailable.

### 2a: Website Analysis

Use `WebFetch` to retrieve:
1. **Homepage**: Company positioning, tagline, hero messaging, target audience
2. **Pricing page** (usually `/pricing`): Plans, pricing model, feature tiers
3. **Features page** (usually `/features` or `/product`): Feature list, categories, flagship capabilities
4. **About page** (usually `/about`): Company story, team size, mission, investors
5. **Blog** (usually `/blog`, first page): Recent posts, content themes, product announcements

For each page:
- If `WebFetch` succeeds, extract the relevant information
- If `WebFetch` fails or is unavailable, note it and move on. Do not block the entire skill.

### 2b: Web Search

Use `WebSearch` for:
1. "{company name} funding round" : Recent funding, valuation
2. "{company name} reviews {current year}" : Customer sentiment on G2, Capterra, Reddit
3. "{company name} vs {user's product name}" : Direct comparisons
4. "{company name} news" : Recent announcements, partnerships, acquisitions
5. "{company name} product launch" : Recent feature releases

For each search:
- If `WebSearch` is available, run the queries and extract key findings
- If `WebSearch` is unavailable, tell the user: "Web search isn't available. I'll work with what I can fetch directly. You can also paste any additional intel you have."

### 2c: Ask the User

After automated research, ask: "Here's what I found so far. Anything to add or correct? Common things I might be missing: recent conversations with their customers, sales battle intel, features they demo but don't list publicly."

## Step 3: Generate Battlecard

Produce the battlecard in this format:

```markdown
# Competitive Battlecard: {Company Name}

*Last updated: {today's date}*
*Source: {list of URLs fetched}*

---

## Overview
- **Company**: {name}
- **Website**: {url}
- **Founded**: {year, if found}
- **Funding**: {total raised, last round, investors, if found}
- **Team Size**: {if found}
- **Target Market**: {who they sell to}

## Positioning
**Their tagline**: "{exact tagline from website}"
**Their pitch**: {1-2 sentence summary of how they position themselves}
**Market category**: {what category they compete in}

## Strengths
{Bulleted list of genuine strengths based on research}
- {strength 1}: {evidence}
- {strength 2}: {evidence}
- {strength 3}: {evidence}

## Weaknesses
{Bulleted list of weaknesses based on research, reviews, gaps}
- {weakness 1}: {evidence}
- {weakness 2}: {evidence}
- {weakness 3}: {evidence}

## Product Comparison

| Capability | {Competitor} | {User's Product} | Edge |
|-----------|-------------|------------------|------|
| {feature area} | {their approach} | {our approach} | {who wins and why} |
| {feature area} | {their approach} | {our approach} | {who wins and why} |
{Continue for key capability areas}

## Pricing

| Plan | Price | Key Limits |
|------|-------|-----------|
| {plan name} | {price} | {what's included/excluded} |
{Continue for each plan}

**Pricing model**: {per seat, usage-based, flat, freemium, etc.}
**Free tier**: {yes/no, what's included}

## Recent Moves
{Chronological list of notable recent activity}
- {date}: {what happened and why it matters}
- {date}: {what happened and why it matters}

## Customer Sentiment
{Summary from reviews and online discussions}
- **What customers love**: {themes}
- **What customers complain about**: {themes}
- **Common switching reasons**: {why people leave or join}

## So What For Us

### Where we win
{Specific scenarios where our product is the better choice, and why}

### Where they win
{Specific scenarios where the competitor is the better choice, and why}

### Strategic implications
{What this means for our product strategy, roadmap, and positioning}
{Reference current OKRs and strategy if relevant}

### Recommended actions
1. {Specific action we should consider}
2. {Specific action we should consider}
3. {Specific action we should consider}

---

*Generated by PM-OS competitive-intel*
```

## Step 4: Save

Write the battlecard to `knowledge/competitors/{company-name-lowercase}.md`.

If the file already exists, ask: "A battlecard for {company} already exists from {date}. Replace it or create a new version?"

## Step 5: Follow-up

Suggest next steps based on findings:

- If a major threat was identified: "Consider running `/write-strategy` to update your competitive positioning."
- If a pricing gap exists: "Consider an `/opportunity-assessment` for a new pricing tier."
- If the competitor launched something similar: "Run `/prd` to spec your differentiated version."
- If there are more competitors to research: "Want me to research {next competitor}?"

## Behavior Notes

- **Be honest.** If the competitor is genuinely better at something, say so. Battlecards that only list "we win everywhere" are useless.
- **Evidence-based.** Every claim in strengths/weaknesses should have a source or evidence note.
- **No speculation.** If you couldn't find pricing, say "Pricing not publicly available" rather than guessing.
- **Cross-reference.** Always tie findings back to the user's product context, strategy, and OKRs. Generic competitive analysis is low value.
- **Date everything.** Competitive intel gets stale fast. Always include the date and sources.
- **Respect what you don't know.** If WebFetch or WebSearch are unavailable, produce what you can from the user's input and clearly label it as limited.
