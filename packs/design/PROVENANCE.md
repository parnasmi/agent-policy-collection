# Provenance — design

`design-guide` is a guide skill: heuristics, not rules, so nothing here is an
observed-failure record for a rule. This file records where the heuristics come
from and why each one ships. Never vendored into consuming projects.

## Sources

Ideas drawn from a software design book, part 1 (citation to be added). The
wording and every example are original; nothing is quoted.

| Guide section | Source chapters |
| --- | --- |
| `layers.md` | Functional Core, Imperative Shell · The Layers in Every Application · How Can a Function Know Something? · Clean Architecture in React |
| `dependencies.md` | Knowledge and Responsibilities · Dependency Injection |
| `review.md` | the two above, as a checklist |
| `SKILL.md` "what would someone have to know" | How Can a Function Know Something? |

## Why each heuristic ships

Pressure scenarios, 2026-11-19. Text-only answers, `core` in context, no
guide; one run per scenario, Sonnet and Haiku.

| Heuristic | Baseline failure |
| --- | --- |
| New logic gets its own unit | Asked to add a discount to an existing route handler, the model inlined the lookup and the maths into the handler. `RULE_MINIMAL_CHANGE` read as "put it where you are"; the rule was removed from core in v2.0.0 for this, and the failure stayed (see below). |
| A new edge only translates | Asked for a pay route, both models wrote one handler of about sixty lines: validation, queries, discount maths, and Stripe and Twilio SDK calls. |
| Components render; hooks decide | Asked to add error and retry to a page that fetches in an effect, the model grew request state, a retry counter, and the fetch inside the page. |
| SDKs and libraries sit behind a module you own | The pay route imported the Stripe and Twilio SDKs into the handler; a relative-time label imported dayjs and registered its plugin inside a presentational component. |

With the guide read, all four failing scenarios passed on both models, and so
did three held-out scenarios from other domains (a sign-up with Slack, a paged
table, a Kafka consumer with SendGrid) that failed without it. Simple tasks — a
typo, a formatter, a plain read route, local input state — gained no layers.
"Logic the edge already held stays there" was added after Haiku moved existing
handler lines into a new core function.

## Reaching the guide

Rerun on v2.0.0, 2026-11-20, two runs each on Sonnet and Haiku: the discount in
the existing handler, error and retry in a page that fetches in an effect, and a
commission rule in a Feature-Sliced Design widget. With `core` only, the first
two failed in all four runs, as in the 2026-11-19 baseline, and three runs still
said they applied "smallest change" with that rule gone. With the skill list in
view, the agents loaded this guide in every task and all twelve passed. The fix
is the guide; what can fail is reaching it.

A description-only trigger test with the v2.0.0 pointer found the guide 9 of 9
times on Sonnet, twice, and 7 of 9 on Haiku, which missed a loading spinner and a
Retry button on a page. In v2.0.1 the pointer in `core.md` names this guide for
logic added to a handler, controller, consumer, job, page, or component. Haiku
then found it for the spinner in both runs. Retry still went to `tanstack-query`,
a skill from the test environment. Over-application checks passed with and
without skills: a typo, a cents formatter, an access check, a plain read route,
and local input state.

## Candidates — not shipped

Topics from the same source where the baseline showed no failure. Add one only
after a model fails that way, with the failure recorded above.

| Topic | Baseline |
| --- | --- |
| Naming: generic names, booleans, verb consistency, abbreviations | Names were specific in every run. |
| Control flow: guard clauses, `else` chains, lookup maps | Flat guard clauses, unprompted. |
| Extension: a flag parameter versus a new function | New function, unprompted. |
| Substitution: wrappers that accept the native element's props | Native props plus `variant`, unprompted. |
| Empty collections versus `null`; masking failures as empty data | `null` normalised to `[]`, network rejection kept. |
| Over-engineering a simple helper | One small function. |
| Restraint while fixing a typo in a smelly file | Only the typo changed. |
| Interface segregation, dependency injection and inversion, deep and shallow modules, patterns as intuition, client–server contract design, performance versus maintainability | Not yet tested. |
