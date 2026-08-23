---
name: nextjs-rules
description: Next.js-specific rules — server and client component boundaries, keeping secrets server-side, choosing framework-native primitives over reimplementation, URL state, and the App Router data-fetching and caching strategy. Consult before adding "use client", writing a route handler or server action, reading an environment variable, or fetching data in a Next.js app.
---

# Next.js rules

These sit on top of `frontend-rules`, which still applies to every component in a
Next.js app. Read that skill for React craft; read this one for what Next.js adds.

| Touching… | Read |
| --- | --- |
| `"use client"`, a server/client split, a bundle-size surprise | [rules/boundaries.md](rules/boundaries.md) |
| Environment variables, tokens, API keys, privileged config | [rules/secrets.md](rules/secrets.md) |
| Routing, layouts, metadata, navigation, images, fonts | [rules/primitives.md](rules/primitives.md) |
| Search, filters, sorting, pagination, selected tab | [rules/url-state.md](rules/url-state.md) |
| Fetching, caching, revalidation, route handlers, server actions | [rules/data-fetching.md](rules/data-fetching.md) |

Next.js changes fast and its caching semantics have changed across major
versions. If a caching or rendering question turns on version-specific
behaviour, verify it against the docs for the version in this project's
`package.json` — use the `docs-explorer` skill — rather than relying on
recollection.
