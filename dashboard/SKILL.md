# Dashboard Skill

## Purpose

Build single-file HTML dashboards using one of two bundled themes — **Coral Pulse** (light, editorial) and **Dark Indigo** (dark, data-focused). Dashboards are visually polished and designed to be opened directly in a browser — no build step, no server, no dependencies beyond CDN resources. Companion to the [Agent Disco](../README.md) discovery skill: use this bundle to render the visual `dashboard.html` that sits alongside a feature's discovery markdown.

## When to Use

- Presenting data analysis findings (CX trends, product metrics, financial summaries)
- Creating product discovery dashboards for open questions, decisions, and personas
- Building shareable visual reports for leadership or stakeholders
- Any situation where the deliverable is "a beautiful page with data on it"

## Theme Selection

Choose the theme before building. If the calling skill specifies a theme, use it. If not, use this decision guide:

| | Coral Pulse | Dark Indigo |
|---|---|---|
| **Best for** | Product discovery, stakeholder presentations, editorial summaries | Data analysis, metrics, reporting, trend dashboards |
| **Content type** | Personas, open questions, flows, decisions | Charts, tables, stat cards, callouts |
| **Charts** | Inline visualizations (no Chart.js) | Chart.js (native integration) |
| **Tone** | Editorial, exploratory | Analytical, operational |
| **Sidebar** | Standard (collapsible, scroll-spy) | Optional (simple scrollable layout) |
| **Fonts** | Plus Jakarta Sans + Manrope (CDN) | System fonts (no CDN) |
| **CSS approach** | Tailwind CDN + config object | Plain CSS custom properties |

**Rule of thumb:** If the dashboard has Chart.js charts or is primarily quantitative data, use Dark Indigo. If it's a product discovery dashboard or stakeholder-facing summary, use Coral Pulse.

## Required Reading Before Building

1. **`BEST_PRACTICES.md`** — Lessons from prior builds. Read this first.
2. **Theme design reference** (pick one):
   - **`theme/coral_pulse/DESIGN.md`** — Light editorial theme. Full token reference.
   - **`theme/dark_indigo/DESIGN.md`** — Dark data theme. Full token reference with Chart.js integration guide.
3. **`reference/dashboard.html`** — Complete reference dashboard with all component patterns. Copy this as the starting skeleton.

## Architecture

Every dashboard is a **single `.html` file**. The structure differs slightly by theme:

### Coral Pulse

```
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Google Fonts: Plus Jakarta Sans (headings) + Manrope (body) -->
    <!-- Material Symbols Outlined (icons) -->
    <!-- Tailwind CDN + config -->
    <style>/* Component styles */</style>
</head>
<body>
    <aside><!-- Fixed 256px sidebar, collapsible to 64px icon-only --></aside>
    <main>
        <!-- Section 1: Hero with headline stat -->
        <!-- Section 2-N: Data sections -->
    </main>
    <script>/* Scroll spy for sidebar active states */</script>
</body>
</html>
```

### Dark Indigo

```
<!DOCTYPE html>
<html lang="en">
<head>
    <!-- Google Fonts: Plus Jakarta Sans (headings) + Manrope (body) -->
    <!-- Material Symbols Outlined (icons) -->
    <!-- Optional: Chart.js CDN (for data dashboards) -->
    <style>
        :root { /* Dark Indigo color tokens */ }
        /* Component styles */
    </style>
</head>
<body>
    <aside><!-- Fixed 256px sidebar, collapsible to 64px icon-only --></aside>
    <main>
        <!-- Section 1: Hero with headline stat -->
        <!-- Section 2-N: Data sections -->
    </main>
    <script>
        /* Sidebar toggle + scroll spy */
        /* Optional: Chart.js initializations */
    </script>
</body>
</html>
```

## Core Color Palette (Dark Indigo)

| Token | Hex | Usage |
|---|---|---|
| Background | `#0f1117` | Page background |
| Surface | `#1a1d27` | Cards, sidebar, containers |
| Surface Alt | `#232734` | Hover, nested containers |
| Border | `#2d3348` | Card borders, dividers |
| Text | `#e4e6ef` | Primary text, headings |
| Text Muted | `#8b8fa8` | Labels, captions |
| Accent | `#6366f1` | Primary interactive, active states |
| Accent Light | `#818cf8` | Hover accent, links |
| Red | `#ef4444` | Critical, blocking, errors |
| Amber | `#f59e0b` | Warnings, caution |
| Green | `#22c55e` | Healthy, resolved |
| Cyan | `#06b6d4` | Informational |

For the Coral Pulse palette, see `theme/coral_pulse/DESIGN.md`. For the full Dark Indigo token set and extended chart palette, see `theme/dark_indigo/DESIGN.md`.

## Component Library

The reference dashboard (`reference/dashboard.html`) demonstrates every component.

| Component | Use For |
|---|---|
| Hero card with gradient | Dashboard header |
| Stat cards (label/value/detail) | KPI metrics |
| Persona cards | User persona profiles |
| Flow steps | Process visualization |
| Discovery area cards | Feature area summaries |
| Open question cards with leaning | OQ tracking, blind spots |
| Decision list (ranked) | Pending decisions |
| Cross-reference cards | Dependency chains |
| Callout boxes (accent, danger, success) | Key findings, alerts |
| Chart cards (doughnut, bar, hbar) | Data visualization (Dark Indigo) |
| Table cards with bar fills | Detailed data tables (Dark Indigo) |

### Dark Indigo Components

Component patterns are also defined in `theme/dark_indigo/DESIGN.md` §5 and the complete skeleton in §10. Core components:

| Component | Use For |
|---|---|
| Stat cards (`.stat-card`) | Key metrics with label/value/detail |
| Chart cards (`.chart-card`, `.chart-card.wide`) | Chart.js canvas containers |
| Callout cards (`.callout`, `.callout.danger`) | Prose findings with colored left border |
| Table cards (`.table-card`) with inline bar fills | Companion data tables for charts |
| Badges (`.badge-red`, `.badge-amber`, etc.) | Semantic status indicators |

## Data Dashboard Extensions

For data-focused dashboards (metrics, trends, projections), add Chart.js:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
```

Chart.js global defaults for this theme:
```javascript
Chart.defaults.color = '#8b8fa8';
Chart.defaults.borderColor = 'rgba(45,51,72,.6)';
Chart.defaults.font.family = "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";
```

See `theme/dark_indigo/DESIGN.md` §6 for chart type guidance and doughnut configuration.

## Build Workflow

1. **Copy the reference dashboard.** Start from `reference/dashboard.html`, not from scratch.
2. **Define the headline.** What is the single most important number or status? This drives the hero.
3. **Sketch sections.** Each section = one sidebar nav item. 4–7 sections is the sweet spot.
4. **Build the hero first.** Get the headline card and stat cards right before anything else.
5. **Build sections.** Use component patterns from the reference. Adapt, don't reinvent.
6. **Wire up the sidebar.** Add nav items with `data-nav` attributes matching section IDs.
7. **Add charts if needed.** Include Chart.js CDN and initialize charts in a `<script>` block.
8. **Sanity-check every number.** Add math as HTML comments. Verify projections against actuals.
9. **Test in a browser.** Open the file directly — check contrast, sidebar toggle, scroll spy.

## Output Location

For Agent Disco discovery dashboards, save as `dashboard.html` at the root of the feature folder:

```
requirements/<FeatureName>/dashboard.html
```

For standalone use, save as `dashboard.html` in the product, project, or report folder. The dashboard is a single self-contained file — its location only matters for discoverability.

## Anti-Patterns

- Do NOT create a Python viewer/server for static HTML dashboards
- Do NOT split CSS/JS into separate files — everything in one `.html`
- Do NOT use green status text on purple/dark card backgrounds (either theme)
- Do NOT use light/white backgrounds in Dark Indigo — all surfaces are `--surface` or `--surface-alt`
- Do NOT use box-shadow for elevation in Dark Indigo — dark backgrounds absorb shadows
- Do NOT present numbers without verifiable math (use HTML comments)
- Do NOT skip `overflow: hidden` on gradient cards with decorative elements
- Do NOT use Chart.js default white segment borders on dark backgrounds (set `borderWidth: 0`)
- Do NOT mix themes within a single dashboard

## Reference Implementations

### Coral Pulse
- **Reference dashboard** — `reference/dashboard.html`. Skeleton with sidebar, hero, personas, flows, open questions, decisions, and cross-references. Copy as the starting point for any Coral Pulse build.

### Dark Indigo
- **Complete skeleton** — `theme/dark_indigo/DESIGN.md` §10. Copy the skeleton HTML as the starting point for any Dark Indigo build.
