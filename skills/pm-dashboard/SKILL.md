---
name: pm-dashboard
description: "Reads all knowledge files and MCP integrations to produce a single-page product status dashboard with OKR progress, sprint status, decisions, blockers, launches, and metrics."
---

# PM Dashboard: Product Status Overview

You produce a single-page status dashboard by reading everything in the `knowledge/` directory and pulling live data from any available MCP integrations.

## Before Running

1. Check that `knowledge/` exists. If not, tell the user: "No knowledge base found. Run `/pm-setup` first."
2. Read `knowledge/pm-context.md` to get product context and preferences.

## Data Collection

Read all available knowledge files. For each section below, note what data exists and what is missing.

### Files to Read

Read these files (skip any that don't exist):

- `knowledge/pm-context.md` : Product context
- `knowledge/okrs.md` : OKR definitions and progress
- `knowledge/team.md` : Team info
- `knowledge/roadmap/*.md` : Roadmap items
- `knowledge/sprints/*.md` : Sprint scoping (look for the most recent)
- `knowledge/decisions/*.md` : Recent decisions (last 5)
- `knowledge/launches/*.md` : Upcoming or recent launches
- `knowledge/metrics/*.md` : Latest metrics snapshots
- `knowledge/feedback/synthesis-*.md` : Latest feedback synthesis
- `knowledge/priorities/*.md` : Current prioritization
- `knowledge/experiments/*.md` : Active experiments

### MCP Integrations (Check and Use If Available)

Check if these tools are accessible. Use them if they are, skip gracefully if not.

**Linear / Jira** (project management tools):
- Try to list current sprint tickets, their status, and assignees
- Pull velocity or burndown if available
- Note blocked tickets

**GitHub** (source control):
- Check for open PRs and their review status
- Look for recent deployments or releases
- Note any failing CI checks

**Slack** (communication):
- Look for recent product-related threads or escalations
- Check for unresolved questions in product channels

For each integration that is NOT available, add a small note in the dashboard: "(Live data unavailable, install {tool} MCP for real-time updates)"

## Dashboard Output

Present the dashboard in this format:

```markdown
# Product Dashboard: {product_name}
*Generated: {today's date and time}*

---

## OKR Progress
{For each objective, show the objective and each key result with progress if known}
{If no progress data, show the OKRs and mark "Progress: not yet tracked"}

| Objective | Key Result | Status |
|-----------|-----------|--------|
| {obj} | {kr} | {status or "Not tracked"} |

---

## Sprint Status
{Current sprint name/number if known}
{Ticket counts by status: To Do, In Progress, Done}
{Blocked items highlighted}
{If no sprint data: "No sprint data. Run /sprint-scope to plan your next sprint."}

---

## Recent Decisions
{Last 3-5 decisions with date, title, and outcome}
{If none: "No decisions recorded. Use /decision-record after your next key decision."}

---

## Open Blockers
{Any blocked items from sprint, launches, or decisions}
{If none: "No blockers identified."}

---

## Upcoming Launches
{Next 2-3 launches with target date and readiness status}
{If none: "No launches planned. Use /launch when you're ready."}

---

## Metrics Snapshot
{Latest values for key metrics from pm-context.md}
{Trends if multiple snapshots exist}
{If no data: "No metrics recorded. Use /metrics-check to start tracking."}

---

## Active Experiments
{Any running A/B tests or experiments with status}
{If none: "No active experiments."}

---

## Customer Feedback Themes
{Top 3 themes from latest feedback synthesis}
{If none: "No feedback synthesized. Use /feedback-synthesis to analyze customer feedback."}

---

## What Needs Attention
{Synthesize: what looks off, what's overdue, what's at risk}
{Specific recommendations with skill commands to resolve each}
```

## Behavior Notes

- **Be honest about gaps.** If data is missing, say so clearly and point to the skill that fills that gap. Do not fabricate data.
- **Highlight risks.** If an OKR has no progress updates and the quarter is half over, flag it. If a launch date is approaching with no plan, flag it.
- **Keep it scannable.** Use tables, bullet points, and short phrases. This is a dashboard, not a narrative.
- **Date awareness.** Use today's date to calculate time remaining in the quarter, days until launches, sprint progress.
- **Suggest actions.** End each empty section with a specific command the user can run to fill it.
- **Respect preferences.** Use the tone and format preferences from `pm-context.md`.
