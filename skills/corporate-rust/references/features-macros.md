# Macros and other Rust features

- Macros: Prefer, in order: ordinary Rust constructs, `macro_rules!`, other code generation, then procedural macros. Weigh the benefit against the cost of navigating, debugging, and changing generated code.
- Use `unsafe` only when safe code cannot provide the required functionality or the [performance exception](../SKILL.md#corporate-rules) is satisfied. Encapsulate it in a safe abstraction and isolate it from business logic as much as possible, ideally in a separate crate with few dependencies. Add a `SAFETY:` comment explaining correctness and a technical justification: why not safe, and whether benchmarks exist if performance is the reason. The justification may cover a block, file, or build target.
- Use stable language features unless the project explicitly approves an exception. If a target cannot build on stable Rust, identify the missing capability before proposing a feature gate.
- Accessor names: `foo()` for `&T` or `T: Copy`, `foo_mut()` for `&mut T`, `take_foo()` for taking and resetting, `set_foo()` for setting by value, `replace_foo()` for setting and returning the old value. Closure access uses `with_foo()` or `with_mut_foo()`.
- Closure accessors suit scoped access to `Mutex`/`RefCell` without exposing the guard, or follow-up work/validation when a setter is unsuitable. They may forward the closure's return value. For both read-only and mutable access, `with_foo`/`with_mut_foo` is conventional; otherwise `with_foo` may also mutate.
- API pairs: Implement `is_empty()` alongside `len()`.
- `#[doc(hidden)]`: Treat items marked this way that you do not own as private and do not use them.
- Mark public functions as `const` only if this is expected to remain true or migration is promised if it changes.
- Use operator overloading for the original mathematical/logical purpose, not merely as syntax sugar.
- `#[non_exhaustive]` on enums: Apply to public APIs used by other teams. Exceptions: permanently unchanging ontologies or binary states; alternatively, a commitment to fix future breakages via a large-scale change (LSC). The rule may be ignored when there are few uses.
- `#[non_exhaustive]` on structs: Apply to public structs with public fields in cross-team APIs. Exceptions: fields unlikely to change, a commitment to fix breakages via an LSC, or deliberately enabling functional record update. The rule may be ignored when there are few uses.
- `#[must_use]` on functions: Use when ignoring the return value prevents correct use, or the return value contains errors or critical information. If the return type is already marked, such as `Result`, the function does not need the attribute.
- `#[must_use]` on types: Use for types intended to be returned by such functions, such as error types or lazy computations whose desired effect occurs only when used.
- Do not run code before `main()`, for example using `ctor`.
