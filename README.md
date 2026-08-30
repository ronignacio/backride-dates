# Backride Dates — The Luzon Map

A concept site for the **Backride Dates** YouTube series: an interactive map of 100 motorcycle date destinations, every one of them within a day's ride of Camarin, North Caloocan.

**[View the map »](index.html)**

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
- **Spin the map** — randomly picks a destination from the current filter (skipping ones already ridden), for when you want the map to decide.
- **Date Card** — click any pin or list row to open its card: activity, ride time, and budget.
- **Progress tracking** — mark a destination as ridden; progress is saved locally in the browser (`localStorage`) and reflected in the "X / 100 ridden" tally and the pin styling.
- **Keyboard + screen reader friendly** — pins are focusable and operable via keyboard, with `aria-label`s and `prefers-reduced-motion` support.

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

## Notes for the series

- Book ahead for Masungi, Las Casas, Pinatubo, and Hundred Islands.
- Pawikan (sea turtle) releases run roughly November to February.
- In the rainy months, stay North and East.
