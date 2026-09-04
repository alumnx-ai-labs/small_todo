# Quickstart: Task Management

Validates the feature end-to-end against [spec.md](./spec.md)'s user stories and the [tasks API contract](./contracts/tasks-api.md).

## Prerequisites

- Python 3.11+ with the backend's dependencies installed (`pip install -r backend/requirements.txt`)
- Node.js with frontend dependencies installed (`npm install` in `frontend/`)

## Setup

```bash
# Backend: initializes SQLite DB (see data-model.md) and starts the API
cd backend
uvicorn src.api.main:app --reload --port 8000

# Frontend: starts the dev server, pointed at the backend API
cd frontend
npm run dev
```

## Validation Scenarios

1. **Add and view a task** (User Story 1)
   - Open the app in a browser. List is empty (Edge Cases: empty-list state).
   - Enter "Buy groceries" and submit.
   - Expect: task appears at the top of the list, marked incomplete (FR-005, FR-007, SC-001).
   - Add a second task "Walk the dog".
   - Expect: "Walk the dog" appears above "Buy groceries" (FR-005 scenario 2).

2. **Validation on empty title** (Edge Cases)
   - Submit an empty or whitespace-only title.
   - Expect: no task is created, a clear validation message is shown (FR-003, FR-012).

3. **Complete a task** (User Story 2)
   - Mark "Buy groceries" as completed.
   - Expect: its status visibly changes to completed (FR-008).
   - Reload the page.
   - Expect: it remains completed (FR-011 — persisted via backend/SQLite).

4. **Delete a task** (User Story 3)
   - Delete "Walk the dog".
   - Expect: it no longer appears; "Buy groceries" remains with its title, completed status, and position unchanged (FR-009, FR-010).

5. **Persistence across restarts** (FR-011, SC-004)
   - Stop and restart the backend process.
   - Reload the frontend.
   - Expect: all previously saved tasks retain their titles, order, and completion statuses.

## Automated Test Mapping

- Backend contract tests (`backend/tests/contract/`): one per endpoint in [tasks-api.md](./contracts/tasks-api.md), covering success and error responses.
- Backend integration tests (`backend/tests/integration/`): full add → complete → delete lifecycle against a test SQLite DB.
- Frontend component tests (`frontend/tests/unit/`): `AddTaskForm` validation, `TaskItem` completed-state rendering.
- Frontend integration tests (`frontend/tests/integration/`): scenarios 1–4 above driven through the UI.
