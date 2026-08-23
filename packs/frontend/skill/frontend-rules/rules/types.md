# Types

**[RULE_TYPE_REUSE] — HIGH**
Reuse the project's domain and API types. Where a type is a transformation of an
existing one, derive it — `Pick`, `Omit`, `Partial`, `Required`, `Awaited`,
`ReturnType`, `Parameters` — rather than restating its fields, so it cannot drift
from its source.

The limit: if the derived type takes more than a glance to understand, an
explicit domain type is clearer. Derivation is for keeping a single source of
truth, not for demonstrating type-level cleverness.

**[RULE_RUNTIME_VALIDATION] — HIGH**
TypeScript types are erased at runtime and validate nothing. Validate untrusted
data where it enters the application: URL parameters, form data, API payloads,
external APIs, local and session storage, cookies, environment variables.

Do not add redundant validation for values already guaranteed by code you
control — that is noise, and it hides the boundaries that do matter.
