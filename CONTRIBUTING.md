# Contributing to PM-OS

Thanks for considering contributing to PM-OS! This guide covers how to add new skills, improve existing ones, and maintain consistency across the plugin.

## Adding a New Skill

### 1. Create the skill directory

```
skills/your-skill-name/SKILL.md
skills/your-skill-name/references/    # Optional: reference docs
```

### 2. Write the SKILL.md

Every skill follows this structure:

```markdown
---
name: your-skill-name
description: One-line description of when this skill should trigger
---

# Skill Title

Role statement: who Claude acts as for this skill.

## Inputs
- What files to read from knowledge/
- What MCP tools to check for

## Workflow
### Step 1: ...
### Step 2: ...

## Output Format
What the generated document looks like.

## Output Location
Where to write: knowledge/<subdirectory>/<filename>.md
```

### 3. Follow these conventions

**Interaction pattern:** Every skill should:
- Ask questions conversationally (don't dump a form)
- Prefer URLs over manual input when information lives on the web
- Read `knowledge/pm-context.md` first for product context
- Present drafts for review before finalizing

**MCP integration pattern:** Skills that could use external data must:
- Check if the MCP tool is available
- Use it if present
- Fall back to asking the user or reading local files
- Never fail because an integration is missing

Example:
```markdown
### Step N: Gather Live Data

Check if Linear/Jira MCP tools are available:
- If available: pull current sprint backlog and ticket statuses
- If not available: ask the user to describe current sprint state, or read from knowledge/sprints/
```

**Knowledge directory writes:** Always write to the appropriate subdirectory:
- `competitors/` for competitive intel
- `decisions/` for decision records
- `specs/` for PRDs and feature specs
- Use date-stamped filenames: `YYYY-MM-DD-<topic>.md`

**Style rules:**
- No em dashes. Use colons, commas, or separate sentences.
- Keep skill files focused. If a skill exceeds 200 lines, consider splitting reference material into `references/`.
- Use markdown tables for structured comparisons.
- Use checkbox lists (`- [ ]`) for actionable checklists.

## Improving an Existing Skill

1. Read the current SKILL.md
2. Test it by running the skill in Claude Code
3. Make your changes
4. Verify the skill still follows all conventions above
5. Submit a PR with a description of what you changed and why

## Testing

Since skills are markdown instructions (not code), testing means running them:

1. Install PM-OS locally
2. Run `/pm-setup` to create a knowledge base
3. Run the skill you modified
4. Verify: Does it ask good questions? Does it produce useful output? Does it read/write the right files?

## Suggesting New Skills

Open an issue with:
- **Skill name:** What would you call it?
- **Trigger:** When should it activate?
- **Workflow:** What should the skill do, step by step?
- **Knowledge reads/writes:** What files does it need?
- **Why:** What PM pain point does this solve?

## Pull Request Checklist

- [ ] Skill follows the YAML frontmatter format
- [ ] `description` field clearly defines when the skill triggers
- [ ] Skill reads `knowledge/pm-context.md` for context
- [ ] MCP integrations have graceful fallbacks
- [ ] No em dashes used
- [ ] Output is written to the correct `knowledge/` subdirectory
- [ ] Skill is interactive (asks questions, not just generates)
