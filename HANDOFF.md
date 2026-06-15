# RBSC Prototype — Session Handoff

_Last updated: 2026-06-15. Deployed to GitHub Pages. Read this first._

## Project
- **Deliverable:** `index.html` — single-file, offline, 4-page RBSC horse-racing prototype + Selangor-style homepage. Vanilla JS, hash routing, bilingual TH/EN, mock data. **Now with 6 real racing photos** in `images/` folder.
- **Live link:** https://winteruxui.github.io/rbsc (auto-updates on `git push main`)
- **Design system:** `DESIGN_BIBLE.md` (single source of truth)
- **Repo:** github.com/winteruxui/rbsc (clone at C:\Winter\rbsc)

## Latest Status (commit `ed9fde9`, 2026-06-15)

### What's Complete
✅ **All 4 pages** (Landing, Race Day, Race Card, Horse Profile) — fully built & responsive (375/768/1280px)
✅ **Design polish P1–P3** — icon system, hero, cards, hovers, scrollbars, empty states, reduced-motion guard
✅ **Selangor-inspired homepage modules**
  - เมนูด่วน (8 shortcut tiles: 3 live → pages, 5 "coming soon" → placeholders)
  - วันแข่งถัดไป (upcoming fixtures list, future meetings only)
  - ข่าวสนามแข่ง (race news, 1 featured + 5 regular items)
  - Dual-column layout (desktop), stacks on mobile
✅ **Hero deduplication** — removed duplicate "ดูการแข่งวันนี้" CTA; hero now brand-identity only
✅ **Fixtures deduplicated** — today's meeting removed from upcoming list (shown once in Today card above)
✅ **Brand rebrand to navy+gold** — matched to real rbsc.org/racing theme
  - Navy `#264068` (header/headings/dark surfaces)
  - Mid-navy `#30528A` (buttons/links)
  - Bronze-gold `#BF942D` (accents)
  - Warm gray `#D9D5CF` (dividers)
  - **CSS vars named `--green-*` still, but hold navy values** (don't be fooled by names)
  - Semantic greens (`--good` for "good form") intentionally kept
✅ **6 real photos** (hero, racecourse, horse profile, news ×3) from Unsplash, committed to repo
✅ **Horse profile enhancements**
  - Career stats strip (starts/wins/2nd/3rd/win% with icons + win-rate donut)
  - Form figures timeline (8 recent finishing badges)
  - Icons on past-performance cards (distance/going/time/jockey)
  - 14 past-performance rows (4 added early-career races)
  - Dynamic record line computed from data
✅ **Design fixes**
  - Nav: tightened spacing, collapses to hamburger ≤1024px
  - Font: Noto Serif Thai (looped/มีหัว serif) + IBM Plex Mono for numerals
  - Margins: fixed CSS bug (page padding now preserved), 24px desktop / 16px mobile
  - Search: icon-triggered collapse panel (no select dropdown)

### Fonts (Fixed — Do NOT change)
- **UI:** Noto Serif Thai (looped serif for readability on small screens)
- **Numerals/data:** IBM Plex Mono (monospace, tabular-nums)
- **User request:** Keep these. Do not revert to IBM Plex Sans Thai or switch to Century Gothic (RBSC's brand font, but user wants current choice).

## Architecture Notes
- **Single file:** `index.html` (1413 lines, all JS/CSS/HTML inline)
- **Router:** hash-based (#landing, #raceday, #racecard, #horse)
- **Data:** `DATA` object in `<script>` block; 4 race meetings, 1 featured race (race 3, RBSC Gold Cup), horse profile (Golden Thunder), mock fixtures (4 upcoming), mock news (6 items), shortcuts (8 tiles), search results (5 items)
- **Mock data structure:** Each page renderer consumes specific DATA keys; no API calls
- **Images:** All 6 photos are relative URLs (`images/*.jpg`), safe for local & GitHub Pages
- **Color vars:** CSS custom properties in `:root` — `--green-900/700/50`, `--gold-500`, `--line`, `--bg-alt`, `--good`, etc.

## Known Limitations
- **Dark mode:** Not implemented (deferred). Would need full token mirror + re-test.
- **Data:** All mock. No real race schedules, horse form, or live updates.
- **Other dead pages:** "ผลการแข่ง" (Results), "เรตติ้ง & แฮนดิแคป", "ซ้อมเช้า" (Trackwork), etc. marked "coming soon" with placeholder modals.

## Environment & Deployment
- **Local preview:** `node .claude/serve.js` on port 4173 (or use preview_start in Claude Code)
- **GitHub Pages:** Auto-deploys from `main` branch (enabled in repo Settings → Pages)
- **Live URL:** https://winteruxui.github.io/rbsc (updates ~1 min after push)
- **Branch:** `main` only; no staging or dev branches in use

## Next Steps (if continuing)
1. **Dark mode** — mirror color tokens, test all pages. Low priority (user deferred).
2. **Real data integration** — API to fetch live race schedules, horse form, news. Major refactor.
3. **Copy refinement** — review Thai strings, dress-code line, news excerpts for tone/accuracy.
4. **Performance** — optimize image sizes; consider lazy-loading for photos.
5. **Accessibility audit** — full WCAG 2.1 AA pass (contrast, focus states, ARIA labels).

## Tooling Reminders
- **Syntax check before preview:** `node -e "new Function(scriptBody)..."`
- **Preview quirk:** innerWidth reports ~476 at "mobile" preset but visually renders 375px
- **Screenshot tool:** Flaky in this environment; use `preview_eval` with `getComputedStyle` for color verification
- **Memory system:** Check `C:\Users\porza\.claude\projects\C--Winter-rbsc\memory\` for project context retained across sessions
- **Output style:** Educational blocks (`★ Insight ──`) in responses; Thai replies when user writes Thai

---

**Handoff ready.** All features live on GitHub Pages. Ask questions or request next work via git issues or direct message.
