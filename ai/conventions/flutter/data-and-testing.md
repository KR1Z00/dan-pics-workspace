# Flutter Data, Codegen, And Testing Conventions

## Repository And Service Boundaries

Repositories should own data access.

They should:

- call API clients or lower-level persistence adapters
- map remote payloads into DTOs and domain models
- hide endpoint construction and transport mechanics from upper layers
- expose intention-revealing methods aligned to domain language

Services should own business orchestration.

They should:

- combine repository calls
- enforce workflow rules
- transform repository outputs into values the UI can consume more easily
- remain testable without widget context

If a repository starts coordinating multiple business steps, move that logic into a service.

## Endpoint Construction

The current `_Endpoints` pattern is a good default and should stay consistent.

Conventions:

- keep endpoint path builders private to the repository implementation
- keep path strings centralized rather than scattered across methods
- avoid concatenating raw URLs inline in service code or blocs

## API Client Usage

Networking should enter through shared core infrastructure.

Conventions:

- standardize headers, auth, and error translation in the API client layer where possible
- keep repositories small by extracting repeated transport behavior into shared helpers when repetition appears
- do not let feature or app code depend directly on Dio or transport details unless the package intentionally owns infrastructure

## Code Generation Workflow

Generated code is part of the architecture, not an implementation accident.

Conventions:

- use Freezed and json_serializable for immutable models and DTOs
- rerun build generation after editing model shape or annotations
- commit generated artifacts when the repo already treats them as checked-in source
- prefer deleting conflicting generated output through the existing Melos command instead of manual cleanup

Default commands:

```bash
melos bootstrap
melos run build-runner
melos run generate-strings
```

## Testing Strategy

Testing should match the architecture.

### Unit Tests

Default targets:

- domain services
- repository implementations
- bloc and Cubit behavior
- pure helpers and mappers

Conventions:

- mirror the source path structure under `test/`
- mock external collaborators, not the unit under test
- verify business outcomes, state transitions, and key interactions
- prefer small, deterministic tests over broad integration-style unit tests

### Widget Tests

Use widget tests for:

- screen rendering under meaningful states
- form interactions
- navigation-triggering callbacks
- critical responsive layout behavior

Conventions:

- wrap widgets with the minimum app scaffolding they need
- inject fake blocs or mock services deliberately
- test loading, success, empty, and error states where those states matter to the user

### Integration Tests

Use integration tests selectively for flows that cross app shell, routing, and multiple features.

Good candidates:

- onboarding funnel
- registration and auth flows
- workout execution or assignment flows
- billing entrypoints

## Test Doubles

The default stack should remain `mocktail` and `flutter_test` unless there is a clear reason to change.

Conventions:

- keep mocks close to the feature area that owns them or in shared test helpers when reused broadly
- stub only behavior relevant to the test
- avoid sprawling fixture builders that hide important scenario details

## Workflow And Review Expectations

Before merging substantial Flutter work, the expected minimum is:

1. `melos run check-deps`
2. `melos run lint`
3. `melos run test`
4. `melos run build-runner` if models or codegen were touched
5. `melos run generate-strings` if localization changed

Reviewers should push back on:

- app-level business logic that belongs in packages
- package boundary violations
- unlocalized copy
- mutable domain models
- widget files that are acting as services