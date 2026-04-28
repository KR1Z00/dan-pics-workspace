# Epic 009 — Floor Plan Upload & Barfoot Integration: Estimate

> Estimates apply a **3x AI productivity multiplier** (1 point ≈ 0.33 days).

## User Stories

| Key    | Story                                         | Points | Days |
|--------|-----------------------------------------------|--------|------|
| DAN-61 | Upload Floor Plan File                        | 3      | 1d   |
| DAN-62 | Enter Barfoot Listing ID                      | 3      | 1d   |
| DAN-63 | Push Floor Plan to Barfoot Listing            | 5      | 2d   |
| DAN-64 | Replace Existing Floor Plan                   | 3      | 1d   |
| DAN-65 | Validate Barfoot Integration Access & Testing | 5      | 2d   |

## Summary

| Metric           | Value             |
|------------------|-------------------|
| **Total Points** | **19**            |
| **T-Shirt Size** | **L**             |
| **Duration**     | ~1.5 weeks        |

## Notes

- Barfoot API access must be negotiated and credentials obtained before DAN-63/DAN-65 can start.
- Integration validation (DAN-65) is a hard blocker; if API access is delayed, this epic's timeline is at risk.
- External API integration carries residual risk regardless of AI tooling — kept at full complexity points.
- File upload (DAN-61) and listing ID entry (DAN-62) can proceed independently of API access.
