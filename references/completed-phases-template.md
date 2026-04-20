# Completed Phases Archive Template

This is the structural blueprint for `memory/completed-phases.md`. It stores completed-work history so MEMORY.md can stay focused on current state.

## Structure

```markdown
# Completed Phases - {Project Name}

This archive stores completed issue-level work. MEMORY.md links here from its "Completed Work" section.

## {Phase Name, e.g., Pre-implementation}

- {YYYY-MM-DD}: PRD created in `{prd-filename}`
- {YYYY-MM-DD}: Governance files drafted

## {Phase Name, e.g., Phase A - Project Bootstrap}

- {YYYY-MM-DD}: {Task-ID (#N)} - {brief completed task summary}
- {YYYY-MM-DD}: {Task-ID (#N)} - {brief completed task summary}. Last fix: {only if multiple review rounds}

## Archived Decisions

| Date | Decision | Choice | Reason Archived |
|---|---|---|---|
| {YYYY-MM-DD} | {Decision} | {Choice} | Stable and unlikely to change |
```

## Guidelines

- Organize completed entries by implementation phase.
- Keep each issue-level entry to one line.
- Include only final outcomes, not every review cycle.
- Move stable Key Decisions here when they are no longer active context for MEMORY.md.
- Do not store operational knowledge here if a global memory is available.
