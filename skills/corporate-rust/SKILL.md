---
name: corporate-rust
description: Rust development and code review. Load before writing, editing, or reviewing any Rust code.
---

# Corporate Rust

## Workflow

1. **Scope.** Inspect affected callers and build configuration. Identify which code is reusable library code and which is application code. API design rules bind libraries; they are strong recommendations for applications, where a direct solution may be preferable.
2. **Load.** Before writing, editing, or reviewing Rust, read the [core requirements](references/discipline/08_universal_guidelines.md), even for small changes. Open every sheet below that matches an affected decision; read only the relevant sections of topic sheets.
3. **Apply.** Apply compatible rules together. Check scope, exceptions, and whether each rule is binding. Resolve overlapping requirements in this order: binding project requirements → corporate rules (sections marked **Corporate rule**) → language and API rules → engineering practices. Use idiomatic Rust for the rest. Write comments in American English unless the user requests otherwise.
4. **Verify.** Check every changed area against the selected rules and use the project's formatter and tests. For each review finding, cite the affected code, applicable rule, and concrete fix. Distinguish recommendations from prohibitions and justify any exception you rely on.

## Language and API rules

| Decision | Read |
|---|---|
| Crate and module layout, imports, public re-exports | [Modules and imports](references/module-imports.md) |
| Formatter, formatting exceptions, test harness | [Formatting and tests](references/formatting-tests.md) |
| Borrowing vs. cloning, shared ownership, locks, threads, async | [Ownership and concurrency](references/ownership-concurrency.md) |
| Parameter and return types, error reporting | [Function APIs](references/function-apis.md) |
| Newtypes, traits, derives, generics vs. `dyn` | [Types and traits](references/types-traits.md) |
| Names, constructors, lifetimes, type parameters, doc comments | [Naming and documentation](references/naming-docs.md) |
| Macros, `unsafe`, accessors, `const`, operators, `#[non_exhaustive]`, `#[must_use]`, `#[doc(hidden)]`, pre-`main` initialization | [Language features and macros](references/features-macros.md) |
| Inference, matching, loops, assertions, `expect`, catching panics, strings | [Implementation](references/implementation.md) |

## Engineering practices

| Decision | Read |
|---|---|
| APIs and docs for AI-assisted use | [AI guidelines](references/discipline/01_ai_guidelines.md) |
| Application errors (including CLI and desktop apps), global allocators | [Applications](references/discipline/02_application_guidelines.md) |
| Public API contracts, Rustdoc for items, modules, and re-exports | [Documentation](references/discipline/03_documentation.md) |
| FFI, dynamic libraries, portable data, cross-artifact state | [FFI](references/discipline/04_ffi_guidelines.md) |
| Profiling, throughput, allocations, async yield points | [Performance](references/discipline/06_performance_guidelines.md) |
| `unsafe` (including performance optimizations), soundness, safety contracts, Miri | [Safety](references/discipline/07_safety_guidelines.md) |
| Lint configuration, verification gates | [Static verification](references/discipline/static_verification.md) |
| Library builds, Cargo features, native `-sys` crates | [Building libraries](references/discipline/09_libraries_building_guidelines.md) |
| Public dependency types, native handles, `Send`/`Sync` | [Library interoperability](references/discipline/10_libraries_interoperability_guidelines.md) |
| Statics, mockable I/O, strong types, test utilities | [Library resilience](references/discipline/11_libraries_resilience_guidelines.md) |
| Borrowed parameters; library signatures, errors, builders, services, dependency injection | [Library UX](references/discipline/12_libraries_ux_guidelines.md) |
