# Flutter Architecture Conventions

## Layer Model

PeakReps Flutter should continue to follow a strict layered monorepo.

### Core

`peakreps-core` is the lowest-level package.

It should contain:

- app-wide primitives such as dependency injection helpers and service registration abstractions
- infrastructure clients such as API client setup, caching adapters, and platform helpers
- design system primitives, theme extensions, shared UI helpers, and navigation helpers that are not feature-specific
- cross-cutting utilities that have no business-domain knowledge

It should not contain:

- user, program, billing, onboarding, or auth business rules
- feature-specific widgets or flows
- imports from `peakreps_domain`, `peakreps_feature`, or any app

### Domain

`peakreps_domain` is where business behavior should live.

It should contain:

- immutable models and DTOs
- repository interfaces and implementations
- domain services and orchestration logic
- use cases when a workflow spans multiple repositories or services
- feature-neutral validation and transformation rules

It should not contain:

- Flutter page layout or widget composition
- route-building concerns
- app-shell concerns such as nav chrome or persona-specific screen composition

### Feature

`peakreps_feature` is the presentation layer.

It should contain:

- BLoCs and Cubits
- views, screens, bottom sheets, form widgets, and feature-specific presentation helpers
- route-facing composition code that wires domain services into UI state
- localized copy keys and presentation state models

It should not contain:

- direct networking code
- persistence logic
- app-level bootstrap concerns

### Apps

`apps/*` should stay intentionally thin.

Apps should contain:

- entrypoints and environment bootstrapping
- platform configuration
- app-specific route trees or shell routes
- assembly composition for the persona or surface
- very shallow app-only wrappers when behavior genuinely differs between trainer, athlete, and onboarding apps

Apps should not become a second feature layer. If logic could be shared, move it down into a package.

## Dependency Direction

The allowed direction is:

```text
apps/* -> peakreps_feature, peakreps_domain, peakreps_core
peakreps_feature -> peakreps_domain, peakreps_core
peakreps_domain -> peakreps_core
peakreps_core -> no project-specific packages
```

Conventions for enforcing that direction:

- declare every package dependency explicitly in `pubspec.yaml`
- use `package:` imports instead of relative imports across package boundaries
- never import another package's `src/` internals
- run `melos run check-deps` before merging architectural work

```yaml
# packages/peakreps_feature/pubspec.yaml
dependencies:
  peakreps_domain:
    path: ../peakreps_domain
  peakreps_core:
    path: ../peakreps-core
  flutter_bloc: ^8.1.5
  go_router: ^14.0.0
  # ❌ do NOT add peakreps_feature as a dep from peakreps_domain
```

```dart
// ✅ correct: package: import across boundary
import 'package:peakreps_domain/auth/model/user_profile_model.dart';

// ❌ wrong: relative import that pierces the boundary
import '../../peakreps_domain/lib/auth/model/user_profile_model.dart';

// ❌ wrong: importing src/ internals
import 'package:peakreps_domain/src/auth/internal_helper.dart';
```

## Package Structure

Each package should expose a clear public API.

Preferred structure:

```text
lib/
  <feature or domain area>/
    model/
    repository/
    service/
    bloc/ or cubit/
    view/
    widget/
  app/
  localization/
  <package>.dart or stable barrel exports
```

Conventions:

- organize by domain area first, then by artifact type when needed
- expose stable entrypoints through barrel files deliberately, not by exporting everything automatically
- keep generated files adjacent to the source that owns them
- keep tests mirroring this same structure under `test/`

## Dependency Injection

GetIt with `DependencyAssembly` should remain the default composition model.

Conventions:

- every package should register its own dependencies through assembly classes
- assemblies should be small and grouped by coherent concern, such as auth, profile, library, or onboarding
- apps should compose ordered assembly lists instead of registering dependencies ad hoc in main files
- repositories should usually be registered behind abstract interfaces
- prefer `registerLazySingleton` for stateless shared services and repositories
- prefer `registerFactory` for BLoCs, Cubits, and objects with ephemeral UI lifecycle
- parameterized dependencies should use locator parameters rather than global mutable state

Do not:

- call `register*` from random widgets or feature files
- resolve dependencies at import time
- bypass abstractions by depending directly on a concrete implementation in higher layers

```dart
// packages/peakreps_domain/lib/auth/auth_assembly.dart
class AuthAssembly implements DependencyAssembly {
  @override
  void register(GetIt locator) {
    locator.registerLazySingleton<AuthRepository>(
      () => AuthRepositoryImpl(locator<ApiClient>()),
    );
    locator.registerLazySingleton<EmailVerificationService>(
      () => EmailVerificationServiceImpl(locator<AuthRepository>()),
    );
  }
}

// apps/peakreps-trainer/lib/app/dependency_injection.dart
final assemblies = [
  ...coreAssemblies(),
  ...domainAssemblies(UserPersona.trainer),
  ...featureAssemblies(),
];

void setupDependencies() {
  for (final assembly in assemblies) {
    assembly.register(locator);
  }
}
```

```dart
// resolving — always use the abstract type
final service = locator<EmailVerificationService>();

// blocs with parameters
final bloc = locator<LoginBloc>(param1: UserPersona.trainer);
```

## Routing

GoRouter should be the standard navigation solution.

Conventions:

- define routes as typed, discoverable structures, typically enum-backed or protocol-backed
- keep route constants and path derivation centralized
- resolve required blocs or cubits at the route boundary using `BlocProvider.value` or a small route wrapper
- navigation helpers should hide raw string paths from most feature code
- route builders should compose screens, not contain business logic

Preferred flow:

1. route receives params
2. route resolves dependencies from `locator`
3. route wraps page with providers
4. page triggers events and renders state

```dart
// apps/peakreps-trainer/lib/app/navigation/router_config.dart
enum AppRoute implements RouteProtocol {
  clients(
    routePath: '/clients',
    routeName: 'clients',
  );

  const AppRoute({ required this.routePath, required this.routeName });

  @override final String routePath;
  @override final String routeName;
}

// route builder
GoRoute(
  path: AppRoute.clients.routePath,
  name: AppRoute.clients.routeName,
  builder: (context, state) => BlocProvider.value(
    value: locator<ClientListBloc>(),
    child: const ClientListPage(),
  ),
),
```

```dart
// navigation — use helper, not raw string paths
context.goRoute(AppRoute.clients);
context.pushRoute(AppRoute.clientDetail, extra: clientId);
```

## Feature Boundaries

A new feature should usually be split like this:

- core only if it adds a reusable primitive or infrastructure capability
- domain if it introduces business entities, API access, or cross-screen workflows
- feature if it introduces presentation state and widgets
- app only if it changes app-shell wiring or platform-specific bootstrapping

If a change starts in an app and later appears reusable, move it down one layer rather than copying it across apps.

## State Ownership

Ownership should be predictable:

- widgets own transient UI concerns such as text field focus, controllers, and local animation state
- Cubits own simple local presentation state
- BLoCs own event-driven presentation workflows
- domain services own business operations
- repositories own data access mechanics

If a widget needs to know HTTP details, the boundary is wrong.