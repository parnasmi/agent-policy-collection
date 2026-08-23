---
status: accepted
---

# The Collection ships no lint configuration

Mechanically checkable rules stay in the prompt and additionally name their
enforcer inline (`Enforced by: no-explicit-any, ban-ts-comment`). A
`/agent-policy harden` verb reads whatever linter the project already uses and
reports which rules are mechanically covered and which are only advisory. No
config fragments and no npm dependencies are shipped.

The premise that a lint rule *replaces* a prompt rule is wrong: the prompt rule
prevents the code from being written, the lint rule catches it afterwards.
Deleting `[RULE_TYPE_SAFETY]` because `no-explicit-any` exists means the model
writes `as any`, lint fails, and a turn is spent fixing what should never have
been written.

## Consequences

Shipping configs would mean maintaining fragments for ESLint flat config, ESLint
legacy, oxlint, and Biome — four surfaces per rule change — each of which would
collide with the config a real project already has. Consuming projects here
already use oxlint rather than ESLint, so an ESLint-shaped assumption would have
been wrong on day one.
