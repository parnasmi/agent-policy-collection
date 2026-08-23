# Dependencies

**[RULE_DEPENDENCY_RESTRAINT] — HIGH**
Do not add a dependency when the platform already provides the capability, when
the project already has one that fits, or when a small local implementation is
clearer and safer.

Before adding one, weigh maintenance status, bundle size, security surface,
compatibility with the project's versions, and whether it fits the conventions
already in place.

Never swap an existing library for another because the other is preferred. That
is a migration, and it needs to be a task of its own.
