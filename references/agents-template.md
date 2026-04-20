# AGENTS.md Template

This is the structural blueprint for AGENTS.md. It governs how agents (AI or human) work in the repository.

## Structure

```markdown
# Agent Instructions - {Project Name}

Read `SOUL.md` first for project identity and non-negotiable standards.
Read `MEMORY.md` next for current state, decisions, and blockers.

## Roles

{Define who does what. Be explicit about implementation vs review separation.}

- {e.g., Sonnet is the default implementation model for coding tasks.}
- {e.g., Opus is the reviewer. / Reviewer A is Opus, Reviewer B is Claude Code.}
- {e.g., Both review passes are required by default for issue-driven work.}
- The reviewer role is separate and read-only.
- Implementation is not complete until checks pass and review findings are addressed.

## Repository Boundary

Stay inside the project root.

- Do not read, write, or execute outside this repository.
- Do not modify governance files without explicit user instruction: `SOUL.md`, `AGENTS.md`
- {List any other protected/reference-only files, e.g.:}
- {Treat `prd.md` and `setup-guide.md` as reference documents unless the user explicitly asks to edit them.}

## Current Reality

{Honest snapshot of what exists RIGHT NOW, not aspirational state.}
- {e.g., The repo is still pre-implementation. No application source tree yet.}
- {e.g., Do not assume `src/`, `public/`, or `tests/` already exist.}

## Intended Project Structure

{Target directory tree - what the repo should converge toward.}

```text
src/
  {project-specific directories}
public/
tests/
scripts/
```

## Workflow

1. Read the relevant issue, task, or request carefully.
2. Re-check `SOUL.md`, `MEMORY.md`, and relevant reference docs.
3. Research before implementing.
4. Implement in small, testable increments.
5. Run validation before handoff: {e.g., `pnpm type-check`, `pnpm lint`}.
6. Update `MEMORY.md` after major decisions, completed phases, or newly discovered blockers.

## Review Rules

Reviewer passes are read-only by default (no file edits). The user may explicitly grant write access on a case-by-case basis.

{Define the review flow and what reviewers focus on.}

Minimum review checklist:
- Requirements from the task are fully addressed.
- No prohibited actions were taken.
- Security principles from `SOUL.md` are still satisfied.
- {Stack-specific checks, e.g., "Public routes include validation and abuse protections."}
- No secrets or sensitive plaintext were introduced.

## Git Conventions

- Branch naming: `issue-{number}-{short-description}`
- Commit format: `feat: ...`, `fix: ...`, `chore: ...`, `docs: ...`, `refactor: ...`
- One concern per commit
- Never force-push to `main`/`master`
- Never skip hooks with `--no-verify`

## Prohibited Actions

### Filesystem
- Do not work outside the project root.
- Do not delete top-level project directories.
- Do not write secrets, keys, or credentials into tracked files.
{Add project-specific filesystem prohibitions}

### Git
- No `git push --force` to shared branches.
- No `git reset --hard` unless the user explicitly asks for it.
- No `git clean -fd` unless the user explicitly asks for it.
- Do not amend pushed commits.

### System
- Do not install system packages.
- Do not start long-lived background services unless required and approved.
- Do not modify shell profiles or system environment configuration.
- Do not download and execute remote binaries casually.

### Security
{Project-specific security prohibitions, e.g.:}
- {Do not disable rate limiting or origin checks for convenience.}
- {Do not use privileged access in frontend queries.}
- {Do not output decrypted sensitive content to console or files.}

## Delivery Standard

A task is ready for handoff only when:
- the requested change is implemented or the blocker is clearly documented
- relevant checks were run or an inability to run them is stated plainly
- `MEMORY.md` is updated if the project state changed

## Phase Plan
{If the PRD defines implementation phases, list them here.}

Current roadmap:
- Phase A: {e.g., project skeleton and bootstrap}
- Phase B: {e.g., design system, shell, routing}
- Phase C: {e.g., feature implementations}
- ...
```

## Auto-Develop Policy Example

When the project uses an automated issue-processing pipeline, add this section to AGENTS.md. Adapt the specifics to the actual script and workflow.

```markdown
## Auto-Develop Policy

`auto-develop.sh` is the automation entry point for issue-driven development. The following rules are binding for any automated issue processing:

- Default automated flow: {implementation model} implements, {Reviewer A model} performs Reviewer A, and {Reviewer B model} performs Reviewer B.
- Review diffs are generated with `git diff {base-branch} -- . ':!MEMORY.md'` to include uncommitted working-tree changes while excluding MEMORY.md (which bloats context across review cycles).
- Only open issues with the label `{auto-label}` are eligible for processing.
- `Depends on #N` in the issue body is a hard blocker. All referenced issues must be `CLOSED` before the dependent issue can be started.
- Blocked issues are skipped silently; they do not cause the script to fail.
- Implementation agents write ONE status line to MEMORY.md "Next Up" (overwrite, not append). The pipeline writes the final "Completed Work" entry after review passes.
- If a fix cycle produces no code changes (only MEMORY.md/logs), remaining findings are treated as accepted deviations and the loop breaks.
```

Key patterns this section codifies:
- **MEMORY.md exclusion from diffs**: Prevents context overflow when status lines grow across fix cycles
- **Status line discipline**: One line in "Next Up", overwritten not appended, prevents MEMORY.md bloat
- **No-op fix detection**: Breaks infinite review loops when the implementation agent agrees with deviations
- **Pipeline owns "Completed Work"**: A separate post-review step writes the final concise summary

## Guidelines

- Target ~100-150 lines. Longer means agents skip sections.
- Prohibited actions must be specific - "be careful" is not enforceable, "do not write secrets into tracked files" is.
- The Current Reality section prevents agents from assuming things exist that don't.
- If you have automation (CI/CD, auto-develop scripts), add an "Auto-Develop Policy" section using the example above.
- Review rules should be genuinely useful. If the project is solo/small, a single review pass or even "user reviews" is fine.
