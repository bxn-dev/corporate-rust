# Applications

Applications and repository crates used exclusively by an application may use an application-level error crate instead of defining error types. Once chosen, use one such type consistently rather than mixing application-level error types. Reusable libraries should use the error design in [library UX](12_libraries_ux_guidelines.md).

Applications should use a high-performance global allocator to improve allocation-heavy paths. Follow project requirements and assess the effect in the relevant workload.
