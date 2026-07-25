# goodfood-ms-restaurant

Restaurant, menu & item management microservice for the [GoodFood](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF) platform.

**Status:** 🚧 Scaffold — only a `/health` route exists ([`app/main.py`](app/main.py)). No restaurant/menu logic has been written yet. See [`goodfood-ms-auth`](https://github.com/RMurier/goodfood-ms-auth#readme) for what a fleshed-out service in this platform looks like.

## Table of Contents

- [Intended Purpose](#intended-purpose)
- [Endpoints](#endpoints)
- [Tech Stack](#tech-stack)
- [Environment Variables](#environment-variables)
- [Running Locally](#running-locally)
- [Tests](#tests)
- [CI/CD](#cicd)

## Intended Purpose

Own restaurants, their menus and menu items — the catalog `ms-commandes` orders against and `ms-stock` tracks availability for.

## Endpoints

| Method | Route | Description |
|--------|-------|--------------|
| `GET` | `/health` | Health check, returns `{ "status": "ok" }` |

## Tech Stack

- Python, Flask 3, Gunicorn (production WSGI server)
- SQL Server, via `pyodbc` (driver connection string is wired up, no models/queries exist yet)

## Environment Variables

| Variable | Description |
|----------|--------------|
| `FLASK_ENV` | `development` or `production` |
| `FLASK_APP` | `app/main.py` |
| `DATABASE_URL` | SQL Server connection string, `mssql+pyodbc://...` (database `GoodFood_Restaurant_Dev` in dev) |

## Running Locally

### Via the platform's docker-compose (recommended)

From the [parent repo](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF):

```bash
docker compose -f docker-compose.dev.yml up -d db-sql-dev ms-restaurant-dev
```

Runs on `http://localhost:3002`.

### Standalone

```bash
python -m venv .venv
source .venv/bin/activate  # .venv\Scripts\activate on Windows
pip install -r requirements.txt
flask --app app/main.py run --port 5000
```

## Tests

```bash
pytest tests/
```

Currently a single sanity check ([`tests/test_sanity.py`](tests/test_sanity.py)), there to keep the CI test job green while the service is empty.

## CI/CD

Built, scanned (SonarQube, Trivy, OWASP Dependency-Check, GitGuardian) and published on every push, gated on all of them passing — see the [parent repo's CI/CD Pipeline section](https://github.com/RMurier/BAC-5-CUBE-1-COLLABORATIF#cicd-pipeline) for how the pipeline is wired across repos.
