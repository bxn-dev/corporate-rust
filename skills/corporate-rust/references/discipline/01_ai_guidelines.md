# APIs for AI-assisted use

- Make public and internal APIs idiomatic and compiler-checkable. Use strong types with strict, documented semantics.
- Document modules and public items for readers who know Rust and its standard library, but are not necessarily experts. Explain failures and link related items.
- Provide directly usable documentation examples and more elaborate repository examples. Make fallible examples convenient to use with `?`.
- Make client code testable, including through mocks, fakes, or features when appropriate. Cover observable behavior in the library's own tests.
- See [documentation](03_documentation.md), [resilience](11_libraries_resilience_guidelines.md), and [library UX](12_libraries_ux_guidelines.md).
