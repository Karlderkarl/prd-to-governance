# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

