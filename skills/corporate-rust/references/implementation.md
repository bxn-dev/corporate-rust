# Implementation

- Type inference: Add enough explicit types for someone familiar with the codebase to determine inferred types, especially for generic returns such as `.into()`/`.parse()` and complex iterator results. Prefer type annotations on variables; `_` is usually fine when a turbofish is required.
- Use pattern matching rather than a check followed by `unwrap` when the input rules out a panic.
- Prefer `expect` over `unwrap` when a panic is necessary because an invariant cannot be expressed in the type system; give the reason.
- Matching `non_exhaustive` enums: Prefer listing all known variants; uninteresting variants may go in the wildcard arm. If only specific variants matter, use `if let` or `let ... else`; exception: `match` is clearer.
- Always consider assertion context, particularly instead of a comment, for user-visible failures, helpful values, or to distinguish API misuse from an implementation bug. Do not merely repeat the condition.
- Iterator chains: Avoid long, complex chains; assign a chain to a variable before a `for` loop rather than building it in the loop header. Use `for_each` only with good reason instead of the usually more readable `for`. Prefer short chains and combinators containing substantial logic when they are clearer.
- Do not use `panic::catch_unwind` for error handling. Its only exception is a last-resort guard at a critical entry point in code that already largely avoids panics. Correctness must not depend on catching a panic: a build with `panic=abort` terminates instead.
- Prefer `format!` / `write!` to concatenate more than two strings or perform substitutions in a string literal, rather than repeated `+` or `str::replace`, respectively.
