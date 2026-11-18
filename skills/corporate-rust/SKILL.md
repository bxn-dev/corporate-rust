---
name: corporate-rust
description: Mandatory for all Rust development and code review. Enforce the applicable requirements for API design, documentation, safety, performance, and verification before creating, modifying, refactoring, or approving Rust code.
---

# Corporate Rust

First check the callers and build configuration. Which codebase are you working in? Is the code a reusable library or part of an application? API design rules are binding for libraries and strongly recommended for applications. In application code, prefer a direct solution to the immediate problem over premature generalization.

## Before writing or reviewing Rust

1. Always read the [core requirements](references/discipline/08_universal_guidelines.md), including for small changes. Load the applicable topic sheets below **before** generating code. For long files, read by heading or in segments.
2. Apply compatible rules together. Check their scope, exceptions, and whether they are recommendations or requirements. A platform-only rule is not a conflict outside that platform.
3. **Order of authority:** Binding project requirements take precedence, followed by the corporate rules below, then the language and API rules, then the engineering practices. Apply this order only where requirements overlap in scope.

## Corporate rules

1. **Borrowed inputs:** For read-only text use `&str`; for a borrowed contiguous sequence use `&[T]`. Do not substitute `impl AsRef<_>` for these borrows. Preserve signatures required by traits. An API that only iterates need not require contiguous storage; see [Function APIs](references/function-apis.md).
2. **Public re-exports:** List exported names explicitly. Use a glob only for a technical reason, such as exposing the same interface from target-specific implementations, not to shorten an export list. This does not prohibit `use super::*` inside an inline test module.
3. **Public library docs:** Document each public API item's contract, behavior, or units rather than restating its name. A self-explanatory enum variant needs no separate doc comment.
4. **Unsafe for performance:** A safe alternative does not rule out `unsafe`. Permit the optimization only if a reproducible benchmark shows a gain over the safe version on a representative workload **and** a `SAFETY:` argument establishes why every unsafe operation satisfies its preconditions on every reachable path for all permitted inputs. Encapsulate the unsafe code, test boundary cases, and run them under Miri. Passing tests or Miri is not proof of soundness. If the gain or safety argument is missing, or the Miri check cannot pass or be run, keep the safe version.
5. **Library error types:** A small error that fully describes the failure may be `Copy`. For I/O and chained failures, retain relevant context and the underlying cause; do not discard either to derive `Copy`. Do not add a `Backtrace` solely to satisfy a template.
6. **Panics:** Report expected failures normally, including in developer CLIs. Panic on detected programming errors that violate invariants, unless a binding project rule prohibits panics; in that case use the project's prescribed failure path.
7. **Compliance markers:** Do not add dated guideline-compliance comments to Rust files.

## Language and API rules

Before editing or reviewing code, **open only the topic sheets relevant to the decision**. Combine sheets when the change crosses topics. For example, to remove an unnecessary deep copy, read *Ownership and concurrency*. If borrowing also changes a parameter type, read *Function APIs* as well.

| What are you deciding? | Topic sheet |
|---|---|
| Where should a type, module, re-export, or `use` statement go? | [Modules and imports](references/module-imports.md) |
| Which formatter, formatting exception, or test framework applies? | [Formatting and tests](references/formatting-tests.md) |
| Should you borrow, clone, or share ownership? Use a mutex, threads, or `async`? | [Ownership and concurrency](references/ownership-concurrency.md) |
| What should callers pass in, get back, or handle as an error? | [Function APIs](references/function-apis.md) |
| Newtype or alias? Trait or inherent method? `derive` or manual implementation? Generics or `dyn`? | [Types and traits](references/types-traits.md) |
| How should you name a constructor, lifetime, or type parameter? What belongs in Rustdoc? | [Naming and documentation](references/naming-docs.md) |
| Do you need a macro or `unsafe`? How should you use accessors, `const`, operators, `#[non_exhaustive]`, `#[must_use]`, or initialization before `main`? | [Language features and macros](references/features-macros.md) |
| How should you write a `match`, loop, assertion, `expect` message, or string expression? | [Implementation](references/implementation.md) |

## Engineering practices

| When the task involves… | Read |
|---|---|
| Designing APIs and documentation for AI-assisted use | [AI guidelines](references/discipline/01_ai_guidelines.md) |
| Application errors, CLI or desktop apps, allocators | [Applications](references/discipline/02_application_guidelines.md) |
| Public docs, module docs, re-export documentation | [Documentation](references/discipline/03_documentation.md) |
| Dynamic libraries, portable data, cross-artifact state | [FFI](references/discipline/04_ffi_guidelines.md) |
| Profiling, throughput, allocation, async yield points | [Performance](references/discipline/06_performance_guidelines.md) |
| Unsafe code, soundness, safety contracts, Miri | [Safety](references/discipline/07_safety_guidelines.md) |
| Library builds, Cargo features, native `-sys` crates | [Building libraries](references/discipline/09_libraries_building_guidelines.md) |
| Public dependencies, native handles, `Send`/`Sync` | [Library interoperability](references/discipline/10_libraries_interoperability_guidelines.md) |
| Statics, mockable I/O, re-exports, strong types, test utilities | [Library resilience](references/discipline/11_libraries_resilience_guidelines.md) |
| Library signature design, errors, builders, services, dependency injection | [Library UX](references/discipline/12_libraries_ux_guidelines.md) |

## Apply and verify

- Write comments in American English unless the user requests otherwise. For API docs, write a summary sentence under 15 words, then extended docs and the applicable Examples, Errors, Panics, Safety, and Abort sections.
- Check local constraints before applying a rule. If a service already owns a worker pool, use it for background jobs rather than starting a thread per request.
- Use idiomatic Rust for decisions not covered by these rules.
- Check the diff against the selected rules and use the project's formatter and tests. In a review, identify the affected code, the applicable rule, and a concrete change. Distinguish recommendations from prohibitions and explain why an exception fits the code.
