# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code skill called **prd-to-governance** that generates and audits four project governance files (`SOUL.md`, `AGENTS.md`, `CLAUDE.md`, `MEMORY.md`) from a PRD and the current state of a target repository. Published as a standalone skill package — no build system, no runtime dependencies.

## Repository Structure

- `SKILL.md` — the skill definition (frontmatter + full instructions). This is the entry point Claude Code loads when the skill is invoked. All behavioral logic lives here.
- `references/` — structural blueprints (`soul-template.md`, `agents-template.md`, `claude-template.md`, `memory-template.md`, `completed-phases-template.md`). These are not output files; they define the target structure for generated governance docs.
- `.gitignore` — excludes `.claude/`, `.agents/`, `*.skill`, `*.zip`, `skills-lock.json` (local Claude Code config and packaged artifacts).

## Key Design Decisions

- **Generation order matters**: SOUL.md first (principles), then AGENTS.md + CLAUDE.md (behavior), then MEMORY.md (state). Each file builds on the previous.
- **Two modes**: Generate (bootstrap from PRD) and Audit (inspect existing governance for drift, then optionally update).
- **Uncertainty markers** replace vague "TBD": `[NEEDS PRD CLARIFICATION]`, `[NEEDS CODEBASE DISCOVERY]`, `[USER DECISION REQUIRED]`, `[GOVERNANCE DRIFT]`.
- **Templates are blueprints, not rigid forms** — sections should be adapted or dropped based on project complexity.

## Editing Guidelines

- `SKILL.md` is the authoritative source for skill behavior. Template files in `references/` provide structural examples only; if they conflict with `SKILL.md`, the skill file wins.
- Target line counts: SOUL.md ~60-90, AGENTS.md ~100-150, CLAUDE.md ~50-80, MEMORY.md ~40-60 (initial).
- The skill's `description` field in SKILL.md frontmatter controls when Claude Code triggers this skill — keep it precise.
