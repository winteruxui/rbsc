# RBSC Prototype — Session Handoff

_Last updated: 2026-06-14. Read this first, then continue._

## Project
- **Deliverable:** `index.html` — single-file, offline, 4-page RBSC horse-racing prototype (Landing, Race Day, Race Card, Horse Profile). Vanilla JS hash routing, bilingual TH/EN, mock data.
- **Design system:** `DESIGN_BIBLE.md` (single source of truth). Brief: `RBSC_Prototype_BuildSpec.md`, `RBSC_Prototype_VibeCode_Prompts.md`.

## Status
- **P0–P5 complete & verified.** All 4 pages built; P5 QA passed all 10 BuildSpec §8 criteria (responsive 375/768/1280, AA contrast, ≥44px touch, TH/EN, drill-downs, placeholders).
- **Dual-mode dense tables** (Cards/Table + frozen cols + column-group chips + color badges) on both Race Card and Horse Profile.
- **Git:** separate repo in this folder (NOT home dir). Remote `origin` = https://github.com/winteruxui/rbsc.git, branch `main`. Pushed commit `d2fdab5` = **Phase 1 state only**.

## Design Audit (skill: design-audit) — ✅ ALL 3 PHASES COMPLETE
User approved **all 3 phases**, **"refine within current identity"** (keep green-gold). All implemented, verified (desktop + 375px, no console errors), and pushed.

- **Phase 1 — DONE & verified & pushed.** Fixed horse-hero photo overlap (removed `.placeholder-media` from `.horse-photo`); replaced ALL emoji with inline-SVG icon system (`icon(name,size)` helper + `ICON_PATHS`); recomposed Landing hero (left stack + horseshoe motif + deeper gradient + `.btn-lg`). Added tokens `--radius-lg`, `--shadow-lg`; classes `.badge-feat`, `.inline-link`, `.scroll-hint`, `.ico`.

- **Phase 2 — DONE & verified.** Silks → swatch+diagonal-sash (`--sc` var); `.rc-table-wrap`/`.pp-scroll` → `--radius-lg` + shadow + row hover, `--s3` cell padding; `.cond-card` → `--radius-lg`, hover lift, icon in 48px white disc, value `1.375rem`; `.today` → `--shadow-lg`; refined `horse` icon to a clean filled head silhouette (verified, kept).

- **Phase 3 — DONE & verified.** Card hover-lift (`.rc-card/.pp-card/.audience-card/.news-card` → `translateY(-2px)` + `--shadow-lg`, gold inset preserved on feat/win), button/chip press (`translateY(1px)`), on-brand `::selection`, slim themed scrollbars on dense tables, empty state (`.empty-state`/`.search-empty` + `search-off` icon on search no-results), and a global `@media(prefers-reduced-motion:reduce)` guard (the preview env emulates reduce, so lifts are off there by design).

## Done
- DESIGN_BIBLE.md updated: `--radius-lg`, `--shadow-lg`, icon system, `.badge-feat`, silks, empty state, motion + reduced-motion rules.
- Optional **dark mode** still NOT done (deferred — needs full token mirror + re-test).

## Environment notes
- After editing `index.html`: `cp index.html /tmp/rbsc_preview/index.html` to sync the preview copy.
- Preview: `mcp__Claude_Preview__preview_start` name **`rbsc-static`**, port 4173, serves `/tmp/rbsc_preview` via `/tmp/rbsc_serve.py` (sends no-cache headers). **Flaky — restart if "Server not found".** macOS sandbox blocks serving directly from `Documents/`.
- Preview `innerWidth` quirk: reports ~476 at "mobile" preset but renders 375px visually (confirm via screenshot, not innerWidth).
- Syntax check before sync: `node -e "...new Function(scriptBody)..."`.
- User does **Cmd+Shift+R** first load to bust browser cache.
- **NEVER** push the home-dir git repo (`/Users/patiphans`) — it would publish the whole home folder.

## Tooling reminders
- Output style = explanatory: include `★ Insight ──` educational blocks.
- Project language: replies in Thai when user writes Thai.
