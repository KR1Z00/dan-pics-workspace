# Epic 004 — Booking Request Creation: Estimate

> Estimates apply a **3x AI productivity multiplier** (1 point ≈ 0.33 days).

## User Stories

| Key    | Story                                           | Points | Days |
|--------|-------------------------------------------------|--------|------|
| DAN-39 | Create Booking Request                          | 5      | 2d   |
| DAN-40 | Enter Property Details                          | 5      | 2d   |
| DAN-41 | Select Preferred Date & Time                    | 5      | 2d   |
| DAN-42 | Choose Service Package                          | 5      | 2d   |
| DAN-43 | Add Notes & Receive Submission Confirmation     | 5      | 2d   |
| DAN-44 | View Booking List                               | 5      | 2d   |

## Summary

| Metric           | Value             |
|------------------|-------------------|
| **Total Points** | **30**            |
| **T-Shirt Size** | **XL**            |
| **Duration**     | ~2.5 weeks        |

## Notes

- Remains the largest single epic; multi-step form UI and data model are the core complexity.
- Depends on Epic 003 (auth) and Epic 008 (service packages) for full integration.
- With AI, form scaffolding, validation, and API wiring can be generated quickly; effort is in polish and edge cases.
- DAN-44 (View Booking List) was assigned to this epic; it is also a prerequisite for Epic 005.
