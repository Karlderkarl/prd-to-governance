# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.2.0] - 2026-06-19

### Added

- Producer side of the `governance-to-automation` contract in the templates, so the successor pipeline can be driven from governance instead of local-only configuration. All surfaces are fail-safe — omitting them keeps the pipeline a no-op:
  - AGENTS.md *Skill Policy* section (`agents-template.md`) that seeds the pipeline's `SKILL_MAP` with explicit `label:`/`title:` → skill matchers for deterministic per-task skill routing.
  - `TEST_POLICY` / `TEST_ELIGIBILITY` test-discipline fields inside the *Auto-Develop Policy* example (`agents-template.md`).
  - `TARGETED_TEST_CMD` with a literal `{TARGET}` token in CLAUDE.md *Development Commands* (`claude-template.md`).
- `[NEEDS GOVERNANCE]` uncertainty marker in `SKILL.md`, shared vocabulary with `governance-to-automation`, scoped to partial or missing downstream-automation contracts.
- Audit targets in `SKILL.md` that detect governance-side drift in the new contract fields (malformed or ambiguous matchers, `TEST_POLICY=required` without `TARGETED_TEST_CMD`, partial/contradictory test policy, missing `{TARGET}` token, and `label:` matchers on label-less task sources).
- Interview follow-ups plus generation and quality-checklist guidance for the new contract fields in `SKILL.md`.

### Changed

- `README.md` and the root `CLAUDE.md` now describe the producer side of the governance→automation contract and the new marker.

## [1.0.1] - 2026-06-15

### Changed

- Moved the skill to `skills/prd-to-governance/SKILL.md` for skills.sh discovery (was `prd-to-governance/SKILL.md` in 1.0.0).
- Restructured the repository for skills.sh publishing and rewrote the README to match.
- `README.md` now states `memory/completed-phases.md` is created by default and lists drift notes in the MEMORY.md summary.
- `agents-template.md` now also protects `CLAUDE.md` and gives `MEMORY.md` a narrower, field-scoped update exception.
- Clarified the MEMORY.md archive-split threshold (~15,000 characters) as a buffer below the ~20,000-character context injection limit in `SKILL.md`.

### Added

- README and SKILL.md pointer to the recommended successor skill `Karlderkarl/governance-to-automation`.
- `Governance Drift` section in `memory-template.md` to match the MEMORY.md structure mandated by `SKILL.md`.

### Fixed

- Moved the release version from the unsupported top-level `version` frontmatter key into `metadata.version` so `SKILL.md` passes skill validation. Allowed top-level keys are `name`, `description`, `license`, `allowed-tools`, and `metadata`.

## [1.0.0] - 2026-06-13

### Added

- Initial public release of the `prd-to-governance` Claude Code skill.
- Skill packaging via `prd-to-governance/SKILL.md` with reference templates for `SOUL.md`, `AGENTS.md`, `CLAUDE.md`, `MEMORY.md`, and `memory/completed-phases.md`.
- MIT `LICENSE` for GitHub-recognized repository licensing.
- `SECURITY.md` with GitHub Private Vulnerability Reporting as the disclosure path.

### Changed

- Pinned the skill frontmatter version to `1.0.0` for tagged release delivery.

