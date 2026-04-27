# Flutter Conventions

These conventions define how PeakReps Flutter code should be structured in the ideal case for this monorepo. They are intentionally opinionated, but they stay close to the patterns already used across `peakreps-flutter`.

Use these docs as the default when creating new features, refactoring existing code, or reviewing pull requests.

## Scope

This stack covers:

- `apps/*` application shells
- `packages/peakreps-core`
- `packages/peakreps_domain`
- `packages/peakreps_feature`
- shared tooling through Melos, build_runner, localization generation, and tests

## Golden Rules

1. Keep the dependency direction strict: app -> feature -> domain -> core.
2. Put business logic in domain services and repositories, not in widgets.
3. Keep apps thin. Apps compose assemblies, routes, and platform configuration; they should not become feature containers.
4. Use BLoC or Cubit to coordinate UI state, but do not let blocs become transport or persistence layers.
5. Prefer generated immutable models over handwritten mutable classes.
6. Register dependencies centrally through assemblies and resolve them consistently through `locator`.
7. Treat localization, theming, and responsiveness as default requirements, not cleanup work.
8. Mirror production structure in tests so behavior is easy to trace and maintain.

## Document Map

- `architecture.md` explains layering, package boundaries, dependency injection, and routing.
- `state-and-ui.md` explains BLoC, Cubit, view composition, theming, localization, and widget conventions.
- `data-and-testing.md` explains models, repositories, services, code generation, testing, and workflow commands.

## Practical Reading Order

1. Start with `architecture.md` before adding a new feature.
2. Use `state-and-ui.md` when building screens or editing presentation state.
3. Use `data-and-testing.md` when touching API contracts, models, repositories, services, or tests.