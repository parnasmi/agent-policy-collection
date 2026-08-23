---
name: frontend-rules
description: Craft rules for React and TypeScript work — effects, derived state, hooks, memoization, component shape, types, async and error handling, data boundaries, testing, accessibility, security, dependencies, and code quality. Consult before writing or reviewing any component, hook, or TypeScript module. Not for Next.js server/client boundaries (see nextjs-rules) or layer architecture (see fsd-rules).
---

# Frontend rules

The always-on rules in `AGENTS.md` cover restraint and verification. These cover
craft: what to write, and what not to.

**Read the file that matches what you are touching.** Do not read all of them.
Each is short. If two apply, read both.

| Touching… | Read |
| --- | --- |
| `useEffect`, subscriptions, timers, event listeners | [rules/effects.md](rules/effects.md) |
| State that mirrors props, other state, or server data | [rules/derived-state.md](rules/derived-state.md) |
| Any hook — writing one, calling one, fixing its deps | [rules/hooks.md](rules/hooks.md) |
| `useMemo`, `useCallback`, `memo`, a perf complaint | [rules/memoization.md](rules/memoization.md) |
| Splitting, extracting, or creating a component | [rules/component-shape.md](rules/component-shape.md) |
| Type definitions, generics, validation at a boundary | [rules/types.md](rules/types.md) |
| A new function signature, constants, magic values | [rules/function-signatures.md](rules/function-signatures.md) |
| Anything `async` — fetching, mutations, overlapping requests | [rules/async.md](rules/async.md) |
| Failure paths, `catch`, error UI | [rules/errors.md](rules/errors.md) |
| Turning an API response into something the UI consumes | [rules/data-boundaries.md](rules/data-boundaries.md) |
| Writing or changing a test | [rules/testing.md](rules/testing.md) |
| Interactive UI — buttons, links, forms, focus, dialogs | [rules/accessibility.md](rules/accessibility.md) |
| User input, HTML rendering, redirects, auth-dependent UI | [rules/security.md](rules/security.md) |
| Adding, replacing, or removing a dependency | [rules/dependencies.md](rules/dependencies.md) |
| Comments, dead code, shared mutation, a clever expression | [rules/code-quality.md](rules/code-quality.md) |

If nothing matches, the always-on rules are sufficient. Do not invent a rule that
is not written down here.
