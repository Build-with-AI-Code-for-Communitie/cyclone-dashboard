# cyclone-dashboard

React + MapLibre frontend for the **Cyclone AI Command Center**. Lets a user select a cyclone scenario, run a simulation, and see flood extent, at-risk infrastructure, and an AI-generated action plan on a single live map.

## What this repo does

- Renders satellite/terrain base layers and flood-risk overlays on an interactive map
- Lets the user pick a cyclone scenario and intensity, then trigger a simulation
- Displays critical infrastructure (hospitals, shelters, substations, roads) with risk-based styling
- Shows the Gemini-generated action plan as prioritized cards
- Talks only to `cyclone-api` — no direct data processing happens here

## Repo structure

```
cyclone-dashboard/
├── src/
│   ├── components/
│   │   ├── Map/                  # MapLibre GL map + layer controls
│   │   ├── RiskPanel/            # risk score, wind, rainfall readouts
│   │   ├── InfrastructurePanel/  # hospital/substation/road/shelter counts
│   │   ├── ActionPlan/           # Gemini action plan cards
│   │   └── ScenarioSelector/     # cyclone scenario + intensity picker
│   ├── api/
│   │   └── client.ts             # cyclone-api client
│   ├── hooks/
│   ├── App.tsx
│   └── main.tsx
├── public/
├── index.html
├── vite.config.ts
├── tailwind.config.js
├── package.json
├── .env.example                  # VITE_API_BASE_URL
└── README.md
```

## Setup

```bash
git clone <repo-url>
cd cyclone-dashboard
npm install
cp .env.example .env   # set VITE_API_BASE_URL to your cyclone-api instance
```

## Running locally

```bash
npm run dev
```

Runs on `http://localhost:5173` by default. Requires `cyclone-api` running (see that repo's README) and reachable at the URL in `.env`.

## Build

```bash
npm run build
npm run preview   # test the production build locally
```

## Environment variables

```
VITE_API_BASE_URL=http://localhost:8000
```

## Demo flow this UI is built around

1. Select a coastal region and cyclone scenario (e.g. "Category 4, Kakinada")
2. Click **Simulate** — map animates in flood-risk layers
3. Critical infrastructure inside the flood zone highlights automatically
4. Click **Generate Action Plan** — AI recommendation cards appear, ranked by priority

## Tech choices

- **MapLibre GL JS** (not Leaflet) — needed for smooth layer-opacity transitions when flood zones "appear" on simulate; this is the single most important visual moment in the demo
- **Recharts** for risk/rainfall/wind readouts in the side panel
- **Tailwind** for styling

## Consumes

- [`cyclone-api`](../cyclone-api) — all data and AI-generated content

## Disclaimer

Displayed predictions are decision-support estimates for demo purposes, not operational forecasts. Real disaster response should rely on official IMD/NDMA warnings.
