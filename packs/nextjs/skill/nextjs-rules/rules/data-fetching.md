# Data fetching

**[RULE_DATA_FETCHING] — CRITICAL**
Follow the strategy the project already uses. Do not introduce a second way to
fetch the same kind of data.

Keep the layers distinct: server data belongs to the fetching layer, client state
is what genuinely only exists in the browser, derived state is computed, and URL
state lives in the URL. Do not copy server data into a global client store so it
can be read more conveniently — the copy goes stale and nothing tells you.

Caching and revalidation semantics differ between Next.js major versions, and
defaults have flipped between them. Before relying on a caching behaviour, check
it against the version in `package.json`.
