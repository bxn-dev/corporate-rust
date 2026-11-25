# Documentation

## Corporate rule: public library docs

Document each public library API item's contract, behavior, or units rather than restating its name. A self-explanatory enum variant needs no separate doc comment.

## API documentation

Start API docs with a summary sentence under 15 words. Extended documentation and examples are strongly encouraged. After the summary and any extended documentation, include the applicable sections:

- **Examples:** show usage.
- **Errors:** known failure conditions for a `Result`.
- **Panics:** when a panic can occur.
- **Safety:** every condition the caller must uphold if a function is unsafe or can otherwise cause undefined behavior.
- **Abort:** circumstances that can abort the process.

Explain parameters in prose rather than a parameter table. Public library modules must have `//!` documentation with a short first sentence. Cover relevant contents, when to use or avoid them, examples, subsystem specifications, observable effects and guarantees, and implementation details such as system APIs. Not every module needs every topic; explain important interactions between its types.

Use `#[doc(inline)]` on public re-exports of the crate's own items so they appear with their peers. Do not inline standard-library or third-party types; their external origin should remain visible. For export lists, follow [public re-exports](../module-imports.md#corporate-rule-public-re-exports).
