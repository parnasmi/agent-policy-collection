# Data boundaries

**[RULE_BOUNDARY_NORMALIZATION] — HIGH**
Convert external data into the shapes the application wants at a clear boundary —
the client, the query function, the loader — not scattered across the components
that consume it.

A component should not know that the API returns `created_at` as a string, that
`null` and `""` both mean absent, or that a list arrives wrapped in `{ items }`.
When transport quirks leak into JSX, every consumer has to re-learn them and the
next API change touches twenty files instead of one.

**[RULE_DATA_FETCHING] — CRITICAL**
Follow the project's established data-fetching strategy. Do not add a second
approach for the same kind of data without a concrete reason.

Keep the four kinds of state distinct and do not copy between them:

- **server data** — owned by the fetching layer, cached, refetchable;
- **client state** — genuinely local, does not exist on the server;
- **derived state** — computed, never stored;
- **URL state** — navigation and anything shareable or bookmarkable.
