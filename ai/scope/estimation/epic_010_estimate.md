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
| **T-Shirt Size**         | **M**             |
| **Estimated Duration**   | 1.5–2.5 weeks     |

## Notes

- DAN-66 (discovery) must be completed first to confirm feasibility before any implementation work begins.
- If Ubookr API access is unavailable, this epic will be deferred to a post-launch phase.
- Error handling (DAN-68) carries higher complexity than its sibling stories due to retry and logging requirements.
