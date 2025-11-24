# Backlog: Issues to create for features from OoS-Backend

This file contains prepared GitHub issue drafts for migrating and extending the current `Mergington High School Activities` app with features inspired by `ita-social-projects/OoS-Backend`.

---

## Issue: Add persistent database and migrations
**Description:** Replace the in-memory activity store with a persistent database (SQLite for local/dev, Postgres for production) and add migrations via Alembic.

**Why:** Current data resets on server restart. Persistence is required for real usage and enables schema evolution.

**Acceptance criteria:**
- Add SQLAlchemy models for Activities and Students
- Configure database URL via environment variables
- Add Alembic and initial migration
- Update endpoints to use DB sessions
- Update `requirements.txt` with `sqlalchemy` and `alembic` (and `asyncpg` if using async drivers)

**Labels:** enhancement, backend, database

---

## Issue: Add authentication and role-based access
**Description:** Implement authentication and basic role management (admin, teacher, student). Use OAuth2 password flow with JWTs (FastAPI's `OAuth2PasswordBearer`) or integrate a small OpenID/OAuth provider depending on scope.

**Why:** Endpoints are currently open; auth is required for managing activities, admin operations, and protecting participant data.

**Acceptance criteria:**
- Add user model with hashed password and role field
- Implement login endpoint returning JWT access tokens
- Protect admin endpoints (future admin-only operations)
- Add dependency to get current user and check roles

**Labels:** enhancement, auth, backend

---

## Issue: Add email notifications (confirmation & alerts)
**Description:** Add a configurable email sender to send signup confirmations and admin notifications (SMTP or a 3rd-party provider like SendGrid).

**Why:** Users should receive confirmations of signup and optionally reminders or important notices.

**Acceptance criteria:**
- Add email configuration via environment variables
- Send confirmation email on successful signup
- Include an option to disable email in dev

**Labels:** enhancement, notifications

---

## Issue: Add file/object storage abstraction for uploads
**Description:** Add endpoints and model support to attach files (e.g., flyers or photos) to activities. Provide a storage abstraction with a local filesystem backend and a pluggable S3-compatible storage option.

**Why:** Activities often need images, materials, or documents attached.

**Acceptance criteria:**
- Create interfaces and one implementation (local filesystem)
- Add upload endpoint and metadata storage
- Store file metadata in DB

**Labels:** enhancement, storage, backend

---

## Issue: Add Redis caching for hot endpoints
**Description:** Integrate Redis caching for frequently accessed endpoints (e.g., `/activities`) to improve performance.

**Why:** Caching reduces DB load and speeds up frontend responses.

**Acceptance criteria:**
- Add Redis config and optional caching wrapper for `/activities`
- Add env var toggle to enable/disable cache

**Labels:** enhancement, infra, performance

---

## Issue: Add external provider integration (pluggable)
**Description:** Create a small client/service abstraction to fetch provider/activity listings from an external API (pluggable). Add a fake provider implementation for tests.

**Why:** To sync or import provider data from third-party sources (like OoS's Aikom integration).

**Acceptance criteria:**
- Provider client interface + one fake implementation
- Endpoint or admin job to import providers

**Labels:** enhancement, integration, backend

---

## Issue: Add feature flags and Swagger/OpenAPI customization
**Description:** Add a simple feature flag system (config-driven) and adapt API docs so feature-gated endpoints are visible only when enabled.

**Why:** To rollout features progressively and keep API docs accurate per environment.

**Acceptance criteria:**
- Add a `FEATURES` config object and a decorator to gate endpoints
- Update auto-generated docs to show/hide endpoints per feature toggles (or add docs note)

**Labels:** enhancement, docs

---

## Issue: Add ratings and categories to activities
**Description:** Extend the domain model to support category tags and user ratings for activities and/or providers.

**Why:** Ratings help students find popular activities; categories improve discoverability.

**Acceptance criteria:**
- Add category model and association
- Add rating model (user, entity, score, optional comment)
- Endpoints to submit and fetch ratings

**Labels:** enhancement, backend, api

---

## Issue: Add background job support and scheduled tasks
**Description:** Add a lightweight background job scheduler (e.g., `apscheduler` or Celery + Redis) to run tasks like reminders, periodic imports, or cleanup.

**Why:** Background tasks enable email reminders and scheduled imports/migrations.

**Acceptance criteria:**
- Add a scheduler dependency and a simple scheduled job (example: nightly cleanup or reminders)
- Document how to run the scheduler in dev and production

**Labels:** enhancement, infra

---

## Issue: Add bulk import/admin tooling
**Description:** Add an admin endpoint or bulk import script to create activities from CSV/JSON in bulk.

**Why:** Admins/teachers should be able to onboard many activities/providers quickly.

**Acceptance criteria:**
- Implement CSV upload endpoint or CLI management command
- Validate records and return summary of imports

**Labels:** enhancement, admin

---

## Issue: Add infra artifacts (Docker, simple Terraform)
**Description:** Add a `Dockerfile` and basic `docker-compose.yml` for local development (Postgres + Redis + API). Optionally add simple Terraform skeleton for provisioning cloud resources.

**Why:** Makes deployment reproducible and aligns with OoS infra approach.

**Acceptance criteria:**
- Add `Dockerfile` for the app
- Add `docker-compose.yml` with Postgres and Redis services for local dev
- Add `Terraform/README.md` stub for future infra work

**Labels:** infra, devops

---

*If you want me to open these as real GitHub issues, tell me and I will create them individually. For now I added these drafts to `.github/ISSUES_TO_CREATE.md` so you can review and edit.*
