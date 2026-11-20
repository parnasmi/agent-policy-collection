# Async

**[RULE_ASYNC_SAFETY] — CRITICAL**
Requests do not finish in the order they were started. Wherever asynchronous
operations can overlap, decide deliberately what happens on: a stale response
arriving after a newer one; the caller going away mid-flight — a component
unmounting, a client disconnecting; the same request being fired twice; a
request that should have been cancelled.

The common failure is a search or filter where the response for "ab" arrives
after the response for "abc" and overwrites it. If the project uses a data
library, that library already solves this — use its cancellation and keying rather
than hand-rolling a guard.
