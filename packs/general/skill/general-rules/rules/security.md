# Security

**[RULE_SECURITY_INPUT] — CRITICAL**
Treat everything from outside the code as untrusted: user-generated content,
query parameters, cookies, storage, redirect targets, URLs, and API responses.

Be specific about the dangerous ones. `dangerouslySetInnerHTML` needs sanitised
input. A redirect target taken from a query parameter needs an allow-list, or it
is an open redirect. A URL rendered into `href` needs its scheme checked, or
`javascript:` works.

**[RULE_AUTHORIZATION] — CRITICAL**
Hiding a button or a route in the UI is UX, not security. The data was already
fetched, the route is still reachable, and the API still answers.

Every sensitive operation is authorised at the trusted boundary — the server, the
route handler, the database policy. Client-side checks exist so users are not
shown things they cannot use, and for no other reason.
