# Full Stack FastAPI Template

[![Test Docker Compose](../../actions/workflows/test-docker-compose.yml/badge.svg)](../../actions/workflows/test-docker-compose.yml)
[![Test Backend](../../actions/workflows/test-backend.yml/badge.svg)](../../actions/workflows/test-backend.yml)

## Technology Stack and Features

- ⚡ [**FastAPI**](https://fastapi.tiangolo.com) for the Python backend API.
  - 🧰 [SQLModel](https://sqlmodel.tiangolo.com) for the Python SQL database interactions (ORM).
  - 🔍 [Pydantic](https://docs.pydantic.dev), used by FastAPI, for the data validation and settings management.
  - 💾 [PostgreSQL](https://www.postgresql.org) as the SQL database.
- 🚀 [React](https://react.dev) for the frontend.
  - 🧩 Built into the backend application and served by FastAPI on the same domain as the API.
  - 💃 Using TypeScript, hooks, [Vite](https://vitejs.dev), and other parts of a modern frontend stack.
  - 🎨 [Tailwind CSS](https://tailwindcss.com) and [shadcn/ui](https://ui.shadcn.com) for the frontend components.
  - 🤖 An automatically generated frontend client.
  - 🧪 [Playwright](https://playwright.dev) for end-to-end testing.
  - 🦇 Dark mode support.
- ☁️ [FastAPI Cloud](https://fastapicloud.com) for deployment.
- 🐋 [Docker Compose](https://www.docker.com) for local services and self-hosted deployment.
  - 📞 [Traefik](https://traefik.io) as a reverse proxy with automatic HTTPS.
- 🔒 Secure password hashing by default.
- 🔑 JWT (JSON Web Token) authentication.
- 📫 Email-based password recovery.
- ✉️ [React Email](https://react.email) for email templates.
- 📬 [Mailpit](https://mailpit.axllent.org) for local email testing during development.
- ✅ Tests with [Pytest](https://pytest.org).
- 🏭 CI (continuous integration) and CD (continuous deployment) based on GitHub Actions.

### Dashboard Login

![Dashboard login screenshot](img/login.png)

### Dashboard - Admin

![Admin dashboard screenshot](img/dashboard.png)

### Dashboard - Items

![Items dashboard screenshot](img/dashboard-items.png)

### Dashboard - Dark Mode

![Dark mode dashboard screenshot](img/dashboard-dark.png)

### React Email Templates

![Email templates screenshot](img/react-email.png)

### Mailpit - Local Email Testing

![Mailpit screenshot](img/mailpit.png)

### Interactive API Documentation

![API docs](img/docs.png)

## How to Use It

Click the **Use this template** button at the top of this page to create a new repository.

## Backend Development

Backend docs: [backend/README.md](./backend/README.md).

## Frontend Development

Frontend docs: [frontend/README.md](./frontend/README.md).

## Deployment

FastAPI Cloud deployment: [deployment.md](./deployment.md).

Self-hosted deployment with Docker Compose: [deployment-docker-compose.md](./deployment-docker-compose.md).

## Development

General development docs: [development.md](./development.md).

This includes the local FastAPI and Vite workflow, Docker Compose services, `.env` configuration, and more.

## Release Notes

Check the file [release-notes.md](./release-notes.md).

## License

The Full Stack FastAPI Template is licensed under the terms of the MIT license.

## Deploying to Railway

This fork is adapted for [Railway](https://railway.app):

- The backend binds to `0.0.0.0:$PORT` (Railway injects `PORT`).
- The container runs Alembic migrations + initial superuser seeding on every
  start (`scripts/prestart.sh` before `fastapi run` in the Dockerfile `CMD`,
  mirrored in `railway.json` `startCommand`). Both steps are idempotent.
- `railway.json` at the repo root builds the multi-stage `Dockerfile` (which
  compiles the frontend and serves it from FastAPI) and health checks
  `/api/v1/utils/health-check/`.
- The backend already rewrites Railway's `postgres://` `DATABASE_URL` to the
  `postgresql+psycopg://` driver scheme (`backend/app/core/config.py`).

### One-click deploy

Deploy this repo as a Railway template:

```
https://railway.app/new?github_url=https://github.com/lNamelessl/fastapi-fullstack-railway
```

Or from the CLI: `railway init` in this directory, then `railway up`.

### Required environment variables

Add a **PostgreSQL** database service and reference its `DATABASE_URL` in the
backend service. Then set:

| Variable | Example |
|---|---|
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` (reference from the Postgres service) |
| `SECRET_KEY` | `openssl rand -hex 32` |
| `FIRST_SUPERUSER` | `admin@example.com` |
| `FIRST_SUPERUSER_PASSWORD` | a strong password |
| `PROJECT_NAME` | `My Project` |
| `FRONTEND_HOST` | your Railway public domain, e.g. `https://my-app.up.railway.app` |

Note: the app refuses to start with default secret values (`changethis`) in
production, so `SECRET_KEY` and `FIRST_SUPERUSER_PASSWORD` must be changed.
