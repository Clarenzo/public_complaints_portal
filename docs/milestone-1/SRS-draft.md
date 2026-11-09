# Public Complaints & Feedback Portal - Draft SRS

**Document status:** Draft for review
**Milestone:** 1 - Requirements and MVP
**Date:** 9th November 2026
**Version:** 0.1

## 1. Purpose
This document captures the first requirements baseline for a general, configurable, multi-department public complaints and feedback portal. It is based on the supplied role/module outline and the decisions made during project kickoff. It is a project prototype specification, not a statement that the system is operated by or officially affiliated with a Kenyan government body.

## 2. Background and problem statement
People need a clear way to submit complaints and feedback about public services, identify the responsible department and understand what is happening to their case. Departments need a consistent way to receive, assign, investigate, respond to and report on complaints. The proposed portal will provide a shared workflow and auditable case history across multiple departments.

The existing process and its specific shortcomings have not yet been documented. These should be validated with intended users before the requirements are treated as final.

## 3. Goals and objectives
- Give citizens a secure way to submit complaints and follow their progress. 
- Route cases to the appropriate department and staff member.
- Make complaint status, communications and decisions traceable.
- Give department leaders and administrators useful operational statistics.
- Support feedback on completed cases and continuous service improvement.
- Allow departments and categories to be configured without changing application code.

## 4. Stakeholders and user roles
### 4.1 Citizen / general public (authenticated)
- Register, sign in and manage their profile.
- Submit a complaint or feedback with a department, category, subject, description, location, incident data and optional supporting evidence.
- Receive a unique reference number and view their own complaint history and status updates.
- Respond to requests for additional information.
- View department responses and case history.
- Rate a resolved case and subject to workflow rules, request that it be reopened.

### 4.2 System administrator
- View and oversee complaints across departments.
- Create, update and deactivate departments and complaint categories.
- Create, manage, deactivate and assign staff accounts and roles.
- Route or reassign complaints and investigate escalations.
- Configure resolution targets/deadlines.
- View system-wide statistics and authorized reports.
- Review audit records for administrative and other significant actions.

### 4.3 Departmental head
- Access cases belonging to their department.
- Assign cases to that department's officers.
- Monitor workload, overdue cases and officer performance.
- Review investigation findings and proposed resolutions; approve or reject them.
- Respond to citizens, escalate cases and view departmental reports.

### 4.4 Departmental officer
- Access only cases assigned to them, within 
- Review complaint details and permitted attachments.
- Record investigation findings and supporting documents.
- Update a case through permitted workflow actions and communicate with citizens.
- Request additional information and propose a resolution for review.

### 4.5 Public visitor (unauthenticated)
- View general information, FAQs, department directory, public announcements, submission guidance, and aggregate public service statistics.
- Cannot view private complaint details or submit a complaint in the MVP.

## 5. Scope

### 5.1 Proposed MVP scope

1. Public information pages and configurable department/category directory.
2. Citizen account registration and sign-in, secure role-based access, and profile basics.
3. Complaint creation with a generated reference, optional attachment support, and citizen-owned case listing/details.
4. Department routing and staff assignment.
5. Complaint status workflow and time-stamped history with actor and explanatory comments.
6. Department-head review and approval of proposed resolutions.
7. Citizen/officer communication for case-related updates and requests for information.
8. In-app and email notifications for key complaint events.
9. Citizen feedback/rating for resolved cases and a reopen request subject to review.
10. Role-appropriate dashboards and basic operational reports.
11. Administrative audit records for important security and case-management actions.

### 5.2 Excluded from the MVP

- Anonymous complaint submission.
- SMS notifications.
- Native Android/iOS application.
- Public access to identifiable or case-level complaint information.
- Advanced analytics, scheduled report delivery, or sophisticated report-builder features.
- Production deployment to a real government environment until hosting, operational ownership, privacy, and security requirements are approved.

### 5.3 Items to confirm during design/planning

- Whether report downloads are required in the first release and which formats are essential.
- Attachment storage provider, permitted file types, size limits, scanning, and retention.
- Email provider and sender/domain configuration.
- Exact complaint deadlines, escalation rules, and status transition permissions.
- Retention, deletion, privacy notice, consent, and data-subject request processes for the intended operator.

## 6. Functional requirements

Requirement IDs are stable references for backlog tasks and tests.

### Accounts and authorization
- **FR-001:** Citizens can register and sign in using validated credentials.
- **FR-002:** Passwords are stored using a modern password-hashing algorithm; plaintext passwords are never stored or logged.
- **FR-003:** The backend enforces role and department authorization on every protected operation; hiding a frontend page is not sufficient authorization.
- **FR-004:** Staff accounts are created/managed by authorized administrators; there is no public staff self-registration in the MVP.
- **FR-005:** Deactivated accounts cannot authenticate or perform protected actions.

### Complaint handling
- **FR-006:** An authenticated citizen can submit a complaint with subject, description, department, category, location (optional), incident date (optional), and optional evidence.
- **FR-007:** The system creates a unique, non-guessable complaint reference and records submission time and initial status.
- **FR-008:** Citizens can view only their own complaints and their permitted case history; staff access is limited by role, department, and assignment.
- **FR-009:** Administrators can create, update, and deactivate departments and categories without a code change.
- **FR-010:** Administrators can assign/reassign cases to departments; department heads can assign department cases to officers.
- **FR-011:** Authorized users can perform only allowed status transitions. Each transition records previous and new status, timestamp, actor, and an optional comment.
- **FR-012:** Officers can record findings, request citizen information, and propose resolutions.
- **FR-013:** Department heads can approve or reject proposed resolutions and provide a reason.
- **FR-014:** Citizens can add requested information and see citizen-visible updates and responses.
- **FR-015:** A citizen can rate a resolved complaint and request reopening when permitted; the request and its outcome are recorded.

### Notifications, dashboards, reporting, and audit
- **FR-016:** The system creates in-app notifications for key case events and sends email notifications where configured.
- **FR-017:** Dashboards show data relevant to the signed-in user's role and, for department users, only their department/assigned work.
- **FR-018:** Authorized administrators and department heads can view summary statistics and operational reports within their access scope.
- **FR-019:** The system records significant actions, including authentication events, assignments, status changes, user/department changes, and resolution/closure actions.
- **FR-020:** Only authorized administrators can view system-wide audit records.
- **FR-021:** Unauthenticated visitors can access only public information and aggregate statistics that do not expose personal or case-identifying data.

## 7. Proposed complaint workflow

Initial statuses, subject to final transition design:

`Submitted → Under review → Assigned → Under investigation → Awaiting citizen information / Awaiting response → Resolution proposed → Resolved → Closed`

Additional statuses/actions: `Rejected`, `Escalated`, `Reopened`.

A status name alone does not define permission. The design milestone will specify valid transitions, actors, citizen-visible wording, and required comments. All status changes must be auditable.

## 8. Non-functional and security requirements

- **NFR-001 Usability:** Responsive interface usable on common mobile and desktop screen sizes, with clear validation and accessible labels.
- **NFR-002 Security:** Apply least privilege, server-side validation, safe password storage, secure token/session handling, and protected secrets in environment configuration.
- **NFR-003 Privacy:** Collect only data needed for the service; restrict complaint details and evidence to authorized users; avoid exposing sensitive information in logs and public statistics.
- **NFR-004 Integrity:** Use validation and appropriate database constraints/indexes to protect identities, references, role assignments, and case history.
- **NFR-005 Reliability:** Provide clear errors without leaking secrets or internal stack traces; record operational errors safely.
- **NFR-006 Maintainability:** Keep frontend and backend separated, document configuration and API behavior, and test core business rules.
- **NFR-007 Performance:** Establish measurable response-time and concurrent-user targets before production deployment; no production capacity target has yet been supplied.
- **NFR-008 Auditability:** Record important actions with actor and UTC timestamp, and prevent ordinary users from editing or deleting audit history.

Production privacy, security, retention, and legal obligations must be validated with the organization that would operate the portal before real personal data is collected.

## 9. Technology and constraints

- Frontend: React.js and Tailwind CSS.
- Backend: Python and FastAPI.
- Database: MongoDB Atlas.
- IDE/version control: Visual Studio Code, Git, and GitHub.
- The API is the security boundary; frontend role checks are for user experience only.
- Secrets and environment-specific settings must not be committed to Git.
- Attachments should be stored through an appropriate file/object-storage mechanism rather than assumed to be embedded in MongoDB documents; implementation choice remains open.

## 10. Acceptance criteria for the requirements milestone

Milestone 1 is ready for approval when:

1. The general multi-department portal context and five user roles are accepted.
2. MVP inclusions and exclusions are accepted, including account-required complaints and no SMS in the first release.
3. Functional requirements, core workflow, and permission boundaries have been reviewed.
4. Open decisions are recorded rather than silently assumed.
5. The initial backlog is sufficiently clear to plan system design.

## 11. Glossary

- **Complaint:** A case submitted by a citizen about a public service or experience.
- **Department:** A configurable organizational unit responsible for a service area.
- **Reference:** A unique identifier citizens can use when discussing a complaint; it is not by itself authorization to see private case data.
- **RBAC:** Role-based access control; permissions are granted based on a user's assigned role and relevant department/assignment scope.
- **Audit record:** Append-oriented record of a significant system action, actor, and time.
