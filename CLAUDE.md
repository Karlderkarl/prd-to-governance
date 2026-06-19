# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code skill called **prd-to-governance** that generates and audits four project governance files (`SOUL.md`, `AGENTS.md`, `CLAUDE.md`, `MEMORY.md`) from a PRD and the current state of a target repository. Published as a standalone skill package - no build system, no runtime dependencies.

## Repository Structure

- `skills/prd-to-governance/SKILL.md` - the skill definition (frontmatter + full instructions). This is the entry point Claude Code loads when the skill is invoked. All behavioral logic lives here.
- `skills/prd-to-governance/references/` - structural blueprints (`soul-template.md`, `agents-template.md`, `claude-template.md`, `memory-template.md`, `completed-phases-template.md`). These are not output files; they define the target structure for generated governance docs.
- `.gitignore` - excludes `.claude/`, `.agents/`, `*.skill`, `*.zip`, `skills-lock.json` (local Claude Code config and packaged artifacts).

## Key Design Decisions

- **Generation order matters**: SOUL.md first (principles), then AGENTS.md + CLAUDE.md (behavior), then MEMORY.md (state). Each file builds on the previous.
- **Two modes**: Generate (bootstrap from PRD) and Audit (inspect existing governance for drift, then optionally update).
- **Uncertainty markers** replace vague "TBD": `[NEEDS PRD CLARIFICATION]`, `[NEEDS CODEBASE DISCOVERY]`, `[USER DECISION REQUIRED]`, `[GOVERNANCE DRIFT]`, `[NEEDS GOVERNANCE]` (the last is shared with `governance-to-automation` and only for its downstream-automation contract).
- **Templates are blueprints, not rigid forms** - sections should be adapted or dropped based on project complexity.
- **Downstream automation contract**: the generated governance is the contract consumed by the `governance-to-automation` skill. AGENTS.md and CLAUDE.md can carry optional, fail-safe fields it reads - an AGENTS.md *Skill Policy* (seeds the pipeline's `SKILL_MAP`), `TEST_POLICY` / `TEST_ELIGIBILITY` inside the *Auto-Develop Policy*, and a `TARGETED_TEST_CMD` (with a literal `{TARGET}` token) in CLAUDE.md *Development Commands*. Omitting them is a valid no-op; matchers use the `<type>:<pattern>=<value>` form (`type` is `label` or `title`, and a pattern may contain `:` but never `=`).

## Editing Guidelines

- `skills/prd-to-governance/SKILL.md` is the authoritative source for skill behavior. Template files in `skills/prd-to-governance/references/` provide structural examples only; if they conflict with `SKILL.md`, the skill file wins.
- Target line counts: SOUL.md ~60-90, AGENTS.md ~100-150, CLAUDE.md ~50-80, MEMORY.md ~40-60 (initial).
- The skill's `description` field in SKILL.md frontmatter controls when Claude Code triggers this skill - keep it precise.
