# RBSC Racing Website — Vibe-Code Prompt Pack (Production-Ready)

> **What this is:** Copy-paste prompts to build the 4-page RBSC prototype with a vibe-code tool (Claude, Cursor, v0, etc.). Designed to **plan first, build one page at a time**, and **minimize token cost**.
>
> **Companion files:** `RBSC_Prototype_BuildSpec.md` (the spec — attach it), `RBSC_UX_Research_TH.md` (research, reference only).
>
> **How to use:** Run prompts in order P0 → P1 → P2 → P3 → P4 → P5. Attach the BuildSpec once at P0; after that, reference it by section number (e.g. "spec §4.2") instead of re-pasting. See the cost guide at the bottom.

---

## Locked decisions (from BuildSpec §7)

These are fixed for this build — bake them into every prompt:

- **Language:** TH/EN toggle switches **nav + headings only**; body stays Thai. (saves ~30% tokens vs full-string i18n)
- **Color:** Green-gold heritage palette (BuildSpec §3.3)
- **Race Card default view:** **"Simple"** (test that newbies understand; insiders toggle to "Full")
- **Races:** 8
- **Images:** **Gradient placeholders only** (no external image URLs — keeps it offline-safe for user testing)
- **Page scope:** **4 pages only.** Leaderboard / Results / People profiles = dead links with a "outside prototype scope" tooltip.

---

## Global rules block (paste at the top of EVERY build prompt)

```
GLOBAL RULES (apply to all output):
- One file: index.html with inline <style> and <script>. Vanilla JS only. No frameworks, no CDNs, no build step.
- Hash routing: #landing #raceday #racecard #horse. One showPage() swaps sections.
- Fonts: IBM Plex Sans Thai (body+UI) + IBM Plex Mono (numbers/form/time only), via one Google Fonts <link>; system-font fallback.
- Design tokens as CSS :root vars exactly per spec §3 (spacing 4px grid, green-gold palette, line-height TH ≥1.55, transitions 0.18s ease only).
- Mobile-first. Touch targets ≥44px. Visible focus rings. Contrast AA. No parallax/auto-carousel/decorative animation.
- TH/EN toggle switches nav + headings only; body Thai.
- Placeholder buttons (PDF / video / tickets) must give feedback (alert or modal), never silent.
- Keep comments minimal. Do not regenerate code you've already written — only output what each step asks for.
```

---

## P0 — PLAN FIRST (no code yet)

> Goal: lock architecture + shared shell so later pages slot in without rework. Cheapest possible planning pass.

```
You are building a production-ready clickable prototype of the RBSC horse-racing website.
Attached: RBSC_Prototype_BuildSpec.md — follow it exactly.

[paste GLOBAL RULES block]
[paste Locked decisions]

STEP 0 — PLANNING ONLY. Do NOT write the full app yet. Output only:

1. File skeleton: the <head> (fonts, full :root design tokens from spec §3, base/reset CSS, shared component CSS — buttons, cards, tables, tooltip, breadcrumb, nav) and an empty <body> with: sticky <header> (logo, 6-item nav, TH/EN toggle, typed search), 4 empty <section id> page containers, <footer>.
2. The JS shell: DATA object stub (keys only, per spec §5 — no full values yet), showPage() hash router, TH/EN toggle fn, tooltip helper, $ qs helper.
3. A 1-line confirmation of the build order: Landing → RaceDay → RaceCard → Horse.

This skeleton must be COMPLETE and reusable so each later page only adds its own render function + its slice of DATA. Keep it tight.
```

**✅ Review gate:** tokens defined? shell renders blank pages without errors? nav + toggle present? → proceed.

---

## P1 — Landing page (spec §4.1)

```
[paste GLOBAL RULES block]

STEP 1 — Build ONLY the Landing page (spec §4.1). Add to the existing skeleton:
- Its DATA slice: meeting summary (date, track, weather, going, status, 8 race chips), news[3], searchMock[3].
- renderLanding(): Hero (gradient + Z-layout + "ดูการแข่งวันนี้" CTA), "แข่งวันนี้/วันแข่งถัดไป" module (header card with weather+going+race count+first post, horizontal race-number chips → racecard, "ดู Race Day แบบเต็ม" → raceday, PDF placeholder), the two dual-audience cards ("มือใหม่หัดดู?" / "ฟอร์ม & ข้อมูล"), latest-news cards, footer heritage line.
- Wire: hamburger, TH/EN, chips, all CTAs, search dropdown (mock 2-3 results).

Output ONLY: the Landing DATA slice + renderLanding() + any Landing-specific CSS. Do not reprint the skeleton.
```

**✅ Review gate:** hero + "today's racing" module + dual cards + news render; chips/CTAs navigate; mobile hamburger works.

---

## P2 — Race Day Detail (spec §4.4)

```
[paste GLOBAL RULES block]

STEP 2 — Build ONLY the Race Day Detail page (spec §4.4). Add:
- DATA: meeting.races[8] (each: no, time, name, class, dist, runnerCount, status) + runnersToday list.
- renderRaceDay(): breadcrumb; meeting header + status chip; ⭐ conditions panel (weather/going/surface/temp as icon cards, 2×2 on mobile); day-at-a-glance race list (each row → racecard or result); "horses racing today" list → horse profiles; media/experience block ("วางแผนมาชม", tickets placeholder).
Output ONLY: RaceDay DATA additions + renderRaceDay() + page-specific CSS.
```

**✅ Review gate:** conditions panel prominent; race list navigates to Race Card; horse links work; responsive 2×2.

---

## P3 — Race Card (spec §4.2) — the hardest page, do it carefully

```
[paste GLOBAL RULES block]

STEP 3 — Build ONLY the Race Card page (spec §4.2). This is the most complex page — implement fully:
- DATA: raceDetail (no:3, name, dist, class, going, prize, runners[10]: num, silksColor, nameTH, nameEN, jockey, trainer, weight, gate, rating, ratingDelta, daysSince, gear, form"6 digits", formTag good|warn|bad).
- renderRaceCard(): breadcrumb; race header (fixed order per spec); horizontal race-no tabs (1-8); ⭐ view toggle "ง่าย/เต็ม" (default Simple).
  • Simple = card-per-horse: num, silks swatch, horse name (link), jockey, trainer, weight, gate, colored form bar + plain-language tag, tooltips on labels.
  • Full = dense table: cols [num][silks][horse(link)] | form6 | trainer | jockey | weight | gate | rating | +/- | daysSince | gear. Sticky header. Freeze left 3 cols. Horizontal scroll for the rest WITH a peek of the next column. Click column header = sort (num/rating/weight). Conditional highlight: top rating + best form in race. Form = mono, latest right, tooltip "5=5th, 0=10th+".
- Handicap box + tooltip; collapsible gear legend.
- Wire every horse/jockey/trainer name → drill-down.

CRITICAL mobile test target: Full view on phone must keep frozen cols + scroll without breaking layout.
Output ONLY: RaceCard DATA + renderRaceCard() + its CSS.
```

**✅ Review gate (strict):** toggle switches; sort works; frozen-cols + horizontal scroll survive at 375px; tooltips fire; names drill down. If anything fails, fix *only that function* (don't regenerate).

---

## P4 — Horse Profile (spec §4.3)

```
[paste GLOBAL RULES block]

STEP 4 — Build ONLY the Horse Profile page (spec §4.3). Add:
- DATA: horse (nameTH/EN, age, sex, color, sire, dam, foaled, breeder, stable, trainer, owner, rating, type, form[6 rows: date, track, dist, going, class, pos, weight, jockey, ratingThen, comment, hasVideo], ratingHistory[8 nums], stewards[]).
- renderHorse(): breadcrumb+back; identity hero (gradient photo, name TH/EN, headline facts row, rating prominent); basic-info key-value panel (2-col → stacked); ⭐ past-performance table (default 6 rows + "ดูทั้งหมด"; each row expands to full comment + ▶ video modal placeholder; date → raceday); simple SVG rating-history line + caption; collapsible stewards; related-entity links (dead + demo tag).
Output ONLY: Horse DATA + renderHorse() + its CSS.
```

**✅ Review gate:** rows expand; video modal; "show all"; dates link back; rating chart renders.

---

## P5 — Final QA pass (spec §8)

```
[paste GLOBAL RULES block]

STEP 5 — QA & polish ONLY. Do not rebuild. Go through Acceptance Checklist spec §8 and report PASS/FAIL per item, then output ONLY the minimal diffs needed to fix any FAILs:
- 4 pages + full click-through Landing→RaceDay→RaceCard→Horse
- Race Card toggle + sort + frozen cols on mobile
- all name/date drill-downs
- form-line + handicap tooltips
- weather/going panel prominent
- responsive at 375 / 768 / 1280
- TH/EN nav toggle
- touch ≥44px, focus rings, AA contrast
- placeholder buttons give feedback
Give each fix as "replace function X" or "add CSS rule Y" — never a full-file reprint.
```

---

## Cost-Efficiency Guide (step by step)

**Principle:** the expensive thing is re-emitting code you already have. Every rule below exists to avoid that.

1. **Attach the spec once (at P0), never re-paste it.** Afterward say "per spec §4.2". The model keeps it in context; re-pasting burns tokens each turn.
2. **Plan before code (P0).** A blank-shell pass is cheap and prevents the costliest failure: rewriting all 4 pages because the shell was wrong.
3. **One page per prompt (P1–P4).** Bounded output = fewer tokens per turn and a clean review gate. If a tool tends to "helpfully" reprint everything, the prompts already say *"Output ONLY … Do not reprint the skeleton."*
4. **Build order = dependency order.** Landing → RaceDay → RaceCard → Horse. Each page only links *forward* to pages already built, so no placeholder rework.
5. **Reuse one tiny DATA set.** 1 meeting, 8 races, 1 detailed race (10 horses), 1 detailed horse. Other rows reuse/derive. Rich data only where the test needs it (spec §5).
6. **Iterate by patch, never by regenerate.** Bugs → "replace function renderRaceCard only" or "add this CSS rule". A full-file regen can cost 5–10× a targeted fix.
7. **Stop at the review gate.** Don't proceed to the next page until the current one passes its ✅ gate — fixing a broken shared component later (after 4 pages depend on it) is far more expensive.
8. **Keep comments minimal** (global rules enforce this) — comments are output tokens too.
9. **If a page output gets truncated,** resume with "continue from renderRaceCard, do not restart" rather than re-running the whole step.
10. **Batch your review feedback.** Collect all fixes for a page into ONE patch prompt instead of many small turns — fewer round-trips = fewer repeated-context charges.

**Rough sequence cost shape:** P0 small · P1/P2/P4 medium · P3 largest (Race Card) · P5 small. Budget the most attention/tokens for P3.

---

## Quick reference — prompt order

| Step | Builds | Output bound | Gate |
|------|--------|--------------|------|
| P0 | Shell + tokens + router | skeleton only | tokens + blank pages OK |
| P1 | Landing | 1 render fn + data slice | hero/today/cards/nav |
| P2 | Race Day | 1 render fn + data | conditions panel + links |
| P3 | Race Card ⭐ | 1 render fn + data | toggle+sort+frozen mobile |
| P4 | Horse Profile | 1 render fn + data | expand+modal+chart |
| P5 | QA | diffs only | full §8 checklist |
