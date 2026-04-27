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

```dart
// lib/auth/bloc/login_bloc.dart
part 'login_event.dart';
part 'login_state.dart';

class LoginBloc extends Bloc<LoginEvent, LoginState> {
  LoginBloc(this._authService) : super(const LoginState.initial()) {
    on<LoginSubmitted>(_onSubmitted);
  }

  final AuthService _authService;

  Future<void> _onSubmitted(LoginSubmitted event, Emitter<LoginState> emit) async {
    emit(const LoginState.loading());
    try {
      final user = await _authService.signIn(event.email, event.password);
      emit(LoginState.success(user));
    } on AuthException catch (e) {
      emit(LoginState.failure(e.message));
    }
  }
}
```

```dart
// lib/auth/bloc/login_event.dart
part of 'login_bloc.dart';

sealed class LoginEvent extends Equatable {
  const LoginEvent();
  @override List<Object?> get props => [];
}

final class LoginSubmitted extends LoginEvent {
  const LoginSubmitted({ required this.email, required this.password });
  final String email;
  final String password;
  @override List<Object?> get props => [email, password];
}
```

```dart
// lib/auth/bloc/login_state.dart
part of 'login_bloc.dart';

@freezed
sealed class LoginState with _$LoginState {
  const factory LoginState.initial()            = LoginInitial;
  const factory LoginState.loading()            = LoginLoading;
  const factory LoginState.success(UserModel u) = LoginSuccess;
  const factory LoginState.failure(String msg)  = LoginFailure;
}
```

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

```dart
// packages/peakreps_domain/lib/client/model/client_model.dart
import 'package:freezed_annotation/freezed_annotation.dart';

part 'client_model.freezed.dart';
part 'client_model.g.dart';

@freezed
class ClientModel with _$ClientModel {
  const factory ClientModel({
    required String id,
    required String firstName,
    required String lastName,
    required String email,
    String? avatarUrl,
  }) = _ClientModel;

  factory ClientModel.fromJson(Map<String, dynamic> json) =>
      _$ClientModelFromJson(json);
}
```

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

```dart
// page widget: thin orchestrator
class ClientListPage extends StatelessWidget {
  const ClientListPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(tr(LocaleKeys.clients_title))),
      body: BlocBuilder<ClientListBloc, ClientListState>(
        builder: (context, state) => switch (state) {
          ClientListLoading() => const _LoadingBody(),
          ClientListEmpty()   => const _EmptyBody(),
          ClientListLoaded(:final clients) => _ClientList(clients: clients),
          ClientListError(:final message) => _ErrorBody(message: message),
        },
      ),
      floatingActionButton: FloatingActionButton(
        key: const Key('clientList_addClient'),
        onPressed: () => context.pushRoute(AppRoute.inviteClient),
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

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