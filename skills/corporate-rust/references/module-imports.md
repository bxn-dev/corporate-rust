# Modules and imports

A crate is a compilation unit with a root module; modules organize code and control visibility.

- Follow the existing crate layout. Use a module for internal organization; consider a separate crate when callers need to depend on the functionality independently.
- File and module names: Name leaf modules with a primary user-defined item after that item; other leaf modules can be named freely.
- Non-leaf modules: Include only module declarations and re-exports; a small amount of core code tying things together may be acceptable. `mod test` and cross-language bindings such as `#[cxx::bridge]` do not disqualify a module from being a leaf.
- Avoid glob imports. The following are optionally allowed: `use super::*` in inline modules, `use enum::*` in enum methods and functions primarily concerned with that enum, and merging C++ namespaces in a private module solely for that purpose. Third-party preludes are at the reviewer's discretion; avoid defining your own preludes.
- One module per `use`: Import items from only one module per statement; no `::` inside `{...}`.
- Prefer placing trait imports in functions unless their methods are used extensively throughout the file.
- Qualify free functions from other modules when the module name clarifies the call, as in `config::load()`. Import an unambiguous function directly when qualification adds no useful context or the module name collides.
- Normally import types and traits and use them unqualified. Exceptions: ambiguity (renaming is also possible), large groups of types, macros or generated code, and very large import blocks in code with many dependencies.
