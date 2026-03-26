# prd-to-governance

Codex skill for generating and auditing project governance files from a PRD and the current repository state.

## Included

- `SKILL.md` - main skill instructions
- `references/soul-template.md` - `SOUL.md` blueprint
- `references/agents-template.md` - `AGENTS.md` blueprint
- `references/claude-template.md` - `CLAUDE.md` blueprint
- `references/memory-template.md` - `MEMORY.md` blueprint

## Purpose

This skill helps bootstrap and maintain:

- `SOUL.md`
- `AGENTS.md`
- `CLAUDE.md`
- `MEMORY.md`

It supports two modes:

- Generate
- Audit

## Notes

- The `.claude/settings.local.json` file from the local Codex installation is intentionally not included here because it contains machine-specific paths.
- This repository is suitable for publishing to GitHub as a standalone skill package.
