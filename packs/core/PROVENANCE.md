# Provenance — core

Why each rule exists. Never vendored into consuming projects; this file is for
maintainers deciding whether a rule still earns its place.

| Rule | Observed failure |
| --- | --- |
| `RULE_TASK_FIDELITY` | Model asked to fix a bug also "improved" three neighbouring components. |
| `RULE_MINIMAL_CHANGE` | Diffs dominated by reformatting, burying the actual fix. |
| `RULE_NO_OPPORTUNISTIC_REFACTOR` | Model narrates "I also cleaned up X while I was there" on unrelated files. |
| `RULE_NAME_STABILITY` | Renaming to match model-preferred conventions, breaking call sites and API fields. Called out by the original habr author as one of the two rules that matter most. |
| `RULE_STYLE_CONSISTENCY` | Model imposes its own import ordering and component structure. Also called out by the habr author. |
| `RULE_ARCHITECTURE_CONSISTENCY` | New data-fetching pattern introduced alongside the existing one. |
| `RULE_API_STABILITY` | Return shape changed to something "cleaner" without checking consumers. |
| `RULE_LEGACY_BEHAVIOR_PRESERVATION` | Refactors that silently fix quirks downstream code depends on. Added 2026-08-23 at the developer's request. |
| `RULE_INSPECT_BEFORE_CHANGE` | Implementation written against an assumed project structure. |
| `RULE_NO_GUESSING` | Model recalls how a library "usually" works rather than reading the repo. |
| `RULE_ASSUMPTIONS` | Guessed behaviour reported as established fact. |
| `RULE_CLARIFY_BEFORE_ASSUMING` | From DFD `[RULE_AMBIGUITY]`. Twenty minutes spent confidently building the wrong thing. Scoped to exclude anything discoverable, to avoid fighting `RULE_NO_GUESSING`. |
| `RULE_USER_OVERRIDE` | From DFD. Model decides an instruction outranks a rule and drops the rule without saying so; discovered only in the diff. |
| `RULE_REUSE_EXISTING` | Third `formatDate` helper added to a repo that already had two. |
| `RULE_ABSTRACTION_RESTRAINT` | Generic factory introduced for a single call site. |
| `RULE_ROOT_CAUSE` | `setTimeout` added to make a race disappear. |
| `RULE_TYPE_SAFETY` | `as any` used to silence a real mismatch. |
| `RULE_NAMED_PARAMS` | Added 2026-08-23 at the developer's request, after https://tahazsh.com/blog/named-parameters-in-js/. Scoped: the article recommends named params universally including single-parameter functions and lists no downsides, which would make a model destructure callbacks and retrofit existing signatures. |
| `RULE_ENGLISH_OUTPUT` | From DFD. Prompting in Russian produces Russian identifiers and comments, which then ship. |
| `RULE_VERIFICATION` | "Tests pass" reported without any test having been run. Merged with the former `RULE_DIFF_REVIEW`, whose checklist is now the first half of this rule. |

## Rejected

| Rule | Why |
| --- | --- |
| DFD `RULE_FULL_CODE` | "Provide complete code including unchanged parts, no placeholders." A chat-era rule. In an editing agent it means rewriting whole files, which directly violates `RULE_MINIMAL_CHANGE`. |
| DFD `RULE_OPTIONS` | "Propose 2–3 approaches with pros and cons." Too heavy for every task; the harnesses already have plan modes for the cases that warrant it. |
| DFD `RULE_NAMING` type prefixes (`bDebugMode`, `sTargetDir`) | Hungarian notation. Conflicts with `RULE_STYLE_CONSISTENCY` in every TypeScript codebase. |
| DFD `RULE_VERSION_OUTPUT`, `RULE_STATE_PERSISTENCE`, `RULE_CONTROLLED_LOOPS`, `RULE_DEBUG` | Script and daemon oriented; no frontend equivalent. |
