# Data fetching

**[RULE_DATA_FETCHING] — CRITICAL**
Follow the project's established data-fetching strategy. Do not add a second
approach for the same kind of data without a concrete reason.

Keep the four kinds of state distinct and do not copy between them:

- **server data** — owned by the fetching layer, cached, refetchable;
- **client state** — genuinely local, does not exist on the server;
- **derived state** — computed, never stored;
- **URL state** — navigation and anything shareable or bookmarkable.

Reshaping an API response for the UI: `[RULE_BOUNDARY_NORMALIZATION]` in
general-rules.
