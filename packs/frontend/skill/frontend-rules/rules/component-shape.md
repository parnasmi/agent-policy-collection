# Component shape

**[RULE_COMPONENT_RESPONSIBILITY] — HIGH**
A component should have one clear responsibility. Extract logic or markup when
doing so materially improves readability, reuse, testability, or separation of
concerns.

Do not split a component to reduce its line count. Do not create a tiny component
that wraps simple local markup and hides it behind a name that says less than the
markup did. A 150-line component that reads top to bottom is better than six
30-line components that have to be assembled in your head.
