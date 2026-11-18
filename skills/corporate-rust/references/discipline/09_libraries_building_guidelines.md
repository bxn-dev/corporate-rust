# Building libraries

## Features

Library features must be additive, and all combinations must work when each feature works on the current platform. Use a `std` feature rather than `no-std`. Enabling a feature must not disable or alter a public item; adding variants to a `#[non_exhaustive]` enum is allowed. Features must not require users to enable dependencies manually or require a parent to suppress a child's feature.

## Usable builds

Libraries must work on supported platforms unless expressly platform- or target-specific. Non-platform crates must build on Tier 1 platforms without additional prerequisites beyond Rust and Cargo, including a C compiler and linker. A Tier 1 platform may be unsupported temporarily if abstractions, such as an internal platform layer and fallback, make later support straightforward.

Do not require extra tools or environment variables by default. Run generators before publication and include generated artifacts. Gate platform-specific dependencies with conditional compilation or opt-in features; these requirements apply transitively.

## Native bindings

For a `foo`/`foo-sys` pair, control the native build from `build.rs`, normally with `cc` rather than Makefiles or external build scripts. Make external tools optional, include native source and enough information to verify it (such as repository URL and revision hash), pre-generate bindings where possible, and support static and dynamic linking.

An independently usable build-system or tool crate can replace direct `cc` calls. If size limits require downloading source, use a server with availability comparable to the package registry, store and verify acceptable hashes, and allow alternative source roots for hermetic builds.
