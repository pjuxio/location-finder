# AGENTS.md — Location Finder

Guidance for AI coding agents working in this repository.

## Project Purpose

A single-file browser app (`index.html`) that steps a user through 652 US communities and lets them confirm or correct a geocoded latitude/longitude for each one. Output is a CSV with `lat` and `lon` columns appended.

## File Structure

```
index.html                 — entire app (HTML + CSS + JS, no build step)
source/
  communities_list.csv     — input data (name, state, alias)
README.md                  — user-facing documentation
AGENTS.md                  — this file
```

## Key Conventions

- **No build tooling.** Everything is vanilla HTML/CSS/JS. Do not introduce a bundler, framework, or package manager unless explicitly requested.
- **Single file.** Keep all code in `index.html`. Do not split into separate `.js` or `.css` files unless asked.
- **No API keys.** Geocoding uses Nominatim (OpenStreetMap). Do not swap in a service that requires credentials.
- **CRLF-aware CSV parsing.** The source CSV uses Windows line endings (`\r\n`). The custom `parseCSV()` function handles this — do not replace it with a naive `split('\n')`.
- **localStorage key:** `tclp-location-finder-v1`. If the storage schema changes, bump the version suffix to avoid loading stale data.

## State Model

| Variable | Type | Purpose |
|----------|------|---------|
| `rows` | `Object[]` | Parsed CSV rows |
| `csvHeaders` | `string[]` | Original header order (used when re-serializing) |
| `results` | `{ [rowIndex]: { lat, lon } }` | Confirmed coordinates |
| `skipped` | `Set<number>` | Row indices the user skipped |
| `currentIndex` | `number` | Row currently shown (`-1` = all done) |
| `selectedLat/Lon` | `number\|null` | Pin position pending confirmation |

## Geocoding

- API: `https://nominatim.openstreetmap.org/search`
- Params: `q`, `format=json`, `limit=5`, `countrycodes=us`
- Nominatim requires `Accept-Language: en` header and allows 1 req/s. Requests are user-paced so no rate-limit throttle is implemented.
- Alias field (semicolon- or ` AND `-separated) provides alternative search terms for ambiguous community names.

## Map

- Library: Leaflet 1.9.4 (CDN)
- Tiles: CartoDB Voyager (shows town labels at low zoom)
- Default zoom after geocode: **14** (street level)
- Pin is draggable; map click also repositions pin
- Manual lat/lon inputs are two-way bound with the pin

## CSV Export

`buildCSV(rows)` re-serializes all rows. It appends `lat` and `lon` columns if not already present in `csvHeaders`, and fills values from `results[i]` or falls back to the original row values. Output uses `\r\n` line endings.
