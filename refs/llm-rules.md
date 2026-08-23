# React & Next.js — Universal LLM Development Rules

These rules apply to all React and Next.js development tasks.

The goal is to produce **correct, minimal, maintainable, production-quality changes** while preserving the existing architecture and avoiding unnecessary modifications.

---

## 1. Task Fidelity

### [RULE_TASK_FIDELITY] — CRITICAL

Implement exactly what the task requires.

- Do not expand the scope without a concrete technical reason.
- Do not add features that were not requested.
- Do not "improve" unrelated parts of the system.
- If several valid solutions exist, prefer the simplest one that fully satisfies the task.
- Every changed line should be explainable by the requested task.

---

## 2. Minimal Changes

### [RULE_MINIMAL_CHANGE] — CRITICAL

Make the smallest change necessary to solve the task correctly.

Avoid:

- unrelated refactoring;
- unrelated formatting;
- unnecessary file movement;
- unnecessary renaming;
- speculative abstractions;
- rewriting working code without a clear benefit.

Before finishing, review the diff and remove changes that are not required.

---

## 3. Preserve Existing Names

### [RULE_NAME_STABILITY] — CRITICAL

Do not rename existing:

- variables;
- functions;
- components;
- hooks;
- types;
- interfaces;
- files;
- folders;
- API fields;
- routes;
- public exports;

unless renaming is explicitly required by the task or necessary to fix a real correctness problem.

Prefer stable names over stylistic preferences.

---

## 4. Preserve Existing Style

### [RULE_STYLE_CONSISTENCY] — CRITICAL

Follow the style and conventions of the surrounding code.

Preserve existing patterns for:

- naming;
- formatting;
- imports;
- exports;
- component structure;
- hook structure;
- error handling;
- data fetching;
- file organization;
- TypeScript conventions.

Do not reformat or rewrite unrelated code merely to match personal preferences.

Local project consistency is more important than generic style preferences.

---

## 5. Understand Before Editing

### [RULE_INSPECT_BEFORE_CHANGE] — CRITICAL

Before modifying code:

1. inspect the target code;
2. inspect its callers and consumers when relevant;
3. inspect related types;
4. inspect existing utilities/hooks/components that may already solve the problem;
5. understand the existing architecture and data flow.

Do not implement based only on assumptions about how the project probably works.

---

## 6. Respect Existing Architecture

### [RULE_ARCHITECTURE_CONSISTENCY] — CRITICAL

Do not introduce a new architectural pattern when the project already has an established one.

Follow existing boundaries between:

- UI;
- business logic;
- data access;
- state;
- utilities;
- features;
- server code;
- client code.

Do not bypass established abstractions merely because a shorter implementation is possible.

Do not move responsibilities between layers unless the task requires an architectural change.

---

## 7. Reuse Before Creating

### [RULE_REUSE_EXISTING] — HIGH

Before creating a new:

- component;
- hook;
- utility;
- helper;
- type;
- service;
- state store;
- abstraction;

check whether the project already contains something suitable.

Prefer extending or reusing existing project primitives when appropriate.

Do not create duplicate abstractions.

---

## 8. Avoid Premature Abstraction

### [RULE_ABSTRACTION_RESTRAINT] — HIGH

Do not introduce abstractions for hypothetical future requirements.

Avoid creating:

- generic factories;
- excessive configuration layers;
- unnecessary wrappers;
- base classes;
- generic hooks;
- helper layers;

unless they solve an actual repeated problem.

Prefer clear duplication over the wrong abstraction when reuse is uncertain.

Create an abstraction when there is a concrete reusable concept, not merely similar-looking code.

---

## 9. Keep Code Simple

### [RULE_SIMPLICITY] — HIGH

Prefer straightforward code over clever code.

Optimize primarily for:

1. correctness;
2. readability;
3. maintainability;
4. consistency;
5. performance where it materially matters.

Avoid compressed or overly clever expressions that make behavior harder to understand.

---

# TypeScript

## 10. Preserve Type Safety

### [RULE_TYPE_SAFETY] — CRITICAL

Do not weaken the type system to make code compile.

Avoid using:

```ts
any
as any
@ts-ignore
@ts-nocheck
```

unless there is a documented and unavoidable reason.

Prefer:

- proper types;
- narrowing;
- generics;
- discriminated unions;
- type guards;
- `unknown` over `any`.

Do not use type assertions to hide actual type mismatches.

---

## 11. Do Not Duplicate Source-of-Truth Types

### [RULE_TYPE_REUSE] — HIGH

Reuse existing domain and API types when appropriate.

Do not manually duplicate a type that can safely be derived using mechanisms such as:

```ts
Pick;
Omit;
Partial;
Required;
Awaited;
ReturnType;
Parameters;
```

However, do not create complicated derived types when an explicit domain type would be clearer.

---

## 12. Preserve Runtime Reality

### [RULE_RUNTIME_VALIDATION] — HIGH

Remember that TypeScript types do not validate runtime input.

Validate untrusted data at appropriate boundaries, especially:

- URL parameters;
- form data;
- API payloads;
- external APIs;
- local/session storage;
- cookies;
- environment variables.

Do not add redundant runtime validation for values that are already guaranteed internally.

---

# React

## 13. Follow React's Data Flow

### [RULE_REACT_DATA_FLOW] — CRITICAL

Prefer React's normal unidirectional data flow.

Do not introduce global state when:

- local state is sufficient;
- derived state can be calculated;
- state can naturally remain in a parent component.

Keep state as close as practical to where it is used.

---

## 14. Avoid Unnecessary State

### [RULE_DERIVED_STATE] — HIGH

Do not store values in state when they can be derived from existing:

- props;
- state;
- server data;
- URL state.

Prefer:

```ts
const fullName = `${firstName} ${lastName}`;
```

over synchronizing derived state with `useEffect`.

---

## 15. Use Effects Only for External Synchronization

### [RULE_EFFECT_DISCIPLINE] — CRITICAL

Do not use `useEffect` as a generic mechanism for application logic.

Use effects primarily for synchronizing React with external systems, such as:

- browser APIs;
- subscriptions;
- timers;
- imperative third-party libraries;
- external event listeners.

Avoid effects for:

- deriving values;
- reacting to user actions that can be handled directly;
- synchronizing one React state variable with another;
- data transformations.

Always clean up subscriptions, timers, and listeners when necessary.

---

## 16. Do Not Optimize React Prematurely

### [RULE_REACT_OPTIMIZATION] — HIGH

Do not add:

```ts
useMemo;
useCallback;
memo;
```

automatically.

Use memoization when there is a concrete reason, such as:

- expensive computation;
- meaningful referential stability requirements;
- measured rendering problems;
- library API requirements.

Do not make code more complex for theoretical performance gains.

---

## 17. Stable Component Responsibility

### [RULE_COMPONENT_RESPONSIBILITY] — HIGH

A component should have a clear responsibility.

Extract logic or UI when doing so materially improves:

- readability;
- reuse;
- testing;
- separation of concerns.

Do not split components merely to reduce line count.

Do not create tiny components that hide simple local markup without providing meaningful value.

---

## 18. Preserve Hooks Rules

### [RULE_HOOK_SAFETY] — CRITICAL

Follow React Hooks rules.

Hooks must:

- be called unconditionally;
- be called at the appropriate React level;
- have correct dependencies;
- not rely on stale closures unintentionally.

Do not disable hook dependency lint rules simply to silence warnings.

---

## 19. Event Logic Belongs in Events

### [RULE_EVENT_LOGIC] — HIGH

Logic caused directly by a user action should generally remain in the relevant event handler instead of being converted into an effect.

Prefer:

```ts
onSubmit();
onClick();
onChange();
```

for action-driven behavior.

---

# Next.js

## 20. Respect Server and Client Boundaries

### [RULE_NEXT_BOUNDARIES] — CRITICAL

In Next.js, explicitly respect server/client boundaries.

Prefer server-side execution when browser capabilities are not required.

Use Client Components only when functionality requires things such as:

- browser APIs;
- event handlers;
- client-side state;
- client-only libraries;
- interactive React hooks.

Do not add `"use client"` unnecessarily.

Do not accidentally pull large server-oriented component trees into the client bundle.

---

## 21. Keep Secrets Server-Side

### [RULE_SERVER_SECRETS] — CRITICAL

Never expose secrets through client-side code.

Do not send private credentials, tokens, API secrets, or privileged configuration to the browser.

Treat publicly exposed environment variables as public information.

---

## 22. Use Correct Next.js Primitives

### [RULE_NEXT_PRIMITIVES] — HIGH

Prefer framework-native Next.js mechanisms when they appropriately solve the problem.

Examples include:

- routing;
- layouts;
- metadata;
- navigation;
- server/client boundaries;
- caching;
- route handlers;
- image optimization;
- font handling.

Do not recreate framework functionality without a reason.

At the same time, do not force a Next.js abstraction where normal React or web platform functionality is simpler and sufficient.

---

## 23. Keep URL State in the URL

### [RULE_URL_STATE] — HIGH

State that represents navigation or shareable application state should generally remain in the URL.

Examples:

- search queries;
- filters;
- sorting;
- pagination;
- selected tabs when navigation-relevant.

Do not create duplicated React state that must constantly synchronize with URL state unless necessary.

---

## 24. Respect Data-Fetching Boundaries

### [RULE_DATA_FETCHING] — CRITICAL

Follow the project's established data-fetching strategy.

Do not mix multiple competing approaches for the same type of data without a concrete reason.

Clearly separate:

- server data;
- client state;
- derived state;
- URL state.

Avoid duplicating server data into global client state unnecessarily.

---

# Async Code

## 25. Handle Race Conditions

### [RULE_ASYNC_SAFETY] — CRITICAL

When asynchronous operations may overlap, consider:

- cancellation;
- stale responses;
- race conditions;
- component unmounting;
- duplicate requests;
- out-of-order completion.

Do not assume requests always finish in the order they were started.

---

## 26. Handle Failures Explicitly

### [RULE_ERROR_HANDLING] — CRITICAL

Failures must have deliberate behavior.

Consider appropriate handling for:

- network failures;
- invalid data;
- empty responses;
- timeouts;
- permission failures;
- unavailable dependencies.

Do not silently swallow errors unless intentionally justified.

Do not expose sensitive technical details to users.

---

## 27. Represent Async UI States

### [RULE_ASYNC_UI] — HIGH

When relevant, account for:

- initial state;
- loading;
- success;
- empty state;
- error state;
- retry behavior.

Avoid UI states where users cannot understand what is happening.

---

# APIs and Data

## 28. Preserve API Contracts

### [RULE_API_STABILITY] — CRITICAL

Do not change public API contracts unless the task explicitly requires it.

Preserve:

- field names;
- return shapes;
- error contracts;
- route behavior;
- function signatures;

when possible.

Consider all consumers before changing shared contracts.

---

## 29. Normalize at Boundaries

### [RULE_BOUNDARY_NORMALIZATION] — HIGH

Convert external data into application-friendly structures at clear boundaries.

Avoid spreading API-specific quirks throughout UI components.

UI components should not need to understand transport details unless that is explicitly their responsibility.

---

# Accessibility

## 30. Preserve Accessibility

### [RULE_ACCESSIBILITY] — HIGH

Do not reduce existing accessibility.

For interactive UI:

- prefer semantic HTML;
- preserve keyboard usability;
- provide accessible names;
- associate labels with form controls;
- use buttons for actions;
- use links for navigation;
- maintain sensible focus behavior.

Do not replace native accessible behavior with custom implementations without necessity.

---

# Security

## 31. Treat External Input as Untrusted

### [RULE_SECURITY_INPUT] — CRITICAL

Never assume external input is safe.

Be cautious with:

- HTML rendering;
- URLs;
- redirects;
- user-generated content;
- query parameters;
- cookies;
- storage;
- API responses.

Avoid introducing injection, XSS, authorization, or data-exposure vulnerabilities.

---

## 32. Authorization Must Not Depend on UI

### [RULE_AUTHORIZATION] — CRITICAL

Client-side visibility checks are UX behavior, not security boundaries.

Do not assume that hiding a button or route in React provides authorization.

Sensitive operations must be protected at the appropriate trusted boundary.

---

# Dependencies

## 33. Avoid Unnecessary Dependencies

### [RULE_DEPENDENCY_RESTRAINT] — HIGH

Do not install a new dependency when:

- the platform already provides the capability;
- the project already has a suitable dependency;
- a small implementation is clearer and safer.

Before adding a dependency, consider:

- maintenance;
- bundle size;
- security;
- compatibility;
- existing project conventions.

Never replace existing libraries merely because another library is preferred.

---

# Testing

## 34. Test Behavior, Not Implementation Details

### [RULE_TEST_BEHAVIOR] — HIGH

Tests should primarily verify observable behavior and important contracts.

Avoid tests that depend unnecessarily on:

- internal implementation details;
- private state;
- exact internal function calls;
- fragile markup structure.

Tests should remain useful after safe internal refactoring.

---

## 35. Add Tests for Meaningful Behavior Changes

### [RULE_TEST_CHANGES] — HIGH

When fixing a bug, prefer adding or updating a test that would fail before the fix and pass afterward when practical.

When adding behavior, test important:

- success paths;
- edge cases;
- failure paths;

based on the risk of the change.

Do not create tests merely to increase coverage numbers.

---

# Code Quality

## 36. Avoid Hidden Side Effects

### [RULE_SIDE_EFFECTS] — HIGH

Functions should not unexpectedly modify unrelated state or data.

Prefer predictable inputs and outputs.

Do not mutate shared objects unless mutation is intentional and consistent with the surrounding architecture.

---

## 37. Use Comments for "Why"

### [RULE_COMMENTS] — MEDIUM

Do not add comments that merely repeat the code.

Good comments explain:

- non-obvious constraints;
- architectural decisions;
- workarounds;
- external limitations;
- unusual behavior.

Prefer clear code over explanatory comments when possible.

---

## 38. Do Not Leave Dead Code

### [RULE_DEAD_CODE] — HIGH

Do not leave behind:

- commented-out implementations;
- unused imports;
- unused variables;
- obsolete branches;
- temporary debug logs;
- abandoned helpers.

Remove code made obsolete by the requested change when it is clearly safe and directly related to that change.

---

# Verification

## 39. Verify Before Completion

### [RULE_VERIFICATION] — CRITICAL

Before declaring the task complete:

1. inspect the final diff;
2. check for unrelated changes;
3. check for type errors;
4. run relevant tests when available;
5. run linting when appropriate;
6. verify imports and exports;
7. check affected call sites;
8. consider relevant edge cases.

Never claim that tests, typechecking, linting, or builds passed unless they were actually executed successfully.

---

## 40. Review the Final Diff

### [RULE_DIFF_REVIEW] — CRITICAL

Perform a final self-review of all changed lines.

Ask:

- Is every change necessary?
- Did I accidentally rename anything?
- Did I change formatting unnecessarily?
- Did I duplicate existing functionality?
- Did I introduce unnecessary abstraction?
- Did I change a public contract?
- Did I weaken type safety?
- Did I create a server/client boundary problem?
- Did I introduce a race condition?
- Did I introduce a security or accessibility regression?

Remove unnecessary changes before finishing.

---

# Agent Behavior

## 41. Do Not Guess About Existing Code

### [RULE_NO_GUESSING] — CRITICAL

When information can be discovered from the repository, inspect the repository instead of guessing.

Search for:

- implementations;
- consumers;
- types;
- tests;
- similar patterns;
- existing utilities;
- configuration.

Prefer evidence from the codebase over assumptions.

---

## 42. Distinguish Facts From Assumptions

### [RULE_ASSUMPTIONS] — HIGH

If an important assumption cannot be verified, make it explicit.

Do not present guessed behavior as established fact.

When possible, design the implementation so that unnecessary assumptions are avoided.

---

## 43. Fix Root Causes

### [RULE_ROOT_CAUSE] — HIGH

Prefer fixing the underlying cause of a problem instead of hiding its symptoms.

Do not add:

- arbitrary delays;
- unnecessary retries;
- duplicated state;
- forced rerenders;
- broad exception swallowing;
- type assertions;

simply to make a symptom disappear.

---

## 44. Avoid Opportunistic Cleanup

### [RULE_NO_OPPORTUNISTIC_REFACTOR] — CRITICAL

While working on a task, you may notice other code that could be improved.

Do not change it unless:

1. it is necessary for the current task;
2. it directly prevents a correct implementation; or
3. leaving it unchanged would introduce a concrete problem in the requested change.

Potential improvements outside the task should remain outside the diff.

---

# Priority Order

When rules appear to conflict, use this priority:

1. **Correctness**
2. **Security**
3. **Explicit task requirements**
4. **Existing project architecture**
5. **Backward compatibility**
6. **Type safety**
7. **Maintainability**
8. **Simplicity**
9. **Performance**
10. **Stylistic preference**

---

# Core Principle

> Make the smallest correct change that fits naturally into the existing codebase.

The result should look as if it was written by a developer who already knows the project, rather than by an external agent attempting to redesign it.
