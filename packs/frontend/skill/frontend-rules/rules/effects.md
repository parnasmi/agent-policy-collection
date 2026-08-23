# Effects

**[RULE_EFFECT_DISCIPLINE] — CRITICAL**
`useEffect` synchronises React with something outside React. Use it for browser
APIs, subscriptions, timers, imperative third-party libraries, and external event
listeners — and clean each of those up.

It is not a general mechanism for application logic. Do not use an effect to
derive a value, to transform data, to react to a user action that a handler could
handle directly, or to copy one piece of React state into another.

The question that settles it: *is there a system outside React that needs to be
told about this?* If not, it is not an effect.

**[RULE_EVENT_LOGIC] — HIGH**
Logic caused directly by a user action belongs in the handler for that action —
`onSubmit`, `onClick`, `onChange` — not in an effect watching for the state the
action produced. An effect that fires "when the form was submitted" is a handler
written in the wrong place, and it will also fire on every other path that reaches
the same state.

*Enforced by:* `react-hooks/exhaustive-deps` catches stale closures but nothing
lints the misuse itself. This one is on you.
