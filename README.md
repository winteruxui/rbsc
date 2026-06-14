# RBSC Racing — Clickable Prototype

A single-file, offline-runnable clickable prototype of the **Royal Bangkok Sports Club (ราชกรีฑาสโมสร)** horse-racing website, built for usability testing.

## What it is

- **One file:** [`index.html`](index.html) — HTML + inline CSS + vanilla JS. No frameworks, no CDNs, no build step.
- **4 pages** via hash routing: Landing → Race Day → Race Card → Horse Profile.
- **Bilingual:** TH/EN toggle (swaps nav + headings; body stays Thai).
- **Mock data only** — offline-safe, no backend.

## Run it

Open `index.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 4173
# then open http://localhost:4173
```

## Key features

- **Dual-audience views** — every dense table (Race Card runners, Horse past-performance) toggles between **Cards (อ่านง่าย)** for newcomers and a **dense Table (เปรียบเทียบ)** for insiders.
- **Frozen-column tables** survive 375px mobile with horizontal scroll.
- **Column-group filtering** + color-coded result badges + plain-language tooltips for Selangor-level data density.
- **Cohesive inline-SVG icon set**, green-gold heritage palette, WCAG AA contrast, ≥44px touch targets.

## Design system

[`DESIGN_BIBLE.md`](DESIGN_BIBLE.md) is the single source of truth — tokens, typography, color, spacing, components, and the per-page layout contract that keeps all four pages in sync.

## Source brief

- [`RBSC_Prototype_BuildSpec.md`](RBSC_Prototype_BuildSpec.md) — the spec (pages, data, acceptance criteria)
- [`RBSC_Prototype_VibeCode_Prompts.md`](RBSC_Prototype_VibeCode_Prompts.md) — build order
