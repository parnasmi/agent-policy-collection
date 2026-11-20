---
name: design-guide
description: Design heuristics for where new logic belongs and what a unit should know about the rest of the system. Consult alongside frontend-rules or any stack skill before adding a rule, calculation, formatting, query, loading or error state, or outside call to an HTTP handler, controller, message consumer, job, page, or component; before importing a vendor SDK or a third-party library (dates, payments, email, SMS, analytics) into one of those or into business logic; and when reviewing a diff for design. A project's own architecture skill decides the names and places.
---

# Design guide

Heuristics, not rules. Each names a **signal** to look for, the **question**
that decides it, the **move**, and when **not** to make it. They inform
judgement; the obligations live in `AGENTS.md` and in rules skills.

The question behind all of them: **to change this unit, what would someone have
to know?** Anything on that list that is not the unit's job belongs in another
unit.

**Scope.** Apply them to code you write and to code the task already changes. A
design smell in code the task does not touch gets one line in your report, not
an edit — `[RULE_NO_OPPORTUNISTIC_REFACTOR]`. `[RULE_MINIMAL_CHANGE]` limits how
much existing code you rewrite; it does not decide where new code lives.

**Precedence.** `AGENTS.md`, the project's own skills, and the conventions
already in the codebase outrank this guide. Where the project names its layers,
use its names and its places.

**Read the file that matches what you are touching.** Each is short.

| Touching… | Read |
| --- | --- |
| Adding logic or request state to a handler, controller, consumer, job, page, or component | [heuristics/layers.md](heuristics/layers.md) |
| Importing or calling a vendor SDK or a third-party library outside a module that wraps it | [heuristics/dependencies.md](heuristics/dependencies.md) |
| Reviewing a diff for design | [heuristics/review.md](heuristics/review.md) |

If nothing matches, the rules in `AGENTS.md` are sufficient.
