# Design System: Dark Indigo

## 1. Overview & Creative North Star

**The Creative North Star: "The Command Center"**

This design system prioritizes data density, readability under sustained viewing, and visual clarity on dark surfaces. It is designed for dashboards where the content is primarily quantitative — metrics, charts, tables, trend lines — and the user is scanning for anomalies, not browsing editorially.

The system uses a restrained palette of deep navy surfaces with indigo accent marks. There are no decorative elements, no extreme corner radii, no editorial flair. Every visual choice serves legibility: high-contrast text on dark backgrounds, semantic color coding for status, and consistent component patterns that let the eye learn the layout once and scan it repeatedly.

**When to use Dark Indigo vs. Coral Pulse:**

| | Dark Indigo | Coral Pulse |
|---|---|---|
| **Best for** | Data analysis, metrics, reporting, trend dashboards | Product discovery, stakeholder presentations, editorial summaries |
| **Content type** | Charts, tables, stat cards, callouts | Personas, open questions, flows, decisions |
| **Chart support** | Chart.js (native) | Not chart-focused (use inline visualizations) |
| **Tone** | Analytical, operational | Editorial, exploratory |
| **Sidebar** | Optional (data dashboards often don't need one) | Standard (collapsible, scroll-spy) |
| **Typography** | System fonts (fast, native feel) | Plus Jakarta Sans + Manrope (editorial feel) |

---

## 2. Colors & Surface Philosophy

The color strategy is rooted in depth and contrast. Dark surfaces reduce eye strain during extended data review and make chart colors pop.

### Core Palette

```css
:root {
  --bg: #0f1117;           /* Page background — near-black with blue undertone */
  --surface: #1a1d27;      /* Card and container background */
  --surface-alt: #232734;  /* Hover states, alternating rows, nested containers */
  --border: #2d3348;       /* Card borders, section dividers, table rules */
  --text: #e4e6ef;         /* Primary text — high contrast on dark surfaces */
  --text-muted: #8b8fa8;   /* Secondary text — labels, captions, axis text */
  --accent: #6366f1;       /* Primary accent — indigo-500. Links, active states, bar fills */
  --accent-light: #818cf8; /* Secondary accent — indigo-400. Hover states, lighter elements */
  --red: #ef4444;          /* Semantic: negative, critical, concerning metrics */
  --amber: #f59e0b;        /* Semantic: warning, caution, moderate concern */
  --green: #22c55e;        /* Semantic: positive, healthy, improving metrics */
  --cyan: #06b6d4;         /* Semantic: informational, current period, neutral highlight */
}
```

### Tonal Hierarchy

- **Base layer:** `--bg` (`#0f1117`) — the page itself. Near-black with a subtle blue-gray undertone that prevents the "pure black OLED" look.
- **Container layer:** `--surface` (`#1a1d27`) — cards, chart panels, stat blocks, tables. One step lighter than the base.
- **Interactive layer:** `--surface-alt` (`#232734`) — hover states on table rows, nested containers within cards, active states. One step lighter than surface.
- **Border layer:** `--border` (`#2d3348`) — card outlines, section dividers, table rules. Visible but not dominant.

Unlike Coral Pulse, **borders are standard in Dark Indigo.** The dark-on-dark surface hierarchy alone doesn't provide enough visual separation for data-dense layouts. Use `1px solid var(--border)` on cards, tables, and section dividers.

### Semantic Colors

Semantic colors are used for data significance, not decoration:

| Color | Token | Use For | Example |
|---|---|---|---|
| Red | `--red` | Concerning, critical, negative trend | "225d average age", "23.5% over 12 months" |
| Amber | `--amber` | Caution, moderate concern | "188d median age", "73.7% untagged" |
| Green | `--green` | Positive, healthy, improving | "150 tagged", upward trends |
| Cyan | `--cyan` | Informational, current period | "Q1 2026 data", neutral highlights |
| Accent | `--accent` | Interactive, structural | Links, bar fills, active nav states |

### Badge Pattern

Badges use a translucent background of their semantic color at 15% opacity:

```css
.badge-red    { background: rgba(239, 68, 68, 0.15); color: var(--red); }
.badge-amber  { background: rgba(245, 158, 11, 0.15); color: var(--amber); }
.badge-green  { background: rgba(34, 197, 94, 0.15); color: var(--green); }
.badge-cyan   { background: rgba(6, 182, 212, 0.15); color: var(--cyan); }
.badge-muted  { background: rgba(139, 143, 168, 0.15); color: var(--text-muted); }
```

---

## 3. Typography

Dark Indigo uses **system fonts** for speed and native feel. No external font loading required.

```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

### Type Scale

| Element | Size | Weight | Color |
|---|---|---|---|
| Page title (h1) | 1.75rem | 700 | `--text` |
| Section title (h2 in section-header) | 1.3rem | 700 | `--text` |
| Card title (h2 in chart-card) | 1.05rem | 600 | `--text` |
| Stat value | 1.8rem | 700 | Semantic (red/amber/green/cyan) or `--text` |
| Stat label | 0.75rem | 400, uppercase, tracking 0.04em | `--text-muted` |
| Stat detail | 0.8rem | 400 | `--text-muted` |
| Body text | 0.9rem | 400 | `--text` or `--text-muted` |
| Callout text | 0.9rem | 400, line-height 1.65 | `--text-muted` (bold items: `--text`) |
| Table header | 0.8rem | 500, uppercase, tracking 0.04em | `--text-muted` |
| Table cell | 0.9rem | 400 | `--text` |
| Badge | 0.75rem | 600 | Semantic color |
| Subtitle / note | 0.85–0.95rem | 400 | `--text-muted` |
| Timestamp | 0.8rem | 400 | `--text-muted` |

---

## 4. Layout

Dark Indigo dashboards use a **single-column, max-width layout** centered on the page. No sidebar by default — data dashboards are typically consumed top-to-bottom in a single reading session, not navigated by section.

```css
max-width: 1200px;
margin: 0 auto;
padding: 2rem;
```

### Section Structure

```
HEADER           Title, subtitle with metadata, date
SECTION N        Section header (title + note) → stat cards → callout → charts → tables
METHODOLOGY      Callout with data source and model details
FOOTER           Timestamp, org attribution
```

Each section follows a consistent pattern:

1. **Section header** — title + contextual note, separated by a bottom border
2. **Stat cards** — responsive grid of key metrics
3. **Callout** — prose summary of findings (optional, border-left colored)
4. **Charts grid** — 2-column grid of chart cards (responsive to 1-column on mobile)
5. **Tables** — full-width companion tables with inline bar indicators

### Adding a Sidebar (Optional)

For longer dashboards with 5+ sections, add a fixed sidebar for navigation. Use the same pattern as Coral Pulse (fixed position, 256px expanded / 64px collapsed), but with Dark Indigo surface colors:

```css
aside {
  background: var(--surface);
  border-right: 1px solid var(--border);
}
.nav-item.active {
  background: var(--surface-alt);
  color: var(--accent-light);
}
```

---

## 5. Components

### Stat Cards

The primary data display unit. Responsive grid, typically 4–6 across.

```css
.stat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.15rem 1.25rem;
}
```

Structure: label (muted, uppercase, small) → value (large, semantic color) → detail (muted, small).

### Chart Cards

Containers for Chart.js canvases. 2-column grid with `.wide` for full-width charts.

```css
.chart-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.5rem;
}
.chart-card.wide { grid-column: 1 / -1; }
```

Standard chart wrapper heights:
- Doughnut: `300px`
- Timeline (bar): `280px`
- Horizontal bar: `480px`

### Callout Cards

Prose summaries with a colored left border indicating severity.

```css
.callout {
  background: var(--surface);
  border: 1px solid var(--border);
  border-left: 4px solid var(--accent);  /* or --red for danger */
  border-radius: 0 12px 12px 0;
  padding: 1.25rem 1.5rem;
}
.callout.danger { border-left-color: var(--red); }
```

### Tables

Every chart should have a companion table for accessibility and precise values.

```css
table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
th { border-bottom: 1px solid var(--border); }
td { border-bottom: 1px solid rgba(45, 51, 72, 0.5); }
tr:hover td { background: var(--surface-alt); }
```

Inline bar indicators in table cells:

```css
.bar-fill {
  height: 8px;
  border-radius: 4px;
  background: var(--accent);
}
```

### Badges

Pill-shaped, inline-block, semantic colors. See Badge Pattern in §2.

```css
.badge {
  display: inline-block;
  padding: 0.15rem 0.5rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
}
```

---

## 6. Chart.js Integration

Dark Indigo dashboards use Chart.js (v4.4+) loaded from CDN.

### CDN

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
```

### Global Defaults

```javascript
Chart.defaults.color = '#8b8fa8';                    // --text-muted
Chart.defaults.borderColor = 'rgba(45, 51, 72, 0.6)'; // --border at 60%
Chart.defaults.font.family = "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";
```

### Chart Palette

A 12-color sequence that reads well on dark backgrounds:

```javascript
const palette = [
  '#6366f1', '#818cf8', '#a78bfa', '#c084fc',
  '#f472b6', '#fb7185', '#f97316', '#fbbf24',
  '#34d399', '#22d3ee', '#60a5fa', '#94a3b8'
];
```

### Standard Doughnut Options

```javascript
function doughnutOpts(legendPosition) {
  return {
    responsive: true,
    maintainAspectRatio: false,
    cutout: '55%',
    plugins: {
      legend: {
        position: legendPosition || 'right',
        labels: { padding: 12, usePointStyle: true, pointStyleWidth: 10, font: { size: 12 } }
      },
      tooltip: {
        callbacks: {
          label: ctx => {
            const total = ctx.dataset.data.reduce((a, b) => a + b, 0);
            return ` ${ctx.label}: ${ctx.raw} (${(ctx.raw / total * 100).toFixed(1)}%)`;
          }
        }
      }
    }
  };
}
```

### Bar Chart Grid Lines

Use `rgba(45, 51, 72, 0.4)` for grid lines — visible but not dominant.

### Doughnut Charts

Always set `borderWidth: 0` on datasets (the default white segment borders look wrong on dark backgrounds). Use `hoverOffset: 8` for subtle interaction feedback.

---

## 7. Corner Radii

Dark Indigo uses moderate, consistent radii — not the extreme super-ellipse radii of Coral Pulse.

| Element | Radius | Notes |
|---|---|---|
| Cards (stat, chart, table, callout) | `12px` | Consistent across all card types |
| Badges | `6px` | Pill-like but not fully round |
| Bar fills (inline table bars) | `4px` | Subtle rounding |
| Chart bars (`borderRadius`) | `6px` | Matches badge rounding |
| Buttons (if used) | `8px` | Slightly more than cards |

---

## 8. Do's and Don'ts

### Do

- **DO** use `1px solid var(--border)` on cards and tables. Dark-on-dark needs borders for structure.
- **DO** apply semantic colors to stat values that carry meaning — red for concerning, green for positive.
- **DO** include a companion table for every chart. Charts show shape; tables show precision.
- **DO** use inline bar indicators (`.bar-fill`) in tables for scannable relative comparisons.
- **DO** add math as HTML comments near computed values for auditability.
- **DO** use `rgba(45, 51, 72, 0.5)` for subtle table row borders (softer than `var(--border)`).

### Don't

- **DON'T** use pure black (`#000000`) for backgrounds. Always use `--bg` (`#0f1117`) which has a blue undertone.
- **DON'T** use colored text on colored backgrounds (e.g., green text on purple cards). On dark cards, use white text with translucent white badges.
- **DON'T** use the Coral Pulse extreme corner radii (3rem). Dark Indigo uses 12px consistently.
- **DON'T** use decorative elements (gradient overlays, floating shapes, asymmetric layouts). Dark Indigo is functional, not editorial.
- **DON'T** load custom fonts unless the dashboard requires editorial sections. System fonts are faster and feel more "operational."
- **DON'T** use Chart.js default white borders on doughnut segments. Always set `borderWidth: 0`.
- **DON'T** forget `border-radius: 12px` on callout cards — they use `border-radius: 0 12px 12px 0` because the left border-radius is overridden by the colored left border.

---

## 9. Responsive Behavior

Dark Indigo dashboards are desktop-first but should degrade gracefully:

```css
/* Chart grid: 2-col on desktop, 1-col on mobile */
@media (max-width: 768px) {
  .charts-grid { grid-template-columns: 1fr; }
}

/* Stat cards: auto-fit responsive grid */
.stats-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
}
```

---

## 10. Complete Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[Dashboard Title]</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --surface-alt: #232734;
    --border: #2d3348;
    --text: #e4e6ef;
    --text-muted: #8b8fa8;
    --accent: #6366f1;
    --accent-light: #818cf8;
    --red: #ef4444;
    --amber: #f59e0b;
    --green: #22c55e;
    --cyan: #06b6d4;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background: var(--bg);
    color: var(--text);
    padding: 2rem;
    min-height: 100vh;
  }
  .header { max-width: 1200px; margin: 0 auto 2rem; }
  .header h1 { font-size: 1.75rem; font-weight: 700; margin-bottom: .25rem; }
  .header .subtitle { color: var(--text-muted); font-size: .95rem; }
  .header .subtitle a { color: var(--accent-light); text-decoration: none; }
  .section-header {
    max-width: 1200px; margin: 2.5rem auto 1.25rem;
    padding-bottom: .5rem; border-bottom: 1px solid var(--border);
  }
  .section-header h2 { font-size: 1.3rem; font-weight: 700; }
  .section-header .note { font-size: .85rem; color: var(--text-muted); margin-top: .25rem; }
  .stats-row {
    display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem; max-width: 1200px; margin: 0 auto 2rem;
  }
  .stat-card {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 12px; padding: 1.15rem 1.25rem;
  }
  .stat-card .label {
    font-size: .75rem; color: var(--text-muted);
    text-transform: uppercase; letter-spacing: .04em; margin-bottom: .35rem;
  }
  .stat-card .value { font-size: 1.8rem; font-weight: 700; }
  .stat-card .detail { font-size: .8rem; color: var(--text-muted); margin-top: .2rem; }
  .charts-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 1.5rem; max-width: 1200px; margin: 0 auto 2rem;
  }
  @media (max-width: 768px) { .charts-grid { grid-template-columns: 1fr; } }
  .chart-card {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 12px; padding: 1.5rem;
  }
  .chart-card h2 { font-size: 1.05rem; font-weight: 600; margin-bottom: 1rem; }
  .chart-card.wide { grid-column: 1 / -1; }
  .chart-wrapper { position: relative; width: 100%; }
  .chart-wrapper.doughnut { height: 300px; }
  .chart-wrapper.timeline { height: 280px; }
  .chart-wrapper.hbar { height: 480px; }
  .table-card {
    grid-column: 1 / -1; background: var(--surface);
    border: 1px solid var(--border); border-radius: 12px;
    padding: 1.5rem; overflow-x: auto;
  }
  .table-card h2 { font-size: 1.05rem; font-weight: 600; margin-bottom: 1rem; }
  table { width: 100%; border-collapse: collapse; font-size: .9rem; }
  th {
    text-align: left; color: var(--text-muted); font-weight: 500;
    font-size: .8rem; text-transform: uppercase; letter-spacing: .04em;
    padding: .6rem .8rem; border-bottom: 1px solid var(--border);
  }
  td { padding: .5rem .8rem; border-bottom: 1px solid rgba(45,51,72,.5); }
  tr:hover td { background: var(--surface-alt); }
  .bar-cell { display: flex; align-items: center; gap: .6rem; }
  .bar-fill { height: 8px; border-radius: 4px; background: var(--accent); }
  .bar-pct { color: var(--text-muted); font-size: .8rem; min-width: 40px; }
  .badge {
    display: inline-block; padding: .15rem .5rem; border-radius: 6px;
    font-size: .75rem; font-weight: 600;
  }
  .badge-red { background: rgba(239,68,68,.15); color: var(--red); }
  .badge-amber { background: rgba(245,158,11,.15); color: var(--amber); }
  .badge-green { background: rgba(34,197,94,.15); color: var(--green); }
  .badge-cyan { background: rgba(6,182,212,.15); color: var(--cyan); }
  .badge-muted { background: rgba(139,143,168,.15); color: var(--text-muted); }
  .callout {
    max-width: 1200px; margin: 0 auto 2rem;
    background: var(--surface); border: 1px solid var(--border);
    border-left: 4px solid var(--accent); border-radius: 0 12px 12px 0;
    padding: 1.25rem 1.5rem; font-size: .9rem; line-height: 1.65;
    color: var(--text-muted);
  }
  .callout strong { color: var(--text); }
  .callout.danger { border-left-color: var(--red); }
  .callout ul { margin: .5rem 0 0; padding-left: 1.25rem; }
  .callout li { margin-bottom: .35rem; }
  .callout h3 { color: var(--text); font-size: .95rem; margin-bottom: .5rem; }
  .timestamp {
    max-width: 1200px; margin: 0 auto;
    color: var(--text-muted); font-size: .8rem; text-align: right;
  }
</style>
</head>
<body>

<div class="header">
  <h1>[Dashboard Title]</h1>
  <div class="subtitle">[Contextual subtitle with links and metadata]</div>
</div>

<div class="section-header">
  <h2>[Section Title]</h2>
  <div class="note">[Contextual note about data source or scope]</div>
</div>

<div class="stats-row">
  <div class="stat-card">
    <div class="label">METRIC</div>
    <div class="value">123</div>
    <div class="detail">supporting context</div>
  </div>
  <!-- More stat cards -->
</div>

<div class="callout">
  <h3>Key Findings</h3>
  <ul>
    <li><strong>Finding one:</strong> explanation with data.</li>
  </ul>
</div>

<div class="charts-grid">
  <div class="chart-card wide">
    <h2>[Chart Title]</h2>
    <div class="chart-wrapper timeline"><canvas id="chart1"></canvas></div>
  </div>
  <div class="chart-card">
    <h2>[Chart Title]</h2>
    <div class="chart-wrapper doughnut"><canvas id="chart2"></canvas></div>
  </div>
  <div class="table-card">
    <h2>[Table Title]</h2>
    <table><thead><tr><th>Column</th><th>Value</th><th>Share</th><th></th></tr></thead><tbody></tbody></table>
  </div>
</div>

<div class="callout">
  <h3>Methodology</h3>
  <ul><li>Data source, model details, parsing notes.</li></ul>
</div>

<div class="timestamp">[Org] · [Date]</div>

<script>
Chart.defaults.color = '#8b8fa8';
Chart.defaults.borderColor = 'rgba(45,51,72,.6)';
Chart.defaults.font.family = "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";

const palette = ['#6366f1','#818cf8','#a78bfa','#c084fc','#f472b6','#fb7185','#f97316','#fbbf24','#34d399','#22d3ee','#60a5fa','#94a3b8'];

// Chart instantiation here
</script>
</body>
</html>
```
