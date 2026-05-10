# Changelog

All notable changes to Agent Disco are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added — Branding

- `logo/agent-disco.png` — project logo (light wordmark on transparent
  background, used in dark themes).
- `logo/agent-disco-dark.png` *(pending export)* — dark wordmark variant for
  light themes. Both `README.md` and `activation/claude-code/load-disco.md`
  already reference it inside a `<picture>` block; the inner `<img>` fallback
  keeps things rendering until the dark variant lands.
- Theme-aware logo rendering via `<picture>` + `prefers-color-scheme` in
  `README.md` and in Disco's startup greeting, so the logo stays legible on
  both light and dark backgrounds (GitHub themes, Claude Code surfaces,
  third-party markdown renderers).
- `.gitignore` excludes the Affinity Designer source file
  (`logo/agent-disco.af`); the exported PNGs are the committed assets.

### Added — Spec-Kit-Inspired Concepts

Borrowed six concepts from [github/spec-kit](https://github.com/github/spec-kit)
without bundling the methodology, tightening prose-level rigor and traceability:

- **Project Principles & Non-Negotiables** section in
  `templates/product-context.md` — a project "constitution" of binding
  constraints (code quality, testing, UX, security, reliability, etc.) that
  governs spec writing, Eng Review, and the Readiness Gate.
- **Clarifications** section in `templates/spec-template.md` — structured Q&A
  log (date, OQ ID, question, answer, asked-by, source) so vague intake
  language is graduated into precise spec language with full traceability.
- **Cross-Artifact Consistency** sub-section in the Step 8b Readiness Gate
  (`SKILL.md`) — explicit 1:1 mapping checks across AC ↔ Tests, FR ↔ AC,
  Edge Cases ↔ Failure Mode Tests, State Matrix ↔ Tests, Branches ↔ Tests,
  Contracts ↔ Failing Tests, Figma ↔ Component Map, persona references,
  decisions cited, and constitution alignment.
- **Parallel column** in the Test Boundary Map (`templates/spec-template.md`)
  — `[P]` markers identify tests safe to run independently so the
  implementing agent can fan out across parallel runners or sub-agents.
- **Spec Quality Checklist** ("unit tests for English") in
  `templates/spec-template.md` — prose-level checklist covering requirement
  phrasing, persona specificity, coverage and consistency, terminology
  hygiene, and decision traceability. Must pass before the Readiness Gate.
- **Memory Loading Order** section in `SKILL.md` — formalized 6-layer load
  sequence (Constitution → Product Context → Features → Feature Audits →
  Project Audits → Graphify) with explicit roles and behavior on absence.

### Changed

- **Default persona is now Disco only.** Barry is no longer auto-loaded. The
  Business Analyst persona has been abstracted into its own activation file
  and is brought in on demand via "Activate Barry".

### Added

- `docs/integrations/graphify.md` — optional integration with [Graphify](https://github.com/safishamsi/graphify)
  for deep codebase context. Recommended companion for Step 6 (Ground in Code).
  Maps graphify's `EXTRACTED` / `INFERRED` / `AMBIGUOUS` confidence tags onto
  Agent Disco's `⚠️ Assumed` → `✅ Validated` lifecycle.
- Step 6 in `SKILL.md` now checks for `graphify-out/` and uses the graph to
  ground Codebase Context, Adjacent Code Health, and Component Reuse Audit
  when present. No behavior change for projects without graphify.
- `activation/claude-code/barry-persona.md` — full Barry persona definition,
  ownership rules, and operating rules.
- `activation/claude-code/activate-barry.md` — on-demand activation/deactivation
  triggers for Barry.
- Comprehensive `SKILL.md` covering personas, guided intake, cheap answer detection,
  scaffolding, OQ namespacing, Step 6 grounding, Step 8 review pillars, Readiness Gate,
  Engineering Audit Loop, Review Audits, and operating rules.
- Long-form artifact templates:
  - `templates/readme-template.md`
  - `templates/discovery-file-template.md`
  - `templates/personas-template.md`
  - `templates/spec-template.md` (Acceptance Criteria, State Combination Matrix,
    Contract Definitions, Test Specification, Failing Tests, Failure Mode Tests,
    Risk Assessment, Codebase Context, Adjacent Code Health, Component Reuse Audit
    with Figma-to-Component Map, Test Generation Brief)
  - `templates/adr-template.md`
  - `templates/audit-template.md`
  - `templates/dashboard-guide.md`
- Claude activation artifacts for Simon:
  - `activation/claude-code/activate-simon.md`
  - `activation/claude-code/simon-persona.md`
- Persistent local industry context template at `.claude/context/industry-context.md`.

### Changed

- Reframed the project as a public, industry-neutral discovery skill bundle.
- Renamed the Business Analyst persona to **Barry**.
- Removed all company/product/domain-specific references from core docs.

## [2.1.0] - 2026-05-10

### Added

- Initial public-ready structure with `README.md`, `CONTRIBUTING.md`, `SKILL.md`,
  and `templates/` for reusable context and capability inputs.

[Unreleased]: https://example.com/agent-disco/compare/v2.1.0...HEAD
[2.1.0]: https://example.com/agent-disco/releases/tag/v2.1.0
