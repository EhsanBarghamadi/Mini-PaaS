# Mini-PaaS

A small Heroku/Vercel-style deployment platform built with Django and Docker.

Give it a Git repository (or a Dockerfile) and it builds the image, runs the container, and serves it on its own subdomain.

> **Status:** Work in progress. This is a learning project: it is built in small tasks, and each one is analyzed and documented in [LEARNING.md](LEARNING.md).

## Planned features

- Build images from a Dockerfile and run them as isolated containers
- Automatic subdomain routing through a reverse proxy
- Live build and runtime logs
- Per-app environment variables and secrets
- Asynchronous build queue
- Rollback and zero-downtime deploys
- Multiple users with quotas

See [docs/tasks.md](docs/tasks.md) for the task list and current progress.

## Tech stack

- Python / Django
- Docker (via the Docker SDK for Python)
- PostgreSQL (production), SQLite (development)
- Celery + Redis (build queue)
- Traefik or Caddy (reverse proxy)
- Plain HTML / CSS / JS for the UI

## Security note

This project talks to the Docker socket, which is equivalent to root access on the host machine. Run it on a development machine or VM, and do not run untrusted code with it until the isolation tasks are done.

## License

No license has been chosen yet.
