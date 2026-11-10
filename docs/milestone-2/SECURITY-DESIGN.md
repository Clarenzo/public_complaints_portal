# Security and Privacy Design

**Milestone:** 2 — System design  
**Status:** Draft baseline; production controls require operational/legal review.

## 1. Security objectives

Protect citizen and staff accounts, complaint descriptions and evidence, internal investigation notes, and operational records. Enforce least privilege, safe defaults, auditable case changes, and privacy-preserving public access.

## 2. Authentication and session design

- Citizen registration is public; staff roles are never self-selected at registration.
- Staff accounts are provisioned by administrators and assigned a department as appropriate.
- Hash passwords with a modern, deliberately configured password-hashing algorithm and unique salt; never log or return passwords.
- Use short-lived access credentials and a controlled refresh/revocation strategy. Prefer secure, HttpOnly, SameSite cookies for browser refresh credentials; decide CSRF protections based on the final cookie/CORS design.
- Enforce TLS in deployed environments; configure CORS only for explicitly trusted frontend origins.
- Rate-limit login, registration, password recovery, and other abuse-prone endpoints; use generic authentication failure messages.
- Email verification and recovery flows require single-use, expiring tokens and must not reveal account existence.

## 3. Authorization model

Every protected operation checks authenticated user, active account, role, department, assignment, and resource ownership as applicable. Centralize policy checks and test them at the API/service layer. UI hiding is not authorization. Deny by default. Return a generic not-found response when revealing a case's existence would create a privacy leak.

| Resource/action | Citizen | Officer | Dept head | Admin | Public |
|---|---|---|---|---|---|
| View private complaint | Own only | Assigned only | Own department | System-wide | No |
| Add citizen-visible update | Own case when allowed | Assigned case | Department case | As policy allows | No |
| Add internal investigation note | No | Assigned case | Department case | Escalation oversight | No |
| Assign to department | No | No | No | Yes | No |
| Assign officer | No | No | Own department | Yes | No |
| Review resolution | No | Propose only | Own department | Escalation/oversight policy | No |
| View system audit | No | No | No | Yes | No |
| View safe public aggregate | Yes | Yes | Yes | Yes | Yes |

## 4. Input, output, and file safety

- Validate request bodies, query parameters, lengths, enums, identifiers, and state transitions at the backend.
- Use allowlisted fields for updates; prevent mass assignment of role, actor, owner, reference, timestamps, or status.
- Render user content as text by default; sanitize/escape any rich text if introduced. Avoid unsafe HTML rendering.
- Limit request/body and upload sizes; allowlist file types; verify actual content rather than trusting extension; use malware scanning where available; store files privately and serve only after authorization.
- Do not place secrets or private file URLs in frontend bundles or public responses.
- Apply output field filtering: never serialize password hashes, private tokens, internal notes to citizens, or audit metadata to public endpoints.

## 5. Web/API controls

- HTTPS, secure headers, restrictive CORS, and appropriate cookie flags in deployed environments.
- CSRF defenses for cookie-authenticated state-changing requests; document whether SameSite and CSRF tokens are used.
- Rate limiting and request-size limits at the API/proxy boundary.
- Avoid leaking detailed database errors, stack traces, or existence of private resources.
- Use request IDs for safe support diagnostics; redact PII from logs.
- Use dependency and vulnerability checks in CI once repository setup is established.

## 6. Secrets, audit, and operations

- Read configuration from environment/secret manager; commit only `.env.example` placeholders, never `.env` secrets.
- Use least-privilege MongoDB credentials and restrict Atlas network access; require TLS.
- Record important actions with actor, UTC time, target, outcome, and request ID. Never record passwords, bearer tokens, complete complaint bodies, attachment bytes, or reset tokens.
- Restrict audit access to administrators; define retention, backup, restore, monitoring, and incident response before production.
- Use synthetic data in local/test environments. Separate development, test, staging, and production credentials and databases.

## 7. Privacy and public disclosure

- Complaint data is private by default; public routes expose only approved public information and carefully aggregated statistics.
- Consider re-identification risk in small groups: suppress or coarsen public aggregates where counts are low.
- Collect only fields necessary for the service; define retention, deletion/anonymization, access correction, privacy notice, and operational ownership before real deployment.
- Production use with real citizen data requires review by the operating organization and qualified local privacy/security stakeholders. This document is a technical design, not legal advice.

## 8. Threat scenarios and controls

| Threat | Primary control |
|---|---|
| Citizen guesses another complaint ID/reference | Authentication, ownership policy, non-guessable reference, generic out-of-scope response |
| Officer accesses another department's case | Backend department+assignment checks; authorization tests |
| User changes role in request/client storage | Role is derived from trusted server-side user record; allowlisted updates |
| Malicious attachment | Type/size validation, private storage, scanning, authorized download |
| Credential stuffing | Rate limits, secure hash, generic errors, monitoring/lockout policy decision |
| Stored XSS in complaint text | Plain-text rendering/escaping, validation, CSP where deployed |
| Email notification leaks case information | Minimal notification text, link to authenticated portal, provider adapter |
| Public statistics reveal a complainant | Aggregate/coarsen, small-cell suppression, privacy review |
| Partial status/event write | Transaction or outbox consistency strategy; fail safely |
| Secrets committed to Git | `.gitignore`, environment templates, secret scanning, rotate exposed credentials |

## 9. Security review gate

Before implementation of authentication and file upload, finalize token/refresh behavior, CSRF approach, email verification/recovery, rate-limit policy, file provider/scan/storage controls, and audit event list. Before production, add threat review, privacy/retention approval, backup/restore test, dependency scanning, and deployment hardening.
