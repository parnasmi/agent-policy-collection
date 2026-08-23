# Hooks

**[RULE_HOOK_SAFETY] — CRITICAL**
Hooks are called unconditionally, at the top level of a component or another
hook, with correct dependencies, and without relying unintentionally on a stale
closure.

Never disable a hook dependency lint rule to silence a warning. The warning is
almost always describing a real bug — a value that will be stale, or an effect
that will not re-run when it should. If a dependency genuinely should not
retrigger the effect, restructure so that it is not a dependency: move it into a
ref, move the logic into a handler, or derive it inside.

*Enforced by:* `react-hooks/rules-of-hooks`, `react-hooks/exhaustive-deps`.
