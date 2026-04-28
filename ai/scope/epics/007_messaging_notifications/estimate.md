# Epic 007 — Messaging & Notifications: Estimate

> Estimates apply a **3x AI productivity multiplier** (1 point ≈ 0.33 days).

## User Stories

| Key    | Story                                              | Points | Days |
|--------|----------------------------------------------------|--------|------|
| DAN-53 | Send Agent Enquiry on Booking                      | 5      | 2d   |
| DAN-54 | Receive Booking Confirmation Notification          | 3      | 1d   |
| DAN-55 | Receive Booking Status Change Notification         | 3      | 1d   |
| DAN-56 | Trigger Email Notifications for Booking Events     | 3      | 1d   |

## Summary

| Metric           | Value             |
|------------------|-------------------|
| **Total Points** | **14**            |
| **T-Shirt Size** | **M**             |
| **Duration**     | ~1 week           |

## Notes

- In-app messaging (DAN-53) depends on Epic 004 (booking model).
- Email notifications (DAN-56) require an email delivery provider to be configured as part of Epic 002 infrastructure.
- Notification event wiring is repetitive pattern work — well-suited to AI generation.
- Confirmation (DAN-54) and status-change (DAN-55) alerts depend on Epic 006 status-update capability.
