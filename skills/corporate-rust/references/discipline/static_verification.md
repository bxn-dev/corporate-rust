# Static verification

Projects should use compiler lints, Clippy, rustfmt, dependency vulnerability auditing, feature-combination checks, unused-dependency detection, and Miri for unsafe code. These can run locally and should be part of check-in gates. Miri is not a proof of soundness.

In addition to defaults, projects should enable the compiler lints `ambiguous_negative_literals`, `missing_debug_implementations`, `redundant_imports`, `redundant_lifetimes`, `trivial_numeric_casts`, `unsafe_op_in_unsafe_fn`, and `unused_lifetimes` at warning level.

Enable the major Clippy groups `cargo`, `complexity`, `correctness`, `pedantic`, `perf`, `style`, and `suspicious`; `nursery` is optional because of possible false positives. Opt out of unwanted lints individually. Consider these restriction lints at warning level:

```text
allow_attributes_without_reason  as_pointer_underscore
assertions_on_result_states      clone_on_ref_ptr
deref_by_slicing                 disallowed_script_idents
empty_drop                       empty_enum_variants_with_brackets
empty_structs_with_brackets      fn_to_numeric_cast_any
if_then_some_else_none            map_err_ignore
redundant_type_annotations       renamed_function_params
semicolon_outside_block           string_to_string
undocumented_unsafe_blocks       unnecessary_safety_comment
unnecessary_safety_doc           unneeded_field_pattern
unused_result_ok
```

Allowing `literal_string_with_formatting_args` may be necessary for structured logging templates.
