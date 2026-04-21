---
name: prd-to-governance
description: "Create, update, and audit SOUL.md, AGENTS.md, CLAUDE.md, and MEMORY.md from a PRD and the current repository state. Use when bootstrapping project governance, refreshing governance after PRD changes, or checking governance drift against the codebase."
license: MIT
---

# PRD to Governance Files

This skill generates or audits four project governance files around a PRD (Product Requirements Document) and the current repository state. Together they give Claude Code a durable operating model for the repository.

For this skill, one project equals one folder. Treat the folder that contains the PRD as the default project root. Generate and maintain `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, and `MEMORY.md` in that same folder. If the user explicitly says the PRD lives outside the real project folder, stop and ask which folder should be treated as the project root. After the user confirms that folder, use it consistently for all reads and writes.

| File | Purpose |
|---|---|
| **SOUL.md** | Project identity: stack, architecture, coding standards, security, compliance |
| **AGENTS.md** | How agents work: roles, workflow, review rules, prohibited actions, phase plan |
| **CLAUDE.md** | Claude Code configuration: tool preferences, dev commands, working rules, env vars |
| **MEMORY.md** | Living status: completed work, key decisions, blockers, next steps, drift notes |

The generation order matters because each file builds on the previous:

1. **SOUL.md** first - distills the non-negotiable principles
2. **AGENTS.md** + **CLAUDE.md** - define how agents interact with the repo
3. **MEMORY.md** last - captures initial state and references all other files

## Modes

This skill has two operating modes:

- **Generate** - create or refresh governance files from the PRD, user decisions, and discovered repository reality
- **Audit** - inspect existing governance files against the PRD and actual repository state, report governance drift, then optionally update the files

Use **Generate** when:

- the repo has a PRD but no governance files yet
- the user wants to bootstrap `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, or `MEMORY.md`
- the user wants to regenerate governance from a revised PRD

Use **Audit** when:

- governance files already exist
- the user asks to review, compare, align, update, or check governance
- the user suspects the PRD, governance files, and codebase may have drifted apart

## Uncertainty Markers

Use explicit markers instead of vague "TBD" whenever possible:

- `[NEEDS PRD CLARIFICATION]` - the PRD is the intended source of truth, but it is incomplete or ambiguous
- `[NEEDS CODEBASE DISCOVERY]` - the answer depends on inspecting the actual repository
- `[USER DECISION REQUIRED]` - the choice is strategic or preference-based and should not be inferred
- `[GOVERNANCE DRIFT]` - the PRD, governance files, and current repository state disagree

## Priority Levels

When writing or updating governance, classify important rules using these levels where useful:

- **Critical** - cannot be violated without explicit user approval; use for security, compliance, and architectural boundaries
- **Required** - default operating rule; deviations need explanation
- **Advisory** - recommendation or preferred pattern; not blocking

Do not force priority tags onto every bullet. Use them where they clarify what truly matters.

## Workflow

### Step 0: Select Mode

Determine which mode applies before reading deeply:

- If the repo has a PRD and none of `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, `MEMORY.md` exist, default to **Generate**
- If one or more governance files already exist, default to **Audit** first
- If the user explicitly says "create", "bootstrap", or "generate", prefer **Generate**
- If the user explicitly says "review", "compare", "update", "merge", "check", or "audit", prefer **Audit**
- If both a PRD and governance files exist and the user wants refreshed docs, run **Audit** first, then propose **Update/Generate**

State the chosen mode briefly to the user before proceeding.

### Step 1: Determine Project Root and Existing Governance Files

Before doing anything else, determine the project root:

- If the user provided a PRD file path, the default project root is the PRD file's parent directory
- If the PRD path is deeply nested or appears to live in a documentation-only folder (for example `docs/prd/`, `planning/specs/`, or `.github/`), pause and confirm whether the parent directory is really the project root before writing files
- If the user says the PRD is stored outside the real project folder, ask them to explicitly confirm the target project root before continuing
- If the PRD was pasted inline instead of provided as a file, ask the user which folder should be treated as the project root before writing any files
- Assume one project = one folder. The PRD and governance files belong together in that folder unless the user explicitly says otherwise

Then check whether `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, or `MEMORY.md` already exist in that project root.

- **If none exist:** proceed normally
- **If any exist in Generate mode:** stop and tell the user which files were found; ask explicitly per file:
  - **Overwrite** - replace the file entirely
  - **Update/merge** - read the existing file and merge it with PRD and codebase findings
  - **Skip** - leave the file untouched
- **If any exist in Audit mode:** read them as current governance inputs and do not write anything yet

`MEMORY.md` deserves special caution: it contains accumulated project history that cannot be reconstructed from the PRD. Default to merge for `MEMORY.md` unless the user explicitly says overwrite.

Do not write any file without the user's explicit decision on how to handle existing files.

### Step 2: Read the PRD

Read the user's PRD file completely. Extract and note:

- **Project identity**: company name, product type, domain, goal
- **Tech stack**: framework, CMS, database, language, styling, hosting
- **Architecture**: deployment topology, key integrations, data flow
- **Content model**: collections, globals, blocks, relations
- **Security requirements**: encryption, auth, compliance (GDPR/DSGVO, industry-specific)
- **Compliance requirements**: legal, accessibility, SEO, consent
- **Environment variables**: what the app needs at runtime
- **Phase plan / milestones**: if the PRD defines implementation phases
- **Design tokens**: colors, fonts, design approach

If any of these are missing from the PRD, mark them with `[NEEDS PRD CLARIFICATION]` for the interview or audit summary.

### Step 3: Discover Current Repository Reality

Before generating or updating governance, inspect the repository to find what actually exists right now. Do not rely only on the PRD.

Check for the relevant build and deployment signals:

- `package.json`, workspace files, lockfiles
- `pyproject.toml`, `requirements.txt`, `uv.lock`, `poetry.lock`
- `go.mod`, `Cargo.toml`, `Makefile`
- Docker files, compose files, deployment manifests
- CI/CD config (`.github/workflows`, GitLab CI, etc.)
- app directories, test directories, scripts, infra folders
- existing governance or reference docs beyond the PRD

Use discovery to answer:

- Which stack choices are already real?
- Which commands are confirmed vs only planned?
- Which architectural assumptions from the PRD are already contradicted by the repo?
- Which governance statements need `[NEEDS CODEBASE DISCOVERY]` or `[GOVERNANCE DRIFT]` markers?

If the repo is empty or barely bootstrapped, say so explicitly. "No implementation yet" is a valid discovery result.

### Step 4: Interview the User

Before generating or updating anything, confirm key decisions that shape the governance files. Present what you extracted from the PRD and discovery step, then ask about anything missing, ambiguous, or contradictory.

Always ask these questions unless they were already answered clearly:

1. **Agent roles**: Who implements? Who reviews? How many review passes?
   - Default suggestion: Sonnet implements, Opus reviews (single pass)
   - For larger projects: suggest dual review (Opus = Reviewer A, Claude Code = Reviewer B)

2. **Git conventions**: Branch naming, commit format, main branch name?
   - Default suggestion: `issue-{number}-{short-description}`, conventional commits (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`)

3. **Task management**: GitHub Issues, Linear, Jira, or just `MEMORY.md`?
   - Default suggestion: GitHub Issues as source of truth

4. **Development commands**: What are the key dev/build/test/lint commands?
   - Check build config first and use actual script names where possible
   - If the repo is not yet bootstrapped, infer from the stack but mark commands as `# planned`

5. **Package manager / build tool**: npm, pnpm, yarn, bun, uv, poetry, cargo, go, make?
   - Default suggestion for Node.js: pnpm

6. **Automation**: Does the project use or plan CI/CD, auto-develop scripts, automated issue processing, or guardrail hooks?

7. **Reference documents**: Besides the PRD, are there setup guides, design specs, ADRs, or API docs that agents should consult?

8. **Security boundaries**: Any project-specific prohibited actions beyond the defaults?

9. **Language of governance files**: Default to English for Claude Code compatibility unless the user explicitly prefers another language

10. **Conflict resolution policy**: If PRD, governance, and codebase disagree, should the repo reality, PRD intent, or explicit user instruction win?
   - Default suggestion: user instruction > current working codebase > PRD > templates

Present the interview as a concise checklist, not a wall of text.

### Step 5: Generate or Update SOUL.md

`SOUL.md` is the project's identity document. It captures what is non-negotiable and should not change without explicit discussion.

Read `references/soul-template.md` for the structural blueprint. If the file is not available, use the section list below as the authoritative structure. Then generate or update `SOUL.md` with these sections:

1. **Identity** - one paragraph: what is being built, for whom, why
2. **Core Stack** - table of layer/technology pairs
3. **Architecture** - deployment diagram (text), key principles
4. **Content Model** - core collections and globals if applicable
5. **Design Tokens** - colors, fonts, design approach if applicable
6. **Coding Standards** - language/framework-specific rules distilled from PRD and repo reality
7. **Security Principles** - encryption, auth, secrets handling, input validation
8. **Compliance** - legal, accessibility, SEO requirements
9. **Reference Documents** - pointers to PRD and other key docs

Principles for writing `SOUL.md`:

- Target 60-90 lines; this is a distillation, not a copy of the PRD
- Every item should be something an agent needs before writing code
- Use concrete values (`#e6007a`, `AES-256-GCM`, `PostgreSQL 16`), not vague statements
- If the PRD doesn't specify something, do not invent it; leave it out or mark it with the correct uncertainty marker
- Use **Critical**, **Required**, and **Advisory** labels only where they help clarify non-negotiables

### Step 6: Generate or Update AGENTS.md

`AGENTS.md` governs how agents (human or AI) work in the repository. It is the behavioral contract.

Read `references/agents-template.md` for the structural blueprint. If the file is not available, use the section list below as the authoritative structure. Then generate or update `AGENTS.md` with these sections:

1. **Header** - "Read SOUL.md first" + "Read MEMORY.md next"
2. **Roles** - implementation model, reviewer(s), review requirements
3. **Repository Boundary** - stay inside project root, protected files list
4. **Current Reality** - honest statement of what exists vs what is planned
5. **Intended Project Structure** - target directory tree if known
6. **Workflow** - numbered steps from reading the task to delivery
7. **Review Rules** - what reviewers check, read-only default, minimum checklist
8. **Git Conventions** - branch naming, commit format, force-push policy
9. **Prohibited Actions** - filesystem, git, system, and security prohibitions
10. **Delivery Standard** - when is a task "done"?
11. **Phase Plan** - implementation phases from the PRD if defined

Principles for writing `AGENTS.md`:

- Be specific about prohibitions; vague rules get ignored
- The review section should be genuinely useful, not ceremonial
- If the project has automation (CI/CD, auto-develop scripts), add an "Auto-Develop Policy" section. See `references/agents-template.md` for a concrete example that addresses MEMORY.md bloat prevention, status line discipline, no-op fix detection, and review loop termination
- Keep it under 150 lines
- Mark especially important prohibitions or review gates as **Critical** or **Required** where useful

### Step 7: Generate or Update CLAUDE.md

`CLAUDE.md` is the Claude Code-specific configuration file. It tells Claude Code what tools to use, what commands to run, and how to behave in this specific repo.

Read `references/claude-template.md` for the structural blueprint. If the file is not available, use the section list below as the authoritative structure. Then generate or update `CLAUDE.md` with these sections:

1. **Header** - `@SOUL.md`, `@AGENTS.md`, `@MEMORY.md` references
2. **Role** - what Claude Code does in this repo
3. **Current Project State** - brief honest status
4. **Tool Preferences** - file tools vs shell, CLI tools to use
5. **Development Commands** - code block with dev/build/test/lint commands
6. **Working Rules** - read-before-write, update `MEMORY.md`, stay in project root
7. **Review Boundary** - reviewer roles, sync with `AGENTS.md`
8. **Environment Variables** - required and optional, with descriptions for non-obvious ones

Principles for writing `CLAUDE.md`:

- This is the file Claude Code reads on every conversation start; keep it focused
- The `@references` at the top automatically load the other governance files into context
- Development commands should be copy-pasteable; if inferred rather than confirmed, mark them `# planned`
- Environment variables should list what the app needs, not hosting setup instructions
- Keep it under 80 lines

### Step 8: Generate or Update MEMORY.md

`MEMORY.md` is the living state document. Unlike the other three files, it changes frequently and should reflect both project progress and governance drift.

Read `references/memory-template.md` and `references/completed-phases-template.md` for the structural blueprints. If the files are not available, use the section list below as the authoritative structure. Then generate or update `MEMORY.md` with these sections:

1. **Current State** - active phase, active milestone, coding model, review roles, known blockers
2. **Completed Work** - reference `memory/completed-phases.md`; put detailed completed-work entries in that archive, not inline. Record PRD creation and "Governance files drafted" in the archive only after the selected governance files were written successfully
3. **Key Decisions** - table with date/decision/choice; seed from the PRD and user interview
4. **Key Implementation Notes** - empty initially, filled during implementation
5. **Next Up** - first tasks to tackle
6. **Content Sources** - where content comes from if applicable
7. **Infrastructure** - servers, services, repos if known
8. **Governance Drift** - last governance audit date, open drift items, recently resolved drift items if applicable
9. **Update Rules** - how to maintain this file

Principles for writing `MEMORY.md`:

- Start minimal; aim for 40-60 lines initially
- Create `memory/completed-phases.md` as an archive alongside MEMORY.md by default. It may stay short for small projects, but it prevents context overflow for automated pipelines or projects with many tasks (20+) when MEMORY.md is injected into agent system prompts.
- If the project gitignores the `memory/` directory (e.g. for daily flush files), ensure `memory/completed-phases.md` is not excluded. Use a pattern like `memory/2026-*.md` instead of `memory/`.
- When generating MEMORY.md for an existing project that already has many completed entries, offer to split: move detailed entries to the archive, keep only a summary reference in MEMORY.md.
- For multi-phase projects, organize archived completed work in `memory/completed-phases.md` with `### Phase Name` subheadings.
- Seed only the top 5-7 most important decisions at creation time
- "Next Up" should be actionable
- Use absolute dates, not relative dates
- Never discard project history when merging
- See `references/memory-template.md` "Automation considerations" for guidance on MEMORY.md discipline in automated pipelines

### Step 9: Audit Mode

When operating in **Audit** mode, do not jump straight to rewriting files. First inspect for alignment.

Audit process:

1. Read the current `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, and `MEMORY.md`
2. Read the PRD if present
3. Inspect the actual repository structure and config
4. Identify mismatches in four categories:
   - **Missing** - governance says something should exist, but it does not
   - **Outdated** - governance reflects a past state that is no longer true
   - **Contradicted by codebase** - the repo clearly does something different
   - **Needs user decision** - the mismatch is strategic, not safely inferable
5. Tag significant conflicts with `[GOVERNANCE DRIFT]`
6. Present the audit findings before proposing edits
7. Only after user approval, update the selected governance files

Good audit targets include:

- role mismatches between `AGENTS.md` and `CLAUDE.md`
- commands in `CLAUDE.md` that do not exist in `package.json` or other build files
- phase plans that no longer reflect repository reality
- stack declarations in `SOUL.md` that the repo contradicts
- `MEMORY.md` current state or next steps that are stale
- MEMORY.md exceeding ~15,000 characters (suggest archive split)
- MEMORY.md containing inline completed issue entries instead of only the archive reference
- `memory/completed-phases.md` missing despite a MEMORY.md archive reference

### Step 10: Merge Strategy

When "Update/merge" is chosen, use this conflict resolution strategy:

- **Explicit user instruction** wins over everything else
- **Current, working repository reality** outranks a stale PRD
- **PRD intent** outranks generic template defaults
- **Existing governance additions** that reflect learned project rules should be preserved unless they conflict with the user's current direction

Per file:

- **SOUL.md / AGENTS.md**: The PRD is the intended architecture source, but do not overwrite proven repository reality blindly. If the codebase is clearly further along than the PRD, mark the conflict and ask the user whether to align governance to the repo or the PRD
- **CLAUDE.md**: Merge env vars as a union. Prefer build commands verified by real config files over PRD-inferred commands. Preserve custom working rules unless they are obsolete or contradicted
- **MEMORY.md**: Never delete project history, Key Decisions, Key Implementation Notes, or previously logged drift items. When applying an archive split, move historical Completed Work details and stable Key Decisions to `memory/completed-phases.md` instead of deleting them. Update Current State, Next Up, Infrastructure, and Governance Drift based on the latest known reality

If a conflict is strategic rather than factual, stop and mark it `[USER DECISION REQUIRED]`.

### Step 11: Present and Confirm

After generating or auditing the planned governance files, present a summary to the user:

1. Show the file count and total lines involved
2. Highlight any decisions you made that were not explicit in the PRD
3. List any items marked `[NEEDS PRD CLARIFICATION]`, `[NEEDS CODEBASE DISCOVERY]`, `[USER DECISION REQUIRED]`, or `[GOVERNANCE DRIFT]`
4. In Audit mode, summarize the drift categories and the most important mismatches
5. Ask whether the user wants adjustments before files are written
6. Ask for explicit approval to write the selected files into the project root

Only after explicit approval, write the selected files. Only after the write succeeds should the project memory record "Governance files drafted" or a governance audit/update entry, using `memory/completed-phases.md` for completed-work details. Do not commit; let the user review first.

## Adaptation Guidelines

Not every project needs every section. Use judgment:

- **No CMS?** -> skip Content Model in `SOUL.md`
- **Solo developer?** -> simplify `AGENTS.md` roles to "Claude implements, user reviews"
- **No compliance requirements?** -> skip Compliance in `SOUL.md`
- **Simple project?** -> `MEMORY.md` can start with just Current State, Next Up, and Governance Drift
- **No env vars?** -> skip Environment Variables in `CLAUDE.md`
- **Not TypeScript?** -> adjust Coding Standards to the actual language
- **Monorepo?** -> add workspace/package structure to Intended Project Structure
- **No PRD file, only an inline spec?** -> ask for explicit project root before writing anything
- **No implementation yet?** -> Generate mode should still work; just mark planned commands and unresolved areas honestly

The templates in `references/` are structural blueprints, not rigid forms. Adapt them to fit the project.

## Quality Checklist

Before presenting files or an audit report, verify:

- [ ] Mode selection was stated clearly (`Generate` vs `Audit`)
- [ ] Project root was derived from the PRD location or explicitly confirmed by the user
- [ ] Existing governance files were handled per user decision (`overwrite` / `merge` / `skip`) or treated as audit inputs
- [ ] The PRD was read completely if one was provided
- [ ] Repository reality was inspected before claiming commands, stack details, or phase status
- [ ] `SOUL.md` contains only non-negotiable principles, not implementation noise
- [ ] `AGENTS.md` prohibited actions are specific and enforceable
- [ ] `CLAUDE.md` `@references` point to the correct filenames
- [ ] `CLAUDE.md` commands match actual repo config, or are clearly marked `# planned`
- [ ] `MEMORY.md` preserves historical state and records governance drift where relevant
- [ ] No secrets, passwords, or API keys appear in any file
- [ ] All files are consistent with each other (same role names, same phase names, same repo status)
- [ ] Drift or conflicts are marked explicitly instead of being hidden or silently guessed away
- [ ] Governance overhead is proportional to project complexity
