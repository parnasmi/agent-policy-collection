# Secrets

**[RULE_SERVER_SECRETS] — CRITICAL**
Secrets never reach the browser. No private credentials, tokens, API secrets, or
privileged configuration in client code, in props passed to a client component,
or in anything serialised into the page.

Any environment variable exposed to the client is public information. Treat it as
published the moment it is read — it is in the bundle, and the bundle is
downloadable.

A secret needed by the client is not a secret. It needs a server route that holds
the credential and exposes only the operation.
