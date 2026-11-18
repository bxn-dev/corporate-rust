# Formatting and tests

- Run the formatter used by CI. In a Cargo project, this is typically `cargo fmt --check`. Do not require formatter options unsupported by the project's toolchain.
- Use `#[rustfmt::skip]` only when formatting would obscure useful alignment or when working with generated code, not for personal taste.
- Use the project's test harness. Use `#[test]` for an ordinary unit test; if a test needs an async runtime, follow the pattern in nearby async tests.
