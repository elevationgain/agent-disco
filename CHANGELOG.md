# Changelog

All notable changes to Agent Disco are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

This file is the running source of truth for commit and release notes. Every
change since the last released tag should be appended under `[Unreleased]`.
When cutting a release, rename `[Unreleased]` to the new version + date and
open a new empty `[Unreleased]` section above it.

## [Unreleased]

### Added

- **Bundled dashboard skill at `dashboard/`.** Generalized subset of an
  internal dashboard design system, repackaged as a public companion to
  Agent Disco. Adopters now get a working visual implementation out of the
  box rather than only a content guide.
  - `dashboard/SKILL.md` — theme selection, architecture, palettes, build
    workflow, output conventions tied to
    `requirements/<FeatureName>/dashboard.html`.
  - `dashboard/BEST_PRACTICES.md` — accumulated lessons (theme choice,
    sidebar behavior, hero architecture, card system, typography scale,
    Chart.js integration, common mistakes per theme, pre-share checklist).
  - `dashboard/reference/dashboard.html` — single-file Dark Indigo
    reference dashboard with sidebar, hero, stat cards, and section
    skeletons. Copy as the starting point for new builds.
  - `dashboard/theme/coral_pulse/DESIGN.md` — light editorial theme.
    Recommended for discovery dashboards (personas, decisions, open
    questions). Tailwind CDN + Plus Jakarta Sans + Manrope.
  - `dashboard/theme/dark_indigo/DESIGN.md` — dark data-focused theme.
    Recommended for metrics, audit reports, and chart-heavy dashboards.
    Plain CSS custom properties + Chart.js integration guide + complete
    HTML skeleton.
- **New `## Dashboard` section in `README.md`** explaining the
  stakeholder value (status at a glance, visual hierarchy, zero tooling,
  always in sync) and the four-step build workflow (pick a theme, copy
  the reference, follow the content guide, read the lessons).

### Changed

- **`templates/dashboard-guide.md` reframed as content-focused.** Now
  cross-references the bundled `dashboard/` skill for visual
  implementation, and points to Coral Pulse / Dark Indigo as the
  theme-aware path. Continues to own the "what content goes in a
  discovery dashboard" question independent of theme choice.
- **`SKILL.md` cross-references `dashboard/`** alongside
  `templates/dashboard-guide.md` in both the per-feature folder structure
  block and the templates list.
- **`README.md` Repository Layout extended** with five new rows
  (`dashboard/SKILL.md`, `BEST_PRACTICES.md`, `reference/dashboard.html`,
  and the two `theme/*/DESIGN.md` files); existing
  `templates/dashboard-guide.md` row reworded to clarify its
  content-focused role.

### Notes on the bundled subset

- Examples folder (`examples/rev3_*`) and screenshots from the source
  skill were intentionally not included. The reference dashboard plus the
  two theme `DESIGN.md` files cover the same patterns at a level public
  adopters can build from.

- **New phase: Plan.** Agent Disco now formalizes a Plan step between the
  Readiness Gate and Test/Code, parallel to native plan modes in popular AI
  coding agents (Cursor Plan, Claude Code `/plan`, spec-kit `/tasks`).
  - `SKILL.md` Process Flow gains step **8c. PLAN** between 8b (Readiness
    Gate) and 9 (Write Tests).
  - New section **`## Step 8c — Plan (executable task plan)`** in `SKILL.md`
    covering content requirements (spec references, file paths, `[P]`
    inheritance, sequencing), the four-check approval gate (Coverage, Scope,
    Sequencing, Blast Radius), Plan storage convention
    (`requirements/<FeatureName>/specs/SPEC-NNN/PLAN.md`), and the
    plan-time audit trigger.
- **`SKILL.md` frontmatter description** updated to reflect the current
  skill surface: now lists both Barry and Simon as optional support
  personas, adds new triggers (`"Activate Barry"`, `"Activate Simon"`,
  `"task plan"`, `"semantic map"`), and is reformatted as a YAML folded
  scalar (`>`) so the source is human-readable instead of one
  600-character line. Still parses as a single logical string for skill
  loaders.

- **`## Context-Aware Product Discovery` section in `README.md`** explaining
  the project's premise: adding features to large existing codebases is a
  context problem, and Agent Disco's answer is semantic maps. Section frames
  the two common bad workarounds (full repo access, agentic search), defines
  semantic maps, and inventories the maps the skill produces, grouped by
  lifecycle:
  - **Project-level** — Product Context (incl. Project Principles), Feature
    Inventory.
  - **Feature-level** — Persona Map, Decision Map (ADRs), Audit Map.
  - **Spec-level** — Feature Codebase Map, Feature Contracts, Component
    Reuse Map, Adjacent Code Health Map, State Combination Matrix, Branch
    Enumeration, Test Boundary Map.
  - Closes with a token-economics sub-section comparing semantic maps
    (~15–30k tokens for a populated spec) to full-repo access (an order of
    magnitude more) and a pointer to the Graphify integration for projects
    that want graph-grounded map generation.
- `logo/agent-disco.svg` — vector logo, white wordmark for dark themes.
- `logo/agent-disco-light.svg` — vector logo, black wordmark for light themes.

### Changed

- **`README.md` restructured around workflow phases** instead of the file
  inventory and persona list. What-the-skill-does is now above
  what-files-it-contains. Specifics:
  - Workflow Mermaid expanded to six phases:
    `Discovery → Spec → Plan → Test → Code → Audit`. Replaces the previous
    four-phase diagram. Adds the Plan phase (new) and surfaces the Audit
    Loop callout that was previously only in `SKILL.md`. The
    `Audit → Discovery` feedback edge was deliberately omitted from the
    diagram (it caused the Mermaid layout engine to shift Discovery off the
    far left in GitHub's renderer); the feedback relationship is described
    in prose under the diagram and detailed in `## Audit Loop`.
  - New per-phase top-level sections — `## Discovery`, `## Spec`, `## Plan`,
    `## Test`, `## Code`, `## Audit Loop` — each with a one-line role,
    what-happens prose, artifacts produced, and exit criteria. Core concepts
    (Guided Intake, Cheap Answer Detection, OQ Namespacing, Readiness Gate,
    Cross-Artifact Consistency, Spec Quality Checklist, Adjacent Code
    Health, Component Reuse Audit, etc.) are distributed into the phase
    they belong to.
  - Personas section moved out of the lead and below the workflow phases.
    The lead is now the value proposition, the diagram, and the phase
    walkthrough — what a PM lands on first.
  - `What's In Here` renamed to `Repository Layout` and moved to the
    bottom. Useful reference, not lede.
  - Old `Core Concepts` and `Discovery Output Structure` sections removed;
    their content now lives inside the per-phase sections.
- **Sub-description tightened** from "Structured product discovery and
  specification skill for AI-assisted delivery teams." to **"Structured
  product discovery for AI-native delivery teams."** — drops the
  self-referential "skill" noun and adopts AI-native framing (built around
  AI as a first-class engineer rather than retrofitted with AI tooling).
- **Logo rendering switched from PNG to SVG** in both `README.md` and
  `activation/claude-code/load-disco.md`. The `<picture>` block now selects
  between `agent-disco.svg` (dark theme) and `agent-disco-light.svg` (light
  theme); the `<img>` fallback uses `agent-disco-light.svg` so it stays
  legible on the most common default backgrounds. Display width bumped from
  320px to 480px so the new "PRODUCT DISCOVERY AGENT" subtitle and orbital
  icon stay legible at the wider 3.4:1 aspect ratio.
- **Persona listing in `README.md` now includes Simon** alongside Barry as
  on-demand support personas. The Repository Layout table references
  Simon's activation files (`activation/claude-code/activate-simon.md`,
  `activation/claude-code/simon-persona.md`), the persistent industry
  context file at `.claude/context/industry-context.md`, the SVG logo
  variants, and notes the PNG as a raster fallback rather than the primary
  asset.
- `.gitignore` now also excludes Affinity Designer lockfiles
  (`logo/*.af~lock~`) in addition to the `.af` source.

## [0.1.0] - 2026-05-10

Initial public release. First commit pushed to
[github.com/elevationgain/agent-disco](https://github.com/elevationgain/agent-disco).

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

### Added

- Comprehensive `SKILL.md` covering personas, Guided Intake, Cheap Answer
  Detection, scaffolding, OQ namespacing, Step 6 grounding, Step 8 review
  pillars, Readiness Gate, Engineering Audit Loop, Review Audits, and
  operating rules.
- `docs/integrations/graphify.md` — optional integration with
  [Graphify](https://github.com/safishamsi/graphify) for deep codebase
  context. Recommended companion for Step 6 (Ground in Code). Maps graphify's
  `EXTRACTED` / `INFERRED` / `AMBIGUOUS` confidence tags onto Agent Disco's
  `⚠️ Assumed` → `✅ Validated` lifecycle.
- Step 6 in `SKILL.md` checks for `graphify-out/` and uses the graph to
  ground Codebase Context, Adjacent Code Health, and Component Reuse Audit
  when present. No behavior change for projects without graphify.
- Disco persona (default) defined inline in `SKILL.md`.
- Barry persona artifacts: `activation/claude-code/barry-persona.md` and
  `activation/claude-code/activate-barry.md` (on-demand support persona).
- Simon persona artifacts: `activation/claude-code/simon-persona.md` and
  `activation/claude-code/activate-simon.md` (on-demand Director of
  Engineering / Solution Architect persona). Reuses a local industry context
  file at `.claude/context/industry-context.md` across sessions.
- Persistent local industry context template at
  `.claude/context/industry-context.md`.
- Long-form artifact templates:
  - `templates/product-context.md`
  - `templates/product-features.md`
  - `templates/readme-template.md`
  - `templates/discovery-file-template.md`
  - `templates/personas-template.md`
  - `templates/spec-template.md` (Acceptance Criteria, State Combination
    Matrix, Contract Definitions, Test Specification, Failing Tests, Failure
    Mode Tests, Risk Assessment, Codebase Context, Adjacent Code Health,
    Component Reuse Audit with Figma-to-Component Map, Test Generation Brief)
  - `templates/adr-template.md`
  - `templates/audit-template.md`
  - `templates/dashboard-guide.md`
- Public-ready repository scaffolding: `README.md`, `CONTRIBUTING.md`,
  `LICENSE` (MIT), `.gitignore`.

### Changed

- Reframed the project as a public, industry-neutral discovery skill bundle.
- **Default persona is Disco only.** Barry is no longer auto-loaded. Both
  Barry and Simon are abstracted into separate activation files and brought
  in on demand via "Activate Barry" / "Activate Simon".
- Renamed the Business Analyst persona to **Barry**.
- Removed all company/product/domain-specific references from core docs.

[Unreleased]: https://github.com/elevationgain/agent-disco/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/elevationgain/agent-disco/releases/tag/v0.1.0
