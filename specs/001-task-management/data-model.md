# Phase 1 Data Model: Task Management

## Entity: Task

Represents a single user-created to-do item.

| Field | Type | Constraints | Notes |
|---|---|---|---|
| `id` | INTEGER | Primary key, auto-increment | Used to derive stable creation order |
| `title` | TEXT | NOT NULL, non-empty after trim | Leading/trailing whitespace stripped before storage (FR-004); empty/whitespace-only rejected (FR-003) |
| `completed` | BOOLEAN (stored as INTEGER 0/1) | NOT NULL, default `0` | New tasks start incomplete (FR-007) |
| `created_at` | TEXT (ISO 8601 timestamp) | NOT NULL, default current time | Used with `id` to guarantee newest-first ordering (FR-005) |

### Validation Rules

- `title` MUST be non-empty after trimming whitespace (FR-003). Reject with a validation error otherwise.
- `title` is stored trimmed of leading/trailing whitespace (FR-004).
- `completed` MUST default to `false`/`0` on creation (FR-007).

### State Transitions

- **Create**: task inserted with `completed = 0`.
- **Complete**: `completed` transitions `0 → 1`. No transition back to incomplete is required by the spec (out of scope).
- **Delete**: row removed; all other rows' `id`, `title`, `completed`, `created_at` are untouched (FR-010).

### Ordering

- Task list is returned sorted by `id DESC` (equivalently `created_at DESC`), so the most recently created task appears first (FR-005) and order is stable across completions/deletions (Edge Cases).

### Schema (SQLite DDL)

```sql
CREATE TABLE IF NOT EXISTS tasks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    completed INTEGER NOT NULL DEFAULT 0,
    created_at TEXT NOT NULL DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ', 'now'))
);
```
