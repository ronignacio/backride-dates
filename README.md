# Backride Dates — The Luzon Map

A concept site for the **Backride Dates** YouTube series: an interactive map of 100 motorcycle date destinations, every one of them within a day's ride of Camarin, North Caloocan.

**[View the map »](index.html)** · **[Read the full series concept »](docs/CONCEPT.md)**

## What it is

A single-page, dependency-free interactive map of Luzon plotted in SVG. Each of the 100 destinations is a pin with:

- **Activity** — what to actually do there
- **Ride time** — one way from Camarin, two-up at a moderate pace
- **Budget** — cash in the envelope, covers gas, toll, entrance, and food

Destinations are grouped into four directions radiating from home base:

| Direction | Count | Covers |
|---|---|---|
| North | 28 | Bulacan, Nueva Ecija, Tarlac, Pampanga, Pangasinan |
| West | 22 | Bataan, Zambales, Subic |
| East | 25 | Rizal, Quezon, Marilaque |
| South | 25 | Cavite, Batangas, Laguna, south Quezon |

### Features

- **Filter by direction** — chips at the top narrow the map and list to one region.
- **Spin the map** — randomly picks a destination from the current filter (skipping anything already picked or ridden), for when you want the map to decide. The button shuffles across the whole map, then the camera flies in and frames the winner: every other pin recedes, and the result gets a pulsing white ping. A **Zoom out** button returns to the full extent.
- **Date Card** — click any pin or list row to open its card: activity, ride time, and budget.
- **Challenge pins** — add a destination to the challenge and its pin changes to a starred, ringed marker so the queue is readable at a glance on the map. Once ridden it turns muted with a green tick, and carries its score as a badge.
- **Rating** — score a ridden date out of 10 three ways (his, her, viewer) and log what was left in the envelope.
- **Scoreboard tab** — KPI tiles, progress by direction, the episode log, and a ranking table sorted on the average of all three scores.
- **Export / import** — progress lives in `localStorage`, which is per-browser. Export to JSON after an episode and import on the other device to keep both phones in step.
- **Keyboard + screen reader friendly** — pins are focusable and operable via keyboard, with `aria-label`s and `prefers-reduced-motion` support.

### Pin states

| Pin | Meaning |
|---|---|
| Solid colour, direction-coded | Not yet picked |
| Starred with a dashed ring | Picked for a challenge |
| Muted grey with a green tick | Ridden (score badge above it once rated) |

### Two implementation notes

- **Pin positioning uses a wrapper group.** Each pin is `<g class="pinpos" transform="translate(x,y)"><g class="pin drop">…</g></g>`. The drop-in animation sets a CSS `transform`, and a CSS transform overrides an SVG `transform` presentation attribute on the *same* element — putting both on one `<g>` silently collapses all 100 pins onto the map's origin once the animation finishes.
- **The pin halo is `pointer-events:none`.** It is a 13px circle at `opacity:0`; left interactive, it swallows clicks aimed at neighbouring pins in the dense Rizal and Laguna clusters.
- **The drop-in animation uses `backwards`, never `both`.** A retained final keyframe holds `opacity: 1` at animation precedence, which silently defeats every later opacity rule — both the direction filter's `.dim` and the post-spin spotlight. The class lands on the element and nothing visibly changes.

### Typography

The page title uses **Rubik Bubbles** (a rounded groovy display face); the rest of the interface stays on Barlow Condensed so the map furniture keeps its editorial tone. Swapping the title face means changing `--fun` and the Google Fonts URL together.

### Chart colours

The dashboard reuses the four direction hues, but stepped darker for chart marks (`--n-chart` etc.). The map's pin colours sit above the OKLCH dark-mode lightness band and the cyan falls under the chroma floor, so the bar fills use validated steps in the same hue families — identity still reads across map and dashboard, and adjacent pairs stay separable under protanopia and deuteranopia.

## Tech

Plain HTML, CSS, and vanilla JavaScript. No build step, no framework, no backend — a single `index.html` file. The map itself is a hand-plotted coastline projected onto an SVG `viewBox`, not a tile-based map library.

## Running locally

Just open `index.html` in a browser, or serve the directory with any static file server:

```bash
python3 -m http.server 8080
```

## Deployment

Deployed to Cloudflare via the Git integration, which runs `npx wrangler deploy`. The repo ships a `wrangler.jsonc` that declares it as an assets-only static site — the whole repo root is the asset directory, so there is no build step:

```jsonc
{
  "name": "backride-dates",
  "compatibility_date": "2026-08-30",
  "assets": {
    "directory": "./",
    "not_found_handling": "single-page-application"
  }
}
```

Dashboard settings that must match: **Build command** empty, **Root directory** `/`, and **Production branch** `main`.

Because the asset directory is the repo root, every file here would otherwise be served publicly. `.assetsignore` excludes the repo-only files — `docs/`, `README.md`, and the config files — so the live site serves just `index.html`.

## Repo layout

| Path | Purpose | Served publicly? |
|---|---|---|
| `index.html` | The interactive map — the whole site | Yes |
| `docs/CONCEPT.md` | Full series concept: budget tiers, all 100 places, starter wheel, scoreboard, practical notes | No |
| `wrangler.jsonc` | Cloudflare Workers static-assets config | No |
| `.assetsignore` | Keeps repo-only files off the live site | No |

## Notes for the series

- Book ahead for Masungi, Las Casas, Pinatubo, and Hundred Islands.
- Pawikan (sea turtle) releases run roughly November to February.
- In the rainy months, stay North and East.
