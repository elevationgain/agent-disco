# Dashboard Guide

Every product discovery may produce a single-file HTML dashboard alongside the
markdown files. The dashboard is a visual snapshot of the discovery state — the
same information as the markdown, but a stakeholder can open it in a browser and
scan it in 30 seconds.

The dashboard is **optional but recommended**. It is not a build artifact; it is
a single `.html` file that opens directly in any browser. No build step, no
server, no proprietary format.

This guide describes **what** a discovery dashboard contains and when to update
it. For **how** to build a polished one, use the bundled dashboard skill at
[`dashboard/`](../dashboard/) — it ships two themes (Coral Pulse for editorial
discovery dashboards, Dark Indigo for data dashboards), a reference HTML
skeleton, design tokens, and accumulated best practices.

## When the Dashboard Gets Created

The dashboard is created during the **SCAFFOLD** step (Process Flow step 2),
alongside the folder structure and skeleton markdown files. The initial
dashboard contains:

- A hero section with the project name, tagline, and status badge
- Empty metric cards (0 open, 0 direction set, 0 resolved)
- Placeholder sections for personas and open questions

This skeleton is populated as the discovery progresses.

## Concurrent Update Rule

**Every time you create or modify a markdown file, update the corresponding
dashboard section in the same response.** This is not optional. The dashboard
must stay in sync with the markdown at all times.

| Markdown source | Dashboard section |
|-----------------|-------------------|
| `README.md` — vision, tagline, status | Hero — project name, tagline, status badge |
| Discovery file statuses (🔴 / 🟡 / 🟢 counts) | Metric cards — X open, Y direction set, Z resolved |
| `discovery/00-personas.md` | Personas — card per persona with profile, motivation, pain tolerance |
| Open questions across all discovery files | Open Questions — grouped by status |
| Decisions made across all discovery files | Decisions — card per significant decision |
| Cross-references, risks, blockers | Risks — risk items with severity |
| Core flow / draft models | Flows — step sequences or lifecycle diagrams |

## Required vs. Optional Sections

Every dashboard must have at minimum:

1. **Hero** — even the initial skeleton has this
2. **Metric cards** — counts of open / direction-set / resolved questions
3. **Personas** — once `00-personas.md` is populated
4. **Open Questions** — once any discovery file has open questions

These sections are added only when the corresponding markdown content exists:

- **Decisions** — when any discovery file has entries in "Decisions Made"
- **Risks** — when cross-references or explicit risk items exist
- **Flows** — when a discovery file contains a draft flow, state machine, or step sequence

## Sidebar Navigation

The dashboard sidebar mirrors the discovery structure:

- **Discovery section:** one nav item per discovery file, with status indicators (red = 🔴, amber = 🟡, green = 🟢)
- **Specs section:** one nav item per spec, if any exist
- **Decisions section:** one nav item linking to the decisions area

Active nav items are highlighted as the reader scrolls. A scroll-spy script
(e.g., `IntersectionObserver`) handles active state updates.

A collapsible sidebar is recommended: expanded shows icons + labels; collapsed
shows icons only. Persist the collapsed state via `localStorage`.

## Theming

This guide is content-focused; it does not prescribe a visual style. The bundled
dashboard skill at [`dashboard/`](../dashboard/) ships two ready-to-use themes:

- **Coral Pulse** — light, editorial, recommended for discovery dashboards
  (personas, open questions, decisions). See `dashboard/theme/coral_pulse/DESIGN.md`.
- **Dark Indigo** — dark, data-focused, recommended for metrics dashboards or
  Audit pattern reports. See `dashboard/theme/dark_indigo/DESIGN.md`.

If you bring your own theme, follow these principles:

- Choose a single theme per project and stick with it.
- Use accessible contrast ratios (light or dark theme is fine).
- Use status colors consistently across hero, metric cards, sidebar, and OQ groupings.
- Prefer minimal, scannable layouts over dense data displays — this dashboard is for stakeholders, not power users.

## File Location

Save the dashboard at the root of the feature's discovery folder:

```
requirements/<FeatureName>/dashboard.html
```

Commit it to version control alongside the markdown files.

## Implementation Notes

- The dashboard is a single self-contained `.html` file — inline CSS and JS, no external dependencies.
- Embed status data directly in the HTML; no fetch calls.
- Use a small set of reusable CSS classes (e.g., `.hero-card`, `.stat-card`,
  `.persona-card`, `.oq-card`, `.decision-item`) so updates are predictable.
- Avoid frameworks. The point of a single-file dashboard is portability.
