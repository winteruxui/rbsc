# MRC Racing — Design Bible

> **Single source of truth** for the MRC racing prototype. Every page (Home, Race Day hub,
> Race Card, Horse Profile — and the forward-spec pages: Jockey, Trainer, Horse Directory,
> Event Calendar) MUST build against this document. Runtime tokens live in `index.html`
> `:root` — this file mirrors them 1:1 and adds the *usage rules*, *layout contract*, and
> *narrative patterns* that keep all pages in sync.
>
> **Rule of precedence:** `index.html` `:root` = runtime truth · this bible = human truth.
> If the two disagree, fix the code to match the bible and note it here.
>
> **Companion docs:** §13 Reference Library distills the 22 reference screenshots in `Ref/`.
> `ADAPTATION_BACKLOG.md` turns §13's lessons into prioritized, source-cited work items.

---

## 0. Design Principles (the "why" behind every decision)

Seven principles drive every layout/component choice. When unsure, optimize for these:

1. **Identity → Verdict → Evidence → Action.** The universal content spine seen in *every*
   strong reference. Lead with *what this is* (name, silks, conditions), give the *at-a-glance
   judgement* (rating, going, status, strike-rate), then the *dense evidence* (runner table,
   form history), then *drill-down + tools*. Order every page this way.
2. **Conditions frame the data.** Going / distance / class / weather appear **before** runner
   data, because they change how every number is read. Never bury the conditions below the table.
3. **Storytelling over raw data.** Numbers alone are complete but lifeless (HKJC). Premium feel
   comes from a narrative layer *on top of* the data — rating-trend charts, season standings,
   stat bands, editorial. See §10. **This is the prototype's highest-leverage area.**
4. **Dual-audience, one interface.** Newbie and insider share screens; complexity is *revealed
   on demand*, never forced. → the Race Card "ง่าย/เต็ม" toggle, tooltips, plain-language form tags.
5. **Mobile-first, scannable density.** Design at 375px first. Numbers align in monospace
   columns; traffic-light color encodes form at a glance; dense tables survive a phone via
   frozen columns + horizontal scroll. Information-dense ≠ cluttered.
6. **Progressive disclosure (≤2 levels).** Summary first, detail on tap — accordion rows,
   tooltips, sub-tabs, "ดูทั้งหมด". Never more than two taps to depth.
7. **Information scent / drill-down.** Every entity name (horse / jockey / trainer / sire / dam
   / date) is a link that leads somewhere predictable. Breadcrumbs on every inner page.

**Anti-goals (do NOT add):** parallax, auto-playing video, decorative animation, silent
buttons, full-string i18n, backend/auth, real PDFs. *(Note: a slow, dot-controlled hero text
rotator IS allowed — it is the one sanctioned carousel; see §7.)*

---

## 1. Brand & Voice

- **Identity:** **Mockup Racing Club (MRC)** — anonymized stand-in (ก่อตั้ง 1901 · จัดการแข่งม้าตั้งแต่
  1903). The prototype is shown to test users **without revealing the real client**, so no real
  club name, logo, or place name may appear. Tone = **official + heritage + trustworthy**, not flashy.
- **Color story:** Deep **navy** (authority, tradition, night-at-the-races) + **gold** (prestige,
  the "feature race" accent). Navy replaces the original racing-green; the CSS var names are
  still `--green-*` for historical reasons but hold **navy** values — do not be misled by the name.
- **Logo lockup:** circular gold "MRC" mark + name. Always links to `#landing`.
- **Voice:** Thai-primary, plain and direct. Explain jargon inline (tooltips); never assume the
  reader knows racing terms. Headlines may use an elegant serif + gold underline for heritage feel.

---

## 2. Typography

The prototype pairs a **Thai serif** for display/body with a **mono** for all numerics — a
heritage-but-legible voice. One Google Fonts `<link>`:
`IBM Plex Mono (400;500;600)` + `Noto Serif Thai (400;500;600;700)`.

| Role | Font | Size | Weight | Line-height |
|------|------|------|--------|-------------|
| h1 | Noto Serif Thai | `2rem` | 600 | 1.3 |
| h2 | Noto Serif Thai | `1.5rem` | 600 | 1.3 |
| h3 | Noto Serif Thai | `1.25rem` | 600 | 1.3 |
| body | Noto Serif Thai | `1rem` (16px) | 400 | **1.6** |
| label/medium | Noto Serif Thai | `1rem`/`.875rem` | 500 | 1.5 |
| small | Noto Serif Thai | `.875rem` | 400 | 1.5 |
| table cell | Noto Serif Thai | `.9375rem` | 400 | **1.5** |
| **numbers / form / time / weight / rating / dates-in-tables** | **IBM Plex Mono** | inherit | 400–600 | inherit |

**Rules:**
- **Cap Thai text at weight 600.** Heavier crushes Thai vowel/tone marks. Weight 700 is allowed
  **only** for Latin/numeric accents (badges, the gold date-pill number) — never Thai sentences.
- Thai line-height **≥1.55** always (WCAG 1.4.13). Body is 1.6, tables 1.5 (acceptable for short cells).
- Monospace (`--font-mono`) is **only** for: form line, post time, weight, rating, days-since,
  ratingDelta, table date columns, race numbers. Everything else is Noto Serif Thai. This keeps
  numeric columns aligned for scanning.
- **Serif H1 lockup (heritage):** page H1 may carry an uppercase eyebrow above + a short gold
  underline rule below (`--gold-500`), echoing the reference heritage headers (Bahrain). Optional
  but reserved for top-level page titles, never sub-headings.

---

## 3. Color System

### Tokens (mirror `:root` — names say `green` but hold **navy**)
| Token | Hex | Use |
|-------|-----|-----|
| `--green-900` | `#264068` | Header/footer bar, headings, table `thead`, primary text on light, hero base |
| `--green-700` | `#30528A` | Primary buttons, links, active nav |
| `--green-50` | `#EAEEF5` | Soft fills, hover, chip/icon backgrounds |
| `--gold-500` | `#BF942D` | **Accent ONLY** — feature race, the ONE key CTA, focus ring, H1 rule |
| `--ink-900` | `#1A1A1A` | Primary body text |
| `--ink-600` | `#5C5C5C` | Secondary/muted text, labels |
| `--line` | `#D9D5CF` | Borders, table rules, dividers (warm grey) |
| `--bg` | `#FFFFFF` | Page background |
| `--bg-alt` | `#F6F5F3` | Zebra rows, flat cards (warm off-white) |
| `--good` | `#2E7D32` | Good form (green) |
| `--warn` | `#ED6C02` | Developing form (orange) |
| `--bad` | `#9E9E9E` | Cold/absent form (grey) |

### Usage rules
- **Gold is sacred.** Use only for the single most important CTA on a screen, feature-race
  emphasis, focus rings, and the H1 underline rule. Never gold body text; never two golds
  competing on one screen.
- **Green-700 (navy-700) = interactive.** If it is `--green-700`, it should be clickable. Don't
  paint static text with it.
- **Status colors are semantic, not decorative.** `good/warn/bad` only ever mean form quality.
  Meeting/race status chips use their own pastel set (upcoming=tint, live=red tint, done=grey).
- **Traffic-light = `good/warn/bad`** consistently across Race Card form bars, Horse Profile,
  rating charts, and plain-language tags.
- **Navy-tinted shadows.** `--shadow` / `--shadow-lg` use navy rgba, not neutral black — keeps
  elevation on-brand.

### Contrast (must pass WCAG AA ≥4.5:1 for text)
- ink-900 on bg ✓ · ink-600 on bg ✓ · white on green-900 ✓ · white on green-700 ✓ ·
  green-900 on green-50 ✓ · ink-900 on gold-500 ✓ (gold is a *background* for dark text).
- **Never** put white/light text on gold-500, or green-700 text on green-50 at small sizes
  without checking.

---

## 4. Spacing & Layout

**4px base grid** — use tokens, never arbitrary px:
`--s1:4 --s2:8 --s3:12 --s4:16 --s5:24 --s6:32 --s7:48 --s8:64`

| Context | Token |
|---------|-------|
| Inside card padding | `--s4` (16) / `--s5` (24) for primary modules |
| Between major sections | `--s7` (48), via `.section-gap` |
| Table cell gap/padding | `--s3` (12) |
| Tight inline gaps | `--s2` (8) |
| Page top padding | `--s6` (32) |

- **Container:** `--maxw:1200px`, centered, `--s5` side padding (`.container`); `--s4` at ≤600px.
- **Card radius:** `--radius` (10px) default; `--radius-lg` (14px) for hero, primary modules,
  dense-table containers. Buttons 8px. Chips/pills 999px.
- **Shadow:** two elevations only — `--shadow` (resting cards/tables) and `--shadow-lg` (primary
  modules + hover-lift). Don't invent others.
- **Full-bleed banner pattern:** an element inside `.container` breaks out to the viewport edge
  with `width:var(--vw,100vw); margin-left/right:calc(50% - var(--vw,100vw)/2)`. `--vw` is set in
  JS to `document.documentElement.clientWidth` (excludes the scrollbar — `100vw` includes it and
  causes a sliver gap/scroll). `body{overflow-x:clip}` guards against horizontal scroll while
  preserving the sticky header. This is how the Home hero spans edge-to-edge.

---

## 5. Responsive Breakpoints

| Range | Behavior |
|-------|----------|
| **<600px** mobile (primary test target) | Nav → hamburger; hero stacks (lead → today-card → next-strip); Race Card Simple=cards, Full=frozen-col table + h-scroll; conditions panel 2×2; key-value stacked; news/dual grids 1-col; shortcut grid 2-col |
| **600–860px** small tablet | Hero collapses to single column (lead over card); `home-cols` 1-col |
| **860–1024px** tablet | Horizontal nav; 2-col grids (news, conditions); more table columns |
| **>1024px** desktop | 1200px container; full tables; multi-col grids; hero 2-col |

**Always test at 375 / 768 / 1280.** Two critical cases: **Full-view Race Card at 375px**
(frozen 3 columns + horizontal scroll must not break) and **full-bleed hero alignment** (no
horizontal scroll; banner flush to both viewport edges — re-measure `--vw` after the scrollbar
appears, see §4).

---

## 6. Component Library

Each shared component lives in `index.html`. Reuse these — do **not** re-style per page.

| Component | Class(es) / helper | When / Rules |
|-----------|--------------------|--------------|
| Primary button | `.btn .btn-primary` | Main action (navy). ≥44px touch. |
| Gold CTA | `.btn .btn-gold` | The ONE gold CTA per screen. |
| Ghost button | `.btn .btn-ghost` | Secondary, bordered. |
| Small button | `.btn-sm` | In-row actions ("ดู Race Card"). |
| Large button | `.btn-lg` | Hero CTA. |
| Card / Flat card | `.card` / `.card-flat` | Elevated vs subtle grouped block. |
| Meta strip | `metaItem()` → `.meta-strip` (+`.bare`) | Icon + label + value grid for scannable header facts (race conditions, horse identity). `accent` flag = gold value. |
| Race-no tabs | `.race-tabs button` | Race 1–N selector; reused for **date tabs** (race-day hub) and could host **month tabs**. `.on` active, `.feat` gold border for today/feature. |
| Chip / chips row | `.chip`, `.chips-row` | Race-number quick links; horizontal-scroll row. |
| Status chip | `statusChip()` → `.chip-status` variants | Meeting/race state (upcoming/live/done). |
| Horse status chip | `horseStatusChip()` → `.horse-status-chip.hs-*` | fit/scratched/spell/first-up/vet-watch. |
| Breadcrumb | `breadcrumb()` → `.breadcrumb` | Top of every inner page; links back up the flow. |
| Table | `<table>` / `.rc-table` / `.pp-dense` | Dense data; navy head, zebra, mono numeric cells, frozen left columns on mobile. |
| Tooltip | `tooltip(text)` → `.tip` | "?" next to any jargon label. Keyboard-reachable (`<button>`). |
| Traffic-light form bar | `.form-bar` + `span.g/.w/.b` | 6 segments, latest on right. |
| Form tag | `.tag .tag-good/.tag-warn/.tag-bad` | ฟอร์มดี / กำลังพัฒนา / ห่างหาย. |
| Conditions card | `condCard()` → `.cond-card` / `.conditions-grid` | Weather/going/surface/temp; reused by health panel. |
| Fixture row | `.fixture` / `.up-item` / `.hn-pill` | Upcoming race-day list items (date box + name + meta). |
| Demo / Feature badge | `.badge-demo` / `.badge-feat` | Mark dead links / gold feature-race pill. |
| Icon | `icon(name,size)` → `.ico` | **Inline SVG only** (24px viewBox, `currentColor`); `ICON_PATHS` map. **No emoji.** |
| Silks swatch | `.silks` / `.silks-lg` | Jockey colours via `style="--sc:#hex"`. The visual ID anchor — use in every runner/horse row. |
| Modal / placeholder | `openModal()` / `placeholder()` | All placeholder feedback. Never a silent click. |
| Empty state | `.empty-state` / `.search-empty` | No-data states (icon + message). |

**Layout utilities:** `.flex .between .center .wrap .gap2/3/4 .grid .section-gap .muted .text-gold`.

### Components to ADD (forward — see §10 & backlog)
| New component | Purpose | Source ref |
|---------------|---------|------------|
| **Rating/form line-chart** | SVG polyline of rating-over-time on Horse/Jockey profile — the key narrative device. | Abu Dhabi Ref 4, Bahrain Ref 6 |
| **Stat band / badges** | "Season at a glance" big-number row (race days · horses · prize). | Bahrain Ref 2 |
| **Sub-tabs** | Layered depth on one entity (รายชื่อ/วิเคราะห์/ผลย้อนหลัง). | Bahrain Ref 5 |
| **Calendar-grid widget** | Month grid with race-days highlighted → click-to-day. | Bahrain Ref 2, HKJC Ref 3 |
| **Month accordion** | Compress a full season of fixtures. | Bahrain Ref 4 |
| **Standings table** | Jockey/trainer leaderboard. | Bahrain Ref 2 |
| **Profile stat-strip** | wins/runs/places/strike-rate verdict row. | Bahrain Ref 7 |

**Adding a component?** Add it to `index.html` shared CSS **and** this table — never inline a
one-off in a render function.

---

## 7. Interaction & Motion

- **One transition:** `--tr` = `0.18s ease`. Hover/expand/toggle only.
- **States required:** hover, active (nav), `:focus-visible` (gold ring — global).
- **Touch target ≥44×44px** for every button/link/chip on mobile.
- **Allowed motion:** accordion expand/collapse, tooltip fade, view-toggle, page fade-in,
  card hover-lift (`translateY(-2px)` + `--shadow-lg`), button/chip press (`translateY(1px)`),
  and the **hero text rotator** — slow (≥5s), dot- and arrow-controlled, pauses-on-interaction,
  cross-fade/slide of the *headline text only*. **Nothing else.**
- **Reduced motion:** global `@media(prefers-reduced-motion:reduce)` disables transitions/lifts.
  Keep new motion (incl. the hero rotator's auto-advance) inside this guard.
- **Placeholders give feedback:** PDF / video / tickets / dead links → `placeholder()`/`openModal()`.

---

## 8. Content & Bilingual (TH/EN) Rules

- **Body content stays Thai.** The TH/EN toggle swaps **nav items + section headings only**
  (`.i18n` nodes with `data-th`/`data-en`).
- To make text translatable: add `class="i18n" data-th="…" data-en="…"`. `setLang()` swaps
  `innerHTML` for the active lang. Re-run `setLang()` after any dynamic re-render.
- Headings on each page carry `.i18n` so the toggle visibly works. Tables, body copy, data = Thai.
- Numbers/dates use Thai conventions in prose but stay digit-based (mono) in tables.

---

## 9. Page Layout Contract

Every inner page follows this skeleton, expressing **Identity → Verdict → Evidence → Action**:

```
[Breadcrumb]            ← always, links back up the flow
[Navigation strip]      ← where the page is one-of-many (race tabs, date tabs, month tabs)
[Page header block]     ← H1 (.i18n, serif + gold rule) + conditions/meta strip + status chip
[⭐ Primary module]      ← the page's reason-for-existing, visually dominant (the "verdict/evidence")
[Secondary content]     ← supporting sections, .section-gap between each
[Narrative layer]       ← chart / standings / editorial (see §10) where it adds a story
[Related / actions]     ← drill-down links, placeholder CTAs
```

**Flow (forward-linking, with new drill-downs):**
`Home → Race Day hub → Race Card → Horse Profile → (Jockey / Trainer Profile)`
and `Home → Horse Directory → Horse Profile`. Every page links back via breadcrumb + logo.

**Per-page ⭐ primary module + narrative layer:**
| Page | Built? | ⭐ Primary module | Narrative layer (§10) |
|------|--------|-------------------|------------------------|
| **Home** | ✅ | Full-bleed hero (today card + next-days pills) | Season stat-band + standings + feature story *(add)* |
| **Race Day hub** | ✅ | Date tabs + today's programme | Calendar-grid + month accordion for full season *(add)* |
| **Race Card** | ✅ | View toggle + runner cards/table | Conditions header (have) + Past Winners + evaluation sub-tab *(add)* |
| **Horse Profile** | ✅ | Past-performance table + form bars | **Rating/form line-chart** + filters *(add — top priority)* |
| **Jockey Profile** | ⬜ forward | Stat-strip + rides table | CAREER/SEASON tabs + strike-rate trend |
| **Trainer Profile** | ⬜ forward | Stat-strip + runners table | Same as jockey |
| **Horse Directory** | ⬜ forward | Searchable horse table (Quick-Finder) | — (entry/listing page) |
| **Event Calendar** | ⬜ forward | Calendar-grid + day timeline | Filter bar (month/class/feature) |

---

## 10. Storytelling & Narrative Patterns (NEW)

The difference between "complete" (HKJC) and "premium" (Bahrain, Abu Dhabi) is a **narrative
layer on top of the data**. Use these patterns deliberately — each answers a *question a human
is actually asking*, not just "what's the data".

| Pattern | Answers | Where | Source |
|---------|---------|-------|--------|
| **Rating/form line-chart** | "Is this horse/jockey *on the up*?" | Horse & Jockey profile, after the verdict strip | Abu Dhabi Ref 4, Bahrain Ref 6 |
| **Season stat-band** (big numbers) | "How big is *this season*?" | Home, between utility and news | Bahrain Ref 2 ("13") |
| **Standings / leaderboard** | "Who's *winning the season*?" | Home; links into Jockey/Trainer profiles | Bahrain Ref 2 |
| **Editorial / feature preview** | "What should I *care about* today?" | Home (promote 1 news item to a hero story); Race-Card evaluation tab | Bahrain editorial, Selangor news |
| **Past Winners** | "What *kind of horse* wins this race?" | Race Card, below runners | BHA Ref 3 |
| **Results tracker / form filters** | "Show me only the *wins*." | Horse profile past-performance | Abu Dhabi Ref 4 |

**Rules for the narrative layer:**
- It **augments**, never replaces, the data table. Chart first *as a summary*, table as evidence.
- Charts are **inline SVG** (no chart library), traffic-light colored, with accessible text
  fallback (a caption stating the trend, e.g. "เรตติ้งขยับขึ้น +8 ใน 6 เรซล่าสุด").
- Keep it **honest to the mock data** — derive charts from existing `form[]`/`ratingHistory[]`,
  don't invent trends the table contradicts.
- One narrative module per concern; don't stack three charts. Progressive disclosure still applies.

---

## 11. Mock Data Conventions

One small reusable set, rich only where the test needs it:
- `DATA.meeting` — 1 day, 8 races (summary each), weather, going, status, runnersToday.
- `DATA.raceDetail` — race 3, 10 runners (full fields, incl. `status`/`scratchReason`).
- `DATA.horse` — 1 fully-detailed horse (`form[6+]`, `status`, `stewards`).
- `DATA.fixtures[]` — upcoming race days (date-tab + calendar source).
- `DATA.news[]`, `DATA.shortcuts[]`, `DATA.searchMock[]`.

**Fixed field naming** (so pages share data): `nameTH/nameEN, jockey, trainer, weight, gate/bar,
rating, ratingDelta, daysSince, gears, form (6 chars), formTag (good|warn|bad)`. Reuse everywhere.

**Form string encoding:** 6 chars, latest on the right. `1`=win … `9`=9th, `0`=10th-or-worse.
Tooltip explains "5=ที่5, 0=ที่10+".

**Extend for forward pages (§9):** add `ratingHistory[]` (date→rating, for the §10 chart),
`DATA.jockey` / `DATA.trainer` (stat-strip + rides/runners), `DATA.standings[]` (leaderboard),
`DATA.horses[]` (directory). Keep the same key names across pages — don't rename per page.

---

## 12. Accessibility Checklist (per page, every page)

- [ ] All text contrast ≥4.5:1 (see §3 approved pairs)
- [ ] Every button/link/chip ≥44×44px on mobile
- [ ] Visible focus ring (gold) on all interactive elements — global, don't override
- [ ] Color never the *only* signal (form bar pairs color with position + plain-language tag;
      charts carry a text caption)
- [ ] Tooltips reachable by keyboard (they're `<button>`s)
- [ ] Headings in logical order (one h1 per page)
- [ ] Tables have real `<th>` headers; frozen-column tables still announce headers
- [ ] Hero rotator pausable; auto-advance disabled under reduced-motion
- [ ] No horizontal page scroll at 375px (full-bleed banner clipped, not scrollable)

---

## 13. Reference Library & Rationale

Distilled from `Ref/` (22 screenshots, 6 authorities). For each: **emulate** ✅ / **avoid** ⚠.
The universal spine (§0.1) and conditions-first law (§0.2) hold across all of them.

| Source | Identity | ✅ Emulate | ⚠ Avoid |
|--------|----------|-----------|---------|
| **Bahrain Turf Club** | Navy + gold + red, serif headers | Closest to our identity. Season **standings**, **"13" stat band**, **rating line-chart** (Ref 6), clean **jockey profile** (Ref 7: CAREER/SEASON tabs + stat-strip + rides), **calendar + day-timeline** (Ref 2), **month accordion** (Ref 4), race-card **ENTRIES/EVALUATION/EDITORIAL sub-tabs** (Ref 5) | Red sometimes fights the gold — keep red minimal, gold reserved |
| **Abu Dhabi / Emirates** | Modern light | **Best horse profile (Ref 4):** stat badges + **form line-chart** + WIN/RUNNER-UP **filters** + per-row **video**; runner **cards with greyed scratchings** (Ref 2 — our model) | — |
| **BHA (British)** | Navy + red, left sidebar | Cleanest **fixture/results lists** + **filter bar** (Month/Course/Type) (Ref 5); **Going+Weather band** + **Past Winners** on race card (Ref 3); section sidebar wayfinding | Utilitarian / low warmth — we add heritage serif + gold |
| **Selangor Turf Club** | Purple/red | **Shortcut icon grid**, **race-day fixtures rail**, race-news column, date box (Ref 7 — our race-card origin) | Cluttered, dated nav; too many competing home modules bury the CTA |
| **HKJC (Hong Kong)** | Dated blue | Total **data completeness** + information scent; calendar + country/category filters (Ref 3) | **Density anti-pattern:** wall of tiny text, no hierarchy, chart-less, expert-only, poor mobile, training-calendar micro-grid (Ref 4) |
| **France Galop** | Green | Elegant **pagination-dot** race nav (Ref 6); very compact tables | Low contrast; no visual ID beyond silks |

**One-line takeaway:** build the **data layer like BHA/Abu Dhabi (clean, conditions-first,
silks, cards-on-mobile)**, wear the **skin of Bahrain (navy+gold serif heritage)**, and add the
**narrative layer Abu Dhabi + Bahrain prove out (line-charts, standings, stat bands)** — while
refusing **HKJC's undifferentiated density**.

---

## 14. Per-Page Consistency Checklist

Before closing each page's gate, confirm it:
- [ ] Uses **only** tokens from §3–§4 (no hardcoded hex/px)
- [ ] Reuses §6 components (no one-off restyled buttons/cards/tables)
- [ ] Follows the §9 skeleton (breadcrumb → nav → header → ⭐module → narrative → actions)
- [ ] Orders content **Identity → Verdict → Evidence → Action**, conditions before data
- [ ] Has exactly **one** gold CTA, used for the primary action
- [ ] Adds a **narrative module** (§10) where it answers a real question — not data for its own sake
- [ ] Headings are `.i18n` (TH/EN toggle works after re-render)
- [ ] All entity names link forward; breadcrumb links back; no dead clicks
- [ ] Passes the §12 a11y checklist
- [ ] Works at 375 / 768 / 1280
- [ ] No real client name/logo/place leaks (MRC anonymization holds)

---

*Source files: `index.html` (`:root` tokens + shared CSS/JS), `Ref/**` (reference screenshots,
§13), `ADAPTATION_BACKLOG.md` (prioritized adoption of §13 lessons). Update this bible whenever a
shared token/component changes — code is runtime truth, this is human truth.*
