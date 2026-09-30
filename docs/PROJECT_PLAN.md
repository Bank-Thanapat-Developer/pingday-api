# PingDay API Project Plan

## Overview

PingDay is a personal reminder service that tracks one-time and yearly events and sends scheduled notifications through LINE. This repository will contain the Go HTTP API and backend services; the web client lives in the separate `pingday-web` repository.

## Current Phase and Constraints

Phase 0 inspection found this directory empty: no source files, Go module, README, tests, or `.git` metadata are present. Repository history and Git status therefore cannot be inspected, and there is no existing test/build baseline. Confirm the intended repository checkout before implementation.

## Architecture

Planned dependency direction: HTTP delivery and infrastructure adapters depend on application use cases; use cases depend on domain types and ports; domain code stays independent of Fiber, PostgreSQL, LINE, and scheduler libraries. PostgreSQL repositories, a LINE sender, and a cron adapter will be injected at the application boundary. Business logic should receive time explicitly or use a clock abstraction.

Proposed structure (adapt after confirming the initialized project and chosen library versions):

```text
cmd/api/main.go
internal/domain/
internal/usecase/
internal/repository/postgres/
internal/delivery/http/
internal/infrastructure/config/
internal/infrastructure/database/
internal/infrastructure/line/
internal/infrastructure/scheduler/
migrations/
tests/
docs/
Dockerfile
docker-compose.yml
Makefile
.env.example
.gitignore
go.mod
README.md
```

## Technology Decisions

- Go with Fiber for HTTP delivery.
- PostgreSQL with versioned up/down migrations for persistence.
- Structured logging and environment-based configuration; secrets stay out of source and logs.
- A LINE adapter implements an application-owned notification sender port.
- Scheduler callbacks trigger application logic only; timezone is explicit and defaults to `Asia/Bangkok` at 08:00.
- Defer dependency versions and migration/scheduler library selection until Phase 1 initialization, when current stable compatibility can be assessed.

## Development Phases and Milestones

0. Workspace analysis and planning (current): inspect both repositories and record their state; no application feature work.
1. Foundation: initialize/confirm Go module, configuration, structured logging, graceful shutdown, health/readiness endpoints, PostgreSQL, migrations, local Docker support, and first unit tests.
2. Event domain: event types, repository port and PostgreSQL implementation, CRUD use cases/API, validation, and tests.
3. Reminder rules: event reminder persistence, recurrence/date calculations, and deterministic unit tests.
4. Scheduler: daily processing, notification log and deduplication, retry approach, timezone handling, and tests.
5. LINE integration: sender adapter, formatter, test-send API if still needed, and adapter/use-case tests.
6. Coordinate API contract with dashboard work in `pingday-web`.
7. Quality: lint, tests, API specification, security review, Docker and docs improvements.
8. CI/CD preparation: Jenkins/GitHub workflow only when requested; no deployment in this phase.
9. Cloud deployment remains explicitly postponed.

## Phase 1 Implementation Checklist

- [ ] Confirm or restore the intended Git checkout; inspect status and existing files before initialization.
- [ ] Record Go version and select compatible Fiber, PostgreSQL driver, and migration tool versions.
- [ ] Initialize `go.mod` and the minimal package layout without introducing event features.
- [ ] Add environment configuration with validation, safe defaults, and `.env.example` (no real secrets).
- [ ] Add structured logger with redaction-safe fields.
- [ ] Add HTTP server bootstrap, `/health`, `/ready`, request context propagation, and graceful shutdown.
- [ ] Add PostgreSQL connection/ping wiring and readiness behavior.
- [ ] Add versioned migration tooling and an initial migration strategy; do not invent event schema before domain phase.
- [ ] Add multi-stage, non-root Docker image and local Compose PostgreSQL service with health check.
- [ ] Add focused tests for configuration and health/readiness behavior, using fakes where practical.
- [ ] Document local run, environment, migration, Docker, and development commands in README.
- [ ] Run formatting, `go vet`, and `go test ./...`; report actual results.

## Database Design (planned)

Future core tables: `events`, `event_reminders`, and `notification_logs`. Keep event/source/recurrence/status values as application-level constrained strings unless a reviewed need justifies database enums. Prevent duplicate deliveries with a database unique constraint on event, reminder offset, and scheduled date. Define cascade and retention behavior when implementing the event domain.

## API Plan (planned)

Use `/api/v1` for event CRUD, upcoming events, and notification history as MVP needs are confirmed. Provide `/health` for process health and `/ready` for dependency readiness. Standardize JSON errors and map validation/not-found/conflict/internal failures without exposing SQL or secret details. Add OpenAPI when the contract stabilizes.

## Testing Strategy

Keep domain/use-case tests fast and independent of PostgreSQL, LINE, and cron. Use deterministic dates and fakes. Add targeted database integration tests for migrations, CRUD, uniqueness, and transaction behavior when a test database is available. Never claim checks passed unless run.

## CI/CD Plan

Prepare reproducible formatting, vet, unit-test, and Docker build commands. A future Jenkins pipeline may checkout, lint, test, build, and create an image. Cloud deployment is out of scope.

## Deployment Plan

No cloud deployment in current scope. First support local Compose development; decide hosting and operational details only in the later deployment phase.

## Future Improvements

OpenAPI-generated frontend types, custom reminder intervals, notification retry/backoff and retention policy, additional notification channels, and operational metrics.
