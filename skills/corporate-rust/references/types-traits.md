# Types and traits

- Use a newtype rather than an alias for meaningfully different semantics, a separate API, flexibility to change the underlying type, or when types are easy to confuse.
- Prefer named fields over tuples and tuple structs. Exceptions: single-field newtypes and very local, unambiguous contexts.
- Trait implementations: Foundational libraries should anticipate needs; applications should add implementations as needed.
- Implement standard traits when a method matches their purpose and behavior; an additional custom method for discoverability is allowed. Derive rather than implement manually when possible.
- Prefer generics over `dyn Trait` for library parameters; code size can justify an exception.
- Define traits only for generic programming, `impl Trait`/`dyn Trait`, or extension traits; otherwise use inherent implementations. Do not use traits merely to organize code.
- Extension traits add methods to foreign types, not for generics or dynamic dispatch; name them with `Ext`. Normally the team owning the original type should own the trait. Exception: extensions expected to be well known and especially valuable, such as in foundational libraries or frameworks. Distinguish these from input traits for multiple foreign types.
- Important traits: For public fields, consider traits implemented by all fields. Make error types `Copy` where possible; use `Copy` generally when it is expected to remain possible. Most types should implement `Clone`, except for things such as external resources. Implement `Default` for `new()` or another default state. Prefer `PartialEq` for `==` and `PartialOrd` for `<`/`>`; `Eq` requires reflexivity, and `Ord` a total order. If a type is likely to be used as a map key or cached, implement `Hash` and `Eq`; if it also has a clear total order, implement `Ord` and its prerequisites.
- Complement important `From` implementations that provide a primary construction path with an inherent `new`, `new_from_foo`, or `from_foo`.
- Options structs with entirely public fields, used primarily as a collection of function options, should implement `Default`.
