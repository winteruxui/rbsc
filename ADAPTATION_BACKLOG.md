# MRC Racing — Adaptation Backlog

> Turns the `DESIGN_BIBLE.md` §13 Reference Library lessons into concrete, prioritized work.
> Each item cites its **source reference** (in `Ref/`) and the **target** in `index.html`
> (renderer / `DATA` / CSS area — renderers move, so they're named, not line-numbered).
> Nothing here is implemented yet; this is the menu for "make it better" rounds.
>
> Priority logic: **P1** = storytelling, high impact / low cost · **P2** = user-journey
> completeness · **P3** = UX depth & power-user features.
> Effort: S ≈ <1h · M ≈ 1–3h · L ≈ half-day+.

---

## P1 — Storytelling (highest ROI, do first)

### P1.1 — Horse-profile rating/form **line-chart**  · effort M
*The single highest-leverage upgrade. Turns a rating column into the "is it improving?" story.*
- **Source:** Abu Dhabi `Ref/Horse Jockey Info/Ref 4.png` (results-tracker chart), Bahrain
  `Ref/Horse Jockey Info/Ref 6.png` (handicap-rating performance tracker).
- **Target:** `RENDERERS.horse` — insert a narrative module **after** the career/verdict strip,
  **before** the past-performance table (§9 order). Add `ratingHistory[]` to `DATA.horse`
  (derive from existing `form[].rtg` if present).
- **How:** inline **SVG polyline** (no chart lib), `--good/--warn/--bad` colored points,
  `--gold-500` trend line; **text caption** stating the trend (a11y, §10). New component
  `.rating-chart` in shared CSS + `ratingChart(history)` helper next to `formBar()`.
- **Done when:** chart renders from mock data, has a Thai caption, survives reduced-motion and 375px.

### P1.2 — Home **"season at a glance" stat-band**  · effort S
- **Source:** Bahrain `Ref/Home/Ref 2.png` (the big "13" premier-races band).
- **Target:** `RENDERERS.landing` — new `.stat-band` section between the news blog and audience
  cards (or just under shortcuts). Pull from `DATA.meeting`/`DATA.fixtures` (e.g. race-days,
  total horses, season prize pool).
- **How:** 3–4 big-number badges (mono numerals, gold accents) + Thai labels. New `.stat-band`
  CSS reusing the meta-strip rhythm.
- **Done when:** numbers trace to real mock data; responsive 1-row → 2×2 on mobile.

### P1.3 — Elevated **feature / editorial story** slot  · effort S
- **Source:** Bahrain editorial band (`Ref/Home/Ref 2.png`); Selangor race news (`Ref/Home/Ref 1.png`).
- **Target:** the news blog already has a "highlight left + 3 side" layout in `RENDERERS.landing`.
  Promote the highlight into a true **feature story** with a race tie-in CTA ("ดูเรซ →
  `go('racecard',{no:3})`"). Add a `feature:true` + `raceNo` field on one `DATA.news` item.
- **Done when:** the lead story links into the relevant race card; visually distinct from the 3 side items.

---

## P2 — User journey (completeness of the drill-down graph)

### P2.1 — **Jockey Profile** page  · effort L
*We render jockey names but they dead-end. Refs treat jockey as first-class.*
- **Source:** Bahrain `Ref/Horse Jockey Info/Ref 7.png` (cleanest: CAREER/SEASON tabs →
  stat-strip → linked rides table → pagination).
- **Target:** new `RENDERERS.jockey` + `#jockey` route in `PAGES`/router; `DATA.jockey`
  (stat-strip: wins/runs/2nd/3rd/4th/strike-rate; `rides[]` linking to horses/races). Follow §9
  contract; reuse `.race-tabs` for CAREER/SEASON, `breadcrumb()`, table styles.
- **How:** stat-strip verdict → rides table (date·result·horse·trainer, all linked) → optional
  strike-rate trend (P1.1's chart helper). Wire jockey names in race card + horse form to `go('jockey')`.
- **Done when:** clicking any jockey name lands on a consistent profile that links back and onward.

### P2.2 — **Trainer Profile** page  · effort M
- **Source:** same Bahrain jockey pattern (`Ref 7`), trainer variant.
- **Target:** `RENDERERS.trainer` + `#trainer` route; `DATA.trainer` (stat-strip + `runners[]`).
  Mirror P2.1 to keep them consistent (consider one shared `personProfile()` builder).
- **Done when:** trainer names across the app link to it; mirrors the jockey page.

### P2.3 — **Horse Directory / "Horses" listing**  · effort M
*Lets users browse, not only arrive via a race card — a new top-of-funnel entry.*
- **Source:** Bahrain `Ref/Horse Jockey Info/Ref 5.png` (Quick-Finder search + horse table +
  HORSES/PREVIOUS-RUNNERS tabs + pagination).
- **Target:** new `RENDERERS.horses` + `#horses` route; `DATA.horses[]` (no·name·age·colour·sex·
  rating·trainer/owner, each row → `go('horse')`). Add a nav entry. Reuse table styles + an
  existing search input pattern from the header search.
- **Done when:** a searchable/scannable list reaches the horse profile; nav exposes it.

### P2.4 — **Link every entity name** (scent end-to-end)  · effort S
- **Source:** universal across HKJC/Bahrain/BHA (every name is a link).
- **Target:** audit `RENDERERS.racecard` (sire/dam/jockey/trainer cells) and `RENDERERS.horse`
  (form table jockey/trainer, breeding) — wrap names in `go('jockey')`/`go('trainer')`/`go('horse')`.
- **Done when:** no entity name is plain text where a profile exists.

### P2.5 — **Standings / leaderboard** on Home  · effort M
- **Source:** Bahrain `Ref/Home/Ref 2.png` (season standings table).
- **Target:** `RENDERERS.landing` new `.standings` section; `DATA.standings[]` (jockey/trainer
  rank · wins · strike-rate), rows linking into P2.1/P2.2. A new entry path into profiles.
- **Done when:** top jockeys/trainers shown, ranked, linking onward.

---

## P3 — UX depth & power-user features

### P3.1 — **Calendar-grid widget** + month accordion on the Race Day hub  · effort L
*Our horizontal date-tabs won't scale past a handful of dates; a season needs a grid + grouping.*
- **Source:** Bahrain `Ref/Event Calendar/Ref 2.png` (calendar + day-timeline), Bahrain
  `Ref/Race Card/Ref 4.png` (month accordion), HKJC `Ref/Event Calendar/Ref3.png` (grid + filters).
- **Target:** `RENDERERS.raceday` — add a **calendar-grid** view (month, race-days highlighted in
  gold/navy → click sets the selected day) **alongside** the existing date tabs; group future
  fixtures by **month accordion**. Drive from `DATA.fixtures[]`.
- **Done when:** a user can pick any race day from a grid; the season compresses into months;
  date-tabs remain for the near-term quick switch.

### P3.2 — Fixtures **filter bar**  · effort M
- **Source:** BHA `Ref/Event Calendar/Ref 5.png` (Month / Racecourse / Event-Type + FILTER).
- **Target:** Race Day hub fixtures section — filter `DATA.fixtures[]` by month / class / feature.
- **Done when:** filters narrow the fixture list client-side; empty-state handled.

### P3.3 — Race-Card **Past Winners** block  · effort S
- **Source:** BHA `Ref/Race Card/Ref 3.png` (Past Winners table under the runners).
- **Target:** `RENDERERS.racecard` — add a Past Winners table after the runner list; new
  `pastWinners[]` on `DATA.raceDetail` (year·horse·weight·going). Answers "what wins this race".
- **Done when:** a compact history table renders below runners on the detailed race.

### P3.4 — Race-Card **evaluation / expert-pick sub-tab**  · effort M
- **Source:** Bahrain `Ref/Race Card/Ref 5.png` (ENTRIES / EVALUATION / EDITORIAL sub-tabs);
  HKJC `Ref/Race Card/Ref 1.png` (expert selections).
- **Target:** `RENDERERS.racecard` — add sub-tabs `รายชื่อม้า / วิเคราะห์ / ผลย้อนหลัง` using
  `.race-tabs` styling; "วิเคราะห์" holds a short editorial + top-pick (mock). Progressive
  disclosure — default to รายชื่อม้า.
- **Done when:** sub-tabs switch content within one race without leaving the card.

### P3.5 — Horse past-performance **form filters** + video affordance  · effort S
- **Source:** Abu Dhabi `Ref/Horse Jockey Info/Ref 4.png` (WIN/RUNNER-UP filters + video).
- **Target:** `RENDERERS.horse` — filter chips over the past-performance table (all / wins /
  places); keep the existing `hasVideo` rows triggering `placeholder()`/`openModal()`.
- **Done when:** chips filter the history table; video rows give feedback (never silent).

---

## Sequencing suggestion
1. **P1.1 → P1.2 → P1.3** (storytelling skin on existing pages — biggest perceived jump, low risk).
2. **P2.1 + P2.2 (shared builder) → P2.4 → P2.3 → P2.5** (complete the journey graph).
3. **P3** as depth permits (calendar first if the fixture set grows).

Each item must still pass the §14 Per-Page Consistency Checklist and the §12 a11y checklist in
`DESIGN_BIBLE.md`, and keep the MRC anonymization intact.
