# 🚇 Paris Metro Map

Interactive web application for exploring the Paris Metro system. Features include viewing metro lines, stations, segments, and finding the shortest path between two points using the A* algorithm with transfer penalty support.

## 📋 Table of Contents

- [Features](#-features)
- [Quick Start with Docker](#-quick-start-with-docker)
- [Manual Setup (Without Docker)](#-manual-setup-without-docker)
- [Usage Guide](#-usage-guide)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [API Endpoints](#-api-endpoints)

---

## ✨ Features

- **Interactive Map** — View all 16 Paris Metro lines on an interactive map
- **Pathfinding** — Find the shortest path between any two stations or map coordinates using A* algorithm
- **Transfer Penalty** — Adjust transfer penalty to prefer routes with fewer line changes
- **Line Management** — View and toggle active/inactive status for entire lines
- **Station Management** — Search, filter, and toggle individual stations
- **Segment Management** — Browse, filter by line, and toggle individual segments
- **Accent-insensitive Search** — Type "Chatelet" to find "Châtelet"

---

## 🐳 Quick Start with Docker

> **This is the easiest way to run the project. You only need Docker installed — no Python, no MongoDB, no manual setup required.**

### Step 1: Install Docker

Download and install **Docker Desktop** for your operating system:

| OS | Download Link |
|---|---|
| **Windows** | [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/) |
| **macOS** | [Docker Desktop for macOS](https://www.docker.com/products/docker-desktop/) |
| **Linux** | [Docker Engine for Linux](https://docs.docker.com/engine/install/) |

> **Windows users**: During installation, make sure **WSL 2** is enabled when prompted.

After installing, **open Docker Desktop** and make sure it's running (you'll see the Docker icon in your system tray/taskbar).

### Step 2: Download the project

**Option A — Using Git** (if you have Git installed):

```bash
git clone https://github.com/Tuanducbu1/paris-metro-map.git
cd paris-metro-map
```

**Option B — Download ZIP** (if you don't have Git):

1. Go to [https://github.com/Tuanducbu1/paris-metro-map](https://github.com/Tuanducbu1/paris-metro-map)
2. Click the green **"Code"** button → **"Download ZIP"**
3. Extract the ZIP file
4. Open a terminal/command prompt and navigate to the extracted folder:
   ```bash
   cd path/to/paris-metro-map
   ```

### Step 3: Start the application

Run this single command in the project folder:

```bash
docker compose up --build
```

> ⏳ **The first run** will take a few minutes to download images and build containers. Subsequent runs will be much faster.

Wait until you see output similar to:

```
paris-metro-frontend  | ... start worker process ...
paris-metro-backend   | ... Application startup complete.
```

### Step 4: Open the application

Open your web browser and go to:

👉 **[http://localhost:5500](http://localhost:5500)**

> **That's it!** The application should be fully functional with the map loaded.

### Stopping the application

Press `Ctrl + C` in the terminal where Docker is running, then:

```bash
docker compose down
```

To also remove the database data (start fresh next time):

```bash
docker compose down -v
```

---

## 🔧 Manual Setup (Without Docker)

If you prefer to run the project without Docker, follow these steps.

### Prerequisites

- **Python 3.12+** — [Download](https://www.python.org/downloads/)
- **MongoDB 7+** — [Download](https://www.mongodb.com/try/download/community)
- A modern web browser (Chrome, Firefox, Edge)

### Step 1: Start MongoDB

Make sure MongoDB is running on your machine (default port `27017`).

### Step 2: Setup Backend

```bash
cd backend

# Create and activate virtual environment
python -m venv .venv

# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create environment file
echo MONGO_HOST=localhost > .env
echo MONGO_PORT=27017 >> .env
echo MONGO_DB_NAME=paris_metro_map >> .env

# Start the server
fastapi dev app/main.py
```

The backend will be available at `http://127.0.0.1:8000`.

### Step 3: Open Frontend

Open `frontend/index.html` in your browser, or use a local server:

```bash
# Using Python
cd frontend
python -m http.server 5500

# Or use VS Code Live Server extension (set port to 5500)
```

The frontend will be available at `http://localhost:5500`.

---

## 📖 Usage Guide

### Pathfinding

1. Click the **"Path"** tab in the sidebar
2. Click the **"From"** input field, then click a station on the map (or type to search)
3. Click the **"To"** input field, then click another station
4. Adjust the **Transfer Penalty** slider (0 = shortest distance, higher = fewer transfers)
5. Click **"Find"** to calculate the route
6. The route will be displayed on the map with step-by-step directions

### Viewing Lines

1. Click the **"Lines"** tab to see all 16 metro lines
2. Click a line to zoom to it on the map
3. Use the toggle switch to activate/deactivate entire lines

### Managing Stations & Segments

1. Use the **"Stations"** and **"Segments"** tabs to browse the network
2. Search by name or ID, filter by status (Active/Inactive) or line
3. Click any item to zoom to it on the map
4. Toggle individual stations or segments on/off

---

## 🛠 Tech Stack

| Component | Technology |
|---|---|
| **Frontend** | HTML, CSS, JavaScript, [Leaflet.js](https://leafletjs.com/), [Tailwind CSS](https://tailwindcss.com/) |
| **Backend** | [FastAPI](https://fastapi.tiangolo.com/) (Python) |
| **Database** | [MongoDB](https://www.mongodb.com/) with [Motor](https://motor.readthedocs.io/) (async driver) |
| **Map Tiles** | [CARTO Voyager](https://carto.com/basemaps/) |
| **Containerization** | [Docker](https://www.docker.com/) |

---

## 📁 Project Structure

```
paris-metro-map/
├── docker-compose.yml          # Docker orchestration
├── backend/
│   ├── Dockerfile              # Backend container config
│   ├── requirements.txt        # Python dependencies
│   ├── .env                    # Environment variables (not in repo)
│   └── app/
│       ├── main.py             # FastAPI entry point + CORS
│       ├── api/                # API route definitions
│       ├── core/               # Config & database connection
│       ├── crud/               # Database operations
│       ├── data/               # GeoJSON data (nodes & edges)
│       ├── schemas/            # Pydantic models
│       └── services/           # A* pathfinding algorithm
├── frontend/
│   ├── Dockerfile              # Frontend container config
│   ├── nginx.conf              # Nginx server config
│   ├── index.html              # Main HTML page
│   ├── css/style.css           # Custom styles
│   └── js/app.js               # Application logic
└── README.md
```

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/nodes/` | Get all stations |
| `GET` | `/api/edges/` | Get all segments |
| `PATCH` | `/api/nodes/{id}` | Toggle station active status |
| `PATCH` | `/api/edges/{id}` | Toggle segment active status |
| `PATCH` | `/api/lines/{line}` | Toggle entire line active status |
| `GET` | `/api/path/` | Find shortest path (params: `lon1`, `lat1`, `lon2`, `lat2`, `penalty`) |

---

## 📄 License

This project is for educational purposes.