# Product Context Template

Use this file to ground discovery output in your actual product and operating constraints.

## Product Summary

- Product name:
- One-sentence value proposition:
- Primary problem solved:

## Project Principles and Non-Negotiables

The skill loads this section as the project "constitution" — the governing
principles that every spec, decision, and implementation must respect. List
each principle with a one-line definition and a one-line rationale. Treat
principles as binding constraints, not aspirations.

| # | Principle | Definition | Why It's Non-Negotiable |
|---|-----------|------------|-------------------------|
| 1 | <principle name> | <one-line definition> | <why this cannot be compromised> |
| 2 | <principle name> | <one-line definition> | <why this cannot be compromised> |

Common principle categories to consider (use only those that apply):

- **Code quality** (e.g., type safety, lint cleanliness, no unused exports)
- **Testing standards** (e.g., new code requires tests, coverage thresholds)
- **User experience** (e.g., never lose user input, every action has feedback)
- **Performance** (e.g., specific budgets for hot paths)
- **Security** (e.g., least-privilege, no secrets in client code)
- **Reliability** (e.g., offline-tolerant, no data loss during failure)
- **Accessibility** (e.g., WCAG level, keyboard parity)
- **Privacy and compliance** (e.g., data residency, retention limits)
- **Operability** (e.g., every feature must be observable and rollback-able)

Principles drive decisions during:

- Discovery (a principle violation surfaces as a Risk or Open Question)
- Spec writing (a principle violation blocks the Readiness Gate)
- Eng Review (engineers cite principles when challenging trade-offs)
- Audit Loop (principle violations are high-severity audit entries)

## Users and Buyers

- Primary user personas:
- Economic buyer(s):
- Success criteria by persona:

## Domain Model (Core Entities)

- Entity:
  - Purpose:
  - Key fields:
  - Relationships:

## Platforms and Runtime Contexts

- Client platforms:
- Service/API boundaries:
- Critical integrations:

## Reliability and Operational Expectations

- Availability/SLO targets:
- Data durability requirements:
- Expected scale profile:

## Security and Compliance Constraints

- Relevant policies/standards:
- Sensitive data classes:
- Access control requirements:

## Delivery Constraints

- Team constraints:
- Timeline constraints:
- Adoption or migration constraints:

## Known Risks and Open Questions

- Known risks:
- Open questions:
