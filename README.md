# Cargo Operations System

A full-stack cargo/vehicle route optimization platform for planning delivery routes across a real road network (Kocaeli, Turkey). The backend solves a Vehicle Routing Problem (VRP) using the Clarke-Wright savings algorithm over an OpenStreetMap-derived road graph, and exposes the results through a REST API consumed by a React dashboard.

## Features

- **Route optimization** — Clarke-Wright savings algorithm (`services/vrp_clark_wright.py`) computes near-optimal multi-vehicle delivery routes.
- **Real road network routing** — distances and paths are computed on an actual street graph (via `osmnx`/`networkx`), not straight-line distance.
- **Cargo & fleet management** — models for cargo, vehicles, stations, and system settings.
- **Scenario tracking** — optimization runs are saved and can be replayed/compared.
- **Supabase-backed persistence** — station, vehicle, and cargo data is read from/written to Supabase (Postgres).
- **Interactive map dashboard** — React + Leaflet frontend for visualizing routes and managing operations.

## Tech Stack

| Layer | Technology |
| --- | --- |
| Backend | Python, FastAPI, NetworkX, OSMnx, Supabase (Postgres) |
| Frontend | React 19, Vite, Leaflet / React-Leaflet, Axios |
| Routing algorithm | Clarke-Wright savings (VRP heuristic) |

## Project Structure

```text
CargoOperationsSystem/
├── backend/
│   ├── main.py              # FastAPI app entrypoint
│   ├── routers/             # API route definitions
│   ├── controllers/         # Request handling / orchestration
│   ├── models/               # Pydantic data models
│   ├── services/             # Business logic (VRP solver, Supabase access)
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── pages/           # Route/dashboard pages
│   │   ├── components/      # Reusable UI components
│   │   ├── services/         # API client layer
│   │   └── styles/
│   └── package.json
└── database/                 # SQL schema and sample scenario data
```

> The road-network cache previously committed under `backend/cache/` (graph, distance matrix, geocoding cache — roughly 120 MB) has been removed from version control. These files are regenerated automatically on first run and are now covered by `.gitignore`.

## Getting Started

### Backend

```bash
cd backend
python -m venv venv && source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload
```

Create a `.env` file in `backend/` with your Supabase credentials (`SUPABASE_URL`, `SUPABASE_KEY`) before starting the server — see `services/supabase_service.py` for the expected variable names.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The frontend expects the API at `http://localhost:8000` by default, and the dev server runs on `http://localhost:5173`.

## Database

SQL schema and example scenario data are provided under `database/` (`senaryo1.sql`, `test.sql`, `database.json`) for setting up a local/Supabase instance.

## License

No license specified yet.
