# Personas Template

Use as `requirements/<FeatureName>/discovery/00-personas.md`. Personas inform every
other discovery area, so this file always exists and is loaded first.

3–4 personas is usually right. More than 5 means the team has not prioritized.
Use human names or evocative archetypes ("The Side-Hustler", "The Regional Lead")
— these stick in stakeholders' heads better than "Persona A".

```markdown
# Discovery: User Personas

> Who uses this product, and how do their needs differ?

## Persona 1: <Name / Archetype> ⚠️ Assumed | ✅ Validated

**Profile:** One sentence on who they are.
**Motivation:** Why they would use this product.
**Behavior:** How they would use it differently from other personas.
**Pain tolerance:** How much friction or complexity they will accept.

## Persona 2: ...

## Implications for Product Decisions

A table mapping key decision areas to how each persona would answer them.
This makes persona-aware trade-offs visible at a glance.

| Decision area | Persona 1 | Persona 2 | Persona 3 |
|---------------|-----------|-----------|-----------|
| Default workflow | <implication> | <implication> | <implication> |
| Tolerance for setup | <implication> | <implication> | <implication> |
| Failure expectations | <implication> | <implication> | <implication> |
```

## Validation Lifecycle

Every persona the AI infers starts as `⚠️ Assumed`. The tag sits next to the
persona name so it is visible in every reference. When the user confirms the
persona is grounded in real research, the tag changes to `✅ Validated`.

Personas that stay assumed after the intake phase are honest about their
provenance — they are the AI's best guess, not research findings. This prevents
the team from building features for personas nobody has actually talked to.
