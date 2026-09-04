# API Contract: Tasks

Base path: `/tasks`. All request/response bodies are JSON.

## Task representation

```json
{
  "id": 1,
  "title": "Buy groceries",
  "completed": false,
  "created_at": "2026-09-04T12:00:00.000Z"
}
```

## `GET /tasks`

List all tasks, newest first.

- **Response**: `200 OK`
```json
[
  { "id": 2, "title": "Second task", "completed": false, "created_at": "..." },
  { "id": 1, "title": "First task", "completed": true, "created_at": "..." }
]
```
- Empty list → `200 OK` with `[]` (Edge Cases: empty-list state).

## `POST /tasks`

Create a new task.

- **Request**:
```json
{ "title": "Buy groceries" }
```
- **Response**: `201 Created`, body is the created task (FR-005: appears first in subsequent `GET /tasks`; FR-007: `completed: false`).
- **Errors**: `422 Unprocessable Entity` if `title` is missing, empty, or whitespace-only (FR-003), with a clear error message (FR-012).

## `PATCH /tasks/{id}/complete`

Mark a task as completed.

- **Response**: `200 OK`, body is the updated task with `completed: true` (FR-008).
- **Errors**: `404 Not Found` if `id` does not exist, with a clear error message (FR-012); existing task list is unchanged on error.

## `DELETE /tasks/{id}`

Delete a task.

- **Response**: `204 No Content` on success (FR-009). Other tasks' title, status, and order are unaffected (FR-010).
- **Errors**: `404 Not Found` if `id` does not exist, with a clear error message (FR-012); existing task list is unchanged on error.
