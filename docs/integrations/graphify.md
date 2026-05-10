# Optional Integration: Graphify

[Graphify](https://github.com/safishamsi/graphify) is an AI coding assistant
skill that turns a folder of code, docs, PDFs, images, and videos into a
queryable knowledge graph. It produces three artifacts in `graphify-out/`:

- `graph.json` — the full knowledge graph
- `GRAPH_REPORT.md` — Markdown summary: god nodes, surprising connections, the "why" (NOTE/WHY/HACK comments and design rationale), suggested questions, confidence tags
- `graph.html` — interactive browser-based visualization

This integration is **optional**. Agent Disco runs perfectly without it. If a
project already has graphify installed, the skill should use the graph to
ground its codebase analysis instead of grepping blind.

## When to use Graphify with Agent Disco

Use it before **Step 6 (Ground in Code)** when the feature touches an existing
codebase that is large, unfamiliar, or has significant adjacent code that runs
in the same lifecycle. Specifically, graphify accelerates these spec sections:

- **Contract Definitions** — identifies real types, interfaces, and patterns to extend
- **Codebase Context** — surfaces "god nodes" the feature will inevitably touch
- **Adjacent Code Health** — exposes pre-existing fragilities in code that runs alongside the feature
- **Component Reuse Audit** — maps existing components and dependencies behind them

Graphify is not useful when:

- The feature is greenfield (no codebase yet)
- The feature is purely product/UX with no engineering grounding
- The codebase is small enough to read directly

## Setup (once per project)

```bash
uv tool install graphifyy && graphify install
# or
pipx install graphifyy && graphify install
# or
pip install graphifyy && graphify install
```

Then build the graph for the project:

```bash
graphify .
```

This produces `graphify-out/` containing `graph.json`, `GRAPH_REPORT.md`, and
`graph.html`. Commit `graphify-out/` to version control so the whole team
starts with the same map. Recommended `.gitignore` additions:

```
graphify-out/manifest.json
graphify-out/cost.json
```

For team-wide auto-rebuild on commit:

```bash
graphify hook install
```

## How Disco uses the graph

When `graphify-out/GRAPH_REPORT.md` and `graphify-out/graph.json` exist, the
skill should:

1. **Read `GRAPH_REPORT.md` before Step 6.** Treat it as the high-level map of
   the codebase. Ground all subsequent codebase claims against it.
2. **Use the graph for Codebase Context.** Pull god nodes and module-level
   relationships into the spec's Codebase Context section. Cite real file
   paths from the graph — never placeholders.
3. **Use surprising connections for Adjacent Code Health.** Graphify's
   "surprising connections" surface cross-module dependencies that are exactly
   the kind of pre-existing fragility the spec must document.
4. **Use the graph for Component Reuse Audit.** Walk component nodes and their
   neighbors to identify reuse candidates and design system deviations.
5. **Use queries for targeted grounding.** When a specific question arises
   during spec writing, prefer:

   ```bash
   graphify query "what connects FeatureName to the auth subsystem?"
   graphify path "ComponentA" "ServiceB"
   graphify explain "GodNodeName"
   ```

   over ad-hoc searches.

## Confidence tagging

Every relationship in the graph is tagged `EXTRACTED`, `INFERRED`, or
`AMBIGUOUS`. Carry these tags forward into the spec:

- `EXTRACTED` — high confidence. Cite directly in Contract Definitions / Codebase Context.
- `INFERRED` — agent-derived. Treat like `⚠️ Assumed` in the Agent Disco workflow until validated by the engineer at Step 8.
- `AMBIGUOUS` — flagged for human review. Add as an explicit Open Question or Risk.

This mirrors Agent Disco's existing `⚠️ Assumed` → `✅ Validated` lifecycle.

## What graphify does NOT replace

- It does not replace Step 6's spec-writing work. It feeds Step 6, but the
  spec author still produces Contract Definitions, executable failing tests,
  and the Test Generation Brief.
- It does not replace the **Component Reuse Audit** judgment calls. The graph
  surfaces candidates; Barry (or Disco when Barry is not active) still
  decides reuse strategies and design system deviations.
- It does not replace the **Readiness Gate**. The gate still verifies
  Contract Definitions cite real files, Failing Tests are executable, etc.

## File layout when both are in use

```text
<project root>/
├── graphify-out/
│   ├── graph.json
│   ├── GRAPH_REPORT.md
│   └── graph.html
└── requirements/
    └── <FeatureName>/
        ├── README.md
        ├── dashboard.html
        ├── discovery/
        ├── specs/SPEC-001.md          ← Codebase Context cites graphify findings
        ├── decisions/
        └── audit/
```

The two outputs are complementary:

- `graphify-out/` is **codebase-wide** and rebuilt on commit.
- `requirements/<FeatureName>/` is **feature-scoped** and committed alongside the feature work.

## References

- Graphify GitHub: https://github.com/safishamsi/graphify
- Graphify package on PyPI: `graphifyy` (note the double `y`)
