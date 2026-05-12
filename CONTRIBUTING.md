# Contributing to Agent Disco

Contributions are welcome when they make discovery outputs clearer, more complete, and more useful for both humans and coding agents.

Maintained by Bryan "Jules" Richard ([@elevationgain](https://github.com/elevationgain)). For substantive changes, please open an issue first to discuss direction so we don't build past each other; smaller fixes (typos, broken links, clarifications) can go straight to a PR.

## What This Project Is

- A reusable skill for structured product discovery
- A persona-driven workflow with durable artifact templates
- A portable system that runs in any repo with Markdown files

## What Good Contributions Look Like

- Clarify ambiguous instructions in `SKILL.md`
- Improve template fields so outputs are more testable
- Add stronger readiness checks for implementation handoff
- Add portable activation patterns for additional agent runtimes
- Improve docs with concrete examples from real usage

## Persona Policy

The default persona for this skill is:

- **Disco** (Senior Product Manager)

An optional, on-demand support persona is available:

- **Barry** (Senior Business Analyst) — activated only when the user explicitly requests it

If proposing persona changes, include concrete failure examples from current behavior and explain how the change improves output quality. New personas should follow the same on-demand activation pattern as Barry, not be added to the default load.

## Contribution Guidelines

1. Describe the current failure mode.
2. Show the expected improved output.
3. Keep naming and terminology consistent.
4. Keep content product-agnostic in core files.
5. Update `CHANGELOG.md` under `[Unreleased]`.

## Scope Rules

- Keep core artifacts as Markdown and single-file HTML.
- Avoid vendor lock-in in core skill behavior.
- Prefer small, focused pull requests.

## Versioning

This project follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

- Patch: clarifications and non-behavioral fixes
- Minor: additive workflow/template capability
- Major: breaking workflow, naming, or activation changes
