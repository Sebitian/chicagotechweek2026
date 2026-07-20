# TechChicago Week Calendar — Overview

## Product intent

Help people **plan TechChicago Week intentionally** instead of doom-scrolling a dense calendar.

Inspired by “get clear on why you’re attending” (Align · Direct · Build):

1. Pick a goal (or several)
2. See rooms that actually serve that goal
3. Check registration, location confidence, who’s hosting, and how crowded it is
4. Get directions when the pin is trustworthy

## Views

### Calendar (Day / 3-day / Week)

- **Day** — timed Google Calendar–style grid with overlap packing
- **3-day / Week** — stacked cards per day (readable on phones); tap a day header to jump into Day view
- Block color = **Luma registration status** (not topic)

### Agenda

Flat chronological list with registration pills, guest counts, hosts, goal tags.

### Map

- Pins clustered by Luma coordinates
- Cover-image bubbles; ring color = registration
- Exact vs approximate location called out
- Time scrubber filters “happening now”
- Sky strip on the scrubber follows **Chicago sunrise/sunset** for that calendar day (Jul 20–25, 2026 tables)

### Goals

Multi-select intent bins:

| Goal | Typical signals |
|------|-----------------|
| Meet clients | Exec / CISO / CTO / B2B dinners |
| Pitch & fundraise | Pitch, VC, demo day, funders |
| Get a new job | Career day, workforce, hiring |
| Learn new skills | Workshops, panels, forums |
| Find mentorship | Coffee, 1MC, founder lessons |
| Meet new people | Mixers, fests, networking (not auto-“fun”) |
| PR & marketing | Launch, showcase, media |
| Have fun | Ice cream, party, soirée, art/film — **not** TechPalooza by name alone |

Events can sit in **multiple** bins. Selecting several goals shows the union, ranked by how many selected goals they hit.

## Registration status

From Luma `registration_availability`:

- `open` → green
- `waitlist` → blue
- `sold-out` / closed → red

Invite-only / non-Luma events may be treated as closed or unknown.

**Disclaimer in UI:** status from Luma; may change; confirm on the event page.

## Location accuracy

- `precise: true` — street address + coords from Luma → “Exact pin” / Directions
- `precise: false` — guests-only address; neighborhood-level coords → “Approx. directions” + link to Luma

## Hosts & sponsors

- **Hosts** — Luma `hosts` (name, avatar, website/profile)
- **Sponsors / partners** — inferred from title/description phrases (“hosted by…”, “presented by…”, “Brand × Brand”). Labeled as inferred, not official.

## Guest counts

From Luma `guest_count` (fallback `ticket_count`). Shown as “N people going” / “N going” with a Luma attribution.

## Embedded data blobs in `index.html`

| Constant | Purpose |
|----------|---------|
| `EVENTS` | Core schedule |
| `LUMA_GEO` | Venue, address, lat/lng, precise flag |
| `LUMA_COVERS` | Cover image URLs |
| `LUMA_REG` | open / waitlist / closed |
| `LUMA_HOSTS` | hosts + inferred sponsors |
| `LUMA_GUESTS` | guest counts |
| `CHI_SUN` | Per-day sunrise/sunset minutes for sky UI |

`data/*.json` are snapshots used while building those embeds; the live app does not fetch them at runtime.

## Refreshing Luma data

1. Collect slugs from `EVENTS[].luma`
2. `GET https://api.lu.ma/url?url=<slug>`
3. Update the corresponding `LUMA_*` objects and re-merge fields onto `EVENTS`

Prefer `curl` over bare Python `urllib` (API may 403 some clients).

## Deploy notes

- Static site; framework = none
- Entry: `index.html`
- Leaflet loads from CDN — needs network in the browser
- No secrets in the repo today; keep it that way if you add API keys later (use env + serverless)

## Out of scope / future ideas

- Live refresh of guest counts / registration without a rebuild
- User “my schedule” / ICS export
- Filtering map pins by selected Goals
- PWA / offline cache
