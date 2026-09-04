<!--
Sync Impact Report
- Version change: 3.0.0 -> 4.0.0
- Modified principles: III. Local-First Persistence (SQLite -> MongoDB)
- Added sections: none
- Removed sections: none
- Follow-up TODOs: Confirm the original ratification date; specify MongoDB deployment and
    connection configuration; specify admin credential values, credential storage, and task
    visibility rules before implementation.
-->

# Small Todo Constitution

## Core Principles

### I. Simplicity First
Features MUST remain minimal and focused on the user-visible task they solve. Teams MUST NOT
add speculative abstractions, infrastructure, or features without a documented requirement and
clear maintenance value. This keeps the application understandable and reduces operational cost.

### II. Required Stack
The backend MUST use Java with Spring Boot, the frontend MUST use Angular, and browser-to-server
communication MUST use an explicit, documented REST API contract. A change to these technologies
requires a constitution amendment so that architecture remains predictable.

### III. Local-First Persistence
Application data MUST be stored in MongoDB for local use. Data access MUST be isolated behind a
backend persistence boundary, and collection or index changes MUST be reproducible from a
documented migration or initialisation path. This provides flexible local persistence while
keeping database access replaceable and testable.

### IV. Authentication Boundary
The product MUST provide admin authentication through the same login page used by the
application. Task access MUST be limited according to the authenticated admin identity once
task visibility rules are defined in the feature specification. Credential values and secrets
MUST NOT be committed to the repository or written to normal application logs. Additional roles,
account-management flows, and identity-based features require explicit specification before
implementation.

### V. Testable Delivery
New behaviour MUST have focused automated tests at the layer where it is decided, including API
and persistence tests for backend contracts and component or interaction tests for frontend
behaviour. Every change MUST pass the repository's available checks before review.

## Technology Constraints

The system MUST run locally without a hosted service dependency for its core workflow. The
frontend MUST remain an Angular application, the backend MUST remain a Java Spring Boot
application, and MongoDB MUST be the default database. Secrets, personal data, and credentials
MUST NOT be committed to the repository or written to normal application logs.

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

**Version**: 4.0.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm original adoption date | **Last Amended**: 2026-09-04
