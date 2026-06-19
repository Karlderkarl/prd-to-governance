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
- Do not modify these governance files without explicit user instruction: `SOUL.md`, `AGENTS.md`, `CLAUDE.md`
- `MEMORY.md` is the one exception: update it only for its defined status and history fields (Current State, Next Up, Completed Work reference, Key Decisions, Governance Drift). Do not restructure it or delete existing history.
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

### Test discipline

Omit all of the following to keep the pipeline's test gate `off` — that is the backward-compatible default, not a gap. Add them only when automated tasks should be guarded by a deterministic test gate.

- `TEST_POLICY`: required
- `TEST_ELIGIBILITY` (one matcher per line, `<type>:<pattern>=<include|except>`):
  - `label:backend=include`
  - `title:^(feat|fix):=include`
  - `title:^docs:=except`
- The targeted test command itself lives in CLAUDE.md *Development Commands* as `TARGETED_TEST_CMD` (with a literal `{TARGET}` token), not here.
```

Key patterns this section codifies:
- **MEMORY.md exclusion from diffs**: Prevents context overflow when status lines grow across fix cycles
- **Status line discipline**: One line in "Next Up", overwritten not appended, prevents MEMORY.md bloat
- **No-op fix detection**: Breaks infinite review loops when the implementation agent agrees with deviations
- **Pipeline owns "Completed Work"**: A separate post-review step writes the final concise summary
- **Test discipline (optional, fail-safe to `off`)**: `TEST_POLICY` is `off` | `preferred` | `required`. `preferred` reruns a targeted test after implementation but never blocks; `required` arms a deterministic red→green gate and needs a `TARGETED_TEST_CMD` in CLAUDE.md (without it the pipeline degrades to `preferred` and logs `[GOVERNANCE DRIFT]`). `TEST_ELIGIBILITY` matchers decide which tasks the gate covers: `except` wins over `include`; with only `include` matchers the base is "not eligible" (allowlist), with only `except` matchers it is "eligible" (denylist). A non-`off` policy with empty or inert eligibility is invalid (`[NEEDS GOVERNANCE]`), so always pair it with at least one usable matcher. For label-less task sources (local task-list / MEMORY.md "Next Up"), only `title:` matchers can match.

## Skill Policy Example

Add this section to AGENTS.md when specific tasks should deterministically trigger a specific Claude Code skill during automated development. The `governance-to-automation` pipeline reads it into its `SKILL_MAP` and resolves a skill once per task (before implementation), injecting the result only into the implement/fix/refactor prompts. Omitting the section is a valid no-op (empty map) — never invent matchers just to fill it.

```markdown
## Skill Policy

Each line is one explicit matcher: `<type>:<pattern> = <skill-name>`.

- `<type>` is `label` (matched against a whole issue/task label; multi-word labels are fine) or `title` (an extended regex tested against the task title and body).
- Whitespace around `:` and `=` is optional. The pattern may contain `:` but must never contain `=`.
- Matchers must be unambiguous: if two matchers resolve to different skills for the same task, the pipeline logs `(ambiguous)` and injects nothing. Keep patterns disjoint.

label:area: auth = security-hardening
title:^perf: = performance-tuning
```

Key patterns this section codifies:
- **Deterministic skill routing**: the skill choice lives in governance, not in a model's per-task guess
- **Fail-safe to no-op**: an absent or empty Skill Policy leaves the pipeline running unchanged
- **Ambiguity over guessing**: overlapping matchers inject nothing rather than silently picking one
- **Label-less task sources**: with a local task-list or MEMORY.md "Next Up" (no labels), only `title:` matchers can resolve a skill

## Guidelines

- Target ~100-150 lines. Longer means agents skip sections.
- Prohibited actions must be specific - "be careful" is not enforceable, "do not write secrets into tracked files" is.
- The Current Reality section prevents agents from assuming things exist that don't.
- If you have automation (CI/CD, auto-develop scripts), add an "Auto-Develop Policy" section using the example above.
- If the project uses `governance-to-automation`, AGENTS.md is the producing side of two optional contracts it consumes: the **Skill Policy** section (seeds `SKILL_MAP`) and the **Test discipline** fields inside Auto-Develop Policy (`TEST_POLICY` / `TEST_ELIGIBILITY`, paired with CLAUDE.md `TARGETED_TEST_CMD`). Both are fail-safe: leave them out and the pipeline runs unchanged. Add them only when the behaviour is actually wanted, and keep matchers in the exact `<type>:<pattern>=<value>` form shown above so the pipeline can parse them.
- Review rules should be genuinely useful. If the project is solo/small, a single review pass or even "user reviews" is fine.
