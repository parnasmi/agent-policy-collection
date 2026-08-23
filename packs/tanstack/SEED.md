# Seeding the `tanstack` pack

This pack has no rules yet, by design. TanStack rules written from the official
documentation would describe how the libraries work, not how *this developer's*
projects use them — and a rule that does not come from an observed failure is a
rule nobody needed.

`/agent-policy seed tanstack` runs this interview against a real repository that
already uses TanStack, then opens a PR on the Collection.

## What to read first

Read the repository before asking anything. Most of these have answers in the
code:

- `package.json` — which TanStack packages, at which major versions
- the router definition — file-based or code-based, where routes live, how
  loaders and `beforeLoad` are used, whether search params are typed and validated
- the query setup — `QueryClient` defaults, `staleTime`/`gcTime` conventions,
  whether `queryOptions` factories are used, how query keys are constructed and
  where they live
- mutation call sites — invalidation vs `setQueryData`, whether optimistic
  updates are used and how rollback is handled
- table usage — controlled vs uncontrolled state, where column definitions live,
  whether server-side pagination or sorting is in play
- whether router loaders and Query are integrated, or used independently

## What to ask

Only ask what the code cannot answer.

1. Where query keys are defined, and whether there is a convention that must be
   followed rather than merely one that happens to exist.
2. Whether server state is ever mirrored into a client store, and if so, why —
   this is usually a `[RULE_DATA_FETCHING]` violation with a real reason behind it.
3. Which of the observed patterns are deliberate conventions and which are
   historical accidents that new code should not copy. Ask about specific files.
4. What models get wrong repeatedly in this stack. This is the important question
   and it is the only source of a genuinely good rule.

## Candidate topics

Cover only the ones the interview produced real answers for. An empty rule file
is worse than a missing one.

`query-keys.md` · `caching-and-invalidation.md` · `mutations.md` ·
`router-loaders.md` · `search-params.md` · `table-state.md` ·
`suspense-and-error-boundaries.md`

## Output

`packs/tanstack/skill/tanstack-rules/SKILL.md` — a router table in the same shape
as `packs/frontend`, plus one file per topic under `rules/`. Record the observed
failure behind each rule in `packs/tanstack/PROVENANCE.md`. Set
`"status": "populated"` in `pack.json`.
