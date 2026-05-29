# VicPol CAD Lineup Generator

A web-based tool for creating realistic Victoria Police CAD radio lineups for stations across Victoria.

## Features

- **All Metro Regions** — North, South, East, and Western Metro stations pre-loaded
- **Custom/Rural** — Enter any station code (e.g. `ESP` for Shepparton) for regional stations
- **Shift Awareness** — Morning (0700), Afternoon (1500), and Night (2300) shifts, with appropriate unit numbers auto-suggested
- **Full Unit Type Coverage** — Sedans, Divisional Vans, RRU, SOCIT, FVIU, CIU, HWP, Foot/Bicycle patrol, PACER, Supervisors, Crime Desk, Dog Squad, and more
- **Smart Callsign Generation** — Callsigns auto-generated based on station code + unit type + shift (e.g. `ESP311` for a Night shift Div Van at Shepparton)
- **Station Links** — Shows backing stations, PSA, CIU, CRI, and HWP links where known
- **Status Tracking** — Assign C1/C2/C3/C6 status to each unit
- **Export** — Copy a formatted CAD-style lineup to clipboard

## Hosting on GitHub Pages

1. Fork or create a new repository
2. Upload `index.html` to the repository root
3. Go to **Settings → Pages**
4. Set **Source** to `Deploy from a branch`, select `main`, folder `/root`
5. Click **Save** — your site will be live at `https://yourusername.github.io/repo-name`

## Unit Number Reference

| Range | Unit Type |
|-------|-----------|
| 150 | Duty Officer (Inspector) |
| 200–299 | Station sedans / general patrol |
| 250 | Station SGT |
| 251/252 | District Patrol Supervisor |
| 260 | Senior SGT |
| 265 | Divisional Supervisor |
| 290–292 | PACER / MHaP |
| 300–399 | Divisional Van |
| 440–449 | Regional Response Unit |
| 450–499 | SOCIT / FVIU |
| 500–599 | CIU |
| 600–699 | Highway Patrol |
| 700–799 | District Support Services |
| 900–999 | Station base / fixed positions |

### Shift Numbers
- **Sedans**: MS=207, AS=203, NS=211
- **Div Vans**: MS=307, AS=303, NS=311
- **CIU**: MS=507, AS=503/520, NS=541–546

## Callsign Examples

| Callsign | Meaning |
|----------|---------|
| `ESP311` | Shepparton — Night shift Divisional Van |
| `NMN207` | Melbourne North — Morning shift Sedan |
| `SDG503` | Dandenong — Afternoon CIU unit |
| `WGL651` | Geelong — HWP Sergeant |
| `CAN211` | Canine unit |

## Notes

- `XXX` in callsigns means a station code is substituted (e.g. `ESP`, `BND`, `WBN`)
- Special units (POLAIR, Transit, SOG, Counter Terrorism) use fixed prefixes regardless of station
- State Highway Patrol (SHP/TRF) units work across divisions from fixed locations

## Disclaimer

This tool is for simulation, training, and enthusiast purposes only. Not affiliated with Victoria Police.
