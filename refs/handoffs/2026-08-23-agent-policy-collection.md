# Handoff — agent-policy-collection

**Date:** 2026-08-23 · **State:** built, verified, **uncommitted**, **unpublished**

The whole system was designed in a grilling session and implemented in one pass.
Read these first — they are the design, and this document does not repeat them:

| Read | For |
| --- | --- |
| [`README.md`](../../README.md) | what it is, the pack table, the verbs, the layout |
| [`AGENTS.md`](../../AGENTS.md) | **the two invariants**, how to add a rule or a pack, the verification loop |
| [`CONTEXT.md`](../../CONTEXT.md) | vocabulary — Pack, Core block, Vendoring, Drift, Fork, Provenance, Verb |
| [`docs/adr/0001`–`0007`](../../docs/adr/) | why each structural decision went the way it did, and what was rejected |
| [`packs/core/PROVENANCE.md`](../../packs/core/PROVENANCE.md) | why each core rule exists, and the four DFD rules rejected outright |

## Where things stand

Committed in seven commits on `main`, working tree clean. The remote is
`git@github.com:parnasmi/agent-policy-collection.git`.

`README.md` gives agents the **HTTPS** clone URL deliberately, not the SSH one:
that URL is what `install` writes into every consuming project's lockfile as
`origin`, and `update` re-clones from it. HTTPS works for anyone on a public
repo; SSH would require every teammate to have keys configured. Change it in
both `README.md` and this file if the repository ever moves or goes private.

Two steps remain before the system does anything for anyone:

1. Push, and confirm the repo is **public** — a private repo breaks the
   bootstrap for teammates.
2. `git tag v1.0.0 && git push --tags`. `update` resolves to the latest tag; an
   untagged repo is installable but never updatable.

Then install into a real project to exercise it.

## The two things that must not break

Both are stated in `AGENTS.md`; they are repeated here because every future
modification is judged against them.

**`bin/apc` never learns about content.** It works entirely off the directory
convention (`packs/<pack>/core.md`, `packs/<pack>/skill/<name>/`). If a new rule,
pack, or framework seems to require editing `apc`, the convention is wrong — fix
that instead. This is the entire reason the maintenance burden stays flat, and it
is what was promised when an npm package with a CLI was ruled out.

**Every rule traces to an observed model failure**, recorded in the pack's
`PROVENANCE.md` in the same commit. Not a documented best practice — something a
model actually did. This is the property that makes the rules good, and
provenance is what makes a stale rule prunable later.

## Verification loop

Run this after any change to `bin/apc`, and after adding a pack:

```sh
sh -n bin/apc
T=$(mktemp -d); echo '# Proj' > "$T/AGENTS.md"
./bin/apc sync --source . --target "$T" --ref test agent-policy core frontend nextjs docs-explorer
./bin/apc check --target "$T"
python3 -c "import json;json.load(open('$T/.agents/agent-policy.lock.json'))"
```

If `apc` changed, also confirm the two behaviours the design rests on:

- **a fork survives a re-sync** — `apc fork <path>`, edit the file, sync again at
  a new `--ref`, confirm the edit is still there, still marked `forked`, and that
  the `.claude/` mirror carries the forked content too;
- **project prose above the markers is untouched** — the text before
  `<!-- agent-policy:core` must be byte-identical after a sync;
- **the bridge behaves in both modes** — default gives real directories under
  `.claude/skills`, `--symlinks` gives links, and replacing a link with a text
  file makes `check` report `BROKEN` and exit non-zero.

All were verified on 2026-08-23. The first two were broken in the first draft, so
neither is theoretical.

## Landmines found the hard way

- **Relative links inside a skill break once vendored.** `packs/agent-policy/skill/agent-policy/verbs/*.md`
  sits four levels below the repo root, but two levels below the *project* root
  after install. ADR references in the verbs are plain text for this reason — do
  not turn them back into links. Links *within* a skill directory are fine.
- **Markdown tables silently drop extra columns.** The verb table in `SKILL.md`
  shipped with three body cells against a two-cell header and rendered wrong.
- **`AGENTS.md` must be uppercase.** Lowercase works on a default macOS volume
  because APFS is case-insensitive and matches nothing on Linux CI, so Codex and
  opencode would find no instructions at all. `install` offers to `git mv` it.
- **An empty pack must not install an empty skill.** `install` deliberately skips
  packs with `"status": "empty"` and points at `seed` instead.
- **Never make symlinks the default bridge again.** It was the original design
  and was reversed before release — see ADR-0002 for why, and note that the
  failure it causes is silent, which is what makes it worth a rule.

## Open items

**Three packs ship empty on purpose** — `tanstack`, `fsd`, `turborepo`. Each has
a `SEED.md` that is the interview. This was a deliberate call: rules written from
official docs describe how a library works rather than how these projects use it,
and canonical Feature-Sliced Design would fight the customised layer set in use
here. Seed them from a real repository, not from documentation.

`naiton-web-application` (under `Projects/freelance/naiton/`) is the natural
first target for all three — it is Turborepo + pnpm + oxlint, has TanStack and a
React Native workspace, already carries a lowercase `agents.md`, and already uses
the `.agents/` + `.claude/` symlink pattern this system formalises.

**Two harnesses are inferred, not confirmed.** Claude Code, Codex, and opencode
discovery paths were verified against primary documentation. Antigravity's
`.agents/{skills,rules,agents}` came from secondary sources only, and Pi's
*project-level* skill path was never found documented — the `.agents/` convention
is assumed, following the machine-level `~/.pi/agent/skills → ~/.agents/skills`
mirror already in place. Neither is load-bearing for the other three. Worth
confirming if either harness starts being used seriously on a shared repo.

**A Claude Code plugin marketplace was considered and deferred**, not rejected —
see ADR-0001. It may be added later as a convenience layer over the same packs,
never as the source of truth.

**Windows drove the bridge design; it is still untested on an actual Windows
machine.** Most contributors to the target repos are on Windows and some use
Claude Code, so `.claude/skills` is mirrored as **real copies**, not symlinks —
git checks a committed symlink out as a text file wherever `core.symlinks` is
off, and Claude Code then shows no skill and no error. `sync --symlinks` opts
back in for an all-Unix repo, and `check` reports a text-file-where-a-link-
belongs as `BROKEN` with a non-zero exit. What remains unverified is only that a
real Windows clone behaves as the git documentation says; the mechanism and the
detection were both exercised on macOS by replacing a link with its target text.

`bin/apc` is `#!/bin/sh`, so Windows contributors need Git Bash or WSL to run
`install`, `update`, or `check` — but not to *use* an installed project, where
everything is plain files.

## If the next task is…

**Adding or changing a rule** — follow `AGENTS.md` § *Adding a rule*. The tier
decision is the one that matters: core costs context in every session in every
project, so almost everything belongs in a skill. Read a neighbouring rule file
and match its shape before writing.

**Seeding a pack** — read that pack's `SEED.md` and
`packs/agent-policy/skill/agent-policy/verbs/seed.md`. Exhaust the repository
before asking the developer anything; arrive with observations to confirm.

**Changing `apc`** — re-read the first invariant, then run the full verification
loop above including the fork and prose checks.

**Reworking the architecture** — the rejected alternatives are already written
down in the ADRs with the reasons. Read them before re-proposing one.

## Suggested skills

| Skill | When |
| --- | --- |
| `grill-with-docs` | Any change to the architecture. This is how the current design was reached — it grills one question at a time, verifies facts against live docs instead of guessing, and writes ADRs before implementation. |
| `domain-modeling` | Whenever a new term appears. `CONTEXT.md` is a glossary only — no implementation detail. |
| `context7-mcp` / the `docs-explorer` pack | Any claim about how a harness discovers skills or reads instructions. These change; the current facts were verified on 2026-08-23 and have a shelf life. |
| `write-a-skill` | Adding a new router skill, for the progressive-disclosure shape. |
| `code-review` | Before publishing, or after any `bin/apc` change. |

Do **not** reach for `improve` or `simplify` on the rule content. Rule prose is
tuned for a model reading it under load — terse, imperative, exemptions inline —
and general-purpose prose polish tends to soften exactly the parts that make a
rule land.
