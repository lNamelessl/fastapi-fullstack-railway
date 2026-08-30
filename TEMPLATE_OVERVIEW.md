# 🚀 Full Stack FastAPI Template

The official [Full Stack FastAPI Template](https://github.com/tiangolo/full-stack-fastapi-template) by tiangolo, rebuilt for Railway. Every deploy compiles the React frontend with Bun, bakes it into a FastAPI container, runs Alembic migrations against Postgres, seeds an admin user, and passes Railway's `/health` check before the release goes live.

One service, one database — and a working product on the first boot:

- 🔐 **Auth, done** — signup, login, JWT sessions, user settings, and an admin panel, secured with hashed passwords
- 📊 **Item dashboard** — a CRUD UI wired end-to-end to the API, generated from OpenAPI into a type-safe client
- 📫 **Password recovery** — email-based reset flow (works with any SMTP provider, disabled automatically when SMTP isn't configured)
- ☁️ **Railway-native** — binds to `0.0.0.0:$PORT`, parses Railway's `DATABASE_URL`, migrates on every deploy, health checks `/health`

## ✨ Features

**Backend (FastAPI, Python 3.14)**
- FastAPI with tagged routes, automatic OpenAPI docs at `/docs`, and typed path operation IDs
- SQLModel ORM + Alembic versioned migrations
- Pydantic v2 settings with env validation — the app refuses to boot with default secrets in production
- OAuth2 JWT access tokens (8-day expiry, configurable), reusable dependency-based permission checks
- Pytest suite covering auth, users, and items; GitHub Actions CI

**Frontend (React + TypeScript)**
- Vite + Tailwind CSS + shadcn/ui, with dark mode
- API client generated from the OpenAPI schema — backend changes surface as TypeScript errors, not runtime surprises
- Playwright end-to-end tests

**Railway adaptations (this fork)**
- Single multi-stage Dockerfile: `oven/bun` builds the frontend, `python:3.14` + `uv` runs the API — one service to deploy and scale, no separate web host, no CORS setup
- Migrations + superuser seeding run at container start via `scripts/prestart.sh` (idempotent — safe on every restart)
- Railway's `postgres://` `DATABASE_URL` is rewritten to the `postgresql+psycopg://` driver automatically
- Redirect-free `/health` endpoint, sized for platform probes

## 🚀 Deploy

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.app/new?github_url=https://github.com/lNamelessl/fastapi-fullstack-railway)

Two services are provisioned: `backend` (FastAPI serving the API and the frontend) and **Postgres** (with a persistent volume). After the first deploy:

1. Generate a public domain for `backend` (Settings → Networking) and set it as `FRONTEND_HOST`.
2. Open the domain and sign in at `/` with `FIRST_SUPERUSER` / `FIRST_SUPERUSER_PASSWORD`.
3. The API lives under `/api/v1`, interactive docs at `/docs`.

A live deployment of this exact template: https://backend-production-7fe75.up.railway.app

# Deploy and Host

Deploying provisions two services and a fully automated release pipeline:

1. **Build** — Railway builds the root `Dockerfile`: Bun compiles the React app, then `uv` installs the locked Python environment (`uv.lock`) on Python 3.14.
2. **Release** — on container start, `alembic upgrade head` brings the schema current and `initial_data.py` seeds the superuser from `FIRST_SUPERUSER` / `FIRST_SUPERUSER_PASSWORD`. Both steps are idempotent, so restarts and redeploys never duplicate work.
3. **Verify** — Railway probes `/health` (up to 300 s for cold starts); a release only goes live once it passes, with automatic restart on failure (up to 10 retries).
4. **Serve** — the API answers at `/api/v1`, OpenAPI at `/docs`, and the compiled React app at `/`, all on one Railway domain. Traffic is routed to the `PORT` Railway injects.

## About Hosting

Hosting FastAPI-Fullstack-Template on Railway runs a stateless `backend` container plus a Postgres database with persistent storage, connected over Railway's private network (`postgres.railway.internal`).

- **Scaling** — the API starts with 4 uvicorn workers; scale vertically (more vCPU/RAM) or horizontally (multiple instances) — safe because the app is stateless, sessions are JWTs, and Postgres is shared.
- **Deploys** — push to GitHub (or `railway up` from the CLI). Every release is health-gated, and failed health checks roll traffic back to the previous deployment.
- **Schema changes** — Alembic migrations run automatically on each deploy, so the database is always in sync with the image that just shipped.
- **No reverse proxy needed** — the frontend is served by FastAPI itself, so there's no second web service, no CDN config, and no CORS to wire up for same-origin use (`FRONTEND_HOST` is only needed if you call the API from external clients).

## Why Deploy

- **It's the upstream template's deployment story, minus the VPS.** Upstream ships with Docker Compose + Traefik for self-hosting — you manage TLS, DNS, updates, and backups yourself. On Railway, the same codebase gets managed Postgres, automatic HTTPS, and health-gated deploys.
- **The PaaS gotchas are already fixed.** Generic "deploy FastAPI" guides leave you to discover the hard parts yourself: binding to `0.0.0.0:$PORT`, Railway's `postgres://` URL scheme vs the `psycopg` driver, running migrations before traffic arrives, and healthcheck endpoints that don't 307-redirect. This fork fixes all four — found and verified on a real Railway deployment.
- **One container instead of two.** The frontend is compiled into the backend image, so there's one service to build, deploy, scale, and pay for.
- **Wiring is automatic.** `DATABASE_URL` is a reference to `${{Postgres.DATABASE_URL}}` — provision the database and the app finds it.

## Common Use Cases

- **SaaS MVP** — auth, user management, settings, and an admin panel ship in the box; you replace the items module with your domain logic and ship.
- **Internal tools and admin panels** — the admin UI, superuser roles, and CRUD patterns are already in place.
- **API backends for mobile/desktop apps** — JWT auth plus a typed OpenAPI schema your clients can generate SDKs from.
- **Hackathons and learning** — a real-world FastAPI codebase (routers, CRUD, models, tests, CI) rather than a toy example.
- **Local-to-cloud parity** — the same repo runs under Docker Compose locally, so dev/prod drift stays small.

## Dependencies for

Hosting FastAPI-Fullstack-Template needs only a Railway account — Railway builds from source, so no local Docker, Python, or Node toolchain is required. The `backend` service needs:

| Variable | Value |
|---|---|
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` — auto-referenced, no setup |
| `SECRET_KEY` | `openssl rand -hex 32` |
| `FIRST_SUPERUSER` | your admin email, e.g. `admin@example.com` |
| `FIRST_SUPERUSER_PASSWORD` | a strong password (default `changethis` refuses to boot) |
| `PROJECT_NAME` | shown in OpenAPI and emails |
| `FRONTEND_HOST` | your Railway domain, set after the first deploy |

Optional: `SMTP_HOST`, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_PORT`, `EMAILS_FROM_EMAIL` enable real password-recovery emails; without them the feature is disabled and everything else works.

### Deployment Dependencies

- Source: [github.com/lNamelessl/fastapi-fullstack-railway](https://github.com/lNamelessl/fastapi-fullstack-railway) (upstream: [tiangolo/full-stack-fastapi-template](https://github.com/tiangolo/full-stack-fastapi-template))
- [FastAPI Documentation](https://fastapi.tiangolo.com/) · [SQLModel](https://sqlmodel.tiangolo.com/) · [Alembic](https://alembic.sqlalchemy.org/) · [React](https://react.dev/) · [Railway Docs](https://docs.railway.app/)

---

Happy coding! 🚀
