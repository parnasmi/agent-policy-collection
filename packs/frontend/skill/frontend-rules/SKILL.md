---
name: frontend-rules
description: Craft rules for React — effects, derived state, hooks, memoization, component shape, data fetching, async UI states, and accessibility. Consult before writing or reviewing any component or hook. Rules that hold outside React (types, errors, testing, security, dependencies, code quality) are in general-rules. Not for Next.js server/client boundaries (see nextjs-rules) or layer architecture (see fsd-rules).
---

# Frontend rules

The always-on rules in `AGENTS.md` cover restraint and verification, and
`general-rules` covers craft that holds in any stack. These cover React: what to
write, and what not to.

**Read the file that matches what you are touching.** Do not read all of them.
Each is short. If two apply, read both.

| Touching… | Read |
| --- | --- |
| `useEffect`, subscriptions, timers, event listeners | [rules/effects.md](rules/effects.md) |
| State that mirrors props, other state, or server data | [rules/derived-state.md](rules/derived-state.md) |
| Any hook — writing one, calling one, fixing its deps | [rules/hooks.md](rules/hooks.md) |
| `useMemo`, `useCallback`, `memo`, a perf complaint | [rules/memoization.md](rules/memoization.md) |
| Splitting, extracting, or creating a component | [rules/component-shape.md](rules/component-shape.md) |
| Loading, empty, error, and retry states of an async surface | [rules/async.md](rules/async.md) |
| Fetching server data, client stores, URL state | [rules/data-boundaries.md](rules/data-boundaries.md) |
| Interactive UI — buttons, links, forms, focus, dialogs | [rules/accessibility.md](rules/accessibility.md) |

If nothing matches, the always-on rules and `general-rules` are sufficient. Do
not invent a rule that is not written down here.
