# Dashboard Best Practices

## Lessons from Building with the Bundled Themes

This document captures what works — and what doesn't — when building dashboards using the Coral Pulse and Dark Indigo themes shipped in this bundle. Lessons accumulated across discovery dashboards, CX trend analyses, bug backlog reports, and other operational dashboards.

---

## 1. Choose Your Theme First

The dashboard system supports two themes. Pick one before writing any HTML — mixing them creates inconsistency.

### Coral Pulse (Light, Editorial)

Best for product discovery dashboards, stakeholder presentations, and editorial summaries. Uses Tailwind CDN, Plus Jakarta Sans + Manrope fonts, and a warm cream palette.

See `theme/coral_pulse/DESIGN.md` for the full token set.

### Dark Indigo (Dark, Data-Focused)

Best for data analysis dashboards, metrics reports, and trend visualizations. Uses plain CSS custom properties, system fonts, and Chart.js for charts.

```css
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
```

Do not introduce new color tokens without updating the relevant DESIGN.md. See `theme/dark_indigo/DESIGN.md` for the full reference, Chart.js integration guide, and complete HTML skeleton.

### Decision Guide

| Signal | Use Coral Pulse | Use Dark Indigo |
|---|---|---|
| Content is primarily text (personas, decisions, flows) | Yes | |
| Content is primarily quantitative (charts, tables, metrics) | | Yes |
| Dashboard uses Chart.js | | Yes |
| Dashboard is a product discovery output | Yes | |
| Dashboard is a reporting/analysis output | | Yes |
| Calling skill specifies a theme | Use what it says | Use what it says |

---

## 2. Single-File HTML Is the Way

Every dashboard is a single `.html` file. No separate CSS, no separate JS, no build step.

**Required externals (CDN only):**
```html
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@700;800&family=Manrope:wght@400;500;600;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/icon?family=Material+Symbols+Outlined" rel="stylesheet">
```

**Optional (for data dashboards):**
```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
```

Everything else goes in `<style>` and `<script>` blocks within the file.

**Tailwind CDN** is used by the Coral Pulse theme but is not required for Dark Indigo. The Dark Indigo theme uses CSS custom properties and plain CSS for all styling.

---

## 3. Sidebar Navigation: Fixed, Collapsible, Smooth-Scroll

Sidebars use 256px expanded and 64px collapsed. Every sidebar must include a collapse toggle.

**Expanded state (default):**
- Fixed position, full height, `background: var(--surface)`, `border-right: 1px solid var(--border)`
- Grouped nav sections with uppercase section labels in `--text-muted`
- Status dots (red, amber, green) with glow for discovery status
- Material Symbols icons for section navigation
- Active state: `background: var(--surface-alt)`, `color: var(--accent-light)`, `font-weight: 700`

**Collapsed state:**
- 64px wide, icons/dots only
- Text labels hidden via `opacity: 0; width: 0; overflow: hidden`
- Tooltips via `title` attribute
- Toggle button chevron flips direction

**Transition:**
```css
transition: width 0.25s cubic-bezier(0.4, 0, 0.2, 1);
```

**Scroll spy pattern:**
```javascript
const sections = document.querySelectorAll('.section');
const navItems = document.querySelectorAll('[data-nav]');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      navItems.forEach(item => item.classList.remove('active'));
      const activeNav = document.querySelector(`[data-nav="${entry.target.id}"]`);
      if (activeNav) activeNav.classList.add('active');
    }
  });
}, { rootMargin: '-20% 0px -70% 0px' });
sections.forEach(section => observer.observe(section));
```

**Sidebar toggle pattern:**
```javascript
function toggleSidebar() {
  const isCollapsed = sidebar.classList.toggle('collapsed');
  mainContent.classList.toggle('sidebar-collapsed', isCollapsed);
  collapseIcon.textContent = isCollapsed ? 'chevron_right' : 'chevron_left';
  localStorage.setItem('sidebar-collapsed', isCollapsed ? '1' : '0');
}
```

---

## 4. Hero Section Architecture

The hero must answer "what is the single most important thing?" in under 3 seconds.

**Structure:**

1. **Status badges** — small pills above the hero card (e.g., "Discovery Open", date)
2. **Hero card** — gradient accent background (`--accent` → `#4f46e5`) with title, tagline, description. Decorative circles using `::before` / `::after` pseudo-elements with `overflow: hidden`.
3. **Stat cards** — 3-column grid of KPIs with semantic value colors
4. **Summary callout** — narrative paragraph in a callout box

**Badge colors on dark cards:**
Use translucent white backgrounds with white text on gradient/accent cards. Never use colored text on colored cards.

```css
/* On accent/gradient cards: */
.hero-badge {
  background: rgba(255, 255, 255, 0.15);
  color: rgba(255, 255, 255, 0.9);
}

/* On surface cards (standard): */
.badge-red { background: rgba(239,68,68,.15); color: var(--red); }
```

---

## 5. Card System

All cards share a base pattern:
```css
background: var(--surface);
border: 1px solid var(--border);
border-radius: 12px;
padding: 1.5rem;
```

Hover state: `border-color: rgba(99,102,241,.3)` — subtle accent glow.

**Card variants:**
- **Stat card** — compact (`.stat-card`): label (uppercase, muted), large value, detail text
- **Chart card** — canvas container (`.chart-card`): heading + chart wrapper
- **OQ card** — open question (`.oq-card`): ID, title, description, leaning box
- **Persona card** — icon header + description
- **Decision item** — ranked list item with severity badge
- **Cross-ref card** — dependency chain with link icon
- **Area card** — discovery area with tags

**Elevation on dark backgrounds (Dark Indigo):**
Do NOT use `box-shadow`. Shadows are invisible on `#0f1117`. Use border lightness shifts and background color steps (`--surface` → `--surface-alt`) for depth.

---

## 6. Typography Scale

| Element | Size | Font | Weight |
|---|---|---|---|
| Hero title | `2.75rem` | Plus Jakarta Sans | 800 |
| Section heading | `1.3rem` | Plus Jakarta Sans | 700 |
| Card heading | `1.05rem` | Plus Jakarta Sans | 600–700 |
| Stat value | `1.8rem` | Plus Jakarta Sans | 700 |
| Body text | `0.875–0.9rem` | Manrope | 400 |
| Labels/captions | `0.7–0.85rem` | Manrope | 500–600, uppercase |
| Badge text | `0.7rem` | Manrope | 600 |
| Sidebar nav | `0.875rem` | Manrope | 400–500 |

---

## 7. Color Usage for Sentiment

| Meaning | Token | Use For |
|---|---|---|
| Accent / interactive | `--accent` / `--accent-light` | Hero, active nav, links |
| Critical / blocking | `--red` | Errors, blockers, concerning metrics |
| Warning / caution | `--amber` | Direction-set items, watch metrics |
| Healthy / resolved | `--green` | Validated items, positive indicators |
| Informational | `--cyan` | New items, neutral highlights |
| Muted / low priority | `--text-muted` | Deferred items, secondary info |

Always pair color with a label or shape. Never use color as the sole differentiator.

---

## 8. Chart.js Integration

For data dashboards, include Chart.js and set global defaults:

```javascript
Chart.defaults.color = '#8b8fa8';
Chart.defaults.borderColor = 'rgba(45,51,72,.6)';
Chart.defaults.font.family = "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif";
```

**Chart types and when to use them:**

| Data | Chart | Settings |
|---|---|---|
| Proportional composition | Doughnut | `cutout: '55%'`, legend right |
| Time series / trends | Vertical bar | `borderRadius: 6`, no border skip |
| Ranked list (long labels) | Horizontal bar | `indexAxis: 'y'` |

**Extended palette for multi-series:**
```
#6366f1  #818cf8  #a78bfa  #c084fc  #f472b6
#fb7185  #f97316  #fbbf24  #34d399  #22d3ee
#60a5fa  #94a3b8
```

Every chart should have a companion table for accessibility.

---

## 9. Callout Boxes

Use callouts for key findings, methodology notes, and alerts:

```html
<div class="callout">          <!-- accent left border -->
<div class="callout danger">   <!-- red left border -->
<div class="callout success">  <!-- green left border -->
```

Structure: `<h3>` title, then `<p>` or `<ul>` content. Background: `--surface`. Border-left: 4px solid.

---

## 10. Common Mistakes

### Both Themes

1. **Building a viewer/server for static HTML.** Single-file HTML opens directly in any browser. No server needed.

2. **Not sanity-checking the math.** Every number must trace to a calculation. Include the math as HTML comments:
```html
<!-- MATH: 4-week avg = (180+179+145+218)/4 = 180.5 ≈ 180 -->
<div class="value">~180</div>
```

3. **Mixing themes.** Do not combine Coral Pulse and Dark Indigo in the same dashboard. Pick one and stay on it.

4. **Skipping `overflow: hidden` on gradient cards.** Decorative pseudo-elements will bleed outside the card border-radius.

5. **Not including `scroll-margin-top` on sections.** Sidebar nav links will scroll sections behind any fixed elements. Use `scroll-margin-top: 100px`.

6. **Forgetting responsive breakpoints.** The sidebar + content layout is desktop-first. Grids should collapse to single-column below 900px.

### Dark Indigo Specific

7. **Using light/white backgrounds.** The theme is dark. All containers use `--surface` or `--surface-alt`. White cards break the entire visual system.

8. **Using box-shadow for depth.** Shadows disappear on dark backgrounds. Use border color and background shifts instead.

9. **Using pure black (`#000000`) for backgrounds.** Always use `--bg` (`#0f1117`) which has a subtle blue undertone.

10. **Leaving Chart.js default white borders on doughnut segments.** Always set `borderWidth: 0` on doughnut/pie datasets.

11. **Colored text on colored badge backgrounds.** Badges use *translucent* backgrounds (15% opacity) with the semantic color as text. Never layer two opaque colors.

12. **Using opacity below 0.5 for text.** On dark backgrounds, low-opacity text becomes unreadable. Minimum `--text-muted` opacity is 0.55.

13. **Skipping companion tables for charts.** Every chart in a Dark Indigo dashboard should have a companion table.

### Coral Pulse Specific

14. **Green text on purple backgrounds.** Unreadable. Use white text with translucent white badge backgrounds on any dark card.

15. **Over-relying on the DESIGN.md token system.** The 40-token palette is for the full app. For most dashboards, 5 core colors suffice.

16. **Using Tailwind CDN when pure CSS suffices.** Coral Pulse uses Tailwind by convention, but if the dashboard is simple, plain CSS is fine.

---

## 11. File Organization

```
dashboard/
├── SKILL.md              # Skill definition — theme selection, architecture, palettes
├── BEST_PRACTICES.md     # This file
├── reference/
│   └── dashboard.html    # Reference dashboard — copy as starting point
└── theme/
    ├── coral_pulse/
    │   └── DESIGN.md     # Light editorial theme — full token reference
    └── dark_indigo/
        └── DESIGN.md     # Dark data theme — full token reference + Chart.js guide + skeleton
```

---

## 12. Checklist Before Sharing

- [ ] Every number has a verifiable source (math in HTML comments)
- [ ] No colored text on dark gradient cards (use white + translucent bg)
- [ ] Hero section answers "what is the most important thing?" in 3 seconds
- [ ] Sidebar navigation works (smooth scroll, active states, collapse toggle)
- [ ] Status date visible at the top
- [ ] Single `.html` file, opens in browser with no server
- [ ] Charts have companion tables (if using Chart.js)
- [ ] Responsive: grids collapse on narrow viewports
- [ ] Theme is consistent throughout (no mixing Coral Pulse and Dark Indigo)
