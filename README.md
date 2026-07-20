# TechChicago Week 2026 Calendar

A mobile-friendly calendar for **TechChicago Week (Jul 20–25, 2026)** — day / 3-day / week views, agenda, Chicago map with a time scrubber, and a Goals view so people can plan by *why* they’re attending.

Single static page: `index.html`. No build step required.

---

## Deploy to Vercel (HTML is fine)

**You do not need to convert this to React/Next.** Vercel hosts static HTML natively.

### Option A — Vercel CLI

```bash
cd chicagotechweek
npx vercel
```

### Option B — GitHub → Vercel

1. Create a GitHub repo and push this folder.
2. In [vercel.com](https://vercel.com) → **Add New Project** → import the repo.
3. Framework preset: **Other** (or leave blank). Build command: empty. Output: `.`
4. Deploy.

`vercel.json` is included so clean URLs work. Root file must stay named `index.html`.

### Local preview

```bash
open index.html
# or
npx serve .
```

---

## Features

| View | What it does |
|------|----------------|
| **Day / 3-day / Week** | Timed day grid; multi-day uses stacked readable cards on phone |
| **Agenda** | Chronological list with hosts, goals, registration, guest counts |
| **Map** | Leaflet map of Chicago; scrub time-of-day; photo pins; sky effect tied to Chicago sunrise/sunset |
| **Goals** | Align → Direct → Build — multi-select intents (clients, pitch, job, learn, mentorship, people, PR, fun). Events can match **multiple** goals |

Also:

- **Registration colors** from Luma: green = open, blue = waitlist, red = closed
- **Directions** from Luma coordinates (exact vs approximate called out)
- **Hosts** from Luma; **sponsors/partners** inferred from description language
- **Guest counts** (“N people going”) from Luma `guest_count`
- Cover images from Luma on map, goals, and modal

---

## Color legend (consistent everywhere)

| Color | Meaning |
|-------|---------|
| Green | Registration **open** (Luma) |
| Blue | **Waitlist** available (Luma) |
| Red | Registration **closed** / sold out (Luma) |

Topic filter chips (AI, Founders, Climate…) are **filters only** — they do not change card colors.

Goal picker buttons use their own accent colors for *intent*; event cards still use green / blue / red for registration.

Status can change on Luma — always confirm on the event page.

---

## Project layout

```
chicagotechweek/
├── index.html          # Full app (HTML + CSS + JS)
├── vercel.json         # Static deploy config
├── README.md           # This file
├── docs/
│   └── OVERVIEW.md     # Deeper product / data notes
└── data/               # Reference exports (not loaded at runtime)
    ├── luma-geo.json
    ├── luma-tcw-events.json
    └── luma-tcw-events-mapped.json
```

Event data, geo, covers, registration, hosts, and guest counts are **embedded in `index.html`** (fetched from Luma during build-of-data, then inlined). The `data/` folder is for reference / re-sync scripts later.

---

## Data sources

- **Luma API** (`api.lu.ma/url?url=<slug>`) — times, covers, geo, `registration_availability`, hosts, `guest_count`
- **Official TechChicago** listings — a few invite-only / non-Luma anchors

To refresh Luma fields later, re-query each event slug and update the `LUMA_*` maps + `EVENTS` array inside `index.html`.

---

## Goals tagging (brief)

Goals are **multi-label**. An event can be “Pitch & fundraise” *and* “Meet new people”. “Have fun” is reserved for leisure (ice cream, parties, art) — not every fest/palooza.

See `docs/OVERVIEW.md` for the rule summary and curated overrides.

---

## Stack

- Vanilla HTML / CSS / JS
- [Leaflet](https://leafletjs.com/) + CARTO tiles (map)
- No npm dependencies required to run
