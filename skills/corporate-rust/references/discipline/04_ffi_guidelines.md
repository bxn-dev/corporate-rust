# FFI and dynamic library state

When multiple Rust dynamic libraries are loaded into one application, share only portable state. Libraries must likewise accept and provide only portable data across foreign library boundaries.

Portable data has a well-defined representation such as `#[repr(C)]`, and:

- has no computational relationship with static or thread-local state or `TypeId`;
- contains no value, pointer, or reference to non-portable data. A reference to portable data inside non-portable storage can be valid.

How data is used matters: transmitting a `u128` can be portable, using it to exchange a transmuted `TypeId` is not. Each separately compiled library can have different Rust layouts, type IDs, statics, and thread-local state. Allocated Rust types, structures with default representation, and libraries relying on statics are particularly risky. Even a method call on an apparently shared object can execute code from one artifact against data and statics from another. Such sharing can corrupt data or cause undefined behavior.
