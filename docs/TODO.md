# PingDay Development TODO

## Phase 0 - Workspace Analysis

- [x] Inspect API directory contents; it is empty.
- [x] Check API Git status; unavailable because this directory is not a Git repository.
- [x] Record that Go initialization and baseline checks are absent.
- [x] Create API project plan, TODO, and decision log.
- [ ] Locate/restore the intended API Git checkout and repeat repository inspection.
- [ ] Inspect the frontend checkout and align the shared workspace plan.

## Phase 1 - Backend Foundation (planned; not started)

- [ ] Confirm existing work and Go version before initializing files.
- [ ] Initialize Go module and minimal application layout.
- [ ] Add validated environment configuration and `.env.example`.
- [ ] Add structured logging and safe redaction practices.
- [ ] Add Fiber bootstrap, health/readiness endpoints, and graceful shutdown.
- [ ] Add PostgreSQL connection and readiness check.
- [ ] Add migration tooling and initial migration workflow.
- [ ] Add Dockerfile and local Compose PostgreSQL service.
- [ ] Add focused foundation tests.
- [ ] Document local development and run formatting, vet, and tests.

## Phase 2 - Event Domain (future)

- [ ] Define event domain and repository port.
- [ ] Implement event persistence and migrations.
- [ ] Implement event CRUD use cases and REST API.
- [ ] Add validation, error mapping, and unit/repository tests.
