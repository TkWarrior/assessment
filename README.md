# OPM System — Backend

A production-grade **Organizational Project Management System** backend built with:

- **FastAPI** — async REST API framework
- **PostgreSQL** — primary database
- **SQLAlchemy 2.0** — ORM
- **Alembic** — database migrations
- **JWT** — stateless authentication (access + refresh tokens)
- **Pydantic v2** — request/response validation

---

## Quick Start

### 1. Prerequisites
- Python 3.11+
- PostgreSQL running on `localhost:5432`

### 2. Setup

```bash
# Create the database
psql -U postgres -c "CREATE DATABASE project_mgmt_db;"

# Activate virtual environment
.\venv\Scripts\activate          # Windows
# source venv/bin/activate       # Linux/macOS

# Install dependencies
pip install -r requirements.txt

# Configure environment
copy .env.example .env
# Edit .env with your DATABASE_URL and SECRET_KEY
```

### 3. Run Migrations

```bash
alembic upgrade head
```

### 4. Start the Server

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Open **http://localhost:8000/docs** for interactive Swagger UI.

---

## Project Structure

```
backend/
├── alembic/              # Database migrations
│   └── versions/         # Migration scripts
├── app/
│   ├── auth/             # JWT authentication
│   ├── models/           # SQLAlchemy ORM models (9 tables)
│   ├── schemas/          # Pydantic request/response schemas
│   ├── routers/          # FastAPI route handlers (thin controllers)
│   ├── services/         # Business logic layer
│   ├── utils/            # Enums, exceptions, code generator
│   ├── config.py         # Settings from .env
│   ├── database.py       # SQLAlchemy engine + session
│   ├── dependencies.py   # Shared FastAPI dependencies
│   └── main.py           # App factory
└── requirements.txt
```

---

## API Endpoints (48 total)

| Module | Base Path |
|---|---|
| Auth | `/api/v1/auth` |
| Projects | `/api/v1/projects` |
| Estimations | `/api/v1/projects/{id}/estimations` |
| Assignments | `/api/v1/projects/{id}/assignments` |
| Tasks | `/api/v1/projects/{id}/tasks` |
| Time Logs | `/api/v1/time-logs` |
| Breach Logs | `/api/v1/breach-logs` |
| Performance | `/api/v1/performance` |
| Delivery | `/api/v1/projects/{id}/delivery` |
| Reports | `/api/v1/reports` |

---

## Performance Score Formula

```
Score = 40% × Completion Rate
      + 30% × Deadline Adherence
      + 20% × Hour Efficiency
      + 10% × Breach Score (100 - breaches × 20)
```

| Score | Tag |
|---|---|
| 85–100 | Exceeding |
| 65–84 | On Track |
| 40–64 | Lagging |
| 0–39 | Critical |

---

## Roles

| Role | Permissions |
|---|---|
| `admin` | Full access including user management and estimation approval |
| `project_manager` | Manage projects, assignments, tasks, delivery |
| `team_member` | Log time, transition own tasks, read-only on most entities |
