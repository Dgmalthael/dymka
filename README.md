# ДЫМКА / HAZE — Hookah Mix Advisor

Single-page hookah mix recommender. Curated library, factory premix catalog,
mood/season/strength quiz, multi-component builder, bowl-type recommendations,
Pro AI mixologist. Bilingual (RU / EN). Static site — no build, no server.

**Live demo:** _(set after Pages goes live)_

---

## What's in this repo

Only two files matter for hosting:

| File | Purpose |
|---|---|
| [`index.html`](index.html) | The entire app — markup, CSS, JS, inline data baseline. |
| `README.md` | This file. |

That's it. Everything else (the Google Sheet, the deployed Apps Script, the
local CSV/JSON seeds) lives outside the repo.

## Quickstart — host on GitHub Pages

1. Fork or upload `index.html` + `README.md` to a new repo.
2. Repo → **Settings → Pages → Source: main / root**.
3. Wait ~1 minute. Site is live at `https://<you>.github.io/<repo>/`.

The site is fully functional offline-style — the inline data in `index.html`
includes **135 homemade recipes**, **41 factory premixes**, and **18 brands**.
Quiz, builder, library and brand catalog all work without any backend.

## Optional — connect Google Sheets

To let users persist saved mixes, publish builder recipes to a community
library, and like recipes, hook up a Google Sheet via Apps Script. Full
walkthrough (≈ 3 minutes):

→ **[GOOGLE_SHEETS_SETUP.md](GOOGLE_SHEETS_SETUP.md)** *(not committed — local-only)*

Short version:

1. Open your sheet → Extensions → Apps Script → paste `apps-script.gs` → Save.
2. Run `setup` once (creates the four tabs: `PremadeMixes`, `Mixes`, `Brands`, `SavedMixes`).
3. Deploy as Web app (Execute as: Me, Who has access: Anyone). Copy the `/exec` URL.
4. Paste the URL into `CONFIG.SHEETS_URL` near the top of `index.html`.
5. Open the site → Premade Mixes → click **📤 Push inline catalog to Sheets** once.

After that, everything is editable from the Sheet — no code change needed.

---

## Features

### Five entry modes on the home screen

| # | Mode | Notes |
|---|---|---|
| 01 | **Flavor quiz** | 7 questions (mood, family, sweetness, cooling, strength, season, time) → 5 ranked recommendations from the 135-recipe library. |
| 02 | **Mix builder** | Up to 6 tobaccos per bowl. Add-flavor grid is filtered to flavors that are **compatible with every selected component** (strict intersection of the pairing graph; permissive fallback if intersection is empty). Per-chip ± controls re-balance percentages so the total always sums to 100. |
| 03 | **Premade mixes** | Filter factory blends (Darkside, Adalya, Sebero, …) by mood and strength. |
| 04 | **Brand catalog** | Filter the 18-brand reference by origin, strength, beginner-friendliness. |
| 05 | **Recipe library** | All 135 community recipes, filterable by mood / season / strength / family. Sorted by **likes ↓**. |

### Recommendation system

- Score considers flavor family match (+30), mood match (+22), and continuous
  closeness on sweetness / cooling / strength (up to +12 each), with bonuses
  for season match (+12) and time-of-day match (+10).
- Each card shows a **bowl-type recommendation** (phunnel, harmonic, killer,
  Egyptian classic, …) derived from the mix's composition and strength.

### Community library

- Builder mixes can be **published to the community library** in one click
  (writes to the `Mixes` tab in Sheets with `likes = 0`).
- Every persistent recipe card has a **🤍 like button**. Likes increment a
  counter in the Sheet; the user's own likes are remembered in `localStorage`.
- Library is sorted by likes so the most popular community mixes float to the top.

### Pro: AI mixologist

A paywalled mode that calls Claude on demand to design a recipe for any prompt
("dessert without chill, dark leaf, no anise", etc). The "Pro" toggle is a
prototype — the modal pretends to charge but unlocks instantly.

### Bilingual

Russian-first, English mirror. Toggle at the top.

### 18+ gate

Age confirmation on first load (no birth-date prompt, just a yes/no).

---

## Repo hygiene

Things kept out of git for privacy / size reasons:

- `apps-script.gs` (your Google Apps Script source — paste it into your own
  Apps Script project, don't host it publicly)
- `GOOGLE_SHEETS_SETUP.md` (links to your private sheet)
- `premade-mixes.csv`, `hookah-brand-catalog.json` (local seeds, superseded by
  the Sheet once connected)

If you fork this and want to omit the Sheets URL from the public commit, set
it from DevTools after each page load:

```js
CONFIG.SHEETS_URL='https://script.google.com/macros/s/.../exec';
loadMixes().then(()=>view==='library'&&renderLibrary());
```

---

## Health warning

Hookah tobacco harms your health. This is an informational prototype for
adults. No data is collected by the site itself — anything saved is written
to *your* Google Sheet.
