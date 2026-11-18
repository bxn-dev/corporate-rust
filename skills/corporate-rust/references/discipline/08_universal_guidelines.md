# Universal Rust discipline

## Names and organization

Prefer informative names over generic suffixes such as `Service`, `Manager`, or `Factory`. A repeatedly usable builder is still a builder; for repeatable instantiation, prefer accepting a function such as `impl Fn() -> Foo` over a builder parameter. Builders are useful for optional construction permutations.

Prefer regular functions for computation that does not belong to a receiver. Associated functions are primarily for construction; associated trait functions remain idiomatic.

Prefer independently usable components as separate crates. Umbrella crates may reunite them; always re-export components split for technical reasons, such as a procedural macro crate, but otherwise re-export sparingly. Features are for functionality that cannot reasonably stand alone, or for enabling constituents of an umbrella crate.

Follow Rust naming conventions for conversions (`as_`, `to_`, `into_`), getters and feature names. Implement common traits where appropriate, including `Copy`, `Clone`, equality, ordering, hashing, `Default`, and `Debug`. Constructors are inherent associated functions; provide `new()` even when `Default` exists.

## Magic values and lints

Production magic values must have comments explaining why they were chosen, non-obvious consequences of changing them, and interacting external systems. Prefer named constants.

For local overrides of project-wide lints, prefer `#[expect]` with a reason so stale overrides are reported. `#[allow]` remains useful for generated code and may appear in macros.

## Logging

Use structured events with named properties and message templates; avoid eagerly formatting strings. Include properties in the template where useful, and name events hierarchically as `<component>.<operation>.<state>`. Use established telemetry attribute conventions for common fields when needed. Never log sensitive data in plaintext; redact items such as email addresses, identifying paths, secret-bearing filenames, personal data, and session identifiers. In Rust formatting macros, escaped braces can preserve literal template placeholders for later formatting.

## Panics and correctness

Panics signal that the program should stop, not a recoverable error channel. Code must remain panic-safe even if a panic is caught; do not assume it will be caught. Report expected failures normally. Detected programming errors that violate invariants must panic unless a binding project rule prohibits panics, in which case follow its prescribed failure path. Do not introduce errors that callers cannot meaningfully handle.

Detection of contract violations need not be added where impossible or costly. Invalid input to an inherently fallible parser is an expected failure, not necessarily a contract violation. Panics can also occur in const contexts, explicitly requested operations such as `unwrap`, and on poisoned locks. Explore type-level designs that prevent invalid states.

## Debug and display

Public types should implement `Debug`; types holding sensitive data should implement it manually and must have unit tests ensuring secrets are not disclosed. Types intended for users or developers to read, including errors and string wrappers, should implement `Display` according to Rust conventions. Apply the same sensitive-data protections to `Display`.

## Static verification

Projects should use compiler lints, Clippy, rustfmt, dependency vulnerability auditing, feature-combination checks, unused-dependency detection, and Miri for unsafe code. These can run locally and should be part of check-in gates. Miri is not a proof of soundness.

In addition to defaults, projects should enable the compiler lints `ambiguous_negative_literals`, `missing_debug_implementations`, `redundant_imports`, `redundant_lifetimes`, `trivial_numeric_casts`, `unsafe_op_in_unsafe_fn`, and `unused_lifetimes` at warning level.

Enable the major Clippy groups `cargo`, `complexity`, `correctness`, `pedantic`, `perf`, `style`, and `suspicious`; `nursery` is optional because of possible false positives. Opt out of unwanted lints individually. Consider these restriction lints at warning level:

```text
allow_attributes_without_reason  as_pointer_underscore
assertions_on_result_states      clone_on_ref_ptr
deref_by_slicing                 disallowed_script_idents
empty_drop                       empty_enum_variants_with_brackets
empty_structs_with_brackets      fn_to_numeric_cast_any
if_then_some_else_none            map_err_ignore
redundant_type_annotations       renamed_function_params
semicolon_outside_block           string_to_string
undocumented_unsafe_blocks       unnecessary_safety_comment
unnecessary_safety_doc           unneeded_field_pattern
unused_result_ok
```

Allowing `literal_string_with_formatting_args` may be necessary for structured logging templates.
