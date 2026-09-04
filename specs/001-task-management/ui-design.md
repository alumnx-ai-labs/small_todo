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
