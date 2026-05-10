# Feature README Template

Use as `requirements/<FeatureName>/README.md`.

```markdown
# <FeatureName> — Tagline

> Optional motto or guiding quote.

## Vision
2–3 sentences. What is this? What problem does it solve? What does it replace?

## Our Role
One sentence on the team's posture (builder, facilitator, platform, etc.).

## Status
Current phase indicator with link to the latest discovery doc.

## Product Principles
3–5 bullets. Non-negotiable philosophical guardrails.
Format: **Principle name.** One sentence explanation.

## Key Concepts (Glossary)
A table of domain terms used precisely throughout the feature. This is critical for agent
consumption — agents need unambiguous terminology to generate correct code, variable names,
API endpoints, and UI copy.

| Term | Definition |
|------|------------|
| **Term** | What it means in this product's context |

## User Personas (Summary)
Brief table referencing the full personas in `discovery/00-personas.md`.

## Document Structure
ASCII tree of the folder with one-line annotations.

## Open Question ID Reference
Table mapping OQ prefixes to their discovery files (e.g., `TF` → `discovery/01-transaction-flow.md`).

## Open Discovery Areas
Checklist of the highest-priority unresolved questions with OQ IDs.

## Key Cross-References
List of tightly coupled OQs across files. These are decisions that cannot be made
in isolation — resolving one affects the others.
```
