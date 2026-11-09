# Project Scope — Draft

**Project:** Public Complaints & Feedback Portal  
**Milestone:** 1 — Requirements and MVP  
**Status:** Draft for review  

## Product vision

Build a secure, responsive, configurable portal where citizens can submit and track complaints about public services, while multiple departments can investigate and resolve cases through a transparent, auditable workflow.

## Confirmed project decisions

- The product is a **general multi-departmental portal**, not a system for a named agency at this stage.
- The five roles are citizen, system administrator, departmental head, departmental officer, and unauthenticated public visitor.
- Anonymous submissions are deferred; MVP complaint submission requires a citizen account.
- Staff accounts are administrator-managed.
- Notifications target in-app and email; SMS is excluded from the MVP.
- Planned stack: React.js, FastAPI, MongoDB Atlas, and Tailwind CSS; development in VS Code with Git/GitHub.

## MVP boundaries

The MVP should prove an end-to-end case journey: a citizen submits a complaint, authorized staff route and investigate it, a department head reviews the proposed resolution, the citizen sees updates and can provide feedback, and authorized users can review relevant summaries. Public visitors see only general information and safe aggregate statistics.

## Outside the MVP

Anonymous submissions, SMS, native mobile applications, public case-level disclosure, and production operation for a real agency are excluded for now. These may be reconsidered after the core portal and its privacy/security requirements are validated.

## Key assumptions to validate

- The portal will use configurable departments and categories rather than hard-coded agency names.
- Complaint information and attachments may contain sensitive personal data, so access is private by default.
- A complaint reference is for correspondence and lookup assistance, not a substitute for authentication.
- Initial implementation and testing use synthetic data only.
- Production hosting, email provider, attachment storage, retention policy, and operational ownership are not yet selected.

## MVP success measures (initial proposals)

- A citizen can submit and later find their own complaint.
- Staff cannot access cases outside their permitted role/department/assignment scope.
- A case can proceed through assignment, investigation, review, resolution, and feedback with a complete history.
- Notifications are generated for agreed key events.
- Public pages and aggregate metrics expose no personal or case-identifying information.
- Core workflows are covered by automated tests and documented manual acceptance checks.
