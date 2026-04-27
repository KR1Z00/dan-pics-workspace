# Flutter State And UI Conventions

## When To Use BLoC Vs Cubit

Use Cubit when the state is simple and the transition model is direct.

Good Cubit cases:

- step indexes
- selected filters
- simple toggles
- lightweight local form progression

Use BLoC when the feature has:

- multiple user intents
- asynchronous side effects
- loading, success, and error branches
- retry logic
- analytics or navigation side effects coordinated with business actions

If state transitions are starting to look like a workflow, use BLoC.

## BLoC Structure

The default structure should remain:

- `feature_bloc.dart`
- `feature_event.dart`
- `feature_state.dart`

Conventions:

- keep event and state files as `part of` the bloc file when following the current repo pattern
- keep events named after user intent or system triggers, not implementation details
- keep states explicit enough to describe rendering clearly
- prefer immutable state updates
- keep side effects inside the bloc, not in widget callbacks beyond dispatching events

Avoid:

- putting validation, DTO parsing, or API details directly in widgets
- using a bloc as a general service locator
- deeply nested state types that make rendering hard to reason about

## Freezed And Immutable Models

Use Freezed for:

- DTOs
- domain models
- complex bloc state
- sealed unions where mutually exclusive states are valuable

Use simpler Equatable classes only when that keeps a very small feature easier to read.

Conventions:

- include `part '*.freezed.dart'` and `part '*.g.dart'` when JSON serialization is needed
- keep constructor defaults and serialization rules close to the model
- avoid mutable setters and manual copy logic
- prefer one model per file unless two types are inseparable and tiny

## Widgets And Screens

Screens should orchestrate sections and providers, not become giant rendering files.

Preferred breakdown:

- page widget for route-facing composition
- body widget or section widgets for major layout regions
- smaller leaf widgets for reusable controls and content blocks

Conventions:

- extract widgets when a screen starts mixing multiple responsibilities
- keep widgets focused on rendering and delegating callbacks
- use meaningful widget names based on user-facing function, not layout trivia
- give important interactive elements stable keys using the existing `Key("{pageName}_{elementName}")` pattern

## Forms

Forms should be progressive and explicit.

Conventions:

- keep field validation close to the form workflow, usually in bloc or form-specific helper logic
- avoid giant single-screen forms when a stepped flow reduces cognitive load
- treat error text, loading state, and submission outcomes as first-class UI states
- only keep raw controllers in the widget layer; keep canonical validated values in bloc or Cubit state

## Theming

Theming should flow through shared theme extensions in core.

Conventions:

- access theme primitives through context helpers such as `context.colorScheme()` and `context.textTheme()` where available
- centralize component styling tokens rather than redefining button, spacing, or card rules per feature
- add new colors, typography, or component styles through theme extensions before introducing ad hoc constants
- keep brand-specific theming explicit when it differs across apps

## Localization

Localization is not optional.

Conventions:

- all user-facing copy should come from generated locale keys
- keep translation keys grouped by feature or domain area
- use clear semantic key names instead of layout-oriented names
- regenerate localization files as part of feature completion, not as a later cleanup step

Avoid hardcoded strings in widgets except for temporary debugging.

## Responsiveness

Every feature should assume it may render on different screen sizes.

Conventions:

- prefer adaptive layout primitives over one-off size checks scattered across the tree
- keep width assumptions explicit and localized
- test narrow widths early for onboarding and web surfaces
- do not rely on overflow-prone row layouts for critical flows

## Side Effects

Navigation, snack bars, dialogs, analytics, and one-off effects should be coordinated carefully.

Conventions:

- emit state that the UI can react to, or use established side-effect channels already present in the repo
- keep analytics names and dispatch points centralized per feature
- do not mix business decisions with presentation-only side effects in the same helper unless the boundary is obvious