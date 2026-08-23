# Memoization

**[RULE_REACT_OPTIMIZATION] — HIGH**
Do not add `useMemo`, `useCallback`, or `memo` reflexively. Each one adds a
dependency array to keep correct and makes the code harder to read, in exchange
for a benefit that is usually zero.

Add memoization when there is a concrete reason:

- a genuinely expensive computation, measured;
- referential stability that something actually depends on — a dependency array,
  a memoized child, a subscription key;
- a rendering problem you have observed in a profiler;
- a library API that requires a stable reference.

"It might re-render" is not a reason. A re-render that renders the same output
cheaply is not a problem.
