# API Contract — Initial Design

**Milestone:** 2 — System design  
**Status:** Draft for review  
**Base path:** `/api/v1`  
**Transport:** HTTPS JSON; multipart/form-data only for upload endpoints if direct upload is selected.

This contract defines intended behavior and authorization; exact schemas will be implemented with Pydantic and versioned if breaking changes are needed. FastAPI-generated OpenAPI will become the executable API reference.

## 1. Common conventions

- Dates/times: ISO 8601 UTC.
- Internal IDs: opaque string representation of MongoDB ObjectId; complaint reference is a separate opaque human-readable identifier.
- Pagination: `limit` (default 20, max 100) and opaque `cursor` where practical; response includes `items`, `next_cursor`.
- List filters must be validated and capped. Sort fields are allowlisted.
- Errors use a consistent envelope:

```json
{
  "error": {
    "code": "forbidden",
    "message": "You do not have permission to perform this action.",
    "request_id": "opaque-request-id",
    "details": []
  }
}
```

Use standard HTTP status codes: 400 invalid transition/request, 401 unauthenticated, 403 unauthorized, 404 not found or deliberately concealed out-of-scope resource, 409 duplicate/conflict, 422 validation error, 429 rate limited, 500 generic server error. Never return stack traces, secrets, or whether a private case exists when that would leak access.

## 2. Authentication and profile

| Method and path | Access | Purpose |
|---|---|---|
| `POST /auth/register` | Public | Create citizen account; staff role cannot be chosen in request. |
| `POST /auth/login` | Public, rate-limited | Authenticate; return access token and refresh mechanism per final token design. |
| `POST /auth/refresh` | Refresh credential | Rotate/refresh access credentials; reject revoked/expired credentials. |
| `POST /auth/logout` | Authenticated | Revoke refresh/session credential where supported. |
| `GET /users/me` | Authenticated | Return current user's safe profile and role scope. |
| `PATCH /users/me` | Citizen | Update allowed profile fields only. |
| `POST /admin/users` | Administrator | Create staff account with approved staff role and department. |
| `GET /admin/users` | Administrator | List/filter accounts. |
| `PATCH /admin/users/{user_id}` | Administrator | Update role/department/activation via validated, audited operations. |

Passwords should not be returned by any endpoint. Email verification and password recovery are requirements in the source role/module outline; exact provider/token workflow is a decision for implementation planning and must be added before auth is considered complete.

## 3. Public directory and content

| Method and path | Access | Purpose |
|---|---|---|
| `GET /public/departments` | Public | List active departments and approved public contact details. |
| `GET /public/categories` | Public | List active categories. |
| `GET /public/announcements` | Public | List published, currently visible announcements. |
| `GET /public/faqs` | Public | FAQ content; can be static initially. |
| `GET /public/guidelines` | Public | Submission guidance; can be static initially. |
| `GET /public/statistics` | Public | Safe aggregate counts only; no personal or case-level data. |
| `POST /admin/departments` | Administrator | Create department. |
| `PATCH /admin/departments/{department_id}` | Administrator | Update/deactivate department. |
| `POST /admin/categories` | Administrator | Create category. |
| `PATCH /admin/categories/{category_id}` | Administrator | Update/deactivate category. |
| `POST /admin/announcements` | Administrator | Create/update/publish announcement. |

## 4. Complaints and case timeline

| Method and path | Access | Purpose |
|---|---|---|
| `POST /complaints` | Citizen | Submit a complaint; server sets citizen, reference, timestamps, status. |
| `GET /complaints` | Citizen / staff | Return only caller's own cases, assigned officer's cases, department-head cases, or admin scope according to role. |
| `GET /complaints/{complaint_id}` | Authorized case participant | Return case fields appropriate to caller and scope. |
| `GET /complaints/{complaint_id}/events` | Authorized case participant | Return citizen-visible events to citizens; permitted internal events to staff. |
| `POST /complaints/{complaint_id}/information` | Citizen | Add requested information; must be in a workflow state accepting it. |
| `POST /complaints/{complaint_id}/events` | Assigned officer / department head | Add authorized case response or investigation update; visibility explicitly set/validated. |
| `POST /complaints/{complaint_id}/status-transitions` | Role/state dependent | Request an allowed transition with target status and required reason/comment. |
| `POST /complaints/{complaint_id}/assignments` | Administrator | Assign/reassign department; records actor/time/reason. |
| `POST /complaints/{complaint_id}/officer-assignment` | Department head | Assign case to an active officer in same department. |
| `POST /complaints/{complaint_id}/resolution-proposals` | Assigned officer | Submit proposed resolution and findings for review. |
| `POST /complaints/{complaint_id}/resolution-review` | Department head | Approve or reject proposal with reason. |
| `POST /complaints/{complaint_id}/escalations` | Department head / administrator | Escalate with reason to authorized scope. |
| `POST /complaints/{complaint_id}/feedback` | Owning citizen, resolved case | Submit rating, comment, adequately-addressed answer. |
| `POST /complaints/{complaint_id}/reopen-requests` | Owning citizen | Request reopening with reason. |
| `POST /complaints/{complaint_id}/reopen-review` | Authorized head/admin | Approve/reject reopen request with reason. |

All write endpoints enforce state, role, ownership, department, and assignment constraints on the backend. Do not accept caller-supplied `citizen_id`, `actor_id`, `created_at`, reference, or arbitrary status as trusted values.

## 5. Attachments (provider-dependent design)

Candidate flow: `POST /complaints/{id}/attachments/upload-intent` returns a narrowly scoped short-lived upload authorization; client uploads to private object storage; `POST /complaints/{id}/attachments/complete` verifies metadata and associates the file. Alternatively, proxy small uploads through FastAPI. The final choice depends on provider, size limits, scanning, and operational cost. Downloads must use authorized short-lived access; public URLs are not acceptable for private evidence.

## 6. Dashboards, reports, and notifications

| Method and path | Access | Purpose |
|---|---|---|
| `GET /dashboard` | Authenticated | Role-specific aggregates; department/assignment scoping on server. |
| `GET /reports/complaints` | Administrator / department head | Filtered report data within permitted scope. |
| `GET /reports/complaints/export` | Administrator / department head | Approved export format(s), after format and field-safety decision. |
| `GET /notifications` | Authenticated | Current user's inbox only. |
| `PATCH /notifications/{notification_id}` | Owner | Mark own notification read. |

Public statistics and authenticated reports use separate routes and authorization policies. Public statistics should aggregate/coarsen sensitive dimensions and avoid small groups that could identify individuals.

## 7. Health and operational endpoints

- `GET /health/live`: process liveness only; no secrets or dependency details.
- `GET /health/ready`: readiness for required dependencies, with safe response details.
- API documentation is enabled in local development; production exposure policy will be set at deployment.

## 8. Example complaint submission

Request:

```json
{
  "subject": "Streetlight not working",
  "description": "The light outside the community hall has been off for several nights.",
  "department_id": "<department-id>",
  "category_id": "<category-id>",
  "location": "Community hall entrance",
  "incident_date": "2026-11-08"
}
```

Successful response (`201 Created`, abbreviated):

```json
{
  "id": "<complaint-id>",
  "reference": "PC-<opaque-token>",
  "status": "submitted",
  "subject": "Streetlight not working",
  "department": {"id": "<department-id>", "name": "Public Works"},
  "created_at": "2026-11-09T12:00:00Z"
}
```

The reference is not an authorization credential. Access to the case still requires an authenticated, authorized user.

## 9. Contract review checklist

- Every protected route states its required role and data scope.
- Actor and ownership identifiers are derived from the authenticated principal.
- Every workflow mutation validates the current state and writes history/audit records.
- Public routes return no private complaint fields.
- Pagination, filtering, validation, error behavior, and export safety are defined before implementation.
- Token refresh/revocation and password recovery/verification details are finalized before authentication implementation.
