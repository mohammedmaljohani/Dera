# Dera — one live picture for disaster response

**MEDATHON 2026 · Track 1: Healthcare Crowd & Patient Flow Management · Team AidFlow (MED-AULCUT)**

**▶ Live demo: https://mohammedmaljohani.github.io/Dera/**
Code: https://github.com/mohammedmaljohani/Dera

Dera is a working web prototype that puts command, dispatch, ambulance crews and hospital emergency departments on one shared screen during a disaster. It decides how serious each incident is from its facts, suggests which unit to send and which hospital can still take each ambulance's patients, and explains every suggestion. A person approves every critical decision, and every change is logged. It works in Arabic and English, on computer and phone.

> **Everything in this prototype is simulated.** The Jeddah flash-flood scenario, incidents, units, GPS positions, reports and messages are scripted demo data. **All hospitals are fictional** and their bed counts are invented. No real system or patient data is connected.
> Severity levels and recommendations come from **fixed, explained rules, not AI**. The rules still need validation by EMS experts. **Dera is not a clinical triage tool.**

---

## Try it in 3 minutes

1. Open the **[live demo](https://mohammedmaljohani.github.io/Dera/)** in Chrome, Edge, Safari or Firefox. Nothing to install.
2. Press **Start demo** at the bottom of the map. On-screen subtitles explain each event as the simulated flood unfolds (turn them off with **CC** in the top bar).
3. Approve a suggestion in **Decision support** on the right, and watch the unit move on the map.
4. Switch role with the menu at the top right: **Commander**, **Dispatcher**, **Hospital ED**, **Field Units**.
5. Switch language with the globe button (**ع** / **EN**). The whole interface turns right-to-left.
6. At the end, open **Report** and press **Generate report** for a summary of what happened and what is still open.

## The problem

In a disaster, ambulance, civil-defense and hospital teams often work in separate systems with no shared live view. Patients can be taken to hospitals that are already full, and commanders lose time separating real emergencies from routine events and unverified reports. The evidence we gathered for this (Saudi studies and expert advice) is in our Filter 2 presentation.

## What Dera does

| Capability | What happens in the prototype |
| --- | --- |
| **Incident levels with reasons** | Each incident gets a level (1 Major Disaster to 5 Routine) from its facts: threat to life, casualties, people affected, spread and units needed. Every rule that fired is shown. A person can change the level, with a reason. |
| **Report verification** | Public reports stay "Unverified" until a dispatcher confirms them, so they are not counted too early. Likely duplicates and clusters of reports are flagged. |
| **Unit suggestions** | Units are ranked by capability, travel time on the road network (avoiding closed roads and flood zones), fuel and supplies. |
| **Hospital choice for each ambulance** | Hospitals are ranked by travel time, capabilities and free ED and ICU beds, **counting patients already on the way and walk-in patients**. If the nearest hospital's ICU is full with incoming patients, the field screen says why it was skipped. |
| **Hospital view** | Emergency departments see incoming ambulances, update capacity, accept or ask to divert units, and report walk-in patients. |
| **Field unit (phone)** | Tasks, status, triage counts (red, yellow, green, black), transport, SOS, offline mode and an End-of-Shift report. |
| **Human control and audit** | The Commander approves Level 1 decisions and hospital diversions. Every action is written to the event log. |

## Tech stack

| Part | Tool and role |
| --- | --- |
| Interface | **React 18** (single page) and **Tailwind CSS**; English and Arabic with full right-to-left layout |
| Map | Offline **SVG map of Jeddah** built from OpenStreetMap road and coastline data; works without internet |
| Routing | **Dijkstra shortest path** on the road graph, re-routed around closures and flood zones |
| Decision engine | Rule-based classification and recommendation engine in **JavaScript**, with every rule's reason shown |
| Build | **esbuild** bundles `src/` into one self-contained page, `index.html` |
| Testing | **Playwright** with Chromium: automated end-to-end tests in English and Arabic, on computer and phone sizes |
| Hosting | **GitHub Pages** (static, no server) |

## Testing

Automated browser tests drive the real app and check each result. **All 264 checks pass**:

| Suite | Checks | What it covers |
| --- | --- | --- |
| Classification scenarios | 111 | 9 scenarios at 1440, 375 and 414 px wide |
| Hospital choice and walk-ins | 35 | Patients on the way counted, skip reasons, walk-ins, wording |
| Demo subtitles | 49 | Order, pause, language switch, CC toggle, reset, layout, judge actions |
| Verification and override | 16 | Verification queue, overrides need a reason, review flags |
| Shift report, offline mode, phone layout | 53 | End-of-Shift report, offline lock, phone screens, map taps |

A layout sweep also checks 22 screens (English and Arabic at 1440, 1280 and 390 px): no cut-off buttons, no sideways scrolling, no console errors.

**Not tested yet:** use by real command-room or ED staff, real data feeds, clinical accuracy of the thresholds, and performance under real load.

### Run the tests yourself

Requires [Node.js](https://nodejs.org) 18 or later. On Windows, run `npm test` from Git Bash.

```bash
git clone https://github.com/mohammedmaljohani/Dera.git
cd Dera
npm install                          # esbuild and Playwright
npx playwright install chromium      # the test browser
npm run build                        # builds index.html from src/
npm test                             # main suites, one PASS/FAIL line each
npm run test:day1                    # hospital choice and walk-ins
npm run test:subtitles               # demo subtitles (computer and phone)
npm run test:layout                  # 22-screen layout sweep
npm run test:standalone              # opens index.html exactly as a visitor would
```

## Project layout

| Path | Contents |
| --- | --- |
| `index.html` | The built prototype (also published as `Dera-prototype.html`). Open either one. |
| `src/` | Source code, combined in file-name order: data, translations, rules, engine, routing, map, screens, app |
| `styles.css`, `template.html` | Page styles and the HTML shell |
| `build.mjs` | Builds `index.html` from `src/` |
| `test/` | Playwright test suites and their helpers |

## Data and limits

- Map: Jeddah roads and coastline © OpenStreetMap contributors, Open Database License (ODbL).
- Classification thresholds are based on university lecture material on levels of disaster; they are working assumptions until reviewed by experts.
- Not built yet: sign-in and role permissions, a server, and links to real hospital, GPS and dispatch systems.

## Team AidFlow

- **Mohammed Motlaq Aljohani** — team leader, domain knowledge and experience
- **Rakan Mazyad Alharbi** — design
- **Tariq Sulaiman Alotaibi** — technology and prototype development
