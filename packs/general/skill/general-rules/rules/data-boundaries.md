# Data boundaries

**[RULE_BOUNDARY_NORMALIZATION] — HIGH**
Convert external data into the shapes the application wants at a clear boundary —
the client, the query function, the loader — not scattered across the code that
consumes it.

A component or handler should not know that the API returns `created_at` as a
string, that `null` and `""` both mean absent, or that a list arrives wrapped in
`{ items }`. When transport quirks leak past the boundary, every consumer has to
re-learn them and the next API change touches twenty files instead of one.
