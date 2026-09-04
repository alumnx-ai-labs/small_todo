<!--
Sync Impact Report
- Version change: unversioned template -> 1.0.0
- Modified principles: template placeholders -> I. Simplicity First; II. Required Stack;
	III. Local-First Persistence; IV. Authentication Boundary; V. Testable Delivery
- Added sections: Technology Constraints; Development Workflow
- Removed sections: none
- Follow-up TODOs: Confirm the original ratification date.
-->

# Small Todo Constitution

## Core Principles

### I. Simplicity First
Features MUST remain minimal and focused on the user-visible task they solve. Teams MUST NOT
add speculative abstractions, infrastructure, or features without a documented requirement and
clear maintenance value. This keeps the application understandable and reduces operational cost.

### II. Required Stack
The backend MUST use Python, the frontend MUST use React, and browser-to-server communication
MUST use an explicit, documented API contract. A change to these technologies requires a
constitution amendment so that architecture remains predictable.

### III. Local-First Persistence
Application data MUST be stored in SQLite for local use. Data access MUST be isolated behind a
backend persistence boundary, and schema changes MUST be reproducible from a documented migration
or initialisation path. This provides reliable local storage without unnecessary infrastructure.

### IV. Authentication Boundary
The initial product MUST NOT include login, authentication, or account-management functionality.
Endpoints MUST NOT imply user identity or claim account-level isolation. Any future identity
requirement MUST be specified and reviewed as a separate governance change before implementation.

### V. Testable Delivery
New behaviour MUST have focused automated tests at the layer where it is decided, including API
and persistence tests for backend contracts and component or interaction tests for frontend
behaviour. Every change MUST pass the repository's available checks before review.

## Technology Constraints

The system MUST run locally without a hosted service dependency for its core workflow. The
frontend MUST remain a React application, the backend MUST remain Python, and SQLite MUST be the
default local database. Secrets, personal data, and credentials MUST NOT be committed to the
repository or written to normal application logs.

## Development Workflow

Each change MUST identify the affected user behaviour, API contract, and persistence impact.
Implementation MUST proceed from a small specification to focused tests and then the smallest
change that satisfies those tests. Reviews MUST check constitution compliance, test coverage, and
whether added complexity is justified.

## Governance
<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

This constitution supersedes conflicting project practices. Amendments MUST document the reason,
the affected principles, the migration impact, and the new semantic version. MAJOR increments are
required for incompatible governance changes or removals; MINOR increments are required for new
principles or materially expanded requirements; PATCH increments are required for clarifications
and non-semantic corrections. Reviews MUST verify compliance, and unresolved violations MUST be
recorded before approval.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm original adoption date | **Last Amended**: 2026-09-04
