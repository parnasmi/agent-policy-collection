# Testing

**[RULE_TEST_BEHAVIOR] — HIGH**
Test observable behaviour and the contracts that matter. Avoid coupling a test to
internal implementation details, private state, exact internal call sequences, or
fragile markup structure — a test that breaks under a safe refactor is costing
you more than it protects.

In a UI test, query the way a user finds things: by role, label, and text, not
by class name or component internals.

**[RULE_TEST_CHANGES] — HIGH**
When fixing a bug, add or update a test that fails before the fix and passes
after, where that is practical. When adding behaviour, cover the success path,
the edge cases, and the failure paths that carry real risk.

Do not write tests to move a coverage number. A test that asserts nothing
meaningful is worse than no test — it implies protection that does not exist.
