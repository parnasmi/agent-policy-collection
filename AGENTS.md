# AGENTS.md — working on the Collection

This repository is the upstream source of agent policy for frontend and
full-stack projects.
It is content, not an application: markdown rules, plus one small script that
moves them. See [README.md](README.md) for what it does and
[CONTEXT.md](CONTEXT.md) for what the words mean.

## The rules that govern this repo

Everything in [`packs/core/core.md`](packs/core/core.md) applies here too —
smallest change, no renaming, no opportunistic refactoring, verify before
claiming done. Read it. It is the product and it is also the policy.

## Two invariants

**`bin/apc` never learns about content.** It copies, symlinks, hashes, splices,
and diffs, working entirely off the directory convention below. If a change to a
rule, a pack, or a framework requires editing `apc`, the design has gone wrong —
fix the convention instead. This is the whole reason the maintenance burden stays
flat, and it is what was promised in
[ADR-0001](docs/adr/0001-vendor-policy-with-a-lockfile.md).

```
packs/<pack>/core.md         → spliced into the consuming AGENTS.md
packs/<pack>/skill/<name>/   → .agents/skills/<name>/
```

**Every rule has an observed failure behind it.** Not a documented best practice,
not a plausible concern — something a model actually did. Record it in the pack's
`PROVENANCE.md` in the same commit. A rule nobody can trace to a failure is a
rule competing for context without earning it, and provenance is what makes that
prunable later.

A guide skill holds heuristics, not rules, so this invariant does not reach it.
Record its sources in the pack's `PROVENANCE.md` instead, and promote a
heuristic to a rule only once a model has actually failed that way.

## Adding a rule

Prefer `/agent-policy capture` from the project where you saw the failure — it
drafts from the real diff and files the PR. By hand:

1. **Tier.** Core only if it fires on every diff with no trigger word. Core costs
   context in every session in every project, so the bar is high. Otherwise a
   skill.
2. **Pack.** `core` universal, `frontend` for React and TypeScript craft, the
   stack packs for stack-specific.
3. **Write it** as `**[RULE_NAME] — SEVERITY**` plus two to five lines. Say what
   to do, not only what to avoid. Put the exemptions in the rule — an unqualified
   rule gets over-applied. Add `*Enforced by:*` when a lint rule covers it;
   `harden` greps for exactly that string.
4. **Router row.** A rule file nothing points at will never be read.
5. **Provenance.** In the same commit.
6. **Tag a release.** Consuming projects resolve to the latest tag; an untagged
   merge reaches nobody.

## Adding a pack

Create `packs/<name>/` with a `pack.json` carrying `detect`, `requires`, and
`status`. Ship it `empty` with a `SEED.md` unless you have real observed rules —
an empty skill installed into a project is noise, and `install` deliberately
skips unseeded packs.

No change to `bin/apc`. If you think you need one, re-read the first invariant.

## Verifying a change

```sh
sh -n bin/apc                          # syntax
DIR=$(mktemp -d) && mkdir -p "$DIR"
./bin/apc sync --source . --target "$DIR" --ref test agent-policy core frontend
./bin/apc check --target "$DIR"
python3 -c "import json;json.load(open('$DIR/.agents/agent-policy.lock.json'))"
```

If `apc` changed, also verify all four behaviours the design rests on:

- a fork survives a re-sync, and the `.claude/` mirror carries the forked content;
- project prose above the markers is byte-identical afterwards;
- `--symlinks` still produces links, and `--copies` (the default) real directories;
- a symlink replaced by a text file — the Windows checkout — is reported as
  `BROKEN` by `check`, with a non-zero exit.

## Style

British-leaning prose, second person, no hedging. Rules are imperative. Do not
pad a rule to look substantial — the shortest form that cannot be misread is the
correct length, and every word is charged to a context window somewhere.
