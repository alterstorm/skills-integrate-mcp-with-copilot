---
name: Add persistent database and migrations
about: Replace the in-memory store with a persistent DB and add migrations
labels: enhancement, backend, database
---

**Description**

Replace the in-memory activity store with a persistent database (SQLite for local/dev, Postgres for production) and add migration support via Alembic.

**Why**

Current data resets on server restart. Persistence is required for real usage and enables schema evolution.

**Acceptance criteria**

- Add SQLAlchemy models for Activities and Students
- Configure database URL via environment variables
- Add Alembic and initial migration
- Update endpoints to use DB sessions
- Update `requirements.txt` with `sqlalchemy` and `alembic` (and `asyncpg` if using async drivers)

**Notes**

This is the first step toward making the app production-ready. Start with SQLite for local dev to keep setup minimal.
