# RBSC Racing — Design Bible

> **Single source of truth** for the RBSC prototype. Every page (Landing, Race Day, Race Card, Horse Profile) MUST build against this document. Runtime tokens live in `index.html` `:root` — this file mirrors them 1:1 and adds the *usage rules* and *layout contract* that keep all pages in sync.
>
> **Rule of precedence:** `index.html` `:root` = runtime truth · this bible = human truth · `RBSC_Prototype_BuildSpec.md` §3 = original intent. If any two disagree, fix the code to match the bible and note it here.

---

## 0. Design Principles (the "why" behind every decision)

These five drive every layout/component choice. When unsure, optimize for these:

1. **Dual-audience, one interface.** Newbie and insider use the same screens; complexity is *revealed on demand*, never forced. → drives the Race Card "ง่าย/เต็ม" toggle, tooltips, plain-language form tags.
2. **Mobile-first.** Thai racing traffic is majority mobile. Design at 375px first, enhance up. Dense tables must survive a phone via frozen columns + horizontal scroll.
3. **Scannable density.** Numbers align in columns (monospace), traffic-light color encodes form at a glance, zebra rows. Information-dense ≠ cluttered.
4. **Progressive disclosure (≤2 levels).** Summary first, detail on tap (accordion rows, tooltips, "ดูทั้งหมด"). Never more than two taps to depth.
5. **Information scent / drill-down.** Every entity name (horse/jockey/trainer/date) is a link that leads somewhere predictable. Breadcrumbs on every inner page.

**Anti-goals (do NOT add):** parallax, auto-carousels, decorative animation, silent buttons, full-string i18n, backend/auth, real PDFs/video.

---

## 1. Brand & Voice

- **Identity:** Royal Bangkok Sports Club (ราชกรีฑาสโมสร). Heritage since 1901, racing since 1903. Tone = **official + heritage + trustworthy**, not flashy.
- **Color story:** Deep racing green (authority, turf, tradition) + gold (prestige, the "feature race" accent).
- **Logo lockup:** circular gold "RBSC" mark + name. Always links to `#landing`.
- **Voice:** Thai-primary, plain and direct. Explain jargon inline (tooltips), never assume the reader knows racing terms.

---

## 2. Typography

| Role | Font | Size | Weight | Line-height |
|------|------|------|--------|-------------|
| h1 | IBM Plex Sans Thai | `2rem` | 600 | 1.3 |
| h2 | IBM Plex Sans Thai | `1.5rem` | 600 | 1.3 |
| h3 | IBM Plex Sans Thai | `1.25rem` | 600 | 1.3 |
| body | IBM Plex Sans Thai | `1rem` (16px) | 400 | **1.6** |
| label/medium | IBM Plex Sans Thai | `1rem`/`.875rem` | 500 | 1.5 |
| small | IBM Plex Sans Thai | `.875rem` | 400 | 1.5 |
| table cell | IBM Plex Sans Thai | `.9375rem` | 400 | **1.5** |
| **numbers/form/time/weight/rating** | **IBM Plex Mono** | inherit | 400–500 | inherit |

**Rules:**
- **Never use weight 700** with Thai — it crushes vowel/tone marks. Cap at 600.
- Thai line-height **≥1.55** always (WCAG 1.4.13). Body is 1.6.
- Monospace (`.mono` class) is **only** for: form line, post time, weight, rating, days-since, ratingDelta. Everything else is Sans Thai. This keeps numeric columns aligned for scanning.
- One Google Fonts `<link>` only; system-font fallback for offline testing.

---

## 3. Color System

### Tokens (mirror `:root`)
| Token | Hex | Use |
|-------|-----|-----|
| `--green-900` | `#0F3D2E` | Header/footer bar, headings, table `thead`, primary text on light |
| `--green-700` | `#1B5E43` | Primary buttons, links, active nav |
| `--green-50` | `#E8F2ED` | Soft fills, hover, chip backgrounds |
| `--gold-500` | `#C9A227` | **Accent ONLY** — feature race, key CTA, focus ring |
| `--ink-900` | `#1A1A1A` | Primary body text |
| `--ink-600` | `#5C5C5C` | Secondary/muted text, labels |
| `--line` | `#E2E2E2` | Borders, table rules, dividers |
| `--bg` | `#FFFFFF` | Page background |
| `--bg-alt` | `#F7F8F7` | Zebra rows, flat cards |
| `--good` | `#2E7D32` | Good form (green) |
| `--warn` | `#ED6C02` | Developing form (orange) |
| `--bad` | `#9E9E9E` | Cold/absent form (grey) |

### Usage rules
- **Gold is sacred.** Use only for the single most important CTA on a screen, feature-race emphasis, and focus rings. Never gold body text, never two golds competing on one screen.
- **Green-700 = interactive.** If it's green-700 it should be clickable (button/link). Don't paint static text green-700.
- **Status colors are semantic, not decorative.** `good/warn/bad` only ever mean form quality. Status chips use their own pastel set (upcoming=green tint, live=red tint, done=grey).
- **Traffic-light = `good/warn/bad`** consistently across Race Card form bars, Horse Profile, plain-language tags.

### Contrast (must pass WCAG AA ≥4.5:1 for text)
- ink-900 on bg ✓ · ink-600 on bg ✓ · white on green-900 ✓ · white on green-700 ✓ · green-900 on green-50 ✓ · ink-900 on gold-500 ✓ (gold is a *background* for dark text, never light text on gold).
- **Never** put white/light text on gold-500, or green-700 text on green-50 at small sizes without checking.

---

## 4. Spacing & Layout

**4px base grid** — use tokens, never arbitrary px:
`--s1:4 --s2:8 --s3:12 --s4:16 --s5:24 --s6:32 --s7:48 --s8:64`

| Context | Token |
|---------|-------|
| Inside card padding | `--s4` (16) |
| Between major sections | `--s7` (48), via `.section-gap` |
| Table cell gap/padding | `--s3` (12) |
| Tight inline gaps | `--s2` (8) |
| Page top padding | `--s6` (32) |

- **Container:** `max-width:1200px`, centered, `--s4` side padding (`.container`).
- **Card radius:** `--radius` (10px) default; `--radius-lg` (14px) for hero, primary modules, and dense-table containers. Buttons 8px. Chips 999px (pill).
- **Shadow:** two elevations — `--shadow` (resting cards/tables) and `--shadow-lg` (hero `today` module + hover-lifted cards). `--shadow-lg` is the only "raised" state; don't invent others.

---

## 5. Responsive Breakpoints (§6)

| Range | Behavior |
|-------|----------|
| **<600px** mobile (primary test target) | Nav → hamburger (keep Racing/แข่งวันนี้/ค้นหา prominent); Race Card Simple=cards, Full=frozen-col table + h-scroll; conditions panel 2×2; key-value stacked; search category select hidden |
| **600–1024px** tablet | Horizontal nav (condensed); table shows more columns; 2-col grids |
| **>1024px** desktop | 1200px container; full table; 3-col grids |

**Always test at 375 / 768 / 1280.** The critical case is **Full-view Race Card at 375px** — frozen 3 columns + horizontal scroll must not break.

---

## 6. Component Library

Each shared component already exists in `index.html`. Reuse these classes — do **not** re-style per page.

| Component | Class(es) | When / Rules |
|-----------|-----------|--------------|
| Primary button | `.btn .btn-primary` | Main action (green). ≥44px touch. |
| Gold CTA | `.btn .btn-gold` | The ONE hero CTA per screen ("ดูการแข่งวันนี้"). |
| Ghost button | `.btn .btn-ghost` | Secondary action, bordered. |
| Small button | `.btn-sm` | In-row actions (e.g. "ดู Race Card"). |
| Card | `.card` | Elevated content block (shadow + border). |
| Flat card | `.card-flat` | Subtle grouped block (bg-alt, no shadow). |
| Race-no chip | `.chip` | Race number 1–8, pill, ≥40px, navigates. |
| Status chip | `.chip-status` + `.chip-upcoming/.chip-live/.chip-done` | Meeting/race state. |
| Breadcrumb | `.breadcrumb` (with `.sep`, `.current`) | Top of every inner page. |
| Table | `<table>` (auto-styled: green head, zebra) | Dense data. Mono for numeric cells. |
| Tooltip | `tooltip(text)` JS helper → `.tip` | Append next to any label needing a "?". Click to open, click-away/Esc closes. |
| Traffic-light form bar | `.form-bar` + `span.g/.w/.b` | 6 segments, latest on right. |
| Form tag | `.tag .tag-good/.tag-warn/.tag-bad` | Plain-language: ฟอร์มดี / กำลังพัฒนา / ห่างหาย. |
| Demo badge | `.badge-demo` | Mark dead/out-of-scope links inline. |
| Feature badge | `.badge-feat` | Gold pill + star icon for the feature race. |
| Icon | `icon(name,size)` JS helper → `.ico` | **All icons are inline SVG** (24px viewBox, 1.75 stroke, `currentColor`). **No emoji.** Set via `ICON_PATHS` map. Color inherits from parent; wrap in a coloured/disc container for emphasis (see `.cond-card .cc-ico`). |
| Silks swatch | `.silks` / `.silks-lg` | Jockey colours: pass `style="--sc:#hex"`; renders a swatch with a diagonal sash. |
| Modal | `openModal(title,node)` / `placeholder(msg)` | All placeholder feedback + video. Never silent. |
| Empty state | `.empty-state` / `.search-empty` | No-data states (icon + message), e.g. search no-results. |
| Gradient hero/media | `.gradient-hero` / `.placeholder-media` | All imagery (no external URLs). |

**Layout utilities:** `.flex .between .center .wrap .gap2/3/4 .grid .section-gap .muted .text-gold .mono`.

**Adding a component?** Add it to `index.html` shared CSS **and** this table — never inline one-off styles in a render function.

---

## 7. Interaction & Motion

- **One transition:** `var(--tr)` = `0.18s ease`. Apply to hover/expand/toggle only. No other durations/easings.
- **States required on every interactive element:** hover, active (nav), `:focus-visible` (gold ring — already global). 
- **Touch target ≥44×44px** for every button/link/chip on mobile.
- **Allowed motion:** accordion expand/collapse, tooltip fade, view-toggle, page fade-in (`@keyframes fade`), card hover-lift (`translateY(-2px)` + `--shadow-lg`), button/chip press (`translateY(1px)`). **Nothing else.**
- **Reduced motion:** a global `@media(prefers-reduced-motion:reduce)` disables all transitions/animations/lifts. Keep new motion inside this guard.
- **Placeholders give feedback:** PDF / video / tickets / stream / dead links → `placeholder()` or `openModal()`. Never a dead click.

---

## 8. Content & Bilingual (TH/EN) Rules

- **Body content stays Thai.** The TH/EN toggle swaps **nav items + section headings only** (`.i18n` nodes with `data-th`/`data-en`).
- To make text translatable: add `class="i18n" data-th="…" data-en="…"`. `setLang()` swaps `innerHTML` for the active lang.
- Headings on each page should carry `.i18n` so the toggle visibly works. Tables, body copy, data = Thai only.
- Numbers/dates use Thai conventions in body but stay digit-based (mono) in tables.

---

## 9. Page Layout Contract (keeps all 4 pages consistent)

Every inner page (Race Day / Race Card / Horse) follows this skeleton top-to-bottom:

```
[Breadcrumb]            ← .breadcrumb, always, links back up the flow
[Page header block]     ← h1/h2 (.i18n) + key meta row + status chip
[⭐ Primary module]      ← the page's reason-for-existing, visually dominant
[Secondary content]     ← supporting sections, .section-gap between each
[Related / actions]     ← drill-down links, placeholder CTAs
```

**Flow (forward-linking only):**
`Landing → Race Day → Race Card → Horse Profile`, every page links back via breadcrumb + logo.

**Per-page ⭐ primary module (the thing that must dominate):**
- Landing → "แข่งวันนี้/วันแข่งถัดไป" module
- Race Day → conditions panel (weather/going)
- Race Card → view toggle + runner table/cards
- Horse → past-performance table

---

## 10. Mock Data Conventions (§5)

One small reusable set, rich only where the test needs it:
- `DATA.meeting` — 1 day, 8 races (summary each), weather, going, status, runnersToday.
- `DATA.raceDetail` — race 3, 10 runners (full fields). Other races reuse/derive.
- `DATA.horse` — 1 fully-detailed horse (form[6], ratingHistory[8], stewards).
- `DATA.news[3]`, `DATA.searchMock[3]`.

**Field naming is fixed** (so pages share data): `nameTH/nameEN, jockey, trainer, weight, gate, rating, ratingDelta, daysSince, gear, form (6 chars), formTag (good|warn|bad)`. Reuse these keys everywhere — don't rename per page.

**Form string encoding:** 6 chars, latest on the right. `1`=win … `9`=9th, `0`=10th-or-worse. Tooltip must explain "5=ที่5, 0=ที่10+".

---

## 11. Accessibility Checklist (per page, every page)

- [ ] All text contrast ≥4.5:1 (see §3 approved pairs)
- [ ] Every button/link/chip ≥44×44px on mobile
- [ ] Visible focus ring (gold) on all interactive elements — global, don't override
- [ ] Color never the *only* signal (form bar pairs color with position + plain-language tag)
- [ ] Tooltips reachable by keyboard (they're `<button>`s)
- [ ] Headings in logical order (one h1 per page)
- [ ] Tables have real `<th>` headers

---

## 12. Per-Page Consistency Checklist (run before closing each page's gate)

For P1–P4, confirm the page:
- [ ] Uses **only** tokens from §3–§4 (no hardcoded hex/px)
- [ ] Reuses §6 components (no one-off restyled buttons/cards/tables)
- [ ] Follows the §9 layout skeleton (breadcrumb → header → ⭐module → secondary)
- [ ] Has exactly **one** gold CTA, used for the primary action
- [ ] Headings are `.i18n` (TH/EN toggle works)
- [ ] All entity names link forward; breadcrumb links back
- [ ] All placeholders fire `placeholder()`/modal (never silent)
- [ ] Passes the §11 a11y checklist
- [ ] Works at 375 / 768 / 1280

---

*Source files: `index.html` (`:root` tokens + shared CSS/JS), `RBSC_Prototype_BuildSpec.md` (§3 design system, §4 pages), `RBSC_Prototype_VibeCode_Prompts.md` (build order). Update this bible whenever a shared token/component changes.*
