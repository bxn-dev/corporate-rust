# Library API design

## Surface and dependencies

Avoid exposing smart pointers and generic wrappers such as `Arc`, `Box`, or `RefCell` in public APIs; hide implementation details behind borrowed or owned values. Exceptions include APIs whose purpose is the wrapper itself and benchmarked, significant performance benefits that justify the complexity.

For async dependencies prefer concrete types, then generics, then `dyn Trait`. For a test-only alternate implementation, consider an internal enum with a mock variant. For user-provided implementations, define narrow traits on top of inherent methods and accept generics unless nesting becomes excessive. If dynamic dispatch is necessary, prefer a dedicated wrapper over exposing nested smart pointers. See [resilience](11_libraries_resilience_guidelines.md) for mockable effects.

Avoid visibly nested parameterized types, especially service types users must name. One level is a useful ceiling for primary service APIs; users may supply their own nested types as parameters. Containers necessarily expose parameters, but should limit their number and nesting. Consider naming, non-user types, bound complexity, and inference impact. Sophisticated type systems may be justified when they demonstrably reduce overall friction.

Implement essential functionality as inherent methods; trait implementations should forward to them.

## Corporate rule: borrowed inputs

For read-only text use `&str`, and for borrowed contiguous sequences use `&[T]`, not `impl AsRef<_>`. Preserve trait-required signatures. APIs that only iterate need not require contiguous storage.

## Inputs

For other clear reference hierarchies, such as paths, consider `impl AsRef<_>` for function parameters where ownership is unnecessary or copying is cheap; do not generally infect stored types with such bounds. When ownership is needed, weigh ergonomics against copying cost.

For one-shot initialization I/O, accept an appropriate I/O trait rather than a concrete file: use `std::io::Read` or `Write` synchronously and runtime-independent async I/O traits for functions targeting multiple runtimes. Types performing continuous runtime-specific I/O need an appropriate runtime abstraction.

Functions accepting numeric ranges must use a range type or trait, not separate hand-rolled bounds. Prefer `impl RangeBounds<T>` when arbitrary ranges are supported.

## Corporate rule: library errors

Preserve relevant context and underlying causes for I/O and chained failures. A small error that fully describes the failure may be `Copy`; do not discard context or causes to derive it, and do not add a `Backtrace` solely for a template.

## Errors

Use situation-specific library error structs, with reusable error types where appropriate and separate types for unrelated failures. Where useful, provide contextual accessors and capture diagnostic backtraces appropriately.

For mixed failure modes, an internal error kind can distinguish cases; expose actionable `is_xxx()` methods rather than exposing internal, unhandleable cases. Error structs must implement `Display` and `std::error::Error`, reporting a useful summary and relevant cause and diagnostic information.

## Construction and services

For complex construction, use a `FooBuilder` with chainable `x()` setters, a final `build()`, and a `Foo::builder()` entry point; the builder should not have a public `new()`. Up to two optional parameters can use inherent constructors; beyond that, builders avoid combinatorial growth. Pass required parameters at builder creation. For multiple required parameters, group them into a dependencies struct and consider `impl Into<Deps>` conversions; a dedicated struct is optional for zero or one simple required parameter, though it helps future evolution. Offer `builder_{runtime}(deps)` methods for runtime-specific dependencies.

Types requiring four or more parameters should group related parameters into helper types and consider newtypes to prevent mix-ups.

Heavyweight services and thread-singleton handles should implement shared-ownership `Clone`, so dependencies can be reused without deep-copying the service. An internal `Arc<Inner>` is one possible implementation.
