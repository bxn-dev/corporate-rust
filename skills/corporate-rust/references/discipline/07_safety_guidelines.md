# Safety

`unsafe` on a function or trait is reserved for contracts whose violation risks undefined behavior, not merely dangerous operations. All safe abstractions must be sound for every permitted use, including unusual safe client code; there is no exception. If an operation cannot be safely encapsulated, expose an unsafe function and document its obligations. Internal safe functions may rely on invariants upheld elsewhere within the same module.

Use unsafe only for a valid reason: a novel abstraction, measured performance, or FFI and platform calls. Do not use ad-hoc unsafe merely to shorten safe code or bypass `Send` or lifetime bounds. Properly designed, sound abstractions may implement such mechanisms.

## Novel abstractions

- Prefer an established alternative where one exists; otherwise keep the abstraction minimal and testable.
- Test adversarial use, including panicking closures and misbehaving safe traits such as `Deref`, `Clone`, and `Drop`. Invalidate or poison an abstraction where necessary if a closure panics.
- Explain the safety of every unsafe use, follow Rust's unsafe code rules, and pass Miri including adversarial cases.

## Corporate rule: unsafe for performance

A safe alternative does not rule out `unsafe`. Use unsafe for performance only if a reproducible benchmark demonstrates a gain over the safe alternative on a representative workload. Encapsulate it and provide a `SAFETY:` argument establishing every unsafe operation's preconditions on every reachable path for all permitted inputs. Test boundary cases and run them under Miri. Tests and Miri do not prove soundness. If the gain or safety argument is missing, or Miri fails or cannot be run, retain the safe version.

## Performance safety details

Explain safety both for calls to unsafe methods and implementations of unchecked APIs.

## FFI

Prefer an established interop library when it avoids unsafe constructs. Follow unsafe code rules and document permissible call patterns and safety obligations for generated bindings.
