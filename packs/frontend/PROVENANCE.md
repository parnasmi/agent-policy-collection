# Provenance — frontend

Why each rule exists. Never vendored into consuming projects; this file is for
maintainers deciding whether a rule still earns its place.

These rules came from `refs/llm-rules.md` in `bebb890` (the file is in history at
`5ddf881`). That source is a list of rules and records no failures, so the rules
predate the provenance invariant. A row that says **None recorded** is the first
candidate when pruning: once a model fails that way, record the failure here.

| Rule | Observed failure | Source |
| --- | --- | --- |
| `RULE_EFFECT_DISCIPLINE` | None recorded. | `llm-rules.md` §15 |
| `RULE_EVENT_LOGIC` | None recorded. | `llm-rules.md` §19 |
| `RULE_DERIVED_STATE` | None recorded. | `llm-rules.md` §14 |
| `RULE_REACT_DATA_FLOW` | None recorded. | `llm-rules.md` §13 |
| `RULE_HOOK_SAFETY` | None recorded. | `llm-rules.md` §18 |
| `RULE_REACT_OPTIMIZATION` | None recorded. | `llm-rules.md` §16 |
| `RULE_COMPONENT_RESPONSIBILITY` | None recorded. | `llm-rules.md` §17 |
| `RULE_ASYNC_UI` | None recorded. | `llm-rules.md` §27 |
| `RULE_DATA_FETCHING` | None recorded. Was also defined in `nextjs-rules`; deduplicated 2026-11-20, and `nextjs-rules` keeps only its caching note. | `llm-rules.md` §24 |
| `RULE_ACCESSIBILITY` | None recorded. | `llm-rules.md` §30 |

## Moved

To `general` on 2026-11-20, v2.0.0, because they hold outside React. IDs
unchanged; their rows are in `packs/general/PROVENANCE.md`:
`RULE_TYPE_REUSE`, `RULE_RUNTIME_VALIDATION`, `RULE_NO_HARDCODED_VALUES`,
`RULE_ASYNC_SAFETY`, `RULE_ERROR_HANDLING`, `RULE_BOUNDARY_NORMALIZATION`,
`RULE_TEST_BEHAVIOR`, `RULE_TEST_CHANGES`, `RULE_SECURITY_INPUT`,
`RULE_AUTHORIZATION`, `RULE_DEPENDENCY_RESTRAINT`, `RULE_SIMPLICITY`,
`RULE_SIDE_EFFECTS`, `RULE_COMMENTS`, `RULE_DEAD_CODE`.
