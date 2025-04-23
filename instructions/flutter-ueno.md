# Effective Dart Rules

### Naming Conventions
1. Use terms consistently throughout your code.
2. Follow existing mnemonic conventions when naming type parameters (e.g., `E` for element, `K`/`V` for key/value, `T`/`S`/`U` for generic types).
3. Name types using `UpperCamelCase` (classes, enums, typedefs, type parameters).
4. Name extensions using `UpperCamelCase`.
5. Name packages, directories, and source files using `lowercase_with_underscores`.
6. Name import prefixes using `lowercase_with_underscores`.
7. Name other identifiers using `lowerCamelCase` (variables, parameters, named parameters).
8. Capitalize acronyms and abbreviations longer than two letters like words.
9. Avoid abbreviations unless the abbreviation is more common than the unabbreviated term.
10. Prefer putting the most descriptive noun last in names.
11. Consider making code read like a sentence when designing APIs.
12. Prefer a noun phrase for non-boolean properties or variables.
13. Prefer a non-imperative verb phrase for boolean properties or variables.
14. Prefer the positive form for boolean property and variable names.
15. Consider omitting the verb for named boolean parameters.
16. Use camelCase for variable and function names.
17. Use PascalCase for class names.
18. Use snake_case for file names.

### Types and Functions
1. Use class modifiers to control if your class can be extended or used as an interface.
2. Type annotate variables without initializers.
3. Type annotate fields and top-level variables if the type isn't obvious.
4. Annotate return types on function declarations.
5. Annotate parameter types on function declarations.
6. Write type arguments on generic invocations that aren't inferred.
7. Annotate with `dynamic` instead of letting inference fail.
8. Use `Future<void>` as the return type of asynchronous members that do not produce values.
9. Use getters for operations that conceptually access properties.
10. Use setters for operations that conceptually change properties.
11. Use a function declaration to bind a function to a name.
12. Use inclusive start and exclusive end parameters to accept a range.

### Style
1. Format your code using `dart format`.
2. Use curly braces for all flow control statements.
3. Prefer `final` over `var` when variable values won't change.
4. Use `const` for compile-time constants.

### Imports & Files
1. Don't import libraries inside the `src` directory of another package.
2. Don't allow import paths to reach into or out of `lib`.
3. Prefer relative import paths within a package.
4. Don't use `/lib/` or `../` in import paths.
5. Consider writing a library-level doc comment for library files.

### Structure
1. Keep files focused on a single responsibility.
2. Limit file length to maintain readability.
3. Group related functionality together.
4. Prefer making fields and top-level variables `final`.
5. Consider making your constructor `const` if the class supports it.
6. Prefer making declarations private.

### Usage
1. Use strings in `part of` directives.
2. Use adjacent strings to concatenate string literals.
3. Use collection literals when possible.
4. Use `whereType()` to filter a collection by type.
5. Test for `Future<T>` when disambiguating a `FutureOr<T>` whose type argument could be `Object`.
6. Follow a consistent rule for `var` and `final` on local variables.
7. Initialize fields at their declaration when possible.
8. Use initializing formals when possible.
9. Use `;` instead of `{}` for empty constructor bodies.
10. Use `rethrow` to rethrow a caught exception.
11. Override `hashCode` if you override `==`.
12. Make your `==` operator obey the mathematical rules of equality.

### Documentation
1. Format comments like sentences.
2. Use `///` doc comments to document members and types; don't use block comments for documentation.
3. Prefer writing doc comments for public APIs.
4. Consider writing doc comments for private APIs.
5. Consider including explanations of terminology, links, and references in library-level docs.
6. Start doc comments with a single-sentence summary.
7. Separate the first sentence of a doc comment into its own paragraph.
8. Use square brackets in doc comments to refer to in-scope identifiers.
9. Use prose to explain parameters, return values, and exceptions.
10. Put doc comments before metadata annotations.
11. Document why code exists or how it should be used, not just what it does.

### Testing
1. Write unit tests for business logic.
2. Write widget tests for UI components.
3. Aim for good test coverage.

### Widgets
1. Extract reusable widgets into separate components.
2. Use `StatelessWidget` when possible.
3. Keep build methods simple and focused.

### State Management
1. Choose appropriate state management based on complexity.
2. Avoid unnecessary `StatefulWidget`s.
3. Keep state as local as possible.

### Performance
1. Use `const` constructors when possible.
2. Avoid expensive operations in build methods.
3. Implement pagination for large lists.

## Architecture
1. Separate your features into a UI Layer (presentation), a Data Layer (business data and logic), and, for complex apps, consider adding a Domain (Logic) Layer between UI and Data layers to encapsulate business logic and use-cases.
2. You can organize code by feature: The classes needed for each feature are grouped together.
    The folder structure should reflect this separation. For example:
    ```
    features/
    ├── auth/
    │   ├── data/
    │   │   ├── datasources/
    │   │   │   ├── auth_local_data_source_impl.dart
    │   │   │   ├── auth_remote_data_source_impl.dart
    │   │   │   └── datasources.dart
    │   │   ├── repositories/
    │   │   │   ├── auth_repository_impl.dart
    │   │   │   └── repositories.dart
    │   ├── domain/
    │   │   ├── datasources/
    │   │   │   ├── auth_data_source.dart
    │   │   │   └── datasources.dart
    │   │   ├── models/
    │   │   │   ├── user_model.dart
    │   │   │   └── models.dart
    │   ├── presentation/
    │   │   ├── providers/
    │   │   │   ├── user_provider.dart
    │   │   │   └── providers.dart
    │   │   ├── screens/
    │   │   │   ├── login_screen.dart
    │   │   │   └── screens.dart
    │   │   ├── widgets/
    │   │   │   ├── login_form.dart
    │   │   │   └── widgets.dart
    │   └── auth.dart
    ```
3. Only allow communication between adjacent layers; the UI layer should not access the data layer directly, and vice versa.
4. Clearly define the responsibilities, boundaries, and interfaces of each layer and component (Screens, Providers, Repositories, Datasources).
5. Further divide each layer into components with specific responsibilities and well-defined interfaces.
6. In the UI Layer, use Screens to describe how to present data to the user; keep logic minimal and only UI-related.
7. Pass events from Screens to Providers in response to user interactions.
8. In Providers, contain logic to convert app data into UI state and maintain the current state needed by the view.
9. Expose callbacks (commands) from Providers to Screens and retrieve/transform data from repositories.
10. In the Data Layer, use Repositories as the single source of truth (SSOT) for model data and to handle business logic such as caching, error handling, and refreshing data.
11. Only the SSOT class (usually the repository) should be able to mutate its data; all other classes should read from it.
12. Repositories should transform raw data from Datasources into domain models and output data consumed by Providers.
13. Use Datasources to wrap API endpoints and expose asynchronous response objects; Datasources should isolate data-loading and hold no state.
14. Use dependency injection to provide components with their dependencies, enabling testability and flexibility.

### Data Flow and State
1. Follow unidirectional data flow: state flows from the data layer through the logic layer to the UI layer, and events from user interaction flow in the opposite direction.
2. Data changes should always happen in the SSOT (data layer), not in the UI or logic layers.
3. The UI should always reflect the current (immutable) state; trigger UI rebuilds only in response to state changes.
4. Screens should contain as little logic as possible and be driven by state from Providers.

### Best Practices
1. Strongly recommend following separation of concerns and layered architecture.
2. Strongly recommend using dependency injection for testability and flexibility.
3. Recommend using MVVM as the default pattern, but adapt as needed for your app's complexity.
4. Use key-value storage for simple data (e.g., configuration, preferences) and SQL storage for complex relationships.
5. Use optimistic updates to improve perceived responsiveness by updating the UI before operations complete.
6. Support offline-first strategies by combining local and remote data sources in repositories and enabling synchronization as appropriate.
7. Keep views focused on presentation and extract reusable widgets into separate components.
8. Use `StatelessWidget` when possible and avoid unnecessary `StatefulWidget`s.
9. Keep build methods simple and focused on rendering.
10. Choose state management approaches appropriate to the complexity of your app.
11. Keep state as local as possible to minimize rebuilds and complexity.
12. Use `const` constructors when possible to improve performance.
13. Avoid expensive operations in build methods and implement pagination for large lists.
14. Keep files focused on a single responsibility and limit file length for readability.
15. Group related functionality together and use `final` for fields and top-level variables when possible.
16. Prefer making declarations private and consider making constructors `const` if the class supports it.
17. Follow Dart naming conventions and format code using `dart format`.
18. Use curly braces for all flow control statements to ensure clarity and prevent bugs.

# Dart 3 Updates

### Branches
1. Use `if` statements for conditional branching. The condition must evaluate to a boolean.
2. `if` statements support optional `else` and `else if` clauses for multiple branches.
3. Use `if-case` statements to match and destructure a value against a single pattern. Example: `if (pair case [int x, int y]) { ... }`
4. If the pattern in an `if-case` matches, variables defined in the pattern are in scope for that branch.
5. If the pattern does not match in an `if-case`, control flows to the `else` branch if present.
6. Use `switch` statements to match a value against multiple patterns (cases). Each `case` can use any kind of pattern.
7. When a value matches a `case` pattern in a `switch` statement, the case body executes and control jumps to the end of the switch. `break` is not required.
8. You can end a non-empty `case` clause with `continue`, `throw`, or `return`.
9. Use `default` or `_` in a `switch` statement to handle unmatched values.
10. Empty `case` clauses fall through to the next case. Use `break` to prevent fallthrough.
11. Use `continue` with a label for non-sequential fallthrough between cases.
12. Use logical-or patterns (e.g., `case a || b`) to share a body or guard between cases.
13. Use `switch` expressions to produce a value based on matching cases. Syntax differs from statements: omit `case`, use `=>` for bodies, and separate cases with commas.
14. In `switch` expressions, the default case must use `_` (not `default`).
15. Dart checks for exhaustiveness in `switch` statements and expressions, reporting a compile-time error if not all possible values are handled.
16. To ensure exhaustiveness, use a default (`default` or `_`) case, or switch over enums or sealed types.
17. Use the `sealed` modifier on a class to enable exhaustiveness checking when switching over its subtypes.
18. Add a guard clause to a `case` using `when` to further constrain when a case matches. Example: `case pattern when condition:`
19. Guard clauses can be used in `if-case`, `switch` statements, and `switch` expressions. The guard is evaluated after pattern matching.
20. If a guard clause evaluates to false, execution proceeds to the next case (does not exit the switch).

### Patterns
1. Patterns are a syntactic category that represent the shape of values for matching and destructuring.
2. Pattern matching checks if a value has a certain shape, constant, equality, or type.
3. Pattern destructuring allows extracting parts of a matched value and binding them to variables.
4. Patterns can be nested, using subpatterns (outer/inner patterns) for recursive matching and destructuring.
5. Use wildcard patterns (`_`) to ignore parts of a matched value; use rest elements in list patterns to ignore remaining elements.
6. Patterns can be used in:
   - Local variable declarations and assignments
   - For and for-in loops
   - If-case and switch-case statements
   - Control flow in collection literals
7. Pattern variable declarations start with `var` or `final` and bind new variables from the matched value. Example: `var (a, [b, c]) = ('str', [1, 2]);`
8. Pattern variable assignments destructure a value and assign to existing variables. Example: `(b, a) = (a, b); // swap values`
9. Every case clause in `switch` and `if-case` contains a pattern. Any kind of pattern can be used in a case.
10. Case patterns are refutable; if the pattern doesn't match, execution continues to the next case.
11. Destructured values in a case become local variables scoped to the case body.
12. Use logical-or patterns (e.g., `case a || b`) to match multiple alternatives in a single case.
13. Use logical-or patterns with guards (`when`) to share a body or guard between cases.
14. Guard clauses (`when`) evaluate a condition after matching; if false, execution proceeds to the next case.
15. Patterns can be used in for and for-in loops to destructure collection elements (e.g., destructuring `MapEntry` in map iteration).
16. Object patterns match named object types and destructure their data using getters. Example: `var Foo(:one, :two) = myFoo;`
17. Use patterns to destructure records, including positional and named fields, directly into local variables.
18. Patterns enable algebraic data type style code: use sealed classes and switch on subtypes for exhaustive matching.
19. Patterns simplify validation and destructuring of complex data structures, such as JSON, in a declarative way. Example: `if (data case {'user': [String name, int age]}) { ... }`
20. Patterns provide a concise alternative to verbose type-checking and destructuring code.

### Pattern Types
1. Pattern precedence determines evaluation order; use parentheses to group lower-precedence patterns.
2. Logical-or patterns (`pattern1 || pattern2`) match if any branch matches, evaluated left-to-right. All branches must bind the same set of variables.
3. Logical-and patterns (`pattern1 && pattern2`) match if both subpatterns match. Bound variable names must not overlap between subpatterns.
4. Relational patterns (`==`, `!=`, `<`, `>`, `<=`, `>=`) match if the value compares as specified to a constant. Useful for numeric ranges and can be combined with logical-and.
5. Cast patterns (`subpattern as Type`) assert and cast a value to a type before passing it to a subpattern. Throws if the value is not of the type.
6. Null-check patterns (`subpattern?`) match if the value is not null, then match the inner pattern. Binds the non-nullable type. Use constant pattern `null` to match null.
7. Null-assert patterns (`subpattern!`) match if the value is not null, else throw. Use in variable declarations to eliminate nulls. Use constant pattern `null` to match null.
8. Constant patterns match if the value is equal to a constant (number, string, bool, named constant, const constructor, const collection, etc.). Use parentheses and `const` for complex expressions.
9. Variable patterns (`var name`, `final Type name`) bind new variables to matched/destructured values. Typed variable patterns only match if the value has the declared type.
10. Identifier patterns (`foo`, `_`) act as variable or constant patterns depending on context. `_` always acts as a wildcard and matches/discards any value.
11. Parenthesized patterns (`(subpattern)`) control pattern precedence and grouping, similar to expressions.
12. List patterns (`[subpattern1, subpattern2]`) match lists and destructure elements by position. The pattern length must match the list unless a rest element is used.
13. Rest elements (`...`, `...rest`) in list patterns match arbitrary-length lists or collect unmatched elements into a new list.
14. Map patterns (`{"key": subpattern}`) match maps and destructure by key. Only specified keys are matched; missing keys throw a `StateError`.
15. Record patterns (`(subpattern1, subpattern2)`, `(x: subpattern1, y: subpattern2)`) match records by shape and destructure positional/named fields. Field names can be omitted if inferred from variable or identifier patterns.
16. Object patterns (`ClassName(field1: subpattern1, field2: subpattern2)`) match objects by type and destructure using getters. Extra fields in the object are ignored.
17. Wildcard patterns (`_`, `Type _`) match any value without binding. Useful for ignoring values or type-checking without binding.
18. All pattern types can be nested and combined for expressive and precise matching and destructuring.

### Records
1. Records are anonymous, immutable, aggregate types that bundle multiple objects into a single value.
2. Records are fixed-sized, heterogeneous, and strongly typed. Each field can have a different type.
3. Records are real values: store them in variables, nest them, pass to/from functions, and use in lists, maps, and sets.
4. Record expressions use parentheses with comma-delimited positional and/or named fields, e.g. `('first', a: 2, b: true, 'last')`.
5. Record type annotations use parentheses with comma-delimited types. Named fields use curly braces: `({int a, bool b})`.
6. The names of named fields are part of the record's type (shape). Records with different named field names have different types.
7. Positional field names in type annotations are for documentation only and do not affect the record's type.
8. Record fields are accessed via built-in getters: positional fields as `$1`, `$2`, etc., and named fields by their name (e.g., `.a`).
9. Records are immutable: fields do not have setters.
10. Records are structurally typed: the set, types, and names of fields define the record's type (shape).
11. Two records are equal if they have the same shape and all corresponding field values are equal. Named field order does not affect equality.
12. Records automatically define `hashCode` and `==` based on structure and field values.
13. Use records for functions that return multiple values; destructure with pattern matching: `var (name, age) = userInfo(json);`
14. Destructure named fields with the colon syntax: `final (:name, :age) = userInfo(json);`
15. Using records for multiple returns is more concise and type-safe than using classes, lists, or maps.
16. Use lists of records for simple data tuples with the same shape.
17. Use type aliases (`typedef`) for record types to improve readability and maintainability.
18. Changing a record type alias does not guarantee all code using it is still type-safe; only classes provide full abstraction/encapsulation.
19. Extension types can wrap records but do not provide full abstraction or protection.
20. Records are best for simple, immutable data aggregation; use classes for abstraction, encapsulation, and behavior.

# Riverpod Rules

### Using Ref in Riverpod
0. Installation
```flutter pub add flutter_riverpod
flutter pub add riverpod_annotation
flutter pub add dev:riverpod_generator
flutter pub add dev:build_runner
flutter pub add dev:custom_lint
flutter pub add dev:riverpod_lint
```
1. The `Ref` object is essential for accessing the provider system, reading or watching other providers, managing lifecycles, and handling dependencies in Riverpod.
2. In functional providers, obtain `Ref` as a parameter; in class-based providers, access it as a property of the Notifier.
3. In widgets, use `WidgetRef` (a subtype of `Ref`) to interact with providers.
4. The `@riverpod` annotation is used to define providers with code generation, where the function receives `ref` as its parameter.
5. Use `ref.watch` to reactively listen to other providers; use `ref.read` for one-time access (non-reactive); use `ref.listen` for imperative subscriptions; use `ref.onDispose` to clean up resources.
6. Example: Functional provider with Ref
   ```dart
   final otherProvider = Provider<int>((ref) => 0);
   final provider = Provider<int>((ref) {
     final value = ref.watch(otherProvider);
     return value * 2;
   });
   ```
7. Example: Provider with @riverpod annotation
   ```dart
   @riverpod
   int example(Ref ref) {
     return 0;
   }
   ```
8. Example: Using Ref for cleanup
   ```dart
   final provider = StreamProvider<int>((Ref ref) {
     final controller = StreamController<int>();
     ref.onDispose(controller.close);
     return controller.stream;
   });
   ```
9. Example: Using WidgetRef in a widget
   ```dart
   class MyWidget extends ConsumerWidget {
     @override
     Widget build(BuildContext context, WidgetRef ref) {
       final value = ref.watch(myProvider);
       return Text('$value');
     }
   }
   ```

### Combining Requests
1. Use the `Ref` object to combine providers and requests; all providers have access to a `Ref`.
2. In functional providers, obtain `Ref` as a parameter; in class-based providers, access it as a property of the Notifier.
3. Prefer using `ref.watch` to combine requests, as it enables reactive and declarative logic that automatically recomputes when dependencies change.
4. When using `ref.watch` with asynchronous providers, use `.future` to await the value if you need the resolved result, otherwise you will receive an `AsyncValue`.
5. Avoid calling `ref.watch` inside imperative code (e.g., listener callbacks or Notifier methods); only use it during the build phase of the provider.
6. Use `ref.listen` as an alternative to `ref.watch` for imperative subscriptions, but prefer `ref.watch` for most cases as `ref.listen` is more error-prone.
7. It is safe to use `ref.listen` during the build phase; listeners are automatically cleaned up when the provider is recomputed.
8. Use the return value of `ref.listen` to manually remove listeners when needed.
9. Use `ref.read` only when you cannot use `ref.watch`, such as inside Notifier methods; `ref.read` does not listen to provider changes.
10. Be cautious with `ref.read`, as providers not being listened to may destroy their state if not actively watched.

### Auto Dispose & State Disposal
1. By default, with code generation, provider state is destroyed when the provider stops being listened to for a full frame.
2. Opt out of automatic disposal by setting `keepAlive: true` (codegen) or using `ref.keepAlive()` (manual).
3. When not using code generation, state is not destroyed by default; enable `.autoDispose` on providers to activate automatic disposal.
4. Always enable automatic disposal for providers that receive parameters to prevent memory leaks from unused parameter combinations.
5. State is always destroyed when a provider is recomputed, regardless of auto dispose settings.
6. Use `ref.onDispose` to register cleanup logic that runs when provider state is destroyed; do not trigger side effects or modify providers inside `onDispose`.
7. Use `ref.onCancel` to react when the last listener is removed, and `ref.onResume` when a new listener is added after cancellation.
8. Call `ref.onDispose` multiple times if needed—once per disposable object—to ensure all resources are cleaned up.
9. Use `ref.invalidate` to manually force the destruction of a provider's state; if the provider is still listened to, a new state will be created.
10. Use `ref.invalidateSelf` inside a provider to force its own destruction and immediate recreation.
11. When invalidating parameterized providers, you can invalidate a specific parameter or all parameter combinations.
12. Use `ref.keepAlive` for fine-tuned control over state disposal; revert to automatic disposal using the return value of `ref.keepAlive`.
13. To keep provider state alive for a specific duration, combine a `Timer` with `ref.keepAlive` and dispose after the timer completes.
14. Consider using `ref.onCancel` and `ref.onResume` to implement custom disposal strategies, such as delayed disposal after a provider is no longer listened to.

### Eager Initialization
1. Providers are initialized lazily by default; they are only created when first used.
2. There is no built-in way to mark a provider for eager initialization due to Dart's tree shaking.
3. To eagerly initialize a provider, explicitly read or watch it at the root of your application (e.g., in a `Consumer` placed directly under `ProviderScope`).
4. Place the eager initialization logic in a public widget (such as `MyApp`) rather than in `main()` to ensure consistent test behavior.
5. Eagerly initializing a provider in a dedicated widget will not cause your entire app to rebuild when the provider changes; only the initialization widget will rebuild.
6. Handle loading and error states for eagerly initialized providers as you would in any `Consumer`, e.g., by returning a loading indicator or error widget.
7. Use `AsyncValue.requireValue` in widgets to read the data directly and throw a clear exception if the value is not ready, instead of handling loading/error states everywhere.
8. Avoid creating multiple providers or using overrides solely to hide loading/error states; this adds unnecessary complexity and is discouraged.

### First Provider & Network Requests
1. Always wrap your app with `ProviderScope` at the root (directly in `runApp`) to enable Riverpod for the entire application.
2. Place business logic such as network requests inside providers; use `Provider`, `FutureProvider`, or `StreamProvider` depending on the return type.
3. Providers are lazy—network requests or logic inside a provider are only executed when the provider is first read.
4. Define provider variables as `final` and at the top level (global scope).
5. Use code generators like Freezed or json_serializable for models and JSON parsing to reduce boilerplate.
6. Use `Consumer` or `ConsumerWidget` in your UI to access providers via a `ref` object.
7. Handle loading and error states in the UI by using the `AsyncValue` API returned by `FutureProvider` and `StreamProvider`.
8. Multiple widgets can listen to the same provider; the provider will only execute once and cache the result.
9. Use `ConsumerWidget` or `ConsumerStatefulWidget` to reduce code indentation and improve readability over using a `Consumer` widget inside a regular widget.
10. To use both hooks and providers in the same widget, use `HookConsumerWidget` or `StatefulHookConsumerWidget` from `flutter_hooks` and `hooks_riverpod`.
11. Always install and use `riverpod_lint` to enable IDE refactoring and enforce best practices.
12. Do not put `ProviderScope` inside `MyApp`; it must be the top-level widget passed to `runApp`.
13. When handling network requests, always render loading and error states gracefully in the UI.
14. Do not re-execute network requests on widget rebuilds; Riverpod ensures the provider is only executed once unless explicitly invalidated.

### Passing Arguments to Providers
1. Use provider "families" to pass arguments to providers; add `.family` after the provider type and specify the argument type.
2. When using code generation, add parameters directly to the annotated function (excluding `ref`).
3. Always enable `autoDispose` for providers that receive parameters to avoid memory leaks.
4. When consuming a provider that takes arguments, call it as a function with the desired parameters (e.g., `ref.watch(myProvider(param))`).
5. You can listen to the same provider with different arguments simultaneously; each argument combination is cached separately.
6. The equality (`==`) of provider parameters determines caching—ensure parameters have consistent and correct equality semantics.
7. Avoid passing objects that do not override `==` (such as plain `List` or `Map`) as provider parameters; use `const` collections, custom classes with proper equality, or Dart 3 records.
8. Use the `provider_parameters` lint rule from `riverpod_lint` to catch mistakes with parameter equality.
9. For multiple parameters, prefer code generation or Dart 3 records, as records naturally override `==` and are convenient for grouping arguments.
10. If two widgets consume the same provider with the same parameters, only one computation/network request is made; with different parameters, each is cached separately.

### FAQ & Best Practices
1. Use `ref.refresh(provider)` when you want to both invalidate a provider and immediately read its new value; use `ref.invalidate(provider)` if you only want to invalidate without reading the value.
2. Always use the return value of `ref.refresh`; ignoring it will trigger a lint warning.
3. If a provider is invalidated while not being listened to, it will not update until it is listened to again.
4. Do not try to share logic between `Ref` and `WidgetRef`; move shared logic into a `Notifier` and call methods on the notifier via `ref.read(yourNotifierProvider.notifier).yourMethod()`.
5. Prefer `Ref` for business logic and avoid relying on `WidgetRef`, which ties logic to the UI layer.
6. Extend `ConsumerWidget` instead of using raw `StatelessWidget` when you need access to providers in the widget tree, due to limitations of `InheritedWidget`.
7. `InheritedWidget` cannot implement a reliable "on change" listener or track when widgets stop listening, which is required for Riverpod's advanced features.
8. Do not expect to reset all providers at once; instead, make providers that should reset depend on a "user" or "session" provider and reset that dependency.
9. `hooks_riverpod` and `flutter_hooks` are versioned independently; always add both as dependencies if using hooks.
10. Riverpod uses `identical` instead of `==` to filter updates for performance reasons, especially with code-generated models; override `updateShouldNotify` on Notifiers to change this behavior.
11. If you encounter "Cannot use `ref` after the widget was disposed", ensure you check `context.mounted` before using `ref` after an `await` in an async callback.

### Provider Observers (Logging & Error Reporting)
1. Use a `ProviderObserver` to listen to all events in the provider tree for logging, analytics, or error reporting.
2. Extend the `ProviderObserver` class and override its methods to respond to provider lifecycle events:
   - `didAddProvider`: called when a provider is added to the tree.
   - `didUpdateProvider`: called when a provider is updated.
   - `didDisposeProvider`: called when a provider is disposed.
   - `providerDidFail`: called when a synchronous provider throws an error.
3. Register your observer(s) by passing them to the `observers` parameter of `ProviderScope` (for Flutter apps) or `ProviderContainer` (for pure Dart).
4. You can register multiple observers if needed by providing a list to the `observers` parameter.
5. Use observers to integrate with remote error reporting services, log provider state changes, or trigger custom analytics.

### Performing Side Effects
1. Use Notifiers (`Notifier`, `AsyncNotifier`, etc.) to expose methods for performing side effects (e.g., POST, PUT, DELETE) and modifying provider state.
2. Always define provider variables as `final` and at the top level (global scope).
3. Choose the provider type (`NotifierProvider`, `AsyncNotifierProvider`, etc.) based on the return type of your logic.
4. Use provider modifiers like `autoDispose` and `family` as needed for cache management and parameterization.
5. Expose public methods on Notifiers for UI to trigger state changes or side effects.
6. In UI event handlers (e.g., button `onPressed`), use `ref.read` to call Notifier methods; avoid using `ref.watch` for imperative actions.
7. After performing a side effect, update the UI state by:
   - Setting the new state directly if the server returns the updated data.
   - Calling `ref.invalidateSelf()` to refresh the provider and re-fetch data.
   - Manually updating the local cache if the server does not return the new state.
8. When updating the local cache, prefer immutable state, but mutable state is possible if necessary.
9. Always handle loading and error states in the UI when performing side effects.
10. Use progress indicators and error messages to provide feedback for pending or failed operations.
11. Be aware of the pros and cons of each update approach:
    - Direct state update: most up-to-date but depends on server implementation.
    - Invalidate and refetch: always consistent with server, but may incur extra network requests.
    - Manual cache update: efficient, but risks state divergence from server.
12. Use hooks (`flutter_hooks`) or `StatefulWidget` to manage local state (e.g., pending futures) for showing spinners or error UI during side effects.
13. Do not perform side effects directly inside provider constructors or build methods; expose them via Notifier methods and invoke from the UI layer.

### Testing Providers
1. Always create a new `ProviderContainer` (unit tests) or `ProviderScope` (widget tests) for each test to avoid shared state between tests. Use a utility like `createContainer()` to set up and automatically dispose containers (see `/references/riverpod/testing/create_container.dart`).
2. In unit tests, never share `ProviderContainer` instances between tests. Example:
   ```dart
   final container = createContainer();
   expect(container.read(provider), equals('some value'));
   ```
3. In widget tests, always wrap your widget tree with `ProviderScope` when using `tester.pumpWidget`. Example:
   ```dart
   await tester.pumpWidget(
     const ProviderScope(child: YourWidgetYouWantToTest()),
   );
   ```
4. Obtain a `ProviderContainer` in widget tests using `ProviderScope.containerOf(BuildContext)`. Example:
   ```dart
   final element = tester.element(find.byType(YourWidgetYouWantToTest));
   final container = ProviderScope.containerOf(element);
   ```
5. After obtaining the container, you can read or interact with providers as needed for assertions. Example:
   ```dart
   expect(container.read(provider), 'some value');
   ```
6. For providers with `autoDispose`, prefer `container.listen` over `container.read` to prevent the provider's state from being disposed during the test.
7. Use `container.read` to read provider values and `container.listen` to listen to provider changes in tests.
8. Use the `overrides` parameter on `ProviderScope` or `ProviderContainer` to inject mocks or fakes for providers in your tests.
9. Use `container.listen` to spy on changes in a provider for assertions or to combine with mocking libraries.
10. Await asynchronous providers in tests by reading the `.future` property (for `FutureProvider`) or listening to streams.
11. Prefer mocking dependencies (such as repositories) used by Notifiers rather than mocking Notifiers directly.
12. If you must mock a Notifier, subclass the original Notifier base class instead of using `implements` or `with Mock`.
13. Place Notifier mocks in the same file as the Notifier being mocked if code generation is used, to access generated classes.
14. Use the `overrides` parameter to swap out Notifiers or providers for mocks or fakes in tests.
15. Keep all test-specific setup and teardown logic inside the test body or test utility functions. Avoid global state.
16. Ensure your test environment closely matches your production environment for reliable results.

TOTAL CHAR COUNT:    15993