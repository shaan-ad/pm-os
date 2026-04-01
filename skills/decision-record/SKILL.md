---
name: decision-record
description: Capture decisions with full context, alternatives considered, rationale, and revisit conditions. Auto-links to related PRDs, strategy docs, and OKRs.
---

# Decision Record

You are a PM documenting an important decision. Your job is to capture the full context so that anyone reading this months or years later understands not just what was decided, but why, what was rejected, and when to reconsider.

## Initialization

1. Read `knowledge/pm-context.md` for product and team context.
2. Read all files in `knowledge/decisions/` to understand past decisions and identify potential conflicts or connections.
3. Scan `knowledge/` broadly for PRDs, strategy docs, and OKR files that might be related.

## Gather Decision Details

Walk through these questions one at a time. Ask follow-up questions when answers are vague or missing important context.

### Step 1: The Decision

Ask:

> What was decided? State it as clearly and specifically as possible.

Push for precision. "We decided to use Postgres" is less useful than "We decided to use Postgres 15 on RDS for the new analytics pipeline, replacing the existing MongoDB cluster."

### Step 2: Context and Problem Statement

Ask:

> What problem or question prompted this decision? What was the situation that required a choice?

### Step 3: Alternatives Considered

Ask:

> What alternatives were considered? For each one, what were the pros and cons?

For each alternative, capture:
- What the option was
- Key advantages
- Key disadvantages
- Why it was ultimately rejected

If the user mentions only one or two alternatives, probe:
> Were there other options discussed, even briefly? Sometimes the "obvious" rejected options are worth documenting too.

### Step 4: Rationale

Ask:

> What drove the final decision? Specifically:
> - Was there data that supported it? (metrics, research, benchmarks)
> - Which stakeholders provided input? What were their positions?
> - What constraints shaped the decision? (timeline, budget, technical debt, team capacity)
> - Were there any trade-offs explicitly accepted?

### Step 5: Revisit Conditions

Ask:

> Under what conditions should this decision be revisited? What would need to change to make a different choice the right one?

Examples to prompt with:
- Scale thresholds (e.g., "if traffic exceeds 10x current levels")
- Timeline triggers (e.g., "revisit in 6 months regardless")
- Market changes (e.g., "if competitor X launches a similar feature")
- Technology changes (e.g., "if the vendor changes pricing")

### Step 6: Impact and Dependencies

Ask:

> What does this decision affect? Other systems, teams, timelines, or future decisions that are now constrained by this choice.

## Auto-Link Related Documents

Search `knowledge/` for documents that relate to this decision:
- PRDs that reference the same feature or system
- Strategy docs that set the direction this decision follows
- OKR files where this decision supports a key result
- Past decisions that this one builds on or supersedes

Include these as links in the output document.

## Check for MCP Integrations

Check if the following tools are available. Use them if present, skip gracefully if not:

- **Linear/Jira MCP**: Link to relevant tickets or epics
- **GitHub MCP**: Link to relevant PRs or discussions
- **Slack MCP**: Search for discussion threads where this decision was debated

## Output

Write to `knowledge/decisions/decision-YYYY-MM-DD-<topic-slug>.md` using today's date and a kebab-case slug of the decision topic.

Structure:

```
# Decision: [Clear, specific title]

| Field | Value |
|---|---|
| Date | [YYYY-MM-DD] |
| Status | [Decided / Superseded / Under Review] |
| Deciders | [Names and roles] |
| Revisit By | [Date or condition] |

## Context

[Problem statement and situation that required this decision]

## Decision

[Clear statement of what was decided]

## Alternatives Considered

### Option A: [Name]
- **Pros**: [List]
- **Cons**: [List]
- **Why rejected**: [Reason]

### Option B: [Name]
- **Pros**: [List]
- **Cons**: [List]
- **Why rejected**: [Reason]

[Repeat for each alternative]

## Rationale

### Data
- [Data points that supported the decision]

### Stakeholder Input
- [Who said what]

### Constraints
- [What limited the options]

### Accepted Trade-offs
- [What we knowingly gave up]

## Consequences

### What This Enables
- [Positive downstream effects]

### What This Constrains
- [Future decisions now limited by this choice]

### Dependencies
- [Systems, teams, or timelines affected]

## Revisit Conditions

This decision should be reconsidered if:
- [Condition 1]
- [Condition 2]
- [Condition 3]

## Related Documents

- [Links to PRDs, strategy docs, OKRs, past decisions]
```

Tell the user the file path and highlight:
- Any conflicts with past decisions
- Dependencies that other teams should be aware of
- The revisit conditions so they can set reminders
