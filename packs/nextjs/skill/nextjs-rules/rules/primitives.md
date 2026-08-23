# Framework primitives

**[RULE_NEXT_PRIMITIVES] — HIGH**
Where Next.js provides the mechanism, use it: routing, layouts, metadata,
navigation, caching, route handlers, image optimisation, font handling. Do not
reimplement framework functionality — the reimplementation loses the prefetching,
streaming, and optimisation the framework was doing for you.

The reverse also holds. Do not force a Next.js abstraction where plain React or a
web platform API is simpler and sufficient. Not every fetch needs a route
handler, and not every piece of markup needs a layout.
