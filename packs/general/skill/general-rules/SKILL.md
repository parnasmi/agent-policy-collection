---
name: general-rules
description: Craft rules that hold in any stack — types and runtime validation, function signatures and constants, overlapping async requests, error handling, normalising external data, testing, security, dependencies, and code quality. Consult before writing or reviewing any module, function, or test. Stack skills such as frontend-rules add to these.
---

# General rules

The always-on rules in `AGENTS.md` cover restraint and verification. These cover
craft that holds in any stack. Stack skills such as `frontend-rules` add to
them.

**Read the file that matches what you are touching.** Do not read all of them.
Each is short. If two apply, read both.

| Touching… | Read |
| --- | --- |
| Type definitions, generics, validation at a boundary | [rules/types.md](rules/types.md) |
| A new function signature, constants, magic values | [rules/function-signatures.md](rules/function-signatures.md) |
| Anything `async` — fetching, mutations, overlapping requests | [rules/async.md](rules/async.md) |
| Failure paths, `catch`, error messages | [rules/errors.md](rules/errors.md) |
| Turning external data — an API response, a payload — into the shapes the code uses | [rules/data-boundaries.md](rules/data-boundaries.md) |
| Writing or changing a test | [rules/testing.md](rules/testing.md) |
| User input, HTML rendering, redirects, authorisation | [rules/security.md](rules/security.md) |
| Adding, replacing, or removing a dependency | [rules/dependencies.md](rules/dependencies.md) |
| Comments, dead code, shared mutation, a clever expression | [rules/code-quality.md](rules/code-quality.md) |

If nothing matches, the always-on rules are sufficient. Do not invent a rule that
is not written down here.
