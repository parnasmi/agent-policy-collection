## Development rules

These rules govern every change made in this repository. They are ordered by the
priority ladder at the end: when two rules appear to conflict, the higher entry
wins.

Task-specific craft rules are **not** here. They live in skills under
`.agents/skills/*-rules/`, whose `SKILL.md` is a table mapping what you are
touching to the rule file that covers it. Consult the matching skills before
writing code: `general-rules` for craft in any stack, plus the stack's own skill
for React, Next.js, TanStack, or architecture-layer code. Before adding a rule, a
calculation, request state, or an outside call to a handler, controller,
consumer, job, page, or component, also read `design-guide`.

### Scope restraint

**[RULE_TASK_FIDELITY] — CRITICAL**
Implement exactly what the task requires. Do not add unrequested features or
improve unrelated parts of the system. Every changed line must be explainable by
the requested task. Where several valid solutions exist, take the simplest one
that fully satisfies it.

**[RULE_NO_OPPORTUNISTIC_REFACTOR] — CRITICAL**
You will notice other code that could be improved. Leave it alone unless it is
necessary for this task, actively prevents a correct implementation, or would
introduce a concrete problem in the requested change. Improvements outside the
task stay outside the diff — mention them instead.

### Stability

**[RULE_NAME_STABILITY] — CRITICAL**
Do not rename existing variables, functions, components, hooks, types, files,
folders, API fields, routes, or public exports, and do not move files or
folders, unless the task explicitly requires it or a real correctness problem
demands it. Stable names and paths beat stylistic preference.

**[RULE_STYLE_CONSISTENCY] — CRITICAL**
Follow the conventions of the surrounding code — naming, formatting, imports and
exports, component and hook structure, error handling, data fetching, file
organisation, TypeScript idiom. Local consistency outranks generic style
preference. Never reformat unrelated code to match a personal preference.

**[RULE_ARCHITECTURE_CONSISTENCY] — CRITICAL**
Do not introduce a new architectural pattern where the project already has one.
Respect the existing boundaries between UI, business logic, data access, state,
utilities, features, and server versus client code. Do not bypass an established
abstraction because a shorter path exists, and do not move responsibilities
between layers unless the task is an architectural change.

**[RULE_API_STABILITY] — CRITICAL**
Do not change a public contract — field names, return shapes, error contracts,
route behaviour, function signatures — unless the task requires it. Identify
every consumer before changing anything shared.

**[RULE_LEGACY_BEHAVIOR_PRESERVATION] — CRITICAL**
When refactoring existing code, preserve observable behaviour exactly, including
edge cases and quirks that look like bugs. If a behaviour change seems necessary
or desirable, stop and ask the developer for explicit confirmation before making
it. A refactor whose diff changes what the code does is not a refactor.

### Evidence over assumption

**[RULE_INSPECT_BEFORE_CHANGE] — CRITICAL**
Before modifying code, read it, read its callers and consumers, read the related
types, and check for existing utilities, hooks, or components that already solve
the problem. Do not implement against an assumption about how the project
probably works.

**[RULE_NO_GUESSING] — CRITICAL**
When something is discoverable in the repository, discover it. Search for
implementations, consumers, types, tests, similar patterns, and configuration.
Evidence from the codebase beats recollection of how such things usually work.

**[RULE_ASSUMPTIONS] — HIGH**
If an assumption is load-bearing and cannot be verified, say so explicitly rather
than presenting it as fact. Prefer implementations that do not require the
assumption at all.

**[RULE_CLARIFY_BEFORE_ASSUMING] — HIGH**
If the task is ambiguous in a way that changes the implementation, ask before
building. Ask only about decisions that are genuinely the developer's to make — a
product choice, a trade-off, an intent. If the answer is discoverable in the
repository, discover it instead; see [RULE_NO_GUESSING].

**[RULE_USER_OVERRIDE] — CRITICAL**
When an instruction conflicts with a rule here, name the rule and ask for
explicit confirmation before proceeding. Never drop a rule silently. Once the
developer confirms, follow the instruction and note which rule was overridden.

### Restraint in design

**[RULE_REUSE_EXISTING] — HIGH**
Before creating a component, hook, utility, type, service, or store, check
whether the project already has one that fits. Extend or reuse existing
primitives rather than creating a parallel one.

**[RULE_ABSTRACTION_RESTRAINT] — HIGH**
Do not abstract for hypothetical future requirements. Clear duplication beats the
wrong abstraction when reuse is uncertain. Abstract when there is a concrete
repeated concept, not merely code that looks similar. This limits generality,
not placement: giving a new rule or calculation its own function at its first
use is not an abstraction.

**[RULE_ROOT_CAUSE] — HIGH**
Fix the underlying cause, not the symptom. Do not paper over a problem with
arbitrary delays, retries, duplicated state, forced rerenders, broad exception
swallowing, or type assertions.

### Code

**[RULE_TYPE_SAFETY] — CRITICAL**
Do not weaken the type system to make code compile. No `any`, `as any`,
`@ts-ignore`, or `@ts-nocheck` without a documented and unavoidable reason.
Prefer narrowing, generics, discriminated unions, type guards, and `unknown` over
`any`. Never use an assertion to hide a real type mismatch.
*Enforced by:* `@typescript-eslint/no-explicit-any`, `ban-ts-comment`.

**[RULE_NAMED_PARAMS] — MEDIUM**
A new function taking two or more parameters takes a single options object, so
call sites read as `addToCart({ productId, quantity })` rather than
`addToCart(456, 2)`.
Exempt: callbacks and predicates passed to array or library methods;
single-parameter functions; React component props, which are already named; and
signatures fixed by an external API.
Never retrofit an existing signature to satisfy this rule — that is a
[RULE_API_STABILITY] violation unless the task asks for it.

**[RULE_ENGLISH_OUTPUT] — HIGH**
Code identifiers, comments, commit messages, log output, and error strings are
written in English regardless of the language of the conversation. This does not
apply to user-facing UI copy, which belongs to the project's localisation setup.

### Finishing

**[RULE_VERIFICATION] — CRITICAL**
Before reporting a task complete, read the final diff line by line and ask: is
every change necessary; did I rename or move anything; did I reformat anything
unrelated; did I duplicate something that already existed; did I add an
unnecessary abstraction; did I change a public contract; did I weaken type
safety; did I introduce a race condition, a server/client boundary problem, or a
security or accessibility regression. Then run the project's typecheck, lint,
and relevant tests.
Never state that tests, typechecking, linting, or a build passed unless it was
actually executed and actually passed. If something was not run, say so.

### Priority ladder

When rules conflict: correctness, then security, then explicit task
requirements, then existing project architecture, then backward compatibility,
then type safety, then maintainability, then simplicity, then performance, then
stylistic preference.

> Make the correct change, and make it fit naturally into the existing
> codebase. The result should read as though written by someone who already knows
> this project, not by an outsider redesigning it.
