# Epic 006 — Admin Booking Management Portal: Estimate

> Estimates apply a **3x AI productivity multiplier** (1 point ≈ 0.33 days).

## User Stories

| Key    | Story                          | Points | Days |
|--------|--------------------------------|--------|------|
| DAN-47 | View Booking History           | 5      | 2d   |
| DAN-48 | View Admin Booking Dashboard   | 3      | 1d   |
| DAN-49 | Review Incoming Booking Queue  | 5      | 2d   |
| DAN-50 | Update Booking Status          | 3      | 1d   |
| DAN-51 | Add Admin Notes to Bookings    | 5      | 2d   |
| DAN-52 | Search & Filter Bookings       | 5      | 2d   |

## Summary

| Metric           | Value             |
|------------------|-------------------|
| **Total Points** | **26**            |
| **T-Shirt Size** | **XL**            |
| **Duration**     | ~2 weeks          |

## Notes

- Depends on Epic 003 (RBAC) and Epic 004 (booking data model).
- Admin portal is CRUD-heavy — AI excels at generating these patterns rapidly from conventions.
- Dashboard (DAN-48) and status update (DAN-50) are lower effort; can be de-risked early.
- Search & filter (DAN-52) and admin notes (DAN-51) add significant value for ops but are independent and can be deferred if needed.
