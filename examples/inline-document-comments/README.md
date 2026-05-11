# Example: Inline Document Comments

A worked Agent Disco discovery example — a feature scoped end-to-end with personas, open questions, decisions, and risks captured at a representative mid-discovery moment. Use it to:

- Show stakeholders what a populated `dashboard.html` looks like
- Reference the structure for your own first feature
- Screencap as a marketing asset

## The feature

Add anchored comment threads to long-form documents in **Acme Docs** (a hypothetical SaaS document editor). Reviewers should be able to highlight any range of text and start a discussion thread that survives edits, mentions, and version history without overwhelming participants with notifications.

## What's in this folder

- [`dashboard.html`](dashboard.html) — the discovery dashboard at a representative mid-discovery state. Open it directly in any browser.
- `README.md` — this file.

The dashboard is generated from the same artifacts the discovery folder would normally contain (`discovery/`, `decisions/`, `audit/`). For the purposes of this example, the underlying markdown is summarized inline below rather than split across files — the dashboard is the demo, not the methodology compliance.

## Discovery snapshot

| Area | Prefix | Status | OQs |
|------|--------|--------|-----|
| Anchoring & Resolution | `ANC-*` | 🔴 Open | 3 |
| Notifications | `NTF-*` | 🟡 Direction set | 3 |
| Permissions | `PERM-*` | 🟡 Direction set | 2 |
| Mobile Experience | `MOB-*` | 🔴 Open | 1 |
| Mentions | `MNT-*` | 🟡 Direction set | 2 |
| Version History | `VER-*` | 🟡 Direction set | 2 |
| **Total** | — | — | **13** |

| Status | Count |
|--------|-------|
| 🔴 Open | 7 |
| 🟡 Direction set | 3 |
| 🟢 Resolved | 3 |

## Personas

- **Lina** — Senior PM authoring product specs. Heavy commenter, multi-stakeholder reviewer.
- **Marcus** — Engineering lead doing technical reviews. Wants high signal, low noise.
- **Priya** — Casual reviewer (legal, marketing). Visits a doc maybe once, needs context fast.

## Decisions ratified

- **ADR-001** — Use the existing `MentionPicker` component from the sharing flow. *(Rejected: build a dedicated picker.)*
- **ADR-002** — Email notification cadence: 2-hour digest. *(Rejected: per-comment, daily.)*
- **ADR-003** — Comment anchoring backed by a range-fingerprint algorithm. *(Rejected: explicit character offsets.)*

## Risks tracked

- Notification fatigue (high)
- Comment orphaning on text edits (high — data-loss flavor)
- Mobile UX regression (medium)
- Performance at 1000+ comments per document (medium)
- Privacy: notifying mentioned non-collaborators (medium)
