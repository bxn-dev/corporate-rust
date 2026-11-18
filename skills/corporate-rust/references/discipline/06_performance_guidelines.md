# Performance

Early in development, identify whether the crate materially affects performance or operating cost. If so, benchmark its hot paths, profile CPU and allocations regularly, and communicate performance-sensitive areas. Where possible measure CPU time across threads as well as wall time. Benchmark debug symbols can improve profiler output.

Look for growing or formatted strings, short-lived allocations, cloning collections, repeated hashing, and an unnecessarily expensive hasher. Measure changes rather than relying on estimates.

Optimize libraries for throughput, including items per CPU cycle, without neglecting latency. Partition work into reasonable chunks, process slices independently, sleep or yield while idle, use and offer batch APIs, and exploit cache locality. Avoid spinning, individual processing when batching is feasible, and work stealing for individual items. Share state only when sharing costs less than recomputation.

Long-running async CPU work should yield cooperatively at intervals, including between batches. Regular I/O await points may already yield. If work duration is unpredictable, use the hosting runtime's budget APIs where available. Balance responsiveness against switching and cache costs; 10–100 microseconds of CPU work between yields is a starting point, not a guarantee.

For performance-related unsafe code, apply the benchmark, safety argument, boundary testing, and Miri requirements in [safety](07_safety_guidelines.md).
