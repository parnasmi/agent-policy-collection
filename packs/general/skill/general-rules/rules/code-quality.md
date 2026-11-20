# Code quality

**[RULE_SIMPLICITY] — HIGH**
Prefer straightforward code over clever code. Optimise for correctness, then
readability, then maintainability, then consistency — and for performance where
it materially matters, which is rarer than it feels. A compressed expression that
takes a minute to decode has cost more than it saved.

**[RULE_SIDE_EFFECTS] — HIGH**
A function should not modify state or data unrelated to its stated job. Prefer
predictable inputs and outputs. Do not mutate a shared object unless mutation is
intentional and consistent with how the surrounding code works.

**[RULE_COMMENTS] — MEDIUM**
Comments explain *why*, never *what*. Good comments record a non-obvious
constraint, a workaround and what it works around, an external limitation, or a
decision whose reasoning is not visible in the code.

A comment restating the line below it is noise that goes stale. Prefer making the
code say it.

**[RULE_DEAD_CODE] — HIGH**
Do not leave commented-out implementations, unused imports or variables, obsolete
branches, temporary debug logging, or helpers nothing calls any more.

Remove what your change made obsolete, when it is clearly safe and directly
related to that change. Removing unrelated dead code you happened to notice is a
[RULE_NO_OPPORTUNISTIC_REFACTOR] violation — mention it instead.

*Enforced by:* `no-unused-vars`, `no-console`, and `knip` for unreferenced
exports and files.
