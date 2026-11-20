# Data fetching

`[RULE_DATA_FETCHING]` in frontend-rules governs the strategy and the four kinds
of state. Next.js adds one thing to it.

Caching and revalidation semantics differ between Next.js major versions, and
defaults have flipped between them. Before relying on a caching behaviour, check
it against the version in `package.json`.
