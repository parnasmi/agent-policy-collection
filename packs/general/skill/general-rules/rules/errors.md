# Errors

**[RULE_ERROR_HANDLING] — CRITICAL**
Failure needs deliberate behaviour, not an empty `catch`. Decide what happens for
network failure, invalid data, an empty response, a timeout, a permission
failure, and an unavailable dependency.

Do not swallow an error silently unless you can say why in a comment. Do not
surface stack traces, internal identifiers, or backend messages to users — log
them, show something the user can act on.

A `catch` that only logs and continues is a decision. Make sure it is the
decision you meant.
