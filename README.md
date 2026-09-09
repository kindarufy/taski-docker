**English** | [Русский](README.ru.md)

# Taski Docker

**Taski Docker** is an educational infrastructure project from Yandex Practicum: an existing Django + React application is configured to run with Docker, PostgreSQL, Nginx, and CI/CD.

> This repository is a fork of the original educational project, `yandex-praktikum/taski-docker`. My work covers containerization, infrastructure configuration, and the deployment pipeline, rather than the todo application's original business logic.

## My contributions

- Dockerfile for the Django backend;
- Dockerfile for the React frontend;
- a separate Nginx gateway container;
- running the backend with Gunicorn;
- PostgreSQL in place of the local SQLite configuration;
- Docker volumes for the database and static files;
- `docker-compose.yml` for local setup;
- `docker-compose.production.yml` for server deployment;
- GitHub Actions CI for the backend, frontend, and Docker builds;
- a separate manual workflow for publishing Docker images and SSH deployment;
- post-deployment migration and static file steps;
- Telegram notification after a successful production workflow.

## Tech stack

**Backend:** Python, Django, Django REST Framework, Gunicorn, PostgreSQL  
**Frontend:** JavaScript, React 18, Axios, Bootstrap  
**Infrastructure:** Docker, Docker Compose, Nginx, Docker Hub, GitHub Actions, SSH

## Architecture

```text
Client
  │
  ▼
Nginx gateway
  ├──► React static
  └──► Django REST API
           │
           ▼
       PostgreSQL
```

| Container | Purpose |
| --- | --- |
| `backend` | Django API / Gunicorn |
| `frontend` | React production build |
| `gateway` | Nginx entry point |
| `db` | PostgreSQL |

## Structure

```text
.
├── backend/
├── frontend/
├── gateway/
├── .github/workflows/
│   ├── main.yml              # CI: tests + Docker build
│   └── deploy.yml            # manual production deployment
├── docker-compose.yml
├── docker-compose.production.yml
├── setup.cfg
└── README.md
```

## Local setup

Docker Engine/Desktop and Compose v2 are required. Run all commands from the
project root. After startup, the interface is available at `http://localhost:8000/`.

Clone this fork:

```bash
git clone https://github.com/nikamurkaa/taski-docker.git
cd taski-docker
```

Copy `.env.example` to `.env` (`cp .env.example .env`,
or `Copy-Item .env.example .env` in PowerShell). Example PostgreSQL settings:

```env
POSTGRES_USER=django_user
POSTGRES_PASSWORD=django_password
POSTGRES_DB=django_db
DB_HOST=db
DB_PORT=5432
```

Start:

```bash
docker compose up -d --build
```

Migrations:

```bash
docker compose exec backend python manage.py migrate
```

Collect backend static files:

```bash
docker compose exec backend python manage.py collectstatic --noinput
docker compose exec backend sh -c 'mkdir -p /backend_static/static && cp -r /app/collected_static/. /backend_static/static/'
```

Stop: `docker compose down`. Data is retained in volumes.

## CI/CD

`.github/workflows/main.yml` runs on pushes and pull requests to `main`. It checks the backend, frontend, and local Docker image builds without production secrets.

`.github/workflows/deploy.yml` is triggered manually through `workflow_dispatch`. It publishes images to Docker Hub and deploys over SSH only for a configured production environment.

The Docker Hub namespace is not hardcoded in repository files: `docker-compose.production.yml` uses the `DOCKER_USERNAME` variable, and GitHub Actions uses a secret with the same name.

Docker Hub, SSH, and Telegram secrets are stored in GitHub Actions Secrets and must not be committed to repository files.

## Skills demonstrated

- Docker and Compose;
- multi-container application setup;
- PostgreSQL in a container environment;
- Nginx routing;
- production configuration;
- CI/CD and deployment automation.

## Status

Completed as part of the **Yandex Practicum Python Developer course**, this project demonstrates backend application infrastructure: containerization, CI, and controlled production deployment.

## Infrastructure implementation author

[Nicole Zhurbenko](https://github.com/nikamurkaa)
