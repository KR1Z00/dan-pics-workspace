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
