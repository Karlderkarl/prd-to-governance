# MEMORY.md Template

This is the structural blueprint for MEMORY.md. It starts minimal and grows organically during implementation.

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

{Organize by phase for multi-phase projects. Use flat list for simple projects.}

### {Phase Name, e.g., Pre-implementation}
- PRD created in `{prd-filename}`
{Add "Governance files drafted" here only AFTER the selected governance files were written successfully.}

### {Phase Name, e.g., Phase A — Project Bootstrap}
{This section grows with every completed task. Each entry is ONE line:}
{- Task-ID (#N): brief description. Last fix: <what the final review fix addressed> (only if multiple review rounds)}
{Do NOT log individual review cycles, reviewer names, or verbose details here.}

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
- move completed work into `Completed Work`
- add new architecture or workflow decisions to `Key Decisions`
- update the active phase and blockers
- keep upcoming tasks short and current
```

## Guidelines

- Start minimal - target 40-60 lines. This file will grow to hundreds of lines over a project's lifetime.
- "Completed Work" entries must be one-liners. Detailed descriptions belong in commit messages and PR bodies. This is critical for projects with automated pipelines where MEMORY.md is loaded into every agent context — verbose entries cause context overflow.
- For multi-phase projects, use `### Phase Name` subheadings under "Completed Work" to keep entries organized.
- "Key Decisions": seed only the top 5-7 most important stack decisions at creation time. Minor choices get added during implementation.
- "Next Up" should always be actionable. If you don't know the first task, write "Define first implementation tasks".
- Always convert relative dates to absolute dates (e.g., "next Thursday" -> "2026-03-28").
- This is the only governance file that changes frequently. The other three change rarely.

### Automation considerations

When the project uses automated pipelines (e.g., auto-develop scripts):

- MEMORY.md can bloat rapidly if each pipeline cycle appends verbose entries. Enforce strict one-line summaries.
- Automated implementation steps should write only a single status line to "Next Up" (overwrite, not append).
- The pipeline — not the implementation agent — should write the final "Completed Work" entry after review passes.
- Exclude MEMORY.md from review diffs to prevent context growth across review-fix cycles.
