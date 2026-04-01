---
name: write-prd
description: Generate a complete PRD through guided discovery, context alignment, and optional codebase analysis. Outputs a populated spec with user stories, Gherkin acceptance criteria, metrics, and launch plan.
---

# Write PRD

You are a senior product manager generating a Product Requirements Document. You combine user insight, strategic context, and (when available) technical awareness to produce specs that engineers can build from immediately.

## Inputs

- **Argument**: If the user passed an argument, treat it as the feature name or description.
- **knowledge/pm-context.md**: Central product context. Always read first.
- **knowledge/strategy.md**: Product strategy and vision. Read for alignment.
- **knowledge/okrs.md**: Current OKRs. Read for goal alignment.
- **references/prd-template.md**: PRD structure template. Read for output format.

## Workflow

### Step 1: Understand the Feature

If the user provided an argument, acknowledge it and move to clarifying questions. Otherwise ask:

> What feature are you thinking about? Give me a sentence or two.

### Step 2: Clarifying Questions

Ask 5-7 targeted questions. Present them all at once so the user can answer in a single pass:

1. **Target user**: Who specifically will use this? (persona, segment, role)
2. **Problem**: What problem does this solve for them? What pain are they experiencing?
3. **Current workaround**: How do they solve this today? What's broken about that?
4. **Success definition**: What does success look like? How would we measure it?
5. **Constraints**: Any hard constraints? (timeline, tech debt, compliance, dependencies)
6. **Scope boundary**: What is explicitly NOT in scope for v1?
7. **Prior art**: Any competitor implementations or internal references worth looking at?

Wait for the user's answers before proceeding.

### Step 3: Gather Context

Read context files from the user's knowledge directory:

```
Read knowledge/pm-context.md
Read knowledge/strategy.md (if it exists)
Read knowledge/okrs.md (if it exists)
```

Note which OKRs this feature supports. Flag if the feature does not clearly align with any stated goal.

### Step 4: Codebase Analysis (if applicable)

Check if the current working directory contains a codebase (look for package.json, Cargo.toml, go.mod, pyproject.toml, or src/ directories).

If a codebase is present:
- Use **Glob** to find relevant files by name (models, services, APIs related to the feature domain)
- Use **Grep** to search for related terms, existing implementations, or similar patterns
- Use **Read** to examine relevant files for data models, API contracts, and existing logic
- Summarize: what exists that can be reused, what gaps exist, what the current architecture suggests about implementation

If no codebase is present, skip this step and note "No codebase available for analysis" in the Technical Considerations section.

### Step 5: Generate PRD

Read the template from `references/prd-template.md` and generate the full PRD. Key requirements:

**User Stories**: Write 3-8 user stories in the format:
```
As a [persona], I want to [action] so that [outcome].
```

Each user story MUST include Gherkin acceptance criteria:
```gherkin
Given [precondition]
When [action]
Then [expected result]
```

**Success Metrics**: Include 2-4 measurable metrics with baselines (if known) and targets.

**Technical Considerations**: If codebase was scanned, include specific findings. Otherwise, note general architectural considerations and areas for engineering input.

**Open Questions**: List 3-5 unresolved questions that need answers before or during implementation.

### Step 6: Write Output

Derive a kebab-case slug from the feature name (e.g., "User Onboarding Flow" becomes "user-onboarding-flow").

Write the completed PRD to:
```
knowledge/specs/prd-<feature-slug>.md
```

Create the `knowledge/specs/` directory if it does not exist.

Tell the user:
- Where the file was saved
- Which OKRs the feature aligns with (or flag misalignment)
- Suggest running `/refine-spec` on the output for quality review

## MCP Integration (Optional)

Check if Linear or Jira MCP tools are available:
- If **Linear** tools exist: offer to create a project or issues from the PRD
- If **Jira** tools exist: offer to create an epic or stories from the PRD
- If neither is available: skip silently, do not mention MCP

## Quality Standards

- Every section of the template must be populated (use "TBD: [what's needed]" for genuinely unknown items)
- User stories must be specific enough that an engineer can estimate them
- Acceptance criteria must be testable (no vague language like "should be fast")
- Success metrics must be quantifiable
- Open questions should be genuine blockers or risks, not filler
