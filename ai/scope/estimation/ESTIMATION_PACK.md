# Project Estimation Pack

## Master Summary

# Project Estimates — Master Summary

## Estimation Scale

| Points | Relative Size | T-Shirt |
|--------|--------------|---------|
| 1      | Trivial      | XS      |
| 2      | Small        | XS      |
| 3      | Small–Medium | S       |
| 5      | Medium       | M       |
| 8      | Large        | L       |

Epic T-shirt sizing bands:

| Size | Points |
|------|--------|
| S    | ≤ 10   |
| M    | 11–15  |
| L    | 16–25  |
| XL   | 26+    |

---

## Epic Estimates

| #   | Epic                                     | Stories | Points | T-Shirt | Estimated Duration    |
|-----|------------------------------------------|---------|--------|---------|-----------------------|
| 001 | Discovery                                | 5       | 13     | M       | 1–1.5 weeks           |
| 002 | Project Foundation & Infrastructure      | 5       | 15     | M       | 1–1.5 weeks           |
| 003 | Authentication & Agent Account Mgmt      | 4       | 10     | S       | 0.5–1 week            |
| 004 | Booking Request Creation                 | 6       | 30     | XL      | 2–3 weeks             |
| 005 | Booking Tracking & Status Management     | 2       | 10     | S       | 0.5–1 week            |
| 006 | Admin Booking Management Portal          | 6       | 26     | XL      | 1.5–2.5 weeks         |
| 007 | Messaging & Notifications                | 4       | 14     | M       | 1–1.5 weeks           |
| 008 | Services, Packages & Pricing Rules       | 4       | 20     | L       | 1–2 weeks             |
| 009 | Floor Plan Upload & Barfoot Integration  | 5       | 19     | L       | 1–2 weeks ⚠️          |
| 010 | Ubookr Third-Party Integration *(opt.)*  | 3       | 18     | L       | 1.5–2.5 weeks ⚠️      |
| 011 | QA, UAT & Release Readiness              | 4       | 20     | L       | 1–2 weeks             |
| 012 | Launch & Deployment                      | 3       | 13     | M       | 1–1.5 weeks           |
|     | **TOTAL (core, excl. Epic 010)**         | **48**  | **190**|         | **11.5–20 weeks**     |
|     | **TOTAL (incl. optional Epic 010)**      | **51**  | **208**|         | **13–22.5 weeks**     |

> ⚠️ Epic 009 duration is subject to Barfoot API access being confirmed. If credentials are delayed, this epic's timeline may extend beyond the upper bound independently of all other work.

---

## Total Project Estimate

| Scenario                         | Story Points | Duration Range       |
|----------------------------------|-------------|----------------------|
| Core scope (excl. Epic 010)      | 190         | 11.5–20 weeks        |
| Full scope (incl. optional 010)  | 208         | 13–22.5 weeks        |

Duration ranges reflect a ±20% error margin applied across all epics to account for integration complexity, stakeholder availability, and scope clarification during delivery.

---

## Delivery Sequence & Dependencies

```
001 Discovery
  └─▶ 002 Foundation & Infra
        └─▶ 003 Auth
              └─▶ 004 Booking Creation ◀── 008 Services & Pricing (parallel)
                    └─▶ 005 Booking Tracking
                    └─▶ 006 Admin Portal
                          └─▶ 007 Messaging & Notifications
                                └─▶ 009 Barfoot Integration
                                └─▶ 010 Ubookr (optional)
                                      └─▶ 011 QA / UAT
                                            └─▶ 012 Launch & Deployment
```

---

# Epic 001 — Discovery: Estimate

## User Stories

| Key    | Story                              | Points |
|--------|------------------------------------|--------|
| DAN-25 | Define Project Vision              | 3      |
| DAN-26 | Gather Requirements                | 2      |
| DAN-27 | Define User Roles & Permissions    | 1      |
| DAN-28 | Create User Journeys & Wireframes  | 5      |
| DAN-29 | Publish SRS and Initial Backlog    | 2      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **13**            |
| **T-Shirt Size**         | **M**             |
| **Estimated Duration**   | 1–1.5 weeks       |

## Notes

- This epic is a prerequisite for all implementation epics.
- Wireframing (DAN-28) is the highest-effort item; stakeholder availability is the key scheduling risk.
- Duration assumes single practitioner working sequentially with stakeholders available for timely feedback.

---

# Epic 002 — Project Foundation & Infrastructure Setup: Estimate

## User Stories

| Key    | Story                               | Points |
|--------|-------------------------------------|--------|
| DAN-30 | Setup Flutter Project Foundation    | 3      |
| DAN-31 | Configure Environments              | 3      |
| DAN-32 | Setup Auth & API Schema Foundations | 3      |
| DAN-33 | Setup CI/CD Pipeline                | 3      |
| DAN-34 | Enable Logging & Monitoring         | 3      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **15**            |
| **T-Shirt Size**         | **M**             |
| **Estimated Duration**   | 1–1.5 weeks       |

## Notes

- Can begin in parallel with late-stage Discovery (DAN-28/DAN-29) once roles and tech decisions are confirmed.
- CI/CD pipeline (DAN-33) unblocks continuous delivery for all subsequent epics and should be prioritised.

---

# Epic 003 — Authentication & Agent Account Management: Estimate

## User Stories

| Key    | Story                              | Points |
|--------|------------------------------------|--------|
| DAN-35 | Agent Login                        | 2      |
| DAN-36 | Password Reset                     | 2      |
| DAN-37 | Manage Agent Profile               | 3      |
| DAN-38 | Enforce Role-Based Access Control  | 3      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **10**            |
| **T-Shirt Size**         | **S**             |
| **Estimated Duration**   | 0.5–1 week        |

## Notes

- Depends on Epic 002 (auth schema foundations — DAN-32 must be complete).
- RBAC (DAN-38) is a cross-cutting concern that should be validated early to unblock admin portal work.

---

# Epic 004 — Booking Request Creation: Estimate

## User Stories

| Key    | Story                                           | Points |
|--------|-------------------------------------------------|--------|
| DAN-39 | Create Booking Request                          | 5      |
| DAN-40 | Enter Property Details                          | 5      |
| DAN-41 | Select Preferred Date & Time                    | 5      |
| DAN-42 | Choose Service Package                          | 5      |
| DAN-43 | Add Notes & Receive Submission Confirmation     | 5      |
| DAN-44 | View Booking List                               | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **30**            |
| **T-Shirt Size**         | **XL**            |
| **Estimated Duration**   | 2–3 weeks         |

## Notes

- Largest single epic in the project; multi-step form UI and booking data model are the core complexity.
- Depends on Epic 003 (auth) and Epic 008 (service packages) for full integration.
- DAN-44 (View Booking List) is included here and is a prerequisite for Epic 005.

---

# Epic 005 — Booking Tracking & Status Management: Estimate

## User Stories

| Key    | Story                        | Points |
|--------|------------------------------|--------|
| DAN-45 | View Booking Details         | 5      |
| DAN-46 | Track Booking Status Changes | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **10**            |
| **T-Shirt Size**         | **S**             |
| **Estimated Duration**   | 0.5–1 week        |

## Notes

- Depends on Epic 004 (booking creation) and Epic 006 (admin status update capability).
- Status change tracking (DAN-46) requires the event-log infrastructure delivered in Epic 007.

---

# Epic 006 — Admin Booking Management Portal: Estimate

## User Stories

| Key    | Story                          | Points |
|--------|--------------------------------|--------|
| DAN-47 | View Booking History           | 5      |
| DAN-48 | View Admin Booking Dashboard   | 3      |
| DAN-49 | Review Incoming Booking Queue  | 5      |
| DAN-50 | Update Booking Status          | 3      |
| DAN-51 | Add Admin Notes to Bookings    | 5      |
| DAN-52 | Search & Filter Bookings       | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **26**            |
| **T-Shirt Size**         | **XL**            |
| **Estimated Duration**   | 1.5–2.5 weeks     |

## Notes

- Depends on Epic 003 (RBAC) and Epic 004 (booking data model).
- Dashboard (DAN-48) and status update (DAN-50) are lower-complexity items and can be de-risked early.
- Search & filter (DAN-52) and admin notes (DAN-51) are independent and can be deferred to a later increment if needed.

---

# Epic 007 — Messaging & Notifications: Estimate

## User Stories

| Key    | Story                                              | Points |
|--------|----------------------------------------------------|--------|
| DAN-53 | Send Agent Enquiry on Booking                      | 5      |
| DAN-54 | Receive Booking Confirmation Notification          | 3      |
| DAN-55 | Receive Booking Status Change Notification         | 3      |
| DAN-56 | Trigger Email Notifications for Booking Events     | 3      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **14**            |
| **T-Shirt Size**         | **M**             |
| **Estimated Duration**   | 1–1.5 weeks       |

## Notes

- In-app messaging (DAN-53) depends on Epic 004 (booking model).
- Email notifications (DAN-56) require an email delivery provider to be configured as part of Epic 002 infrastructure.
- Confirmation (DAN-54) and status-change (DAN-55) alerts depend on Epic 006 status-update capability.

---

# Epic 008 — Services, Packages & Pricing Rules: Estimate

## User Stories

| Key    | Story                          | Points |
|--------|--------------------------------|--------|
| DAN-57 | Manage Service Catalogue       | 5      |
| DAN-58 | Configure Service Packages     | 5      |
| DAN-59 | Apply Special Agent Pricing    | 5      |
| DAN-60 | Display Pricing Breakdown      | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **20**            |
| **T-Shirt Size**         | **L**             |
| **Estimated Duration**   | 1–2 weeks         |

## Notes

- This epic is a dependency of Epic 004 (Choose Service Package — DAN-42) and should be delivered in parallel.
- Special pricing rules (DAN-59) carry the most backend complexity; pricing model design should be confirmed during Epic 001 discovery.

---

# Epic 009 — Floor Plan Upload & Barfoot Integration: Estimate

## User Stories

| Key    | Story                                         | Points |
|--------|-----------------------------------------------|--------|
| DAN-61 | Upload Floor Plan File                        | 3      |
| DAN-62 | Enter Barfoot Listing ID                      | 3      |
| DAN-63 | Push Floor Plan to Barfoot Listing            | 5      |
| DAN-64 | Replace Existing Floor Plan                   | 3      |
| DAN-65 | Validate Barfoot Integration Access & Testing | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **19**            |
| **T-Shirt Size**         | **L**             |
| **Estimated Duration**   | 1–2 weeks         |

## Notes

- ⚠️ Barfoot API credentials must be obtained before DAN-63 and DAN-65 can commence — this is the primary schedule risk for this epic.
- File upload (DAN-61) and listing ID entry (DAN-62) can proceed independently of API access.
- Integration validation (DAN-65) must pass before this epic can be considered complete.

---

# Epic 010 — Ubookr Third-Party Integration: Estimate

> ⚠️ **Optional Phase** — delivery subject to feasibility outcome of DAN-66.

## User Stories

| Key    | Story                                    | Points |
|--------|------------------------------------------|--------|
| DAN-66 | Discover Ubookr Integration Requirements | 8      |
| DAN-67 | Sync Booking Data with Ubookr            | 5      |
| DAN-68 | Handle Ubookr Integration Errors         | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **18**            |
| **T-Shirt Size**         | **L**             |
| **Estimated Duration**   | 1.5–2.5 weeks     |

## Notes

- DAN-66 (discovery) must be completed first to confirm feasibility before any implementation work begins.
- If Ubookr API access is unavailable, this epic will be deferred to a post-launch phase.
- Error handling (DAN-68) carries higher complexity than its sibling stories due to retry and logging requirements.

---

# Epic 011 — QA, UAT & Release Readiness: Estimate

## User Stories

| Key    | Story                                  | Points |
|--------|----------------------------------------|--------|
| DAN-69 | Execute Functional & Device Testing    | 5      |
| DAN-70 | Remediate QA & UAT Defects             | 5      |
| DAN-71 | Run User Acceptance Testing            | 5      |
| DAN-72 | Complete Release Readiness Checklist   | 5      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **20**            |
| **T-Shirt Size**         | **L**             |
| **Estimated Duration**   | 1–2 weeks         |

## Notes

- All feature epics (003–009) must be complete before functional testing (DAN-69) can begin.
- Defect remediation (DAN-70) is variable; duration depends on defect load identified during testing.
- UAT (DAN-71) requires stakeholder availability and agreed test scenarios prepared in advance.
- Release readiness checklist (DAN-72) is a hard gate for Epic 012.

---

# Epic 012 — Launch & Deployment: Estimate

## User Stories

| Key    | Story                          | Points |
|--------|--------------------------------|--------|
| DAN-73 | Deploy to Production           | 5      |
| DAN-74 | Execute Go-Live Support        | 5      |
| DAN-75 | Prepare Handover Documentation | 3      |

## Summary

| Metric                   | Value             |
|--------------------------|-------------------|
| **Total Points**         | **13**            |
| **T-Shirt Size**         | **M**             |
| **Estimated Duration**   | 1–1.5 weeks       |

## Notes

- Depends on Epic 011 release readiness checklist (DAN-72) being fully signed off.
- Go-live support (DAN-74) is calendar time, not development effort — schedule accordingly.
- Handover documentation (DAN-75) can be prepared in parallel with go-live stabilisation.
