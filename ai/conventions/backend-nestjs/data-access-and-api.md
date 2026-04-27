# Backend Data Access And API Conventions

## DTO Design

PeakReps currently favors interface-based DTOs, and that should remain the default unless there is a strong reason to adopt class-based DTOs for validation.

Conventions:

- separate create, edit, and read DTOs clearly
- keep DTO naming explicit, such as `CreateBusinessDto`, `EditBusinessDto`, `BusinessDto`
- avoid leaking database naming quirks into DTOs when a cleaner API name is available
- keep optionality honest; do not mark fields optional just to reduce short-term friction

Read DTOs should represent the contract clients are allowed to rely on, not a mirror of Prisma schema.

## Schema Conversion Pattern

The existing select-and-convert approach is worth standardizing.

Preferred pattern:

- define a static default Prisma select shape in a schema helper
- define a `SelectedX` type or interface that reflects that selected shape
- expose a mapper such as `convertXToDto()` that translates persistence data to the public DTO

Benefits:

- controllers and services do not handcraft large select objects repeatedly
- DTO mapping stays centralized
- null-to-undefined conversion and URL expansion happen in one place
- changes to response shape are easier to review

## Prisma Access

Prisma should remain behind service boundaries.

Conventions:

- prefer one service as the owner of a given aggregate's data-access rules
- keep reusable `where`, `select`, and mapping helpers close to that aggregate
- use Prisma input types explicitly when building dynamic filters
- do not duplicate complex query fragments across controllers and services

When a feature repeatedly needs a complex search or filter rule, extract a helper rather than rebuilding the object inline each time.

## Null, Undefined, And Storage Boundaries

Database shape and API shape are not the same thing.

Conventions:

- convert Prisma `null` values to `undefined` where the API contract expects missing optional fields
- keep internal storage keys, secrets, and implementation IDs out of public DTOs
- transform file references into presigned URLs or stable externally consumable representations at the API boundary

## Validation And Errors

Validation belongs at the correct layer.

Controller-level validation should handle:

- request shape
- parameter parsing
- basic transport concerns

Service-level validation should handle:

- resource existence
- ownership and business rules beyond simple auth decorators
- conflict detection
- workflow preconditions

Use NestJS exceptions intentionally:

- `BadRequestException` for invalid input or impossible combinations
- `UnauthorizedException` or auth guard behavior for unauthenticated requests
- `ForbiddenException` for denied access
- `NotFoundException` for missing resources
- `ConflictException` for uniqueness or state conflicts

Avoid generic `Error` unless it will be caught and translated immediately.

## Async Work

Background or fire-and-forget behavior should be explicit.

Conventions:

- only detach async work when the request should not wait for it
- always log failures from detached operations
- keep the detached side effect idempotent when practical
- graduate recurring background work into a job or queue abstraction when reliability requirements increase

## API Shape

API contracts should be stable and unsurprising.

Conventions:

- prefer resource-oriented routes
- keep controller method names aligned with route intent
- use consistent pagination, filtering, and sorting patterns across modules
- return structured responses rather than mixed ad hoc payload shapes for similar endpoints
- document edge cases when an endpoint intentionally deviates from normal resource semantics