# Tasks

Each task = one branch (`task/NN-short-name`) = one pull request.
A task is done when its acceptance criteria are met, tests are green, and the three questions in `LEARNING.md` are answered.

## Progress

- [x] **Task 0: Repository setup**
- [ ] **Task 1: Project skeleton and container service layer**
- [ ] **Task 2: Models and build from Dockerfile**
- [ ] **Task 3: Automatic subdomain routing**
- [ ] **Task 4: Live logs**
- [ ] **Task 5: Environment variables and secrets**
- [ ] **Task 6: Build queue**
- [ ] **Task 7: Rollback and zero-downtime deploy**
- [ ] **Task 8: Multi-user support and quotas**

---

## Task 1: Project skeleton and container service layer

Branch: `task/01-container-service`

Acceptance criteria:
- Django project with split settings (`base`, `dev`, `prod`) selected by `DJANGO_ENV`
- `.env.example` created together with the settings that read it
- App `deployments` with a service module providing: run, stop, remove, and get status for a container
- Every container carries the label `paas.managed=true`
- No views and no models yet
- Tests run a real container and always clean up

Things to analyze:
- Why access to the Docker socket is equivalent to root on the host
- Why labeling containers matters (orphan cleanup later)
- `detach=True` versus a blocking run

## Task 2: Models and build from Dockerfile

Branch: `task/02-models-and-build`

Acceptance criteria:
- Models for an app and its deployments, with a status field and valid status transitions
- Build an image from a Dockerfile through the SDK and store the result and build output
- Run the built image and record the container id and status

Things to analyze:
- Image layers and build cache
- Where build logs come from and how to capture them
- How to model deployment states so invalid transitions are impossible

## Task 3: Automatic subdomain routing

Branch: `task/03-routing`

Acceptance criteria:
- A reverse proxy (Traefik or Caddy) runs in Docker
- A new app becomes reachable at `<app>.localhost` without manual proxy configuration
- Removing an app removes its route

Things to analyze:
- Static versus dynamic proxy configuration (labels or API)
- Docker networks and container name resolution
- Why app containers must not publish host ports

## Task 4: Live logs

Branch: `task/04-live-logs`

Acceptance criteria:
- Build logs and runtime logs are streamed to the browser in real time
- Plain HTML/JS page, no framework

Things to analyze:
- SSE versus WebSocket and when each fits
- Streaming responses in Django (sync versus async)
- What happens when the client disconnects mid-stream

## Task 5: Environment variables and secrets

Branch: `task/05-env-and-secrets`

Acceptance criteria:
- Per-app environment variables injected into the container at run time
- Secret values are never shown in the UI or written to logs
- Changing variables triggers a redeploy

Things to analyze:
- Where secrets are stored and whether they should be encrypted at rest
- Build-time versus run-time variables
- Ways secrets leak (logs, image layers, error pages)

## Task 6: Build queue

Branch: `task/06-build-queue`

Acceptance criteria:
- Builds run asynchronously through Celery with Redis
- Source can come from a Git repository (clone, then build)
- Build status is visible while the job runs, and failed builds are reported clearly
- Limit on concurrent builds

Things to analyze:
- Why builds must not run inside the request cycle
- Retry behavior and idempotency of a build job
- Cleaning up after a failed or cancelled build

## Task 7: Rollback and zero-downtime deploy

Branch: `task/07-rollback`

Acceptance criteria:
- A new version starts and passes a health check before traffic switches to it
- The previous version stays available for rollback
- Rollback to the previous version works from the UI

Things to analyze:
- Blue-green versus rolling deploys
- Health checks and how long to wait
- What to do when the new version never becomes healthy

## Task 8: Multi-user support and quotas

Branch: `task/08-users-and-quotas`

Acceptance criteria:
- Users only see and manage their own apps
- Per-user limits on number of apps and on CPU/memory per container
- Resource limits are enforced by Docker, not just recorded in the database

Things to analyze:
- Docker resource limits (memory, CPU) and what happens when a limit is hit
- Isolation between users' containers (networks)
- Remaining security gaps and what a real platform would add
