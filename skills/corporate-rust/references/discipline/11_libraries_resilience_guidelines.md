# Library resilience

## State and testability

Avoid static and thread-local state when correctness depends on a consistent view: different versions of a crate can have independent instances. Statics used only for performance are acceptable. They can also hinder tests and create contention.

User-facing types that perform I/O, side-effecting system calls, or nondeterministic, environment-dependent, fragile, or non-reproducible operations should make those effects mockable. This includes clocks and entropy. Libraries should not perform ad-hoc I/O, depend on unmockable effects, create their own I/O core, or offer a default constructor that does so. Instead accept a mockable I/O core or provide mocking themselves. An internal enum can dispatch to native or mock implementations, or extend an existing runtime enum. When providing mocks, usually return `(instance, mock_controller)` rather than accepting a shared controller, avoiding ambiguous state.

Except in kernel-like code, allocations may be treated as deterministic and practically infallible for mocking. Do not assume unlimited memory: memory-hungry code and external-input processing should offer bounds or chunking.

Guard test utilities with feature flags, including mocks, sensitive-data inspection, safety overrides, and fake data. Prefer one clearly named `test-util` feature.

## Exports and types

For export lists, follow [public re-exports](../module-imports.md#corporate-rule-public-re-exports).

Use the strongest appropriate standard type early in an API flow, such as `Path` or `PathBuf` rather than strings for OS paths. Follow conventional numeric API boundaries: ordinary numbers are normally preferable to wrappers such as `NonZero<usize>`.
