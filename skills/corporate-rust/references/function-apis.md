# Function APIs

API design rules are binding for libraries and a strong recommendation for applications; in applications, consider simplicity and immediate needs.

- Flexible references: Borrow immutable data directly where possible: `&T` instead of `&Box<T>`, `&[T]` instead of `&Vec<T>`, `&str` instead of `&String`, `Option<&T>` instead of `&Option<T>`. Exception: expected non-linear character data such as Rope/Cord.
- Prefer returning iterators over `Vec<T>`. A `Vec` is particularly appropriate when most callers need one, the implementation naturally creates one, random access is needed, a `for` loop is better and allocation matters little, the purpose is to create a new owned collection, or an existing vector is returned. In applications, `Vec` is also fine when it simplifies the solution and iterator benefits are not clearly needed.
- Iterator parameters: Highly reusable libraries should accept iterators or `impl IntoIterator` when they only need sequential access. Applications may use slices for simplicity when flexibility and performance benefits are not clearly needed.
- `Into`: In libraries, consider `impl Into<T>` when different input types are likely and call-site convenience is particularly important; weigh generic code size and inference costs. Almost never use such parameters in applications.
- Prefer `enum` over `bool` when a parameter's purpose is not obvious; for `bool`, consider an inline comment at the argument.
- Reporting errors: Use `Option` for simple expected failures without useful details; `Result` for recoverable failures requiring context; `panic!` for bugs and violated invariants. Individual codebases may require `Result` even for invariants. Developer tools must report expected errors normally; tests may panic for failed assertions. For `catch_unwind`, see the separate restriction in [implementation.md](implementation.md).
