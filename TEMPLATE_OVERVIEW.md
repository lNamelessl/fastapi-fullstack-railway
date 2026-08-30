# 🚀 Full Stack FastAPI Template

A production-ready, one-click-deployable fullstack starter, combining **FastAPI** (Python), **React** (TypeScript), and **PostgresDB**. This is the official [Full Stack FastAPI Template](https://github.com/tiangolo/full-stack-fastapi-template) by tiangolo, adapted for Railway — migrations, superuser seeding, dynamic port binding, and health checks all work out of the box.

- ⚡ **FastAPI** — high-performance Python API with typed OpenAPI docs
- 🚀 **React** — TypeScript + Vite + Tailwind frontend, built and served by FastAPI on the same domain
- 💾 **PostgresDB** — with Alembic migrations that run automatically on every deploy
- 🔐 JWT auth, secure password hashing, and email-based password recovery built in
- ☁️ **Railway-native** — binds to `0.0.0.0:$PORT`, parses Railway's `DATABASE_URL`, health checks `/health`

## ✨ Features

**Backend (FastAPI)**
- FastAPI with automatic OpenAPI documentation
- SQLModel + Pydantic v2 for ORM and validation
- JWT authentication with secure password hashing
- Email-based password recovery (optional SMTP)
- Pytest suite with CI

**Frontend (React)**
- React + TypeScript built with Vite
- Tailwind CSS + shadcn/ui components, dark mode included
- Type-safe API client generated from OpenAPI
- Playwright end-to-end tests

**Railway adaptations**
- Single multi-stage Dockerfile (Bun builds the frontend, uv manages Python)
- Runs Alembic migrations and seeds the initial superuser on every deploy (idempotent, safe on restarts)
- Automatic rewrite of Railway's `postgres://` URL to the `postgresql+psycopg://` driver

## 🚀 Deploy

Deploy on Railway:

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.app/new?github_url=https://github.com/lNamelessl/fastapi-fullstack-railway)

**Services deployed:** a `backend` service (FastAPI + built frontend in one container) and a **Postgres** database with a persistent volume.

**After deploying:** generate a public domain for the `backend` service and set it as `FRONTEND_HOST`, then sign in at `/` with `FIRST_SUPERUSER` / `FIRST_SUPERUSER_PASSWORD`. The app refuses to start with default secrets, so set real values for `SECRET_KEY` and `FIRST_SUPERUSER_PASSWORD`.

# Deploy and Host

FastAPI-Fullstack-Template is a modern, production-ready starter kit for building fullstack web applications. It combines a FastAPI backend, a React frontend served on the same domain, and PostgresDB for storage — deployed from a single containerized build. It is designed for rapid fullstack development, scalability, and easy deployment to cloud platforms like Railway.

## About Hosting

Hosting this template deploys a containerized fullstack application: one `backend` service running FastAPI (which serves both the REST API and the compiled React frontend) alongside a Postgres database with a persistent volume. Alembic migrations and superuser seeding run automatically on every deploy, so the database is always in sync with the latest image. Railway handles networking, HTTPS, restarts, and rollbacks — you go from zero to a live, authenticated application in minutes with minimal operational overhead.

## Why Deploy

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying FastAPI-Fullstack-Template on Railway, you get a complete, authenticated fullstack application with minimal burden: the database URL is wired automatically, the app respects Railway's dynamic `PORT`, migrations run on every deploy, and a `/health` endpoint lets Railway verify every release.

## Common Use Cases

- Rapid prototyping of SaaS products with built-in auth and user management
- Scalable REST APIs paired with a modern React dashboard
- Internal tools and admin panels
- Educational projects, hackathons, and learning fullstack FastAPI + React

## Dependencies for

- A Railway account (no local Docker or toolchain required — Railway builds from the repo)
- Environment variables on the `backend` service: `SECRET_KEY`, `FIRST_SUPERUSER`, `FIRST_SUPERUSER_PASSWORD`, `PROJECT_NAME`, `FRONTEND_HOST` (`DATABASE_URL` is auto-referenced from the Postgres service)

### Deployment Dependencies

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLModel Documentation](https://sqlmodel.tiangolo.com/)
- [Alembic Documentation](https://alembic.sqlalchemy.org/)
- [React Documentation](https://react.dev/)
- [Railway Documentation](https://docs.railway.app/)

---

Happy coding! 🚀
