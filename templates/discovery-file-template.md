# Discovery File Template

Use as `requirements/<FeatureName>/discovery/NN-<area>.md`.

```markdown
# Discovery: <Area Name>

> One-sentence framing question for this area.

## Status: 🔴 Open | 🟡 Direction set | 🟢 Resolved

## What We Know
Bulleted list of established facts and decisions. Be precise — these are the
constraints agents and engineers will treat as ground truth. Tag any bullet
the AI inferred (rather than the user stating) with `⚠️ Assumed`. Remove the
tag when the user validates the assertion.

## <Draft Model / Flow / Structure>
A concrete sketch of how this area works. Could be:

- An ASCII flow diagram
- A state machine (especially valuable for anything with lifecycle/transitions)
- A data structure tree
- A draft field list
- A wireframe description

Make the current thinking tangible, even if it is wrong. Something concrete to
react to is more productive than abstract discussion.

## Open Questions

### <PREFIX>-01: <Question Title>

<Context paragraph explaining why this question matters.>

| Option | Description | Trade-offs |
|--------|-------------|------------|
| **Option A** | What it means | Pros and cons |
| **Option B** | What it means | Pros and cons |

**Leaning toward:** <Current instinct and reasoning, even if tentative.
Write "No strong lean yet" if genuinely open. This field helps agents make
reasonable assumptions during prototyping and prevents analysis paralysis.
Reference personas where relevant.>

---

### <PREFIX>-02: <Next Question>
...

## Decisions Made

Bulleted list of resolved questions with ✅ marks. When a decision is made,
move the OQ content into this section and create a corresponding ADR in
`/decisions` if the decision is significant.
```
