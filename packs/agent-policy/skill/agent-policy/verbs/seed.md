# seed

Fill in a stack pack that ships empty — `tanstack`, `fsd`, `turborepo` — by
deriving its rules from a repository that actually uses that stack.

These packs are empty on purpose. Rules written from official documentation
describe how a library works, not how this developer's projects use it, and the
FSD case is worse: canonical Feature-Sliced Design would fight a customised
layer set in every project it was installed into.

## 1. Read `SEED.md`

Each empty pack has `packs/<pack>/SEED.md` in the Collection. It lists what to
read in the repository, what to ask, and the candidate topics. **Follow it** —
it encodes what actually needs establishing for that stack.

## 2. Read the repository first

Exhaust the code before asking anything. Most of what a seed interview needs is
visible: the layer set is in the directory tree, the query-key convention is in
the query files, the task graph is in `turbo.json`.

Come to the developer with observations to confirm, not with an empty form:

> Found `src/{shared,entities,features,widgets}` — no `pages` or `processes`
> layer. `entities/*/index.ts` exports a public API and I found four
> cross-slice imports that bypass it, all in `widgets/`. Deliberate, or drift?

## 3. Ask what code cannot answer

The distinctions that matter are intent, and intent is not in the tree:

- which patterns are conventions versus historical accidents new code should not
  copy — ask about specific files;
- where the boundary sits when it is a judgement call;
- **what models get wrong repeatedly in this stack.** This is the important
  question. Everything else produces a description of the codebase; only this
  produces a rule worth its context.

## 4. Write only what the interview supports

An empty rule file is worse than a missing one — it takes up a router row and
returns nothing. Cover the topics that produced real answers and drop the rest.

State the project's actual vocabulary at the top of the skill. An `fsd-rules`
that references a `processes` layer this developer does not use is worse than no
pack at all.

## 5. PR it

Same flow as [capture](capture.md#4-open-the-pr): branch, write
`packs/<pack>/skill/<name>/SKILL.md` plus `rules/*.md`, record provenance in
`packs/<pack>/PROVENANCE.md`, set `"status": "populated"` in `pack.json`, open
the PR.

Then say plainly that the pack is not installed here yet — it arrives on the next
`/agent-policy update` after the release is tagged, or immediately via
`/agent-policy install` if this project has no install yet.
