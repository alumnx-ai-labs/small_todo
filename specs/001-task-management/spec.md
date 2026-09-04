# Feature Specification: Team Task Management

**Feature Branch**: `001-task-management`

**Created**: 2026-09-04

**Status**: Draft

**Input**: User description: "The To-Do application is a simple task management system for team
members and an admin. Team members log in with a case-sensitive name, see only their assigned
tasks, manage task status across todo, in-progress, and done, and create tasks for themselves.
The admin logs in with the name admin, sees all tasks, and creates assigned tasks from meeting
transcripts through structured AI-generated JSON."

## Clarifications

### Session 2026-09-04

- Q: Should a team member be allowed to log in with any alphabetic name, even before any task is
	assigned to that name? → A: Allow login only for names already assigned to at least one task.
	The exact name `admin` remains the administrator exception.
- Q: When a team member marks a task as blocked, should the task keep its current column status
  and show a separate blocked indicator? → A: Keep the task in its current column and show a
  separate blocked indicator.
- Q: Should the description field be optional when a team member creates a personal task? → A:
	Description is optional; the title is required.
- Q: Should the admin review and confirm the AI-generated tasks before they are saved? → A: Save
  generated tasks immediately after successful processing, without an admin confirmation step.
- Q: When the admin uploads a valid transcript while transcript text is already present, should
	the uploaded text replace or append to the existing text? → A: Append the uploaded text to the
	existing transcript text.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Team Member Login and Task Board (Priority: P1)

As a team member, I want to log in with my name and see only tasks assigned to me so that I can
focus on my own work.

**Why this priority**: Identity-based task visibility is the foundation of the team workflow.

**Independent Test**: Log in with two different valid names and verify that each person sees only
their own assigned tasks, displayed in the three required status columns.

**Acceptance Scenarios**:

1. **Given** a person enters a case-sensitive alphabetic name assigned to at least one task,
	**When** they log in, **Then** the application accepts the name exactly as entered, including
	its case.
2. **Given** tasks assigned to multiple people exist, **When** a team member logs in,
	 **Then** only tasks assigned to that exact case-sensitive name are shown.
3. **Given** a person enters an alphabetic name with no assigned tasks, **When** they attempt to
	log in, **Then** the application rejects the login and explains that an assignment is required.
4. **Given** the login name is exactly `admin`, **When** the user logs in, **Then** the application
	opens the admin board regardless of whether any task exists.

---

### User Story 2 - Manage Task Status and Details (Priority: P1)

As a team member, I want to inspect a task and change its status so that the board reflects my
current progress.

**Why this priority**: Status changes provide the basic team coordination workflow.

**Independent Test**: Open an assigned task, verify its details, and move it through the allowed
status changes while confirming the task remains assigned to the same person.

**Acceptance Scenarios**:

1. **Given** a visible task exists, **When** the team member selects it, **Then** another screen
	 shows its description, assignee, current status, and controls for marking it completed or
	 blocked.
2. **Given** a task is in todo, **When** the team member changes its status, **Then** it can move
	 to in-progress and the task appears in that column.
3. **Given** a task is in in-progress, **When** the team member changes its status, **Then** it
	 can move to todo or done and the task appears in the selected column.
4. **Given** a task is in done, **When** the team member changes its status, **Then** it can move
	 back to in-progress.
5. **Given** a team member marks a task as blocked, **When** the task is viewed on the board or
	 detail screen, **Then** its blocked state is visible without creating a fourth board column.

---

### User Story 3 - Create Personal Tasks (Priority: P2)

As a team member, I want to create a task for myself so that I can track work I identify.

**Why this priority**: Team members need to record their own work without admin intervention.

**Independent Test**: Submit a valid task through the add-task form and verify that it is assigned
to the logged-in person and appears first in their todo column.

**Acceptance Scenarios**:

1. **Given** a team member is logged in, **When** they submit a valid task title with or without a
   description, **Then** the task is assigned to that member and starts in todo.
2. **Given** existing tasks are visible, **When** a team member creates a task,
	 **Then** the new task appears before older tasks in the todo column.
3. **Given** the title is empty or contains only whitespace, **When** the form is submitted,
	 **Then** the task is not created and a validation message is shown.

---

### User Story 4 - Admin Review and Transcript Assignment (Priority: P1)

As an admin, I want to review every task and create assigned tasks from a meeting transcript so
that work mentioned in the meeting reaches the correct team members.

**Why this priority**: Admin assignment is the source of shared team work.

**Independent Test**: Log in as admin, verify all tasks and assignees are visible, submit a
transcript naming team members and work, and verify that the returned tasks are assigned with the
required fields.

**Acceptance Scenarios**:

1. **Given** the login name is exactly `admin`, **When** the user logs in,
	 **Then** the admin board shows all tasks in todo, in-progress, and done columns, with each
	 task's assignee name on its card.
2. **Given** the admin board shows a task, **When** the admin selects it,
	 **Then** another screen shows the task description, assignee, current status, and blocked state.
3. **Given** the admin enters a meeting transcript naming team members and work,
	 **When** the admin selects create tasks, **Then** the application produces structured JSON task
	 data using the same names as the transcript and immediately saves the resulting assignments.
4. **Given** the admin has a transcript in `.txt`, `.docx`, or `.pdf` format,
	 **When** the admin uploads the file, **Then** the application extracts its text and appends it
	 to any existing transcript text before making the combined text available for task creation.
5. **Given** the transcript names a person not previously used in the application,
	 **When** tasks are created for that person, **Then** that exact name becomes a valid login name
	 without a separate registration step.

### Edge Cases

- A login name containing anything other than alphabetic characters MUST be rejected.
- A team-member login name with no assigned task MUST be rejected; the exact name `admin` is
	accepted as the administrator exception.
- Login names MUST be case-sensitive; names differing only by case MUST be treated as different
	identities.
- The name `admin` MUST enter the admin view, while `Admin` MUST be treated as a different
	case-sensitive name.
- Empty or whitespace-only task titles MUST be rejected, and leading or trailing whitespace MUST
	be removed from accepted titles.
- A task description MAY be empty for a self-created task, but the title MUST be present.
- A transcript that produces invalid, incomplete, or unassigned task data MUST not create partial
	tasks and MUST show an actionable error.
- Transcript uploads MUST accept `.txt`, `.docx`, and `.pdf` files and MUST reject other file formats with
	a clear validation message.
- An empty or unreadable transcript upload MUST not create tasks and MUST show an actionable error.
- A task action that fails MUST leave the existing task, status, assignee, and order unchanged.
- No task may have a deadline or time-based field in this feature.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The application MUST provide one login page for team members and the admin.
- **FR-002**: The login form MUST accept a name only; no password or registration flow is allowed.
- **FR-003**: A login name MUST contain alphabetic characters only and MUST preserve case exactly.
- **FR-004**: A team-member login MUST be accepted only when at least one task is assigned to
  that exact case-sensitive name.
- **FR-005**: A successful team-member login MUST show only tasks assigned to that exact name.
- **FR-006**: The exact name `admin` MUST open the admin view and no other name may open it,
  regardless of task assignments.
- **FR-007**: The team-member board MUST display tasks in exactly three columns: todo,
	in-progress, and done.
- **FR-008**: The admin board MUST display all tasks in the same three columns and MUST show each
	task's assignee name on its card.
- **FR-009**: Every task card MUST expose its title, status, and blocked state when blocked.
- **FR-010**: Selecting a task MUST open a separate detail screen showing its description,
	assignee, current status, and controls to mark it completed or blocked.
- **FR-011**: Team members MUST be able to create tasks for themselves through an add-task form.
- **FR-012**: A self-created task MUST be assigned to the logged-in team member and start in todo.
- **FR-013**: The application MUST reject empty or whitespace-only task titles and MUST trim
	accepted titles before storing them.
- **FR-014**: Newly created tasks MUST appear before older tasks in their board column.
- **FR-015**: Team members MUST be able to move tasks between todo and in-progress, and between
	in-progress and done, in both directions.
- **FR-016**: The blocked control MUST set or clear a blocked state without adding a fourth column
	or changing the task's current status.
- **FR-017**: The application MUST provide clear feedback for invalid input and failed task actions.
- **FR-018**: The application MUST preserve task titles, descriptions, assignees, statuses,
  blocked states, and newest-first order between visits.
- **FR-019**: The admin MUST be able to paste a meeting transcript and select create tasks.
- **FR-020**: The admin MUST be able to upload a meeting transcript in `.txt`, `.docx`, or `.pdf`
	format.
- **FR-021**: The application MUST extract text from a valid transcript upload and make it
	available for task creation, appending it to any existing transcript text.
- **FR-022**: The application MUST reject transcript uploads in unsupported formats, including
	files that are not `.txt`, `.docx`, or `.pdf`, with clear feedback.
- **FR-023**: Transcript processing MUST produce task data as JSON containing the task title,
	description, assignee name, status, and blocked state for each created task.
- **FR-024**: Successfully processed transcript-generated tasks MUST be saved immediately without
  requiring an admin confirmation step.
- **FR-025**: Transcript-derived assignee names MUST match the names in the transcript exactly.
- **FR-026**: A new assignee name from a created task MUST become a valid login name automatically,
	without registration.
- **FR-027**: An empty or unreadable transcript upload MUST not create tasks and MUST provide an
	actionable error.
- **FR-028**: The first release MUST exclude passwords, registration, deadlines, time tracking,
	task editing, search, reminders, notifications, and features not specified here.

### Key Entities *(include if feature involves data)*

- **Team Member**: A person identified by a case-sensitive alphabetic name. The reserved exact
	name `admin` identifies the administrator.
- **Task**: A work item with a title, optional description, assignee name, status, blocked state,
	and creation order. It has no deadline or time requirement.
- **Meeting Transcript**: Text pasted by the admin or extracted from an uploaded `.txt`, `.docx`, or `.pdf`
	file as the source for creating assigned tasks.
- **Generated Task Data**: Structured task information produced from a transcript before tasks are
	created.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: At least 95% of valid login attempts show the correct personal or admin board on the
	first attempt.
- **SC-002**: In a test set containing at least 50 tasks across five names, 100% of team members
	see only their assigned tasks and 100% of admin cards show the correct assignee.
- **SC-003**: At least 95% of valid status changes place the task in the selected column without
	changing its assignee or relative order among unaffected tasks.
- **SC-004**: At least 95% of valid self-created tasks appear in the creator's todo column within
	10 seconds.
- **SC-005**: At least 90% of valid transcripts produce task data with the mentioned assignee
	names preserved exactly, whether pasted or uploaded as `.txt`, `.docx`, or `.pdf`, or provide a clear
	error without creating partial tasks.
- **SC-006**: In usability checks, at least 90% of participants can log in, open a task, and
	change its status without assistance.

## Assumptions

- The exact name `admin` is the only administrator identity; no password is used in this demo.
- Team-member names are the complete identity record and are case-sensitive.
- Team-member login is available only after at least one task is assigned to the exact name.
- A self-created task is assigned to its creator.
- Blocked is a boolean task state shown alongside one of the three required statuses.
- “Last come first” means newest tasks appear first, and status changes do not reset creation order.
- The admin's transcript task creation uses an AI capability but does not define a specific vendor
	or model in this specification.
- Transcript uploads support plain-text `.txt` files, `.docx` word-processing files, and `.pdf`
	files; other
	formats are outside the first release.
- Successfully generated transcript tasks are saved immediately without admin confirmation.
- Task data is available when users return to the application.








# UI Design: Team Task Management

**Feature**: [spec.md](./spec.md)
**Design canvas**: https://claude.ai/code/artifact/e421d3f4-d362-4169-9cfb-8f6f40567ae7

> This file lives at `specs/001-task-management/ui-design.md`. Source artboards used to
> build the canvas are kept alongside it in `specs/001-task-management/ui-design/`
> (`Main.dc.html`, `LoginScreen.dc.html`, `TaskDetail.dc.html`, `AddTask.dc.html`,
> `AdminBoard.dc.html`, `AdminTranscript.dc.html`, `canvas.json`) so the design can be
> re-seeded and updated later.

## Product name

Working name for this design: **Loopwork**. Not specified in spec.md — chosen only as a
placeholder identity for the mockups (logo mark, wordmark). Rename freely.

## Direction

No existing design system or brand governed this build (fresh repo, no frontend code
yet), so the canvas commits to one original direction rather than presenting alternates:

- **Tone**: calm, utilitarian SaaS — a kanban tool people check many times a day, so it
  stays low-contrast and gets out of the way rather than competing for attention.
- **Type**: Space Grotesk for headings/wordmark, IBM Plex Sans for body — a distinctive
  pairing instead of default Inter/Arial.
- **Color**: a single violet accent (`oklch(55% 0.19 290)`) for primary actions and the
  current-nav state, plus three status hues sharing the same lightness/chroma family
  (amber = in progress, green = done, muted gray = todo) and one semantic red for
  **Blocked**. Backgrounds are a toned-white/near-black pair, not pure `#fff`/`#000`.
- **Cards** avoid the left-border-accent cliché; status is carried by a column-header dot
  and a small pill badge instead.

Static mockups, not a clickable prototype — this is a first pass to review the flow and
visual language before any frontend code exists.

## Screens (6 artboards on one canvas)

Each maps directly to acceptance scenarios in [spec.md](./spec.md):

1. **Login** (`LoginScreen.dc.html`) — single name field, no password (FR-001, FR-002).
   Helper copy calls out that names are case-sensitive, alphabetic-only, and must already
   have an assigned task; a footnote explains `admin` opens the full board (FR-003–006).
2. **Member board** (`Main.dc.html`, canvas entry point) — logged in as a sample member
   "Priya", showing only her tasks across the three required columns — todo, in progress,
   done (FR-005, FR-007). One in-progress card shows the **Blocked** pill to demonstrate
   FR-016: blocked stays in its current column, no fourth column.
3. **Task detail** (`TaskDetail.dc.html`) — a separate screen (not a modal) with
   description, assignee, a status control, and a blocked toggle, matching FR-010's
   requirement that detail is its own screen with controls to complete or block.
4. **New task** (`AddTask.dc.html`) — the add-task form as a focused modal over the
   member board, title required / description optional, with a hint that new tasks start
   in todo and are assigned to the creator (FR-011–013).
5. **Admin board** (`AdminBoard.dc.html`) — all tasks from every person, same three
   columns, with an assignee chip on every card since the admin must be able to tell
   whose task is whose (FR-008).
6. **Admin → transcript** (`AdminTranscript.dc.html`) — paste-transcript textarea, a
   file dropzone for `.txt` / `.docx` / `.pdf` uploads that appends to existing transcript
   text, a generate action, and a live preview of the resulting assigned tasks with a
   confirmation that they saved immediately — no admin confirmation step (FR-019–024).

## Content notes

- All names, task titles, and the sample transcript text are **illustrative placeholder
  data** for reviewing layout and density, not real content — swap freely.
- No deadline, due-date, or time-based field appears anywhere, per the spec's explicit
  exclusion (edge case: "No task may have a deadline or time-based field").

## Known gaps / open questions for a follow-up pass

- **Blocked interaction on the board itself**: the detail screen shows a toggle; whether
  blocked can also be set from the card menu on the board (vs. detail-screen only) isn't
  decided in spec.md and isn't mocked here.
- **Login rejection state** (name with no assigned tasks) and **empty-title validation**
  aren't shown as separate error-state artboards yet — only the happy-path login and add
  task screens are built.
- **Transcript error state** (invalid/unreadable upload) isn't mocked; only the success
  path is shown.

## Design canvas basics

The link above opens the design in Claude Design's canvas editor. Anyone with access can
pan/zoom across all six screens, click into an element to select and restyle it, and — if
saving is enabled for the viewer — hit **Save** to publish edits as a new version for
everyone. Export to PNG/PDF is available from the toolbar either way.
