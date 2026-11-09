# Initial Product Backlog

**Status:** Draft — refine after SRS approval  
**Priority key:** P0 = required for MVP; P1 = important; P2 = later/future  

| ID | Epic / task | Priority | Notes / acceptance summary |
|---|---|---:|---|
| B-01 | Public portal information pages | P1 | Home, FAQs, guidelines, announcements, department directory, safe aggregate statistics. |
| B-02 | Citizen registration and authentication | P0 | Validated registration/login; securely hashed passwords; account status enforced. |
| B-03 | Role and department access control | P0 | Enforced on every backend route; tests prove unauthorized access is denied. |
| B-04 | Staff account administration | P0 | Administrator creates/updates/deactivates staff and assigns permitted roles/departments. |
| B-05 | Department and category management | P0 | Authorized administrator manages configurable departments and categories. |
| B-06 | Complaint submission | P0 | Citizen submits required fields; system generates unique reference and initial history entry. |
| B-07 | Citizen case list and detail | P0 | Citizen sees only their own cases, status, citizen-visible updates, and history. |
| B-08 | Attachment handling | P1 | Define type/size/security/storage rules before implementation; do not commit sample sensitive files. |
| B-09 | Department routing and officer assignment | P0 | Admin routes/reassigns; department head assigns officers; assignment event is recorded. |
| B-10 | Complaint workflow and status history | P0 | Valid transitions, actor, timestamp, and comments are recorded and authorization-tested. |
| B-11 | Investigation and citizen communication | P0 | Officer records findings, requests information, and posts updates; citizen can respond. |
| B-12 | Resolution review | P0 | Officer proposes resolution; department head approves/rejects with rationale. |
| B-13 | Escalation and deadlines | P1 | Admin-defined deadlines and authorized escalation with audit history. |
| B-14 | In-app notifications | P1 | Notifications for submission, assignment, status changes, information requests, and resolution events. |
| B-15 | Email notifications | P1 | Configurable provider; local development supports safe testing without sending real mail by default. |
| B-16 | Citizen feedback and reopen request | P1 | Rating/comments after resolution; reopen request follows configured review rules. |
| B-17 | Role-specific dashboards | P1 | Citizen, administrator, head, and officer views respect data scope. |
| B-18 | Reports and exports | P1 | Define first-release report metrics and export formats during design. |
| B-19 | Audit log | P0 | Significant actions are recorded; system-wide audit access restricted to admins. |
| B-20 | Testing, security review, and deployment preparation | P0 | Unit/integration/API/manual acceptance checks; deployment remains a later milestone. |

## Delivery approach

Implement backlog items in small vertical slices. Each development milestone should end with tests, documentation updates, a review checklist, and a suggested Git commit. No milestone should be considered complete until its acceptance checks pass.
