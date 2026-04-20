# MEMORY.md Template

This is the structural blueprint for MEMORY.md. It starts minimal and evolves during implementation while long-running history stays archived.

## Structure

```markdown
# Project Memory - {Project Name}

This is the living state document for the project. Update it when milestones, blockers, or important decisions change.

## Current State

- Phase: {e.g., Pre-implementation / Phase A in progress}
- Active milestone: {e.g., Phase A - Project Skeleton}
- Main coding model: {e.g., Sonnet via Claude Code}
- Review roles: {e.g., Opus = Reviewer}
- Known blocker: {e.g., none / "waiting for API credentials"}

## Completed Work

All completed phases: `memory/completed-phases.md`

## Key Decisions

| Date | Decision | Choice |
|---|---|---|
| {YYYY-MM-DD} | {e.g., CMS} | {e.g., Payload CMS 3.x} |
| {YYYY-MM-DD} | {e.g., Database} | {e.g., PostgreSQL 16} |
| {YYYY-MM-DD} | {e.g., Hosting} | {e.g., Hetzner + Coolify} |
{Seed with stack decisions from the PRD. New decisions get appended during implementation.}

## Key Implementation Notes

{Empty at start. Filled during implementation with notes about:}
{- Intentional deviations from the PRD or setup guide}
{- Non-obvious architectural decisions and their rationale}
{- Cross-cutting concerns that affect multiple components}

## Next Up

- {First concrete task, e.g., "Initialize project skeleton (Phase A)"}
- {Second task, e.g., "Set up database and initial collections"}
{Keep this section short - 3-5 items max. Update after each completed task.}

## Content Sources
{Only for content-heavy projects (websites, CMS). Skip for APIs/libraries.}

| Source | Status | Usage |
|---|---|---|
| {e.g., Legacy website} | {e.g., pending migration} | {e.g., text, images, URLs} |

## Infrastructure
{Only if relevant infrastructure is known.}

| Resource | Status | Purpose |
|---|---|---|
| {e.g., Hetzner CX22} | {e.g., not provisioned} | {e.g., production server} |
| {e.g., GitHub repository} | {e.g., initialized} | {e.g., version control} |

## Update Rules

When this file changes:
- keep `Completed Work` as a reference to `memory/completed-phases.md`
- Completed issue details go to `memory/completed-phases.md` (archive), not MEMORY.md
- add only current or open architecture and workflow decisions to `Key Decisions`
- Key Decisions that are fixed and unlikely to change should also be moved to the archive periodically
- update the active phase and blockers
- keep upcoming tasks short and current
- Operational knowledge (tool usage, server admin, service configuration) belongs in a global memory outside the project if available
- Ensure `memory/completed-phases.md` is not gitignored (use `memory/2026-*.md` pattern instead of `memory/` if daily flush files should be excluded)
```

## Guidelines

- Start minimal - target 40-60 lines. Keep this file lean; long-running project history belongs in `memory/completed-phases.md`.
- "Completed Work" in MEMORY.md should contain only the archive reference. Do not add issue-level entries inline.
- For multi-phase projects, organize detailed completed entries with `### Phase Name` subheadings in `memory/completed-phases.md`.
- "Key Decisions": seed only the top 5-7 most important stack decisions at creation time. Minor choices get added during implementation.
- "Next Up" should always be actionable. If you don't know the first task, write "Define first implementation tasks".
- Always convert relative dates to absolute dates (e.g., "next Thursday" -> "2026-03-28").
- This is the only governance file that changes frequently. The other three change rarely.

### Archive Strategy

Use the archive reference by default, even for small projects. The archive may stay short at first, but it keeps the MEMORY.md structure stable and becomes critical for projects with more than ~20 completed tasks, where MEMORY.md can exceed typical context injection limits (default: 20,000 characters).

- `memory/completed-phases.md` — detailed issue-level entries, organized by phase
- MEMORY.md — only current state, open decisions, next steps, infrastructure
- Archived entries remain searchable via `memory_search` / `memory_get`
- Use `references/completed-phases-template.md` when creating the archive for the first time

When using `.gitignore` for the `memory/` directory (e.g. to exclude daily flush files), ensure the archive file is explicitly included or use a pattern like `memory/2026-*.md` instead of `memory/`.

### Automation considerations

When the project uses automated pipelines (e.g., auto-develop scripts):

- MEMORY.md can bloat rapidly if each pipeline cycle appends completed-task history. Keep completed entries in the archive.
- Automated implementation steps should write only a single status line to "Next Up" (overwrite, not append).
- The pipeline — not the implementation agent — should write the final completed-work entry after review passes.
- The pipeline's memory-update step should write completed entries to `memory/completed-phases.md`, not MEMORY.md
- MEMORY.md should only receive status line updates in "Next Up" during implementation and fix cycles
- Exclude MEMORY.md from review diffs to prevent context growth across review-fix cycles.
