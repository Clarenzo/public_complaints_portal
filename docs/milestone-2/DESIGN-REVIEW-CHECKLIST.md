# Milestone 2 — Design Review and Acceptance Checklist

Review these design artifacts before closing Milestone 2:

- [ ] Architecture separates React, FastAPI, MongoDB Atlas, private file storage, and email delivery.
- [ ] Access rules for the five roles are clear, including citizen ownership and staff department/assignment limits.
- [ ] Complaint status transitions, actor permissions, and history behavior are understood.
- [ ] MongoDB collections, relationships, indexes, history strategy, and transaction assumptions are acceptable.
- [ ] API route inventory covers the agreed MVP workflows and describes access scopes and common errors.
- [ ] UI screen inventory and wireframes cover citizen, staff, admin, and public visitor journeys.
- [ ] Security and privacy controls are acceptable as a development baseline; production obligations remain explicit.
- [ ] Open questions are resolved or accepted as deferred decisions before the relevant implementation milestone.

## Design decisions proposed for confirmation

1. **Staff department scope:** Each department head/officer belongs to one department in the initial version.
2. **Complaint references:** Generate non-guessable random references; a reference alone never grants private access.
3. **Timeline:** Keep growing case events in a separate `complaint_events` collection rather than embedding indefinitely in complaint documents.
4. **Consistency:** Use MongoDB transactions for complaint mutations plus their timeline/audit records when supported by the Atlas deployment.
5. **Staff sessions:** Use short-lived access credentials and a refresh/revocation design; browser refresh credential preferably in a secure HttpOnly cookie, with CSRF protections matched to that design.
6. **UI language:** English for initial development unless you request multilingual support.
7. **Faq/guidelines:** Static content for the initial version; departments, categories, and announcements are data-managed.
8. **Attachment provider and report export formats:** Deferred until provider/size/security and first-release reporting requirements are chosen.

## Documentation-only verification for this milestone

No executable application exists yet, so there are no code tests in this milestone. Verify the diagrams and route/data design cover every MVP requirement and role, and that no public or lower-privilege route reveals restricted data. Implementation tests will be added with each feature milestone.

## Suggested commit

```bash
git add docs/milestone-2/
git commit -m "docs: define system architecture and API design"
```
