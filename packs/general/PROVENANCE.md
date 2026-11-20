# Provenance — general

Why each rule exists. Never vendored into consuming projects; this file is for
maintainers deciding whether a rule still earns its place.

Every rule here moved from `frontend` on 2026-11-20, v2.0.0, with its ID
unchanged, because it holds outside React. TypeScript counts as universal in
this Collection, as `RULE_TYPE_SAFETY` in core already does. Where a rule's
subject was a React noun it was widened: "the UI" for "React" in
`RULE_AUTHORIZATION`, "a component or handler" in `RULE_BOUNDARY_NORMALIZATION`,
"the caller going away" in `RULE_ASYNC_SAFETY`, and the query-by-role line of
`RULE_TEST_BEHAVIOR` limited to UI tests.

Trigger test, 2026-11-20, Sonnet and Haiku: the core pointer plus the
`general-rules` and `frontend-rules` descriptions, twelve requests in a Next.js
repo. Both models chose `general-rules` for all nine requests that touch these
rules, React components included.

The rules came from `refs/llm-rules.md` in `bebb890` (the file is in history at
`5ddf881`). That source is a list of rules and records no failures, so the rules
predate the provenance invariant. A row that says **None recorded** is the first
candidate when pruning: once a model fails that way, record the failure here.

| Rule | Observed failure | Source |
| --- | --- | --- |
| `RULE_TYPE_REUSE` | None recorded. | `llm-rules.md` §11 |
| `RULE_RUNTIME_VALIDATION` | None recorded. | `llm-rules.md` §12 |
| `RULE_NO_HARDCODED_VALUES` | None recorded. Scoped against single-use constants and a shared `constants.ts`. | DFD `RULE_VARIABLES` |
| `RULE_ASYNC_SAFETY` | None recorded. | `llm-rules.md` §25 |
| `RULE_ERROR_HANDLING` | None recorded. | `llm-rules.md` §26 |
| `RULE_BOUNDARY_NORMALIZATION` | None recorded. | `llm-rules.md` §29 |
| `RULE_TEST_BEHAVIOR` | None recorded. | `llm-rules.md` §34 |
| `RULE_TEST_CHANGES` | None recorded. | `llm-rules.md` §35 |
| `RULE_SECURITY_INPUT` | None recorded. | `llm-rules.md` §31 |
| `RULE_AUTHORIZATION` | None recorded. | `llm-rules.md` §32 |
| `RULE_DEPENDENCY_RESTRAINT` | None recorded. | `llm-rules.md` §33 |
| `RULE_SIMPLICITY` | None recorded. | `llm-rules.md` §9 |
| `RULE_SIDE_EFFECTS` | None recorded. | `llm-rules.md` §36 |
| `RULE_COMMENTS` | None recorded. | `llm-rules.md` §37 |
| `RULE_DEAD_CODE` | None recorded. | `llm-rules.md` §38 |
