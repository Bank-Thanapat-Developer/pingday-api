# PingDay API Decision Log

## ADR-001: Keep domain and use cases independent of delivery and infrastructure

### Decision

HTTP, database, scheduler, and notification integrations will sit at the application boundary. Use cases depend on ports where integration is required.

### Reason

This keeps reminder behavior testable without Fiber, PostgreSQL, cron, or a live LINE account.

### Alternatives considered

Place business logic directly in HTTP handlers or infrastructure adapters.

## ADR-002: Use PostgreSQL with versioned migrations

### Decision

PostgreSQL is the planned primary store, and schema changes will be version controlled as migrations.

### Reason

Relational constraints and transactions support event and notification history, including database-level duplicate protection.

### Alternatives considered

SQLite, MySQL, and document storage.

## ADR-003: Make reminder time zone explicit

### Decision

The scheduler will use an explicitly configured IANA timezone, with `Asia/Bangkok` as the product default and 08:00 as the default notification time.

### Reason

Server-local time can vary across development and deployment environments.

### Alternatives considered

Rely on the host machine's local timezone.
