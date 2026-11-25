# Ownership and concurrency

- `Rc` and `Arc`: Prefer unique ownership and borrowing. Shared ownership is allowed when the alternative would be unreasonably complex to write, understand, or maintain; weigh this in review.
- Borrows over deep copies: Use `.clone()` for genuinely needed deep copies, not as a substitute for borrowing. Cloning can be pragmatic when borrowing relationships become excessively complex; consider performance. Cloning `Rc`/`Arc` is not a deep copy and is an expected way to use them.
- Poison: Never access the contents of a poisoned mutex.
- Match the surrounding execution model. In an async handler, move long blocking operations off the executor. If a service already owns a worker pool, use it for background jobs rather than starting a thread per request.
- Limit lock duration. If a task only needs a snapshot, copy the needed fields while holding the lock and release it before awaiting I/O.
