# PM-OS: The Product Manager's Operating System

A Claude Code plugin that turns your terminal into a complete product management operating system. 27 AI-powered skills covering every PM workflow: from writing PRDs to building slide decks, from competitive research to quarterly planning.

**No empty templates. No manual setup.** PM-OS asks questions, does the research, and generates ready-to-use documents. You review and refine.

## Quick Start

### Install

**Step 1:** Add the marketplace
```bash
claude plugin marketplace add shaan-ad/pm-os
```

**Step 2:** Install the plugin
```bash
claude plugin install pm-os
```

### Set Up Your Product

```
/pm-setup
```

PM-OS asks about your company (drop a URL and it researches everything), your product, team, tools, and goals. It creates a `knowledge/` directory with populated context files that every skill reads.

### Start Using It

```
/write-prd notification-system        # Write a PRD
/create-slide-deck quarterly-review   # Build an HTML slide deck
/pm-briefing                          # Morning briefing
/launch-plan checkout-redesign        # Launch checklist
```

Or just describe what you need in plain language. PM-OS routes to the right skill automatically.

## What's Inside

### 27 Skills Across 11 Categories

| Category | Skills | What They Do |
|----------|--------|-------------|
| **Core** | pm-os, pm-setup, pm-dashboard | Gateway routing, onboarding, status overview |
| **Discovery** | feedback-synthesis, competitive-intel, opportunity-assessment | Customer insights, battlecards, idea validation |
| **Strategy** | write-strategy, okr-writer, quarterly-plan | Strategy docs, OKR creation, quarter planning |
| **User Research** | persona-builder, interview-guide, journey-map | Personas, research scripts, journey mapping |
| **Define** | write-prd, refine-spec, tech-feasibility | PRDs, spec review, codebase-aware feasibility |
| **Plan** | prioritize, roadmap-builder, sprint-scope | RICE/ICE scoring, roadmaps, sprint planning |
| **Deliver** | launch-plan, retro-facilitator | Launch checklists, retrospectives and post-mortems |
| **Communicate** | status-update, decision-record, meeting-prep | Stakeholder updates, decision logging, meeting prep |
| **Present** | create-slide-deck | HTML slide decks with your brand colors, keyboard nav, print-ready |
| **Measure** | metrics-check, experiment-review | KPI reviews, A/B test analysis |
| **Cadence** | pm-briefing | Morning briefing with prioritized action items |

### Slash Commands

| Command | What It Does |
|---------|-------------|
| `/pm-setup` | Interactive onboarding wizard |
| `/write-prd <feature>` | Generate a PRD |
| `/create-slide-deck <topic>` | Build an HTML slide deck |
| `/launch-plan <feature>` | Create a launch checklist |
| `/pm-briefing` | Morning briefing |

Every other skill activates automatically based on what you ask. Say "research Notion as a competitor" and competitive-intel fires. Say "prioritize these features" and the RICE scorer kicks in.

## The Knowledge System

PM-OS creates a `knowledge/` directory in your project that accumulates context over time.

```
knowledge/
  pm-context.md       # Your product, team, and tools (the "brain")
  team.md             # Roster, roles, capacity
  okrs.md             # Current quarter OKRs
  strategy.md         # Product strategy
  competitors/        # Battlecards (one per competitor)
  decisions/          # Decision records with rationale
  specs/              # PRDs and feature specs
  feedback/           # Customer feedback syntheses
  priorities/         # Feature rankings
  roadmap/            # Quarterly roadmaps
  sprints/            # Sprint plans
  launches/           # Launch checklists
  updates/            # Stakeholder updates
  meetings/           # Meeting prep and notes
  metrics/            # KPI reviews
  experiments/        # A/B test analyses
  opportunities/      # Opportunity assessments
  retros/             # Retrospectives and post-mortems
  briefs/             # Daily briefings
  personas/           # User personas
  research/           # Interview guides, journey maps
  feasibility/        # Technical feasibility assessments
```

Every skill reads from and writes to this directory. After a month of use, your knowledge base contains dozens of interconnected documents. Each new skill invocation gets richer because of this accumulated context.

**This is what makes PM-OS an operating system, not just a script collection.** The knowledge compounds.

## Power-Ups (Optional Integrations)

PM-OS works out of the box with zero integrations. Every skill falls back to asking you or reading local files when no external tools are connected.

Install companion plugins to unlock live data:

| Integration | What It Unlocks |
|-------------|----------------|
| **GitHub** | PR data for status updates, release notes, feasibility checks |
| **Linear** | Sprint data, backlog, ticket status for planning skills |
| **Jira** | Sprint data, backlog, ticket status (alternative to Linear) |
| **Slack** | Channel messages for feedback synthesis, standup digests |
| **Notion** | Doc imports, wiki references |
| **Confluence** | Doc imports for enterprise teams |
| **Amplitude / Mixpanel** | Live metrics for KPI reviews and experiment analysis |
| **Figma** | Design references for PRDs and journey maps |

Skills detect installed integrations automatically. Install the Slack plugin and feedback-synthesis starts pulling from channels. Install Linear and sprint-scope reads your actual backlog.

## How It Works

### Design Principles

1. **Claude does the work, you review.** No empty templates. Every skill asks questions, does research, and generates populated documents.
2. **URLs over manual input.** Drop a company URL and PM-OS researches it. Drop a competitor URL and it builds a battlecard. Information that exists on the web should come from the web.
3. **Integration-aware, not integration-dependent.** Every skill checks if MCP tools are available, uses them if present, and gracefully falls back otherwise.
4. **Context compounds over time.** The knowledge directory grows with every skill you use. Your 50th PRD benefits from all the context your first 49 built up.

### What Makes It an "OS"

- **The knowledge directory is the filesystem.** Persistent, version-controlled, human-readable context that every skill shares.
- **The gateway skill is the kernel.** It routes every request to the right skill. You don't memorize 27 commands.
- **Progressive enhancement.** Install a Linear plugin and planning skills auto-upgrade. The OS gets more powerful as you add peripherals.

## Skill Highlights

### Codebase-Aware Product Management

If you run PM-OS inside a code repository, skills like `tech-feasibility` and `write-prd` scan the actual codebase. Feasibility questions that used to take 3-5 days of engineer time now take minutes.

### Multi-Audience Stakeholder Updates

`status-update` generates three versions of the same update: engineering (technical detail), leadership (business impact), customer-facing (what's new). From the same raw context.

### Decision Memory

`decision-record` captures every decision with alternatives, rationale, and "revisit if" conditions. After months of use, anyone can ask "why did we choose X?" and get the real answer with full context.

### Brand-Matched Slide Decks

`/create-slide-deck quarterly-review` builds a full HTML presentation with your company's colors, keyboard navigation, and print-ready layouts. It pulls data from your knowledge directory: metrics for product reviews, battlecards for competitive overviews, OKRs for strategy presentations. No PowerPoint needed.

### PRD-to-Launch Pipeline

Write a PRD with `/write-prd`, refine it with `/refine-spec`, decompose it with `/prioritize`, plan the sprint with `/sprint-scope`, then generate the launch checklist with `/launch-plan`. Each skill reads what the previous one wrote.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, desktop, or IDE extension)
- Claude Pro, Team, or Enterprise plan

## Security

Every skill in PM-OS is a plain markdown file you can read and audit. No compiled code, no obfuscated logic.

The `main` branch is protected: all changes require a pull request with an approved review. No direct pushes, no force pushes, no exceptions. This applies to the maintainer too.

Before installing any Claude Code plugin, you should review what it does. PM-OS skills can read files and run commands through Claude Code's normal permission system, which always asks before executing.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add skills, improve existing ones, or suggest new workflows.

## License

MIT. See [LICENSE](LICENSE).
