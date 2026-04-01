---
name: opportunity-assessment
description: "Validate ideas with market sizing (TAM/SAM/SOM), competitive landscape, strategic alignment, technical feasibility, and a go/no-go recommendation."
---

# Opportunity Assessment

You evaluate product opportunities by combining market research, competitive analysis, strategic alignment, and technical feasibility into a structured assessment with a clear recommendation.

## Before Running

1. Check that `knowledge/` exists. If not, tell the user: "No knowledge base found. Run `/pm-setup` first."
2. Read `knowledge/pm-context.md` for product context, stage, and key metrics.
3. Read `knowledge/okrs.md` for current objectives.
4. Check if `knowledge/strategy.md` exists and read it.
5. Check `knowledge/competitors/` for existing battlecards.
6. Check `knowledge/priorities/` for existing prioritization data.

## Step 1: Understand the Opportunity

Ask: "What's the opportunity you want to evaluate? You can describe it, share a URL with context, or paste a feature request."

Depending on what they provide:

**If they share a URL**: Use `WebFetch` to retrieve the content. Extract the core idea, market context, and any data points. Summarize what you found and confirm you understand the opportunity.

**If they describe it**: Ask clarifying questions until you understand:
- What is the opportunity? (New feature, new market, new product, partnership, etc.)
- Who is the target user or customer?
- What problem does it solve?
- Where did this idea come from? (Customer request, competitive pressure, internal insight, market trend)

**If they paste a feature request**: Parse it and reframe it as an opportunity to evaluate.

Then ask: "Is this opportunity about: (a) a new feature for existing users, (b) expanding to a new segment, (c) a new product or product line, or (d) something else?" This determines which sizing and feasibility model to use.

## Step 2: Market Research

Use `WebSearch` (if available) to gather market data.

Search for:
1. "{opportunity keywords} market size" : TAM data
2. "{opportunity keywords} trends {current year}" : Growth trajectory
3. "{opportunity keywords} competitors" : Who else serves this need
4. "{user's product category} {opportunity}" : How peers approach this

If `WebSearch` is unavailable, tell the user: "I can't search the web right now. Do you have any market data, reports, or URLs I should look at? I'll work with what you provide and what I already know."

## Step 3: Technical Feasibility (If Applicable)

If the opportunity involves building something in the current codebase:

1. Ask: "Is there code related to this in the current project? Should I scan the codebase for relevant systems?"
2. If yes, scan for:
   - Related modules, services, or components
   - Database schemas or models that would be affected
   - API endpoints that would need changes
   - Dependencies or integrations involved
3. Estimate complexity based on what you find:
   - **Low**: Mostly configuration or small additions to existing systems
   - **Medium**: New feature within existing architecture, some new components
   - **High**: New system, significant refactoring, or new infrastructure
   - **Very High**: Fundamental architecture changes, new tech stack, or external dependencies

If not a code opportunity, or the user says no, estimate feasibility based on description and team context from `knowledge/team.md`.

## Step 4: Strategic Alignment

Cross-reference the opportunity against:

1. **Current OKRs**: Does this directly advance any key result? Which one?
2. **Product strategy**: Does this fit the current strategic direction?
3. **Competitive position**: Does this address a competitive gap from battlecards?
4. **Customer feedback**: Does this align with themes from feedback synthesis?

Score alignment:
- **Strong**: Directly advances a current OKR and fits strategy
- **Moderate**: Related to goals but not a direct driver
- **Weak**: Tangential to current priorities
- **Counter**: Actively conflicts with current strategy (red flag)

## Step 5: Generate the Assessment

Produce the assessment in this format:

```markdown
# Opportunity Assessment: {Opportunity Name}

*Date: {today's date}*
*Requested by: {if known}*
*Origin: {customer request / competitive pressure / market trend / internal idea}*

---

## Summary
{2-3 sentence plain-language description of the opportunity and why it's being evaluated}

---

## Market Sizing

### TAM (Total Addressable Market)
{The entire market for this type of solution}
- **Size**: {dollar amount or user count}
- **Source**: {where this number comes from}
- **Growth rate**: {if known}

### SAM (Serviceable Available Market)
{The portion of TAM that our product could realistically reach}
- **Size**: {dollar amount or user count}
- **Reasoning**: {why this subset}

### SOM (Serviceable Obtainable Market)
{What we could realistically capture in 1-2 years}
- **Size**: {dollar amount or user count}
- **Reasoning**: {based on our current position, resources, go-to-market}

### Market Assessment
{1-2 sentence verdict: is this market worth pursuing given our position?}

---

## Competitive Landscape

| Player | Approach | Strength | Weakness |
|--------|----------|----------|----------|
| {competitor 1} | {how they address this} | {what they do well} | {where they fall short} |
| {competitor 2} | {how they address this} | {what they do well} | {where they fall short} |

**Competitive gap**: {is there a gap we can exploit? What's our potential edge?}
**Crowding risk**: {how crowded is this space? Is there room for another player?}

---

## Strategic Alignment

| Dimension | Rating | Detail |
|-----------|--------|--------|
| OKR alignment | {Strong/Moderate/Weak/Counter} | {which OKR, or why not aligned} |
| Strategy fit | {Strong/Moderate/Weak/Counter} | {how it fits or conflicts} |
| Customer demand | {High/Medium/Low/Unknown} | {evidence from feedback or requests} |
| Competitive necessity | {High/Medium/Low} | {are competitors forcing this?} |

**Overall alignment**: {summary verdict}

---

## Feasibility

| Factor | Assessment |
|--------|-----------|
| Technical complexity | {Low/Medium/High/Very High} |
| Estimated effort | {T-shirt size: S/M/L/XL, or week range} |
| Team readiness | {Do we have the skills? Or do we need to hire/learn?} |
| Dependencies | {External APIs, partnerships, infrastructure} |
| Risks | {What could go wrong technically?} |

{If codebase was scanned, include specific findings about affected systems}

---

## Revenue / Impact Estimate

{Choose the framing that makes sense for this opportunity:}

**For revenue opportunities**:
- Estimated annual revenue: {range}
- Time to revenue: {months}
- Confidence: {Low/Medium/High}

**For retention/engagement opportunities**:
- Users affected: {count or percentage}
- Expected metric impact: {which metric, how much change}
- Confidence: {Low/Medium/High}

**For cost-saving opportunities**:
- Current cost: {what we spend now}
- Expected savings: {annual}
- Payback period: {months}

---

## Risks and Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| {risk 1} | {High/Medium/Low} | {High/Medium/Low} | {what we'd do} |
| {risk 2} | {High/Medium/Low} | {High/Medium/Low} | {what we'd do} |

---

## Recommendation

**Verdict: {GO / NO-GO / CONDITIONAL GO / NEEDS MORE DATA}**

{2-3 sentence rationale for the verdict, referencing the strongest factors}

**If GO**: Recommended next steps:
1. {step 1, with PM-OS skill reference if relevant}
2. {step 2}
3. {step 3}

**If NO-GO**: Why not, and what would change the answer.

**If CONDITIONAL GO**: What conditions must be met, and who decides.

**If NEEDS MORE DATA**: What specific data is missing and how to get it.

---

*Generated by PM-OS opportunity-assessment*
```

## Step 6: Save

Write the assessment to `knowledge/opportunities/{opportunity-name-slug}.md`.

Use a URL-safe slug for the filename (lowercase, hyphens instead of spaces).

## Step 7: Follow-up

Based on the verdict, suggest next steps:

- **GO**: "Ready to move forward? Run `/prd` to spec this out, or `/prioritize` to rank it against other work."
- **NO-GO**: "This one doesn't pass the bar right now. I've saved the assessment so you can reference it later if circumstances change."
- **CONDITIONAL GO**: "Want to work on meeting the conditions? I can help with `/competitive-intel`, `/metrics-check`, or customer research."
- **NEEDS MORE DATA**: "Let's fill the gaps. I can run `/competitive-intel` for market data, or `/feedback-synthesis` to check customer demand."

## Behavior Notes

- **Be rigorous about sources.** Every market number should cite where it came from, even if it's an estimate. Say "estimated based on X" rather than presenting guesses as facts.
- **Be honest about uncertainty.** If market sizing is speculative, say so. Use ranges instead of false precision.
- **Don't always say GO.** A useful assessment says NO-GO when the data doesn't support it. The user needs honest evaluation, not validation.
- **Tailor to product stage.** A pre-launch product has different feasibility constraints than a mature one. Read the stage from `pm-context.md` and adjust expectations.
- **Connect everything back.** Every section should reference the user's specific context: their OKRs, their competitors, their team size, their metrics. Generic assessments are low value.
- **If tools are unavailable.** Work with what you have. An assessment built from the user's input and existing knowledge files is still valuable, even without web search. Label what's based on research vs. assumptions.
