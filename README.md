# Victoria Police CAD Tool

A web-based tool for generating realistic Victoria Police CAD radio lineups for use in [MissionChief](https://www.missionchief-australia.com/) and similar emergency services simulation games. Built to reflect real VicPol operational structure, callsign numbering conventions, station classifications, and support relationships.

Hosted on GitHub Pages — no server, no login, no installation required.

---

## Table of Contents

1. [Repository Structure](#repository-structure)
2. [How to Use the Tool](#how-to-use-the-tool)
3. [Station Classifications](#station-classifications)
4. [Callsign Number Logic](#callsign-number-logic)
5. [Available Services](#available-services)
6. [Hosting on GitHub Pages](#hosting-on-github-pages)
7. [Managing Stations (stations.csv)](#managing-stations-stationscsv)
8. [Adjusting Unit Counts (data.js)](#adjusting-unit-counts-datajs)
9. [Adding a New Service Type](#adding-a-new-service-type)
10. [File Reference](#file-reference)
11. [Disclaimer](#disclaimer)

---

## Repository Structure

```
index.html       Page structure — HTML only, no logic
style.css        All visual styling
data.js          Static data: station template, services list, unit pool builders, default counts
app.js           Runtime logic: CSV loading, step navigation, generation, rendering, export
stations.csv     Station database — the main file you will edit regularly
README.md        This file
LICENSE          Project licence
```

The tool loads `stations.csv` automatically on startup. If the CSV is missing (e.g. when testing locally by double-clicking `index.html`), it falls back to the placeholder station template in `data.js` so the tool still opens without errors.

---

## How to Use the Tool

The tool runs as a three-step wizard.

### Step 1 — Station

Select your **Region**, then **Division**, then **Station** from the cascading dropdowns. These are populated from `stations.csv` at load time.

Once a station is selected, the **Station Classification** card appears. This is pre-filled from the CSV but can be overridden here before generating. See [Station Classifications](#station-classifications) for a full description of each type.

The **Supervisor Required** toggle controls whether Command & Supervision units are included in the output. Set to *No* for a unit-only lineup.

### Step 2 — Services

Tick which unit types operate from this station. You can select any combination. See [Available Services](#available-services) for a full list. Command and Supervision units are added automatically based on classification — you do not need to select them here.

### Step 3 — Lineup

The generated output shows all units grouped by service type. Each entry displays a callsign and a role description.

**Callsign numbers vary on each generation** within the valid range for each shift. This reflects the flexibility Victoria Police allows in unit numbering and ensures no two lineups look identical.

#### Adjusting Unit Counts

Each scalable service (Station Cars, Divisional Vans, HWP, CIU, PORT/District Support, RRU) has a **unit count slider**. Drag it left to reduce or right to increase the number of units shown.

The slider always maintains a balanced spread across morning (0700), afternoon (1500), and night (2300) shifts. Reducing from 9 units to 3 gives one per shift — not three morning units.

#### Station Support Links

If PSA, HWP, and CIU codes are set for the selected station in `stations.csv`, a Support Links card appears in the output showing the station name and code for each.

#### Exporting

A formatted plain-text export appears at the bottom of the output. Click **Copy to Clipboard** to copy it. The export updates live as you adjust sliders.

---

## Station Classifications

Classifications control the default number of units generated and the supervision structure. They are set per station in `stations.csv` and can be overridden in Step 1 before generating.

### Metropolitan (24 Hours) — `metro_24`

Metropolitan police stations providing continuous 24/7 operational policing coverage within high-density urban areas. These stations support rapid response capability, manage high call volumes, and typically service large suburban populations. They function as core nodes within the metropolitan policing network and often coordinate with nearby stations for incident response and specialist tasking.

**Examples:** Box Hill, Dandenong, Melbourne West, Sunshine, Werribee

**Default unit output:** large — 12 cars, 6 vans, full supervision stack (250, 251, 252, 260, 265, 100)

---

### Metropolitan (Non-24 Hours) — `metro_non24`

Metropolitan or inner-urban stations that maintain ongoing policing coverage through patrol structures but do not provide continuous 24-hour public counter or on-site station staffing. Operational response is still maintained 24/7 via nearby hub stations or patrol units. These stations generally focus on community engagement, local reporting services, and daytime/front-counter operations.

**Examples:** Brunswick, Epping, Mornington, Rowville, Springvale

**Default unit output:** medium — 7 cars, 3 vans, supervision limited to 250 and 251 (single SGT subject to staffing)

---

### Regional (24 Hours) — `regional_24`

Regional police stations that provide continuous 24/7 operational coverage across large geographic areas outside metropolitan Melbourne. These stations act as key regional hubs, often supporting surrounding smaller towns and rural communities. They typically manage a broad range of policing functions including general duties, traffic enforcement, and incident response coordination.

**Examples:** Bendigo, Geelong, Morwell, Swan Hill, Warragul

**Default unit output:** large — 9 cars, 4 vans, full supervision stack (250, 251, 252, 260, 265, 100)

---

### Regional (Non-24 Hours) — `regional_non24`

Regional or rural stations that maintain continuous policing coverage through mobile patrols and nearby hub stations but do not operate a full-time staffed station or 24-hour public counter. These stations primarily provide local access points for reporting, community engagement, and visible police presence, while operational response is coordinated from larger nearby regional hubs.

**Examples:** Bright, Echuca, Horsham, Portland, Yarrawonga

**Default unit output:** minimal — 3 cars, 2 vans, supervision limited to 250 and 251 (single SGT subject to staffing)

---

## Callsign Number Logic

### Station Code Format

Every callsign starts with the station code — a 2–5 character prefix built from a region letter plus a short station abbreviation.

```
[Region letter] + [Station abbreviation]

N = North West Metro     S = Southern Metro
E = Eastern              W = Western

Examples:
  E + SP  →  ESP    Shepparton
  W + BI  →  WBI    Bendigo
  N + MN  →  NMN    Melbourne North
  S + FK  →  SFK    Frankston
```

### Number Ranges by Unit Type

```
100         Superintendent
200–299     Station Cars (general duties sedans)
250         Station Sergeant
251         District Patrol Supervisor (SGT)
252         District Patrol Supervisor (SGT) — Night shift
260         Senior Sergeant
265         Divisional Supervisor (S/SGT)
290–292     PACER / MHaP
300–399     Divisional Vans
440–449     Regional Response Unit (RRU)
450–499     SOCIT / FVIU
500–599     Criminal Investigation Unit (CIU)
600–699     Highway Patrol (HWP)
700–799     District Support Services / PORT
800–899     Mounted Branch
900–929     Fixed base stations
CAN         Dog Squad (prefix, not station-based)
RES         Search & Rescue
TST         Transit Police
SCY / CIR   SOG / CIRT
POLAIR      Air Wing
ROA         Heavy Vehicle Unit
MOU         Mounted Branch
REG         SOCIT (regional prefix)
```

### Shift Number Convention

For Station Cars and Divisional Vans, the trailing digit of the unit number indicates shift start time:

```
x07  →  Morning shift    (0700 start)   e.g. ESP207, ESP307
x03  →  Afternoon shift  (1500 start)   e.g. ESP203, ESP303
x11  →  Night shift      (2300 start)   e.g. ESP211, ESP311
```

Additional units on the same shift increment outward from those bases — 204, 208, 201 for morning; 206, 209, 202 for afternoon; 214, 217, 212 for night.

---

## Available Services

| Service | Callsign Range | Notes |
|---|---|---|
| Station Cars | 200–299 | General duties sedans |
| Divisional Vans | 300–399 | Cage vans, typically crewed by 2 officers |
| Highway Patrol | 600–699 | Cars (610–649), motorcycles (600–609), Q cars (630–639), SGT (650–659), S/SGT (660–669) |
| PORT / District Support | 700–799 | Special duties, events, foot patrol, licensing, bicycle, guards |
| CIU | 500–599 | Criminal Investigation Unit |
| FVIU | 480–499 | Family Violence Investigation Unit — day/afternoon only |
| SOCIT | REG prefix | Sexual Offences & Child Investigations — regional asset |
| RRU | 440–449 | Regional Response Unit |
| Dog Squad | CAN prefix | Canine units — prefix independent of station code |
| PACER / MHaP | 290–292 | Mental health co-response |
| Search & Rescue | RES prefix | Missing persons, bush, maritime — often callout-based |
| Transit Police | TST prefix | Public transport policing |
| SOG / CIRT | SCY / CIR prefix | Special Operations Group / Critical Incident Response Team |
| POLAIR | POLAIR prefix | Air wing — helicopters and fixed wing |
| Heavy Vehicle Unit | ROA prefix | Heavy vehicle compliance and enforcement |
| Mounted Branch | MOU prefix | High-visibility patrol and crowd control |

Command & Supervision (250, 251, 252, 260, 265, 100) are added automatically when *Supervisor Required* is set to *Yes* in Step 1.

---

## Hosting on GitHub Pages

1. Create a new GitHub repository
2. Upload all files to the repository root: `index.html`, `style.css`, `data.js`, `app.js`, `stations.csv`, `README.md`
3. Go to **Settings → Pages**
4. Under **Source**, select `Deploy from a branch`
5. Set branch to `main`, folder to `/ (root)`, then click **Save**
6. Your tool will be live at `https://yourusername.github.io/repo-name` within about 60 seconds

To update anything after the initial deploy, edit the relevant file directly in GitHub's web editor and commit the change. No local tools or build steps required.

> **Important:** All five files must be in the same folder. The tool fetches `stations.csv` relative to `index.html`, and `index.html` loads `data.js` and `app.js` from the same directory.

---

## Managing Stations (stations.csv)

This is the file you will edit most often. Each row is one station.

### Column Reference

| Column | Required | Description | Example |
|---|---|---|---|
| `code` | Yes | Station callsign prefix — uppercase, 2–5 characters | `ESP` |
| `name` | Yes | Station display name — do not include "Police Station" | `Shepparton` |
| `region` | Yes | Single region letter: `N`, `S`, `E`, or `W` | `E` |
| `region_label` | Yes | Full region name shown in the dropdown | `Eastern` |
| `division` | Yes | Division name — stations with the same value are grouped | `Goulburn Valley (ED3)` |
| `div_code` | No | Short division reference code, internal only | `ED3` |
| `psa` | No | Code of the Police Service Area covering this station | `ESP` |
| `hwp` | No | Code of the Highway Patrol unit servicing this station | `ESP` |
| `ciu` | No | Code of the CIU servicing this station | `ESP` |
| `classification` | Yes | Station type — see values below | `regional_24` |

**Classification values:**

| Value | Meaning |
|---|---|
| `metro_24` | Metropolitan, 24-hour continuous coverage |
| `metro_non24` | Metropolitan, non-24-hour public counter |
| `regional_24` | Regional, 24-hour continuous coverage |
| `regional_non24` | Regional, non-24-hour public counter |

**Rules:**
- Lines starting with `#` are comments and are ignored — use them freely for section labels and notes
- Leave optional fields blank rather than writing `N/A` — just keep the comma so column alignment stays correct
- Stations appear in the dropdown in the order they appear in the file
- The header row (`code,name,region,...`) must stay exactly as written

### Adding a Station

Add a new row under the relevant division comment. For example, adding Kyabram to Goulburn Valley:

```csv
# ── EASTERN — Goulburn Valley (ED3)
ESP,Shepparton,E,Eastern,Goulburn Valley (ED3),ED3,ESP,ESP,ESP,regional_24
EMO,Mooroopna,E,Eastern,Goulburn Valley (ED3),ED3,ESP,ESP,ESP,regional_non24
EKB,Kyabram,E,Eastern,Goulburn Valley (ED3),ED3,ESP,ESP,ESP,regional_non24
```

### Adding a New Division

Use a new value in the `division` column — the tool creates the dropdown group automatically:

```csv
EWA,Wangaratta,E,Eastern,Wangaratta (ED4),ED4,EWA,EWA,EWA,regional_24
EBN,Benalla,E,Eastern,Wangaratta (ED4),ED4,EWA,EWA,EWA,regional_non24
```

### Adding a New Region

**Step 1** — Add rows with a new single-letter region key in the `region` column:

```csv
CBG,Castlemaine,C,Central,Loddon Campaspe,LC,CBG,CBG,CBG,regional_24
```

**Step 2** — Add a matching `<option>` to the Region dropdown in `index.html`. Find the `id="selRegion"` select element and add:

```html
<option value="C">Central</option>
```

The `value` attribute must exactly match the letter used in `stations.csv`.

### Editing a Station

Change any field in the row directly. The change takes effect on the next page load.

### Removing a Station

Delete the entire row. If it was the only station in a division, that division will no longer appear in the dropdown.

### Quick Checklist

Before saving, verify:
- [ ] Every data row has exactly **9 commas** (10 fields)
- [ ] `region` is a single uppercase letter matching a known region key
- [ ] `classification` is one of the four valid values exactly as written
- [ ] The header row is unchanged
- [ ] After uploading, reload the tool and confirm the station appears correctly

---

## Adjusting Unit Counts (data.js)

Default unit counts per classification are defined in the `DEFAULTS` object near the top of `data.js`:

```javascript
const DEFAULTS = {
  metro_24:      { cars: 12, vans: 6, hwp: 11, ciu: 10, port: 12, rru: 5 },
  metro_non24:   { cars: 7,  vans: 3, hwp: 5,  ciu: 5,  port: 6,  rru: 3 },
  regional_24:   { cars: 9,  vans: 4, hwp: 8,  ciu: 7,  port: 8,  rru: 4 },
  regional_non24:{ cars: 3,  vans: 2, hwp: 2,  ciu: 2,  port: 2,  rru: 2 },
};
```

Change any value here to adjust the starting count for that service on that classification. The slider on the output page will start at this value but can still be adjusted at generation time.

The maximum number of units each pool can produce is set in `MAX_UNITS`:

```javascript
const MAX_UNITS = { cars: 15, vans: 6, hwp: 11, ciu: 10, port: 12, rru: 5 };
```

If you increase a `DEFAULTS` value above the corresponding `MAX_UNITS` value, increase `MAX_UNITS` to match — otherwise the slider will be capped below the default.

---

## Adding a New Service Type

Adding a new service requires changes in two files.

### 1. Add to SERVICES in data.js

Open `data.js` and add an entry to the `SERVICES` array:

```javascript
{ id: 'myunit', icon: '🚔', name: 'My Unit Name', desc: 'Short description. Range or prefix.' },
```

The `id` must be unique and contain only lowercase letters and numbers.

### 2. Add a unit block in app.js

Open `app.js` and find the section labelled `// ── Fixed-size services`. Add a new block following the same pattern as the existing ones:

```javascript
if (S.selected.has('myunit')) sections.push({
  id: 'myunit', icon: '🚔', name: 'My Unit Name', pool: null,
  units: [
    { cs: c + '700', desc: 'My Unit — Morning shift',   shifts: ['MS'] },
    { cs: c + '701', desc: 'My Unit — Afternoon shift', shifts: ['AS'] },
  ],
  note: 'Brief description of this unit type and its callsign range.',
});
```

If the service should have a slider (scalable unit count), add it to the `svcDefs` array in the `// ── Scalable services` section instead, and add a corresponding entry to `DEFAULTS` and `MAX_UNITS` in `data.js`. See the existing entries for `cars`, `vans`, `hwp`, `ciu`, `port`, and `rru` as examples.

---

## File Reference

| File | Edit frequency | What it contains |
|---|---|---|
| `stations.csv` | Regularly | All station data — codes, names, regions, divisions, support links, classifications |
| `data.js` | Occasionally | REGION_DATA template, SERVICES list, DEFAULTS counts, MAX_UNITS, pool builders |
| `app.js` | Rarely | CSV loader, step navigation, generation logic, rendering, export |
| `style.css` | Rarely | All visual styling — colours, layout, typography, component styles |
| `index.html` | Very rarely | HTML page structure only — update only if adding new page elements |

---

## Disclaimer

This tool is built for simulation and enthusiast purposes only. It is not affiliated with, endorsed by, or connected to Victoria Police or any government agency. All callsign conventions and operational structures are based on publicly available information and are used for recreational simulation purposes only.
