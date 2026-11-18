# Naming and documentation

- Name methods in the standard library style; use `as_`, `to_`, and `into_` with their respective `self` conventions.
- Give lifetimes descriptive names, especially on types. Exceptions: extremely generic abstractions such as `Cow`, anonymous lifetimes `'_`, and single letters for local input/output relationships or higher-ranked bounds. Up to two such local lifetimes are fine; descriptive names are strongly recommended for more.
- Give generic parameters short names suited to their meaning; choose single letters based on the most important trait bound where possible. `T`/`U` suit largely unconstrained parameters.
- Extension traits take the `Ext` suffix.
- Initialization: Name constructors `new`, or `new_with_foo` when there are multiple, and `try_new` when returning `Result`; name value conversions `from_foo` with a corresponding `From` impl.
- Avoid project-specific abbreviations in public names. Use established forms such as `IpAddr`; otherwise spell out the word.
- Documentation: Add doc comments to public APIs, optionally using `missing_docs`. Consider examples in enough places to convey how the crate works; not every function needs one. Always document panics caused by malformed input, and clearly explain invariants of unsafe functions and traits. Use capitalization and punctuation; do not explain language basics.
- Comments should add information beyond the code: document the contract instead of restating the name. Self-explanatory enum variants need no separate comment.
- Use `///` for the following item, `//!` at the top of the file for the module or library, Markdown, and the official Rust doc-comment rules. Use `//` for implementation notes and TODOs not relevant to users; both comment types may be mixed. Prefer direct doc comments to macro-generated documentation.
