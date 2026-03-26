# CLAUDE.md Template

This is the structural blueprint for CLAUDE.md. It configures Claude Code's behavior for this specific repository.

## Structure

```markdown
# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

@SOUL.md
@AGENTS.md
@MEMORY.md

## Role

Claude Code is the execution environment for this repository.

- {Implementation model, e.g., "Sonnet is the default implementation model for coding work."}
- {Review policy, e.g., "Default review requires one external pass: Opus as Reviewer."}
- {Review completion rule, e.g., "Review pass is required before implementation is considered complete for issue-driven work."}

- Build features, scaffolding, config, scripts, and application code directly in the repo.
- {Reference docs, e.g., "Treat `setup-guide.md` as the implementation reference before writing code."}
- Treat review as a separate phase. Do not consider your own implementation "reviewed" just because you checked it yourself.
- If external review feedback exists, address it before considering work complete.

## Current Project State

- {Honest status, e.g., "The repository is still in documentation and planning mode."}
- {What exists, e.g., "There is no application source tree yet."}
- {Next milestone, e.g., "The next major milestone is Phase A: project skeleton."}

## Tool Preferences

- Prefer file tools for reading and editing repository files.
- Use shell for `git`, `gh`, {package manager}, and validation commands.
- {CI/CD tool, e.g., "Use `gh` for GitHub issues and pull requests."}
- Run checks in the shell: {e.g., `pnpm type-check`, `pnpm lint`, `pnpm build`}.

## Development Commands

```bash
{Database/service startup, e.g., docker compose up -d postgres}
{Dev server, e.g., pnpm dev}

{Type checking, e.g., pnpm type-check}
{Linting, e.g., pnpm lint}
{Build, e.g., pnpm build}
{Start, e.g., pnpm start}
{Tests, e.g., pnpm test}
```

## Working Rules

- Read `SOUL.md` and `MEMORY.md` before substantial work.
- {Reference doc rule, e.g., "Read the relevant section of `setup-guide.md` before implementing."}
- Keep work inside the project root.
- Do not modify `SOUL.md` or `AGENTS.md` unless explicitly asked.
- Update `MEMORY.md` when milestones, blockers, or key decisions change.

## Review Boundary

- Reviewer passes should be read-only.
- {Reviewer config, e.g., "Default reviewer is Opus."}
- {Sync rule, e.g., "Review roles must stay synchronized with `AGENTS.md`."}
- Do not bypass failed checks or reviewer findings by changing process rules.

## Environment Variables
{Only include if the project has env vars.}

Required at runtime:
- `{VAR_NAME}` - {description if non-obvious}
- ...

Optional:
- `{VAR_NAME}` - {description and default behavior}
- ...
```

## Guidelines

- Target ~50-80 lines. Claude Code reads this on every conversation start - keep it lean.
- The `@SOUL.md`, `@AGENTS.md`, `@MEMORY.md` lines at the top auto-load those files into context. This is Claude Code-specific syntax.
- Development Commands must be copy-pasteable - use actual commands, not placeholders. If the repo is not yet bootstrapped, mark inferred commands with a `# planned` comment so agents know these are not yet runnable.
- Environment Variables list what the app needs, not how to configure the hosting provider.
- Current Project State should be honest. Update it (or instruct the user to update it) as the project progresses.
- The Role section should be short. Detailed behavioral rules live in AGENTS.md.
