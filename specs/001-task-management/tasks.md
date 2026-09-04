---

description: "Task list for Task Management feature"
---

# Tasks: Task Management

**Input**: Design documents from `/specs/001-task-management/`

**Prerequisites**: plan.md, spec.md, research.md, data-model.md, contracts/tasks-api.md, quickstart.md

**Tests**: Included — quickstart.md and the constitution's Testable Delivery principle call for pytest (backend) and Vitest+RTL (frontend) coverage.

**Organization**: Tasks are grouped by user story (US1 = Add & View, US2 = Complete, US3 = Delete) to enable independent implementation and testing.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (US1, US2, US3)
- Paths follow the web-app split from plan.md: `backend/`, `frontend/`

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create `backend/` and `frontend/` directory structure per plan.md (backend/src/{models,services,api}, backend/tests/{contract,integration,unit}, frontend/src/{components,pages,services}, frontend/tests/{unit,integration})
- [ ] T002 Initialize Python backend project in `backend/` with FastAPI and uvicorn dependencies in `backend/requirements.txt`
- [ ] T003 Initialize React frontend project in `frontend/` via Vite (React + TypeScript template), with `frontend/package.json` dependencies
- [ ] T004 [P] Configure backend linting/formatting (e.g., ruff/black) in `backend/pyproject.toml`
- [ ] T005 [P] Configure frontend linting/formatting (e.g., ESLint/Prettier) in `frontend/.eslintrc` and `frontend/.prettierrc`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T006 Implement SQLite connection and schema initialization (`tasks` table per data-model.md) in `backend/src/db.py`
- [ ] T007 Create `Task` model/row mapping in `backend/src/models/task.py` reflecting the data-model.md schema (id, title, completed, created_at)
- [ ] T008 Setup FastAPI app instance, CORS config (for local frontend origin), and router registration in `backend/src/api/main.py`
- [ ] T009 [P] Implement shared error-response helper (clear validation/not-found messages per FR-012) in `backend/src/api/errors.py`
- [ ] T010 [P] Implement frontend API client base (fetch wrapper, base URL config) in `frontend/src/services/apiClient.ts`

**Checkpoint**: Foundation ready — user story implementation can now begin in parallel if staffed

---

## Phase 3: User Story 1 - Add and View Tasks (Priority: P1) 🎯 MVP

**Goal**: Users can add a task and view the full list, newest task on top, with title and completion status shown.

**Independent Test**: Enter a task title, add it, and verify it appears at the top of the task list with an incomplete status.

### Tests for User Story 1

- [ ] T011 [P] [US1] Contract test `POST /tasks` (success + 422 on empty/whitespace title) in `backend/tests/contract/test_tasks_post.py`
- [ ] T012 [P] [US1] Contract test `GET /tasks` (list ordering, empty list) in `backend/tests/contract/test_tasks_get.py`
- [ ] T013 [P] [US1] Integration test: add task, appears above existing tasks, trimmed title, incomplete status in `backend/tests/integration/test_add_view_tasks.py`
- [ ] T014 [P] [US1] Frontend component test for `AddTaskForm` validation (rejects empty/whitespace) in `frontend/tests/unit/AddTaskForm.test.tsx`
- [ ] T015 [P] [US1] Frontend integration test: add task and see it at top of list in `frontend/tests/integration/addAndView.test.tsx`

### Implementation for User Story 1

- [ ] T016 [US1] Implement task creation service (trim title, reject empty/whitespace, set completed=false) in `backend/src/services/task_service.py`
- [ ] T017 [US1] Implement task listing service (return all tasks ordered newest-first) in `backend/src/services/task_service.py`
- [ ] T018 [US1] Implement `POST /tasks` and `GET /tasks` routes wired to the service in `backend/src/api/tasks.py`
- [ ] T019 [P] [US1] Implement `TaskItem` component (title, completion status display) in `frontend/src/components/TaskItem.tsx`
- [ ] T020 [P] [US1] Implement `TaskList` component (renders tasks in given order, empty-list state) in `frontend/src/components/TaskList.tsx`
- [ ] T021 [US1] Implement `AddTaskForm` component (title input, validation message on invalid submit) in `frontend/src/components/AddTaskForm.tsx`
- [ ] T022 [US1] Implement `TaskManagementPage` wiring AddTaskForm + TaskList to the API client (fetch on load, refresh on add) in `frontend/src/pages/TaskManagementPage.tsx`
- [ ] T023 [US1] Add `createTask`/`listTasks` methods to API client in `frontend/src/services/taskApi.ts`

**Checkpoint**: User Story 1 should be fully functional and testable independently — this is the MVP.

---

## Phase 4: User Story 2 - Complete Tasks (Priority: P2)

**Goal**: Users can mark a task as completed and see that status persist.

**Independent Test**: Select an incomplete task and verify its status changes to completed in the list.

### Tests for User Story 2

- [ ] T024 [P] [US2] Contract test `PATCH /tasks/{id}/complete` (success + 404 on missing id) in `backend/tests/contract/test_tasks_complete.py`
- [ ] T025 [P] [US2] Integration test: complete a task, status persists on subsequent list fetch in `backend/tests/integration/test_complete_task.py`
- [ ] T026 [P] [US2] Frontend integration test: mark task complete, status visibly updates and survives reload in `frontend/tests/integration/completeTask.test.tsx`

### Implementation for User Story 2

- [ ] T027 [US2] Implement complete-task service method (set completed=true, 404 if not found) in `backend/src/services/task_service.py`
- [ ] T028 [US2] Implement `PATCH /tasks/{id}/complete` route in `backend/src/api/tasks.py`
- [ ] T029 [US2] Add `completeTask` method to API client in `frontend/src/services/taskApi.ts`
- [ ] T030 [US2] Add complete action (e.g., checkbox/button) to `TaskItem` component, calling the API and updating local state in `frontend/src/components/TaskItem.tsx`

**Checkpoint**: User Stories 1 AND 2 should both work independently.

---

## Phase 5: User Story 3 - Delete Tasks (Priority: P3)

**Goal**: Users can delete a task, and all other tasks retain their title, status, and order.

**Independent Test**: Delete a task and verify it no longer appears while other tasks are unaffected.

### Tests for User Story 3

- [ ] T031 [P] [US3] Contract test `DELETE /tasks/{id}` (success + 404 on missing id) in `backend/tests/contract/test_tasks_delete.py`
- [ ] T032 [P] [US3] Integration test: delete one task among several, others retain title/status/order in `backend/tests/integration/test_delete_task.py`
- [ ] T033 [P] [US3] Frontend integration test: delete a task, list updates and other tasks unaffected in `frontend/tests/integration/deleteTask.test.tsx`

### Implementation for User Story 3

- [ ] T034 [US3] Implement delete-task service method (404 if not found, no side effects on other rows) in `backend/src/services/task_service.py`
- [ ] T035 [US3] Implement `DELETE /tasks/{id}` route in `backend/src/api/tasks.py`
- [ ] T036 [US3] Add `deleteTask` method to API client in `frontend/src/services/taskApi.ts`
- [ ] T037 [US3] Add delete action to `TaskItem` component, calling the API and removing it from local state in `frontend/src/components/TaskItem.tsx`

**Checkpoint**: All three user stories should now work independently and together.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements affecting multiple user stories

- [ ] T038 [P] Verify persistence across backend restart per quickstart.md scenario 5 (manual/integration check against `backend/src/db.py`)
- [ ] T039 [P] Add README/run instructions for backend and frontend matching quickstart.md setup steps
- [ ] T040 Run full quickstart.md validation scenarios end-to-end and fix any discrepancies

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — start immediately
- **Foundational (Phase 2)**: Depends on Setup completion — blocks all user stories
- **User Stories (Phase 3-5)**: All depend on Foundational phase completion
  - User Story 1 (P1): Can start after Foundational — no dependency on other stories
  - User Story 2 (P2): Can start after Foundational — independent of US1/US3 at the code level, though naturally built after US1 exists to complete a real task
  - User Story 3 (P3): Can start after Foundational — independent of US1/US2 at the code level
- **Polish (Phase 6)**: Depends on all desired user stories being complete

### User Story Dependencies

- US1 (Add & View): No dependency on other stories — foundational data path
- US2 (Complete): Independent implementation; needs at least one task to exist to be tested end-to-end, so tested after US1 in practice
- US3 (Delete): Independent implementation; same practical note as US2

### Within Each User Story

- Tests (if included) before implementation
- Service layer before route/component wiring
- Backend route before frontend API client method before UI wiring

### Parallel Opportunities

- All Setup tasks marked [P] (T004, T005) can run in parallel after T001-T003
- T009 and T010 (Phase 2) can run in parallel
- All contract/integration test tasks within a story marked [P] can run in parallel (e.g., T011-T015 together)
- T019 and T020 (TaskItem, TaskList) can run in parallel
- Once Foundational phase completes, US1, US2, and US3 implementation could be staffed in parallel by different developers, though US2/US3 are more meaningfully tested once US1 exists

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together (different files, no shared state):
Task: "Contract test POST /tasks in backend/tests/contract/test_tasks_post.py"
Task: "Contract test GET /tasks in backend/tests/contract/test_tasks_get.py"
Task: "Integration test add/view tasks in backend/tests/integration/test_add_view_tasks.py"
Task: "Frontend component test AddTaskForm in frontend/tests/unit/AddTaskForm.test.tsx"
Task: "Frontend integration test add-and-view in frontend/tests/integration/addAndView.test.tsx"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL — blocks everything)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Run quickstart.md scenario 1 and 2 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Setup + Foundational → backend and frontend skeletons run locally
2. Add User Story 1 → validate independently (MVP!)
3. Add User Story 2 → validate independently, deploy/demo
4. Add User Story 3 → validate independently, deploy/demo
5. Polish phase → final quickstart.md full pass
