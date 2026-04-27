# Backend Architecture Conventions

## Module Design

Feature modules should be the main unit of backend organization.

Each module should:

- group its controller, service, DTOs, tests, and schema helpers together
- import only what it needs
- export only providers that other modules genuinely consume
- avoid becoming a dumping ground for unrelated endpoints

Preferred module contents:

```text
src/<feature>/
  <feature>.module.ts
  <feature>.controller.ts
  <feature>.service.ts
  dto/
  schema/
  *.spec.ts
```

Cross-cutting concerns such as Prisma, auth, storage, and email should remain isolated in dedicated modules.

## AppModule Composition

`AppModule` should remain declarative.

Conventions:

- import global or foundational modules first
- keep feature module registration explicit
- avoid burying side-effectful setup in random imports
- if bootstrapping becomes environment-specific, isolate it behind dedicated modules or config providers rather than conditionals spread throughout the app

## Controllers

Controllers should translate HTTP into application calls.

They should:

- declare routes, params, query, and request body shape clearly
- enforce auth guard usage consistently
- apply `@UidRules(...)` or equivalent authorization rules for protected resources
- call one service method per action where practical
- return DTOs or primitive response objects that match the public API contract

They should not:

- build complex Prisma queries
- coordinate multi-step business workflows directly
- contain business validation better owned by services
- silently swallow exceptional cases

## Authentication And Authorization

Protected endpoints should make security obvious.

Conventions:

- use `@UseGuards(FirebaseAuthGuard)` by default on authenticated routes
- pair authentication with explicit authorization rules through `@UidRules(...)`
- prefer reusable rule functions such as signed-in, ownership, business-membership, or role-based checks over inline ad hoc conditions
- keep public endpoints intentionally rare and easy to spot

If a controller method needs nuanced access control, extract or compose a rule rather than embedding access logic in the controller body.

## Services

Services should own business behavior.

They should:

- receive dependencies through constructor injection using `private readonly`
- log at meaningful boundaries, not on every trivial branch
- throw NestJS exceptions when business rules are violated or required resources are missing
- support transaction-scoped execution when they mutate multiple records or are intended for orchestration by other services

Preferred transaction pattern:

- service methods accept `tx?: PrismaTx` when they may run inside a larger transaction
- use `const db = tx ?? this.prisma` or equivalent once near the top of the method
- keep transaction ownership at the orchestration boundary when multiple service calls must succeed or fail together

## Orchestration

When a workflow spans modules, create a service method that coordinates those dependencies rather than distributing orchestration across multiple controllers.

Examples of orchestration-worthy flows:

- onboarding sequences
- invitation acceptance
- billing and entitlement changes
- media upload plus metadata persistence
- user and business creation flows

## Boundary Discipline

A healthy request path should look like:

1. controller parses input and enforces access requirements
2. service performs business work
3. schema mapper converts persistence shape to DTO shape
4. response returns only public contract data

If a controller knows Prisma internals or a service returns raw storage keys to clients, the boundary is wrong.