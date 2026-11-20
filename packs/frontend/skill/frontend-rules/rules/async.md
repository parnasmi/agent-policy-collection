# Async

**[RULE_ASYNC_UI] — HIGH**
An async surface has more than two states. Account for the ones that apply:
initial, loading, success, empty, error, and retry. Empty is not an error, and a
first load is not the same as a refetch.

Never leave a state where the user cannot tell what is happening or what to do
next.

Overlapping requests and stale responses: `[RULE_ASYNC_SAFETY]` in
general-rules.
