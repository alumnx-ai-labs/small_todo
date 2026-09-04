# Phase 0 Research: Task Management

All Technical Context items were resolved directly from the constitution and spec clarification; no items required open research. Recorded below for traceability.

## Decision: Backend framework — FastAPI

- **Rationale**: Lightweight, minimal boilerplate for a small REST API, built-in request validation (Pydantic) fits FR-003/FR-004 title validation, async-ready if ever needed, widely used with Python 3.11.
- **Alternatives considered**: Flask (more manual validation/setup for the same result), Django (far more machinery than a single-resource API needs — conflicts with Simplicity First).

## Decision: Persistence — SQLite via Python's `sqlite3` standard library

- **Rationale**: Constitution mandates SQLite; stdlib `sqlite3` avoids adding an ORM dependency for a single-table schema, keeping the stack minimal per Simplicity First.
- **Alternatives considered**: SQLAlchemy ORM (adds a dependency and abstraction layer not justified by a single `tasks` table).

## Decision: Frontend — React with Vite

- **Rationale**: Constitution mandates React; Vite gives fast local dev/build with minimal config for a small single-page app.
- **Alternatives considered**: Create React App (unmaintained), Next.js (adds server-rendering/routing machinery not needed for one page).

## Decision: Task ordering — `created_at` timestamp (or auto-increment id) descending

- **Rationale**: FR-005 requires newest-first ordering; a monotonically increasing SQLite `INTEGER PRIMARY KEY` id (or timestamp) sorted descending satisfies this deterministically and keeps deletes/completions from disturbing order (FR-010, Edge Cases).
- **Alternatives considered**: A manual `position` column (adds complexity for reordering logic not required by the spec).

## Decision: Frontend/backend contract style — REST JSON over HTTP

- **Rationale**: Constitution requires "an explicit, documented API contract"; REST/JSON is the simplest documented contract style for CRUD-like operations (add, list, complete, delete) and pairs naturally with FastAPI's OpenAPI generation.
- **Alternatives considered**: GraphQL (unnecessary flexibility/complexity for four fixed operations on one resource).

**Output**: All NEEDS CLARIFICATION items resolved. Proceeding to Phase 1.
