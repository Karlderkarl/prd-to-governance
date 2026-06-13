# prd-to-governance

A Claude Code skill that generates and audits project governance files from a PRD (Product Requirements Document) and the current state of your repository.

## Recommended Successor

This repository is the earlier skill. The recommended follow-up is [`Karlderkarl/governance-to-automation`](https://github.com/Karlderkarl/governance-to-automation).

If you want the newer skill, use:

```bash
npx skills add Karlderkarl/governance-to-automation
```

## Install

```bash
npx skills add Karlderkarl/prd-to-governance
```

Requires [Claude Code](https://claude.ai/code).

## Usage

Once installed, Claude Code can auto-select the skill when you ask it to create, refresh, or audit governance from a PRD.

You can also invoke it explicitly:

```
@prd-to-governance use docs/prd.md to generate governance files
```

Point it at a PRD file in your project. The skill will inspect the repository, ask about key decisions, then generate or audit tailored governance files.

## What it does

From a PRD and your repo, the skill produces four core governance files:

| File | Purpose |
|---|---|
| `SOUL.md` | Project identity: stack, architecture, coding standards, security, compliance |
| `AGENTS.md` | Agent behavior: roles, workflow, review rules, prohibited actions, phase plan |
| `CLAUDE.md` | Claude Code config: tool preferences, dev commands, working rules, env vars |
| `MEMORY.md` | Living status: completed work, key decisions, blockers, next steps |

It may also create `memory/completed-phases.md` as an archive alongside `MEMORY.md` when project history needs to be preserved cleanly.

Generation order matters - each file builds on the previous.

## Modes

**Generate** - Bootstrap governance files from a PRD. Use when starting a new project or regenerating after major PRD changes.

**Audit** - Compare existing governance files against the PRD and actual repository state. Report drift, then optionally update files.

The skill does not blindly overwrite files. If governance files already exist, it audits first by default and asks whether each file should be overwritten, merged, or skipped before writing.

## How it works

1. Determines the project root from the PRD location
2. Reads the PRD and extracts stack, architecture, security, and compliance details
3. Inspects the repository for actual build config, dependencies, and structure
4. Interviews you on key decisions (agent roles, git conventions, task management)
5. Generates or updates governance files with concrete values, not vague placeholders
6. Marks anything unclear with explicit uncertainty markers (`[NEEDS PRD CLARIFICATION]`, `[USER DECISION REQUIRED]`, etc.)
7. Presents a summary for review before writing any files

## Repository structure

```
skills/prd-to-governance/
  SKILL.md                              # Skill definition (entry point)
  references/
    soul-template.md                    # SOUL.md blueprint
    agents-template.md                  # AGENTS.md blueprint
    claude-template.md                  # CLAUDE.md blueprint
    memory-template.md                  # MEMORY.md blueprint
    completed-phases-template.md        # Archive blueprint
```

The templates in `references/` are structural blueprints, not rigid forms - sections are adapted or dropped based on project complexity.

## Security

Security issues should be reported through GitHub Private Vulnerability Reporting, not public issues.

## License

This project is licensed under the terms of the MIT license.
