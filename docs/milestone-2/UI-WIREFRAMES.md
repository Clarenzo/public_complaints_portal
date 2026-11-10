# UI Screen Inventory and Low-Fidelity Wireframes

**Milestone:** 2 — System design  
**Status:** Draft for review  
**Approach:** Responsive, accessible, low-fidelity layouts; visual styling comes during implementation.

## 1. Shared layout principles

- Public pages have a header with portal name, navigation, language/accessibility entry points (language selection is not yet scoped), and sign-in/register actions.
- Signed-in screens show role-aware navigation, page title, useful context, and clear sign-out/profile controls.
- Mobile layouts collapse navigation and tables into readable cards.
- Status is shown with a text label and icon/color combination; color is never the only signal.
- Every form has visible labels, inline validation, error summary, and confirmation states.
- Complaint details show only fields/events permitted to the current user.

## 2. Screen inventory

| Screen | User | Primary content/actions |
|---|---|---|
| Public landing page | Visitor | Purpose, how it works, submit guidance, FAQs, departments, announcements, sign-in/register CTA |
| Department directory | Visitor/citizen | Search/filter active departments, descriptions/public contacts |
| FAQ and guidelines | Visitor/citizen | Public help content |
| Register / sign in | Citizen | Account creation and sign-in; field validation and recover-account entry point |
| Citizen dashboard | Citizen | Summary cards, recent cases, status, notification shortcuts, submit complaint CTA |
| New complaint | Citizen | Subject, description, department/category, optional incident/location, evidence (if enabled), review/submit |
| Citizen complaint detail | Citizen | Reference, status, safe timeline/responses, requested-info reply, feedback/reopen actions when eligible |
| Staff dashboard | Admin/head/officer | Role-specific counts, overdue/priority queues, recently changed cases |
| Case queue | Admin/head/officer | Search/filter/sort within authorized scope; assignment and status indicators |
| Staff complaint detail | Authorized staff | Case data, internal/citizen-visible timeline, assignment, investigation, request-info, resolution actions |
| Department management | Administrator | List/create/edit/deactivate department, assign head |
| User management | Administrator | Create/manage staff account, role, department, activation |
| Category management | Administrator | List/create/edit/deactivate categories |
| Reports | Admin/head | Date/filter controls, scoped summary tables/charts, approved export actions |
| Audit log | Administrator | Search/filter significant actions, actor, time, target, outcome |
| Notifications | Signed-in users | Own notifications, unread status, mark read |

## 3. Low-fidelity layouts

### Public landing page

```text
+--------------------------------------------------------------+
| Portal name     How it works  Departments  Help   Sign in    |
+--------------------------------------------------------------+
| Submit feedback with confidence                              |
| [How complaints are handled] [Create an account / Sign in]    |
+--------------------------------------------------------------+
| 1 Submit  ->  2 Department reviews  ->  3 Track updates       |
+--------------------------------------------------------------+
| Departments directory | Latest announcements | FAQs          |
+--------------------------------------------------------------+
| Public service statistics (aggregate, safe, no case details)  |
+--------------------------------------------------------------+
| Privacy / accessibility / contact links                       |
+--------------------------------------------------------------+
```

### Citizen dashboard

```text
+--------------------------------------------------------------+
| Portal | My cases | Notifications | Profile | Sign out       |
+--------------------------------------------------------------+
| Welcome back                         [ + Submit a complaint ] |
| [Total] [In progress] [Awaiting you] [Resolved]               |
+--------------------------------------------------------------+
| Recent complaints                                            |
| Reference | Subject | Department | Status | Updated | View    |
+--------------------------------------------------------------+
| Need help? Guidelines / FAQs                                  |
+--------------------------------------------------------------+
```

### Complaint submission

```text
+--------------------------------------------------------------+
| New complaint                         Step 1 of 2             |
| Subject [____________________________________________]        |
| Description [________________________________________]        |
| Department [Select v]   Category [Select v]                  |
| Location [optional]      Incident date [optional]             |
| Evidence [Add files] (allowed type/size guidance)             |
| [Cancel]                                    [Review details]  |
+--------------------------------------------------------------+
| Review -> confirm privacy guidance -> submit -> reference     |
+--------------------------------------------------------------+
```

### Complaint detail (citizen)

```text
+--------------------------------------------------------------+
| My complaints > PC-...                         Status: ...   |
| Subject / Department / Category / Submitted                   |
| What happens next?                                             |
+--------------------------------------------------------------+
| Timeline (citizen-visible updates only)                        |
| [date] Submitted -> [date] Assigned -> ...                    |
+--------------------------------------------------------------+
| Requested information [reply box] [Send] (when requested)     |
| [Rate handling] / [Request reopening] (when eligible)          |
+--------------------------------------------------------------+
```

### Department staff case queue

```text
+--------------------------------------------------------------+
| Department workspace | Queue | Reports | Notifications        |
+--------------------------------------------------------------+
| Queue: [Assigned to me v] [Status v] [Date range] [Search]    |
| Ref | Subject | Status | Due | Last update | Action            |
| ...                                                          |
+--------------------------------------------------------------+
```

### Staff complaint detail

```text
+--------------------------------------------------------------+
| Reference / status / due date               [valid actions]   |
| Complaint details and attachments (permission checked)        |
| Assignment panel (admin/head only)                             |
| Citizen-visible timeline | Internal investigation notes      |
| Request information / Add update / Propose resolution          |
| Head review: [Approve] [Reject + reason]                       |
+--------------------------------------------------------------+
```

## 4. Interaction and accessibility notes

- Confirm before irreversible or high-impact actions (deactivation, rejection, closure); require reasons for relevant decisions.
- Distinguish citizen-visible responses from internal notes and label each clearly.
- Display a clear empty state and safe error recovery on every list/detail flow.
- Support keyboard navigation, visible focus, semantic headings, form labels, and screen-reader announcements for validation/status changes.
- Avoid displaying sensitive details in notification previews or public search results.
- Do not allow the user to infer another case exists from a different error response.

## 5. Open design questions

- English-only MVP or multilingual public interface? (Default assumption: English for first implementation, to confirm.)
- Is a Figma artifact expected, or are these wireframes sufficient for this learning project?
- Which charts and export formats are essential for first-release dashboards?
- Should users receive an explicit privacy notice/consent step at registration and complaint submission? Exact copy requires organizational review.
