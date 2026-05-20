# Location Finder

A browser-based tool for geocoding community locations from a CSV file. Used to enrich `communities_list.csv` with latitude and longitude data for the TCLP project.

## Usage

1. Open `index.html` in any modern browser (no server or install required)
2. Click **Choose CSV file** and select `source/communities_list.csv`
3. For each community the app will:
   - Auto-geocode the location using [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org/)
   - Display the top suggestions and place a pin on the map
4. Review the pin, then:
   - **Pick a suggestion** from the list, or
   - **Click the map** to place the pin manually, or
   - **Drag the pin** to adjust, or
   - **Type coordinates** directly into the Lat / Lon fields
5. Click **Confirm Location** (or press `Enter`) to save and advance
6. Click **Export CSV** at any time to download the updated file with `lat` and `lon` columns added

Progress is saved automatically to browser `localStorage` — you can close and reopen without losing work.

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Confirm location |
| `→` or `S` | Skip |
| `←` or `B` | Back |

## CSV Format

Input: `name`, `state`, `alias` (alias is optional, semicolon- or ` AND `-separated)

Output: same columns plus `lat`, `lon`

```
name,state,alias,lat,lon
Africatown,AL,,30.73456,-88.07654
Boykin,AL,Gees Bend,32.02311,-87.28901
```

## Data Source

`source/communities_list.csv` — 652 communities across 32 US states.

## Dependencies

All loaded from CDN, no install needed:

- [Leaflet.js](https://leafletjs.com/) — interactive map
- [CartoDB Voyager tiles](https://carto.com/basemaps/) — map tiles with town labels
- [Nominatim](https://nominatim.openstreetmap.org/) — free geocoding API (OpenStreetMap)
