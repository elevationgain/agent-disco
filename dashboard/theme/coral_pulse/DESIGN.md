# Design System: Coral Pulse

## 1. Overview & Creative North Star

**The Creative North Star: "The Editorial Briefing"**

This design system prioritizes readability, warmth, and stakeholder accessibility on light backgrounds. It draws from the aesthetic of editorial magazines and product briefs — approachable, professional, and easy to scan. Data supports narrative, not the other way around.

The system uses **tonal layering on a warm cream base** to create subtle depth. Cards float on the background through white surfaces with gentle shadows, reinforced by warm-toned borders. Corner radii are moderate (`12px`) — rounded enough to feel modern, sharp enough to feel professional.

Typography uses **Plus Jakarta Sans + Manrope** via Google Fonts for an editorial, intentional feel.

**When to use Coral Pulse vs. Dark Indigo:**

| | Coral Pulse | Dark Indigo |
|---|---|---|
| **Best for** | Product discovery, stakeholder presentations, editorial summaries | Data analysis, metrics, reporting, trend dashboards |
| **Content type** | Personas, open questions, flows, decisions | Charts, tables, stat cards, callouts |
| **Chart support** | Inline visualizations (no Chart.js) | Chart.js (native integration) |
| **Tone** | Editorial, exploratory | Analytical, operational |
| **Sidebar** | Standard (collapsible, scroll-spy) | Optional |
| **Typography** | Plus Jakarta Sans + Manrope (CDN) | System fonts (no CDN) |

---

## 2. Color Tokens

### Core Palette

| Token | Variable | Hex | Usage |
|-------|----------|-----|-------|
| Background | `--bg` | `#faf8f5` | Page background — warm cream |
| Surface | `--surface` | `#ffffff` | Cards, sidebar, containers |
| Surface Alt | `--surface-alt` | `#f3f0ec` | Hover states, alternating rows, nested containers |
| Border | `--border` | `#e5e1db` | Card borders, dividers, table lines |
| Text | `--text` | `#1e1e2e` | Primary text, headings |
| Text Muted | `--text-muted` | `#6b6b7b` | Labels, captions, secondary text |
| Accent | `--accent` | `#e8614d` | Primary interactive — coral. Active states, links, hero gradients |
| Accent Light | `--accent-light` | `#f08c7a` | Hover accent, secondary interactive |

### Semantic Colors

| Token | Variable | Hex | Usage |
|-------|----------|-----|-------|
| Red | `--red` | `#dc3545` | Critical, blocking, errors, concerning metrics |
| Amber | `--amber` | `#d97706` | Warnings, caution, direction-set items |
| Green | `--green` | `#16a34a` | Healthy, resolved, positive indicators |
| Cyan | `--cyan` | `#0891b2` | Informational, new, neutral highlights |

### CSS Custom Properties Block

```css
/* Coral Pulse — light editorial theme */
:root {
  --bg: #faf8f5;
  --surface: #ffffff;
  --surface-alt: #f3f0ec;
  --border: #e5e1db;
  --text: #1e1e2e;
  --text-muted: #6b6b7b;
  --accent: #e8614d;
  --accent-light: #f08c7a;
  --red: #dc3545;
  --amber: #d97706;
  --green: #16a34a;
  --cyan: #0891b2;
}
```

---

## 3. Surface & Depth

### Tonal Layering + Shadows

Unlike Dark Indigo, Coral Pulse uses **box-shadow** for card elevation. The light background provides enough contrast for shadows to work.

| Layer | Token | Usage |
|-------|-------|-------|
| Base canvas | `--bg` (`#faf8f5`) | Page body, warm cream background |
| Card / container | `--surface` (`#ffffff`) | All card backgrounds, sidebar |
| Nested / hover | `--surface-alt` (`#f3f0ec`) | Hover row, nested card, alternating stripe |

### Card Shadow

```css
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
}
.card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
```

### Borders

Borders are subtle on light backgrounds. Use `1px solid var(--border)` for structure. Cards combine borders with shadows for a polished look.

### Corner Radii

| Element | Radius |
|---------|--------|
| Cards, callouts, tables | `12px` |
| Badges, pills | `6px` |
| Buttons | `8px` |
| Sidebar | `0` (flush to edge) |

---

## 4. Typography

### Font Stack

Google Fonts (standard for Coral Pulse):
```css
h1, h2, h3, h4, h5, h6 { font-family: 'Plus Jakarta Sans', sans-serif; }
body { font-family: 'Manrope', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; }
```

### Scale

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Page title | `1.75rem` | 700–800 | `--text` |
| Section heading | `1.3rem` | 700 | `--text` |
| Card heading | `0.95rem` | 700 | `--text` |
| Stat value | `1.8rem` | 700 | varies (semantic) |
| Body text | `0.875–0.9rem` | 400 | `--text` or `--text-muted` |
| Label / caption | `0.75rem` | 500–600 | `--text-muted` |
| Badge text | `0.7rem` | 600 | varies (semantic) |
| Sidebar nav | `0.875rem` | 400 | `--text-muted` → `--accent` |

---

## 5. Components

### Hero Card

Gradient background using the coral accent:

```css
.hero-card {
  background: linear-gradient(135deg, var(--accent) 0%, #c94535 100%);
  border: none;
  border-radius: 16px;
  padding: 2.5rem;
  position: relative;
  overflow: hidden;
}
```

Text on the hero is white (`#fff`) with reduced opacity for secondary text.

### Stat Cards

```css
.stat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.15rem 1.25rem;
  box-shadow: 0 1px 3px rgba(0,0,0,0.06);
}
```

Value color is semantic: red for concerning, amber for caution, green for positive, cyan for neutral.

### Badges

Translucent background at 10% opacity (lighter than Dark Indigo's 15%):

| Variant | Background | Text |
|---------|------------|------|
| Red | `rgba(220,53,69,.1)` | `--red` |
| Amber | `rgba(217,119,6,.1)` | `--amber` |
| Green | `rgba(22,163,74,.1)` | `--green` |
| Cyan | `rgba(8,145,178,.1)` | `--cyan` |
| Muted | `rgba(107,107,123,.1)` | `--text-muted` |
| Accent | `rgba(232,97,77,.1)` | `--accent` |

All badges: `padding: .15rem .55rem`, `border-radius: 6px`, `font-size: .7rem`, `font-weight: 600`.

### Callouts

Left-border accent box. Same pattern as Dark Indigo but with coral accent:

```css
.callout {
  background: var(--surface);
  border: 1px solid var(--border);
  border-left: 4px solid var(--accent);
  border-radius: 0 12px 12px 0;
  padding: 1.25rem 1.5rem;
}
.callout.danger { border-left-color: var(--red); }
.callout.success { border-left-color: var(--green); }
.callout.warning { border-left-color: var(--amber); }
```

### Status Dots

10px circles with subtle glow (reduced from Dark Indigo's stronger glow):

```css
.dot-red { background: var(--red); box-shadow: 0 0 6px rgba(220,53,69,0.25); }
.dot-amber { background: var(--amber); box-shadow: 0 0 6px rgba(217,119,6,0.25); }
.dot-green { background: var(--green); box-shadow: 0 0 6px rgba(22,163,74,0.25); }
```

### Sidebar

Fixed position, 256px wide. Uses `box-shadow` for depth:

```css
#sidebar {
  position: fixed;
  left: 0; top: 0; bottom: 0;
  width: 256px;
  background: var(--surface);
  border-right: 1px solid var(--border);
  box-shadow: 1px 0 8px rgba(0,0,0,0.04);
}
```

Sidebar title uses `--accent` (not `--accent-light` — coral reads better on white at full saturation). Active nav items use `--accent` with `--surface-alt` background.

---

## 6. Hover States

On light backgrounds, prefer shadow transitions over border-color changes:

```css
.card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
```

For interactive elements that need more emphasis, use a subtle coral border:

```css
.interactive:hover {
  border-color: rgba(232,97,77,.2);
}
```

---

## 7. Do's and Don'ts

### Do

- **DO** use CSS custom properties for all colors.
- **DO** use `box-shadow` for card elevation — light backgrounds make shadows effective.
- **DO** use the semantic color vocabulary: red = bad, amber = watch, green = good, cyan = info.
- **DO** use Plus Jakarta Sans + Manrope fonts for the editorial feel.
- **DO** keep the warm cream `--bg` as the page background — never use pure white (`#fff`) for the body.

### Don't

- **DON'T** use dark/black backgrounds for any section. All surfaces are `--surface` (white) or `--surface-alt` (warm gray).
- **DON'T** use `--accent-light` for text on white backgrounds — the contrast is too low. Use `--accent` for readable coral text.
- **DON'T** use strong glow effects on status dots — reduce to `0.25` opacity (Dark Indigo uses `0.35`).
- **DON'T** use green text on colored card backgrounds without checking contrast.
- **DON'T** mix Coral Pulse and Dark Indigo in the same dashboard.
- **DON'T** use Chart.js in Coral Pulse dashboards — use inline visualizations or tables instead.
