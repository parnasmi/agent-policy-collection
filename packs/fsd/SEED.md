# Seeding the `fsd` pack

This pack has no rules yet, and canonical Feature-Sliced Design rules would be
actively wrong here: the developer runs a customised FSD, and rules taken from
feature-sliced.design would fight every project they were installed into.

`/agent-policy seed fsd` derives the rules from a repository that already uses
the developer's variant, then opens a PR on the Collection.

## What to read first

The layer set in use is the first thing to establish, and it is visible in the
directory tree:

- which layers exist, and under what names — canonical FSD is
  `app / processes / pages / widgets / features / entities / shared`, and most
  real setups drop or rename some of these
- the internal shape of a slice — which of `ui`, `model`, `api`, `lib`, `config`
  are used, and whether the names match canonical FSD
- whether slices have an `index.ts` public API, and whether cross-slice imports
  go through it or reach into internals
- the actual import direction between layers, measured rather than assumed —
  grep the imports in a few slices and see which way they point
- whether an import boundary linter is configured (`eslint-plugin-boundaries`,
  `@feature-sliced/eslint-config`, `steiger`, or oxlint's import rules), because
  that tells you which rules are already mechanically enforced and belong in the
  `Enforced by` annotation rather than as prose

## What to ask

1. Which layers are deliberate, which were inherited, and which are being phased
   out. Name the directories you found.
2. Where a new piece of code goes when it does not obviously belong to one layer
   — the entity-versus-feature call is where most FSD disagreements live.
3. Whether cross-imports within a layer are permitted, and under what conditions.
4. Which deviations from canonical FSD are intentional, so the rules can state
   them as the convention rather than as an exception.
5. What models get wrong repeatedly — typically putting code one layer too high,
   or importing across a slice's public API.

## Candidate topics

`layer-boundaries.md` · `slice-public-api.md` · `where-new-code-goes.md` ·
`cross-imports.md` · `shared-layer-discipline.md` · `naming.md`

## Output

`packs/fsd/skill/fsd-rules/SKILL.md` plus one file per topic under `rules/`.
State the layer set explicitly at the top of `SKILL.md` — a rule that references
a layer the project does not have is worse than no rule. Record provenance in
`packs/fsd/PROVENANCE.md` and set `"status": "populated"`.
