# Server and client boundaries

**[RULE_NEXT_BOUNDARIES] — CRITICAL**
Run on the server unless a browser capability is required. Reach for a Client
Component only when you need browser APIs, event handlers, client-side state,
interactive hooks, or a client-only library.

Do not add `"use client"` to make an error go away. Read what the error is
telling you: usually a server component is being asked to do something
interactive, and the fix is to extract that interactive part into its own small
client component rather than converting the whole tree.

`"use client"` marks a boundary, not a file. Everything imported below it goes
into the client bundle too, so a single directive high in the tree can drag a
large server-oriented subtree — and its dependencies — into the browser.
