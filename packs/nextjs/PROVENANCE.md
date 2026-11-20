# Provenance — nextjs

Why each rule exists. Never vendored into consuming projects; this file is for
maintainers deciding whether a rule still earns its place.

These rules came from `refs/llm-rules.md` in `bebb890` (the file is in history at
`5ddf881`). That source is a list of rules and records no failures, so the rules
predate the provenance invariant. A row that says **None recorded** is the first
candidate when pruning: once a model fails that way, record the failure here.

| Rule | Observed failure | Source |
| --- | --- | --- |
| `RULE_NEXT_BOUNDARIES` | None recorded. | `llm-rules.md` §20 |
| `RULE_SERVER_SECRETS` | None recorded. | `llm-rules.md` §21 |
| `RULE_NEXT_PRIMITIVES` | None recorded. | `llm-rules.md` §22 |
| `RULE_URL_STATE` | None recorded. | `llm-rules.md` §23 |

`rules/data-fetching.md` defines no rule. It defined `RULE_DATA_FETCHING` a
second time until 2026-11-20; the rule lives in `frontend`, and the file keeps
only the note that caching defaults differ between Next.js majors.
