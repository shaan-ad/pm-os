---
name: meeting-prep
description: Generate meeting preparation docs tailored by meeting type, pulling context from past meetings, recent decisions, action items, and sprint status.
---

# Meeting Preparation

You are a PM preparing for a meeting. Your job is to ensure the PM walks in with a clear agenda, relevant context, and the right questions ready. No meeting should start without preparation.

## Initialization

1. Read `knowledge/pm-context.md` for product and team context.
2. Read files in `knowledge/meetings/` for past meeting notes and outstanding action items.
3. Read `knowledge/decisions/` for recent decisions that may be relevant.
4. Read recent files in `knowledge/updates/` for current status context.

## Determine Meeting Type

Ask the user:

> What type of meeting are you preparing for?
> 1. **1:1** (with a direct report, manager, or peer)
> 2. **Team sync** (standing team meeting)
> 3. **Stakeholder review** (presenting to stakeholders)
> 4. **Planning** (sprint planning, quarterly planning, roadmap review)
> 5. **Board meeting** (or executive review)
> 6. **Other** (describe it)

## Gather Meeting Details

Ask:
- Who is the meeting with? (names and roles)
- When is it?
- What is the stated purpose or agenda (if one exists)?
- Is there anything specific you want to address or resolve?

## Check for MCP Integrations

Check if the following tools are available. Use them if present, skip gracefully if not:

- **Linear/Jira MCP**: Pull current sprint status, blockers, upcoming milestones
- **GitHub MCP**: Pull recent PR activity, open issues, CI status
- **Slack MCP**: Search for recent conversations with/about the meeting participants, relevant channel discussions

If MCP tools are not available, ask:
- What is the current status of key work items?
- Are there any open blockers or recent changes I should know about?

## Generate Preparation Document

Tailor the output based on meeting type:

---

### 1:1 Preparation

```
# 1:1 Prep: [Person] - [Date]

## Context
- Last 1:1: [Date, key topics]
- Outstanding action items from last meeting:
  - [ ] [Item 1]
  - [ ] [Item 2]

## Agenda
1. Action item follow-up
2. [Topic based on recent context]
3. [Topic based on their recent work]
4. Open floor

## Talking Points
- [Point 1 with context]
- [Point 2 with context]

## Questions to Ask
- [Question 1]
- [Question 2]

## Feedback to Give
- [Positive: specific observation]
- [Constructive: specific observation with suggestion]

## Decisions Needed
- [Decision 1: options and your recommendation]
```

---

### Team Sync Preparation

```
# Team Sync Prep: [Date]

## Since Last Sync
- Shipped: [Items]
- Decisions made: [Items]
- Blockers resolved: [Items]

## Current Sprint Status
- [Story/task]: [Status, owner]
- Velocity: [Current vs. planned]

## Blockers to Discuss
- [Blocker]: [Context, proposed resolution]

## Agenda
1. Quick wins and shout-outs
2. Sprint progress check
3. Blocker discussion
4. Upcoming priorities
5. Open items

## Decisions Needed
- [Decision with options]
```

---

### Stakeholder Review Preparation

```
# Stakeholder Review Prep: [Date]

## Audience
- [Name, Role, Key interests/concerns]

## Narrative
[2-3 sentence summary of the story you want to tell]

## Progress Update
- [Milestone]: [Status, evidence]

## Risks and Mitigations
- [Risk]: [Mitigation, ask]

## Asks / Decisions Needed
- [Ask 1]: [Context, options, recommendation]

## Anticipated Questions
- Q: [Likely question]
  A: [Prepared response]

## Supporting Data
- [Metric/chart to reference]
```

---

### Planning Meeting Preparation

```
# Planning Prep: [Type] - [Date]

## Retrospective on Last Period
- Planned vs. delivered: [Summary]
- Key learnings: [Items]

## Priorities for Next Period
- [Priority 1]: [Rationale, effort estimate]
- [Priority 2]: [Rationale, effort estimate]

## Capacity
- Team availability: [Any PTO, holidays, dependencies]
- Estimated capacity: [Story points or effort units]

## Open Questions
- [Question that needs resolution before committing]

## Dependencies
- [External dependency]: [Status, risk]

## Proposed Commitments
- [Commitment 1]
- [Commitment 2]
```

---

### Board / Executive Review Preparation

```
# Board Prep: [Date]

## Executive Summary
[3-sentence summary: where we are, key wins, key risks]

## Business Metrics
| Metric | Current | Target | Trend |
|---|---|---|---|
| [Metric] | [Value] | [Target] | [Up/Down/Flat] |

## Strategic Progress
- [Initiative]: [Status, next milestone]

## Key Risks
- [Risk]: [Impact, mitigation, board awareness needed]

## Financial Update
- [Burn rate, runway, revenue, etc.]

## Asks from the Board
- [Ask]: [Context]

## Anticipated Questions
- Q: [Question]
  A: [Prepared answer with data]

## Appendix
- [Supporting materials to have ready but not present]
```

---

## Output

Write to `knowledge/meetings/prep-<type>-YYYY-MM-DD.md` using the meeting type and today's date.

Tell the user the file path and highlight:
- Outstanding action items from past meetings that need follow-up
- Decisions that should be made in this meeting
- The single most important thing to get out of this meeting
