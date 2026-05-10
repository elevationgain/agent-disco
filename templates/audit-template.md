# Audit Templates

Use within `requirements/<FeatureName>/audit/`. Two artifacts:

1. One file per audit-producing session: `audit/YYYY-MM-DD-<session>.md`
2. The feature's audit index and Lessons Learned: `audit/README.md`

## Audit Entry Schema

Each audit entry is a section in a session log file. The AI populates all fields;
the engineer just confirms.

```markdown
### AUDIT-<NNN>: <Short title>

**Date:** YYYY-MM-DD
**Session:** Plan | Review | Implementation
**Engineer:** <Name or "unattributed">
**Source:** <discovery file, spec, decision, or component audit that was wrong/incomplete>
**Category:** Wrong | Gap | Override
**Severity:** High | Medium | Low

#### What Disco Said
<The specific claim from the source file, quoted or paraphrased>

#### What the Engineer Corrected
<The correction, in the engineer's words>

#### Why
<The reasoning — why the original claim was wrong or incomplete. This is the most
valuable field for skill improvement. Capture the engineer's reasoning, not just the delta.>

#### Skill Implication
<AI-generated hypothesis: what should the skill or templates do differently to avoid
this in future features? The hypothesis seeds the review process.>
```

## audit/README.md Format

```markdown
# Audit — <FeatureName>

## Lessons Learned

> Distilled from audit entries. Updated after each audit review session.
> This section is loaded as context for future discovery sessions.

- <Lesson 1: concrete, actionable statement about what to do differently>
- <Lesson 2: ...>

## Audit Index

| ID | Date | Category | Severity | Source | Title |
|----|------|----------|----------|--------|-------|
| AUDIT-001 | 2026-04-08 | Wrong | High | discovery/02-area.md | <title> |
| AUDIT-002 | 2026-04-08 | Gap | Medium | specs/SPEC-001.md | <title> |

## Patterns

> Updated during "Review Audits" analysis. Tracks recurring themes.

| Pattern | Frequency | Affected Skill Section | Status |
|---------|-----------|------------------------|--------|
| <pattern> | <count across features> | <section> | Open / Fixed in vX.Y.Z |
```

## What the Audit Loop Does NOT Do

- It does not track project management items (deadlines, resource conflicts, process breakdowns).
- It does not replace Eng Review (Step 8). Eng Review evaluates the spec before implementation; audits capture corrections during and after implementation.
- It does not automatically modify the skill file. "Review Audits" proposes; a human approves.
- It does not require engineer participation. If the engineer declines or says "stop asking", the loop pauses for the session.
