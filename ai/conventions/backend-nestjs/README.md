# Backend NestJS Conventions

These conventions define how PeakReps backend code should be built in the ideal case while staying aligned with the current NestJS and Prisma architecture in `peakreps-backend`.

Use them as the default guide for new endpoints, refactors, data-access changes, auth rules, and test coverage.

## Scope

This stack covers:

- NestJS modules, controllers, services, and providers
- Prisma data access and schema conversion patterns
- Firebase-authenticated API endpoints and authorization rules
- storage, email, billing, onboarding, and domain modules
- testing, migrations, seeds, and operational workflow

## Golden Rules

1. Keep controllers thin and deterministic.
2. Put business logic in services, not controllers or decorators.
3. Use transactions deliberately and make transaction support explicit in service methods.
4. Keep Prisma model shape internal and map to API DTOs at the boundary.
5. Favor interface-based DTO contracts and clear read/write separation.
6. Every protected endpoint should express both authentication and authorization clearly.
7. Shared behavior belongs in reusable modules or helpers, not copy-pasted service code.
8. Tests should sit next to the code they prove.

## Document Map

- `architecture.md` covers modules, controllers, services, auth, and transaction boundaries.
- `data-access-and-api.md` covers DTOs, schema conversion, Prisma patterns, validation, and error handling.
- `testing-and-operations.md` covers tests, commands, migrations, seeds, and production-minded workflow.