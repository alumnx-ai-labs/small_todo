# Implementation Plan: Task Management

**Branch**: `001-task-management` | **Date**: 2026-09-04 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-task-management/spec.md`

## Summary

A single-user, login-free to-do application: add, view, complete, and delete tasks, with newest tasks always shown first. Persistence is handled by a Python backend exposing a small REST API over a local SQLite database; the React frontend calls this API and holds no direct storage of its own.

## Technical Context

**Language/Version**: Python 3.11 (backend), TypeScript/JavaScript with React 18 (frontend)

**Primary Dependencies**: FastAPI (backend API + validation), SQLite via Python's built-in `sqlite3` (or SQLAlchemy Core for query building), React (frontend), Vite (frontend build/dev tooling)

**Storage**: SQLite, single local file, accessed only through the backend

**Testing**: pytest (backend API + persistence tests), Vitest + React Testing Library (frontend component/interaction tests)

**Target Platform**: Local web app — backend served on localhost, frontend served/built for a browser, no hosted/cloud dependency required for core workflow

**Project Type**: web application (frontend + backend)

**Performance Goals**: Task list actions (add/view/complete/delete) reflect in the UI within 1s on a local network round trip, consistent with SC-001/SC-002 (visible result within 5-10s including user think time)

**Constraints**: Offline-capable once installed locally (no external network dependency); no authentication/account system (Constitution Principle IV); SQLite as the only persistence layer (Constitution Principle III)

**Scale/Scope**: Single local user, single task list; tens to low hundreds of tasks (per SC-002's 50-task benchmark), no multi-user or concurrent-access requirements

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle | Check | Status |
|---|---|---|
| I. Simplicity First | Single task list, no speculative features; scope matches FR-013's exclusions | PASS |
| II. Required Stack | Backend is Python (FastAPI), frontend is React, communication via a documented REST API contract | PASS |
| III. Local-First Persistence | SQLite storage, isolated behind backend persistence boundary, schema created via an initialization script | PASS |
| IV. Authentication Boundary | No login/account features; API has no user-identity concept | PASS |
| V. Testable Delivery | pytest for backend API/persistence, Vitest+RTL for frontend components, planned in Testing above | PASS |

No violations. Complexity Tracking table not needed.

## Project Structure

### Documentation (this feature)

```text
specs/001-task-management/
├── plan.md              # This file (/speckit-plan command output)
├── research.md          # Phase 0 output (/speckit-plan command)
├── data-model.md        # Phase 1 output (/speckit-plan command)
├── quickstart.md        # Phase 1 output (/speckit-plan command)
├── contracts/           # Phase 1 output (/speckit-plan command)
└── tasks.md             # Phase 2 output (/speckit-tasks command - NOT created by /speckit-plan)
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── models/       # Task model / row mapping
│   ├── services/     # Task business logic (create, list, complete, delete)
│   ├── api/          # FastAPI routes for /tasks
│   └── db.py          # SQLite connection + schema initialization
└── tests/
    ├── contract/      # API contract tests
    ├── integration/   # End-to-end task lifecycle tests against a test DB
    └── unit/          # Service-level unit tests

frontend/
├── src/
│   ├── components/    # TaskList, TaskItem, AddTaskForm
│   ├── pages/         # Single main page (TaskManagementPage)
│   └── services/      # API client for /tasks endpoints
└── tests/
    ├── unit/          # Component tests
    └── integration/   # User-flow tests (add/view/complete/delete)
```

**Structure Decision**: Web application split into `backend/` (Python/FastAPI + SQLite) and `frontend/` (React), matching Constitution Principle II's required stack and Option 2 of the standard project layout. The frontend never touches storage directly — all persistence goes through the backend's `/tasks` API.

## Complexity Tracking

*No constitution violations — table not needed.*
