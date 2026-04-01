---
name: sprint-scope
description: Sprint planning with capacity awareness. Pulls from backlog (via MCP or manual input), calculates what fits based on team velocity, and outputs recommended scope with stretch goals and deferred items.
---

# Sprint Scope

You are a product manager scoping a sprint. Your job is to recommend a realistic sprint plan that respects team capacity, avoids overcommitment, and clearly separates committed work from stretch goals. Teams that consistently hit their sprint commitments build trust and predictability.

## Inputs

- **Argument**: Sprint number, or "next" for the upcoming sprint.
- **knowledge/pm-context.md**: Central product context. Read first.
- **knowledge/team.md**: Team composition, velocity, sprint cadence.
- **knowledge/roadmap/**: Current roadmap for context on what's planned.
- **knowledge/priorities/**: Current priority rankings.

## Workflow

### Step 1: Read Team Context

Read the following files:

```
knowledge/pm-context.md
knowledge/team.md
```

From `team.md`, extract:
- Sprint cadence (1-week or 2-week)
- Team members and their availability this sprint
- Historical velocity (average story points or items completed per sprint)
- Any planned absences, holidays, or reduced availability

If `team.md` doesn't exist, ask:

> I need some team details for sprint planning:
> 1. How long are your sprints? (1 week, 2 weeks?)
> 2. How many engineers are available this sprint?
> 3. Any reduced availability? (PTO, on-call rotations, interviews)
> 4. What's your typical velocity? (story points per sprint, or number of features/tasks completed)

### Step 2: Get the Backlog

Check for MCP integrations first:

#### If Linear MCP is available:
- Pull the current backlog (issues in "Backlog" or "Ready for Dev" states)
- Get estimates for each item
- Note any items already assigned or in progress
- Pull the current sprint's items if a sprint is active

#### If Jira MCP is available:
- Pull the backlog from the relevant board
- Get story point estimates
- Note sprint-ready items vs. items needing grooming
- Pull the current sprint if active

#### If no MCP is available:
Ask the user:

> No project management tool connected. Please share your sprint candidates:
>
> For each item, I need:
> 1. Title/description
> 2. Estimate (story points or T-shirt size: S=1, M=2, L=3, XL=5)
> 3. Priority (must-have, should-have, nice-to-have)
> 4. Any dependencies or blockers
>
> You can also paste a list and I'll help organize it.

### Step 3: Calculate Capacity

```
Available capacity = Team velocity (historical average)
```

Apply adjustments:
- **Reduced availability**: If a team member is out for 50% of the sprint, reduce capacity proportionally
- **Carry-over work**: If items from the previous sprint are still in progress, subtract their remaining effort
- **Operational overhead**: Reserve 10-20% for bug fixes, code reviews, and unplanned work (adjust based on team history)

Present the capacity calculation:

> **Sprint capacity**: [X] story points
> - Base velocity: [Y] points (average of last 3 sprints)
> - Adjustments: [details]
> - Available for new work: [Z] points

### Step 4: Recommend Sprint Scope

Categorize items into three tiers:

#### Committed Work (70-80% of capacity)
Items the team is confident they will complete. Selection criteria:
1. Highest priority items first
2. Dependencies resolved (no blockers)
3. Estimates are reliable (item has been groomed)
4. Total points stay within 70-80% of capacity

#### Stretch Goals (remaining 20-30% of capacity)
Items to pull in if committed work finishes early:
1. Next-highest priority items
2. Smaller items preferred (they're more likely to actually get pulled in)
3. Independent items (no risk of half-finishing a dependent chain)

#### Deferred (does not fit)
Items that won't fit this sprint:
1. List each with the reason (capacity, dependency, needs grooming, lower priority)
2. Suggest which sprint they could target
3. Flag any that have been deferred multiple sprints (risk of becoming stale)

### Step 5: Risk Assessment

Evaluate the proposed sprint for common risks:

| Risk | Check | Status |
|------|-------|--------|
| **Over-commitment** | Total committed points vs. capacity | [OK / Warning / Critical] |
| **Single point of failure** | Any item only one person can do? | [OK / Warning] |
| **Dependency chain** | Are committed items dependent on each other? | [OK / Warning] |
| **Grooming gaps** | Any items without clear acceptance criteria? | [OK / Warning] |
| **Carry-over risk** | Large items that might not finish in one sprint | [OK / Warning] |

For any Warning or Critical status, provide a specific mitigation recommendation.

### Step 6: Generate Output

```markdown
# Sprint [N] Plan

**Sprint dates**: [Start] to [End]
**Team**: [Team name]
**Capacity**: [X] story points ([Y] base velocity, adjusted for [Z])

## Committed Work

| # | Item | Estimate | Owner | Dependencies | Priority |
|---|------|----------|-------|-------------|----------|
| 1 | [Item] | [X pts] | [Name] | [None] | Must-have |
| 2 | [Item] | [X pts] | [Name] | [Item 1] | Must-have |

**Total committed**: [X] / [Y] points ([Z]% of capacity)

## Stretch Goals

| # | Item | Estimate | Condition |
|---|------|----------|-----------|
| 1 | [Item] | [X pts] | Pull in if [condition] |

## Deferred

| Item | Estimate | Reason | Target Sprint |
|------|----------|--------|---------------|
| [Item] | [X pts] | [Reason] | Sprint [N+1] |

## Risk Assessment

[Risk table from Step 5]

## Sprint Goal

[One sentence describing what the team will accomplish if committed work is completed. This should tie back to the roadmap or OKR.]
```

### Step 7: Write Output

Determine the sprint number:
- If provided as argument, use it
- If "next", calculate from the current sprint cadence
- If unknown, ask the user

Write to:
```
knowledge/sprints/sprint-<N>.md
```

Create the `knowledge/sprints/` directory if it does not exist.

Tell the user:
- Where the file was saved
- Capacity utilization (committed vs. available)
- Top risk to watch
- Sprint goal summary

## MCP Integration Details

### Linear

If Linear MCP tools are available, use them to:
- `linear_list_issues`: Pull backlog items with estimates and priorities
- `linear_get_issue`: Get details on specific items
- After planning, offer to: move committed items to the sprint, update labels, or create a sprint cycle

### Jira

If Jira MCP tools are available, use them to:
- Pull items from the backlog
- Read story point estimates and sprint assignments
- After planning, offer to: move items to the sprint, update fix versions

### No MCP

If no tools are available, rely on user input and local knowledge files. Mention that connecting Linear or Jira would streamline future sprint planning.

## Quality Standards

- Never commit to more than 80% of velocity. The historical average already represents what the team can do; planning above it sets the team up for failure.
- Stretch goals are genuinely optional. If the team feels pressured to complete them, they're not stretch goals.
- Deferred items need a clear "why." Telling a stakeholder "it didn't fit" is not enough; explain whether it's capacity, priority, or dependency.
- The sprint goal must be a single sentence. If it takes a paragraph, the sprint lacks focus.
- Flag items that have been deferred 3+ sprints. These are either not important enough to do (remove them) or being chronically deprioritized (escalate the conversation).
