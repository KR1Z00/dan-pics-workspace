# Epic 003 — Authentication & Agent Account Management: Estimate

> Estimates apply a **3x AI productivity multiplier** (1 point ≈ 0.33 days).

## User Stories

| Key    | Story                              | Points | Days |
|--------|------------------------------------|--------|------|
| DAN-35 | Agent Login                        | 2      | 1d   |
| DAN-36 | Password Reset                     | 2      | 1d   |
| DAN-37 | Manage Agent Profile               | 3      | 1d   |
| DAN-38 | Enforce Role-Based Access Control  | 3      | 1d   |

## Summary

| Metric           | Value             |
|------------------|-------------------|
| **Total Points** | **10**            |
| **T-Shirt Size** | **S**             |
| **Duration**     | ~1 week           |

## Notes

- Depends on Epic 002 (auth schema foundations DAN-32 must be complete).
- Auth flows are highly standardised — AI can generate login, reset, and profile scaffolding rapidly.
- RBAC (DAN-38) is a cross-cutting concern that should be validated early to unblock admin portal work.
