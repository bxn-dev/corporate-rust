# Library interoperability

Prefer standard-library types in public APIs. Avoid exposing third-party types when practical: they become part of the contract and can constrain upgrades. Sibling types within an umbrella crate may be exposed freely; relevant feature gates may expose integration types; ungated exposure should bring substantial benefit, such as important ecosystem interoperability. Some embedded or high-performance libraries may prefer `core`.

Types wrapping native handles should offer conversion escape hatches for interoperation, with `unsafe` where the operation imposes safety obligations. Document those obligations and support appropriate ownership-preserving or borrowed access to the native handle.

Public types should be `Send`; all produced futures, explicit or implicit, must be `Send`. Assert that explicit future types are `Send` and check at least the main async entry points. Most other types should be `Send`, since holding a non-`Send` value across an await makes the future non-`Send`. A type used instantaneously, with no reason to hold it across an await, may be an exception. Never bypass bounds with an unsound unsafe implementation.
