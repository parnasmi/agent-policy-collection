# agent-policy-collection

LLM development rules, skills, and subagents for frontend projects — vendored
into a project once, then kept in sync with a lockfile. One installed copy serves
Claude Code, Codex, opencode, Antigravity, and Pi.

---

## For an agent reading this repository

You were probably handed this URL and asked to set up a project. Do this:

```sh
DIR=$(mktemp -d)
git clone --quiet https://github.com/parnasmi/agent-policy-collection.git "$DIR"
TAG=$(git -C "$DIR" describe --tags --abbrev=0 2>/dev/null || echo main)
git -C "$DIR" checkout --quiet "$TAG"
```

Then read `$DIR/packs/agent-policy/skill/agent-policy/verbs/install.md` and
follow it. It tells you what to detect, what to ask, and what to write. Do not
improvise an install — the lockfile has to be right or nothing downstream works.

Everything after the first install runs from inside the project as
`/agent-policy <verb>`; this repository does not need to be cloned again except
by the verbs themselves.

---

## What gets installed

```
project/
├── AGENTS.md              <!-- agent-policy:core v1.0.0 sha256:… -->
│                          the core block, rewritten verbatim on update.
│                          Everything outside the markers is yours, forever.
├── CLAUDE.md              @AGENTS.md
└── .agents/
    ├── agent-policy.lock.json    ref + per-file hash + fork markers
    ├── bin/apc                   so update and check work offline
    └── skills/
        ├── agent-policy/         install · update · check · capture · seed · harden
        ├── frontend-rules/       React and TypeScript craft
        ├── nextjs-rules/         if it is a Next.js project
        └── docs-explorer/        documentation lookup
    .claude/skills/*               mirror of the above, for Claude Code
```

`.agents/` is canonical — Codex, opencode, and Antigravity read it natively.
Claude Code reads only `.claude/skills`, so `apc` mirrors the skills there as
real copies and marks them generated in `.gitattributes` so review collapses
them. Copies rather than symlinks because git checks a committed symlink out as
a *text file* on Windows unless `core.symlinks` is enabled on that machine, which
makes the skill vanish from Claude Code with no error. `sync --symlinks` opts in
where every contributor is on macOS or Linux. See
[ADR-0002](docs/adr/0002-agents-dir-is-canonical.md).

## Two tiers, because `AGENTS.md` has no imports

Codex, opencode, and Antigravity read `AGENTS.md` as flat text — only Claude
Code's `CLAUDE.md` expands `@path`. So the bytes in `AGENTS.md` *are* the
always-on budget, and it cannot be split across files.

| | Where | What | Cost |
| --- | --- | --- | --- |
| **Core** | inlined in `AGENTS.md` | rules that fire on every diff and have no trigger word — restraint, evidence, verification | every session |
| **Craft** | router skills | rules with a recognisable trigger — effects, types, testing, a11y | only when read |

A router skill's `SKILL.md` is a "touching X → read Y" table, not the rules. See
[ADR-0003](docs/adr/0003-two-tier-policy.md).

## Packs

| Pack | Status | Installed when |
| --- | --- | --- |
| `core` | populated | always |
| `agent-policy` | populated | always |
| `docs-explorer` | populated | always, unless declined |
| `frontend` | populated | `react` is a dependency |
| `nextjs` | populated | `next` is a dependency |
| `tanstack` | **empty** | detected → offers `seed` |
| `fsd` | **empty** | detected → offers `seed` |
| `turborepo` | **empty** | `turbo.json` → offers `seed` |

Three packs ship empty on purpose. Rules written from official documentation
describe how a library works, not how these projects use it — and canonical
Feature-Sliced Design would fight a customised layer set in every repo it landed
in. `/agent-policy seed <pack>` derives them from a real repository instead,
guided by that pack's `SEED.md`.

## Adding a rule

From inside whatever project made you notice the problem:

```
/agent-policy capture the model keeps deriving state in a useEffect
```

It drafts the rule from the diff in front of it, places it by tier and pack,
records the observed failure in `PROVENANCE.md`, and opens a PR here. Tag a
release and it reaches every project on its next `/agent-policy update`.

Rules come from observed failures. A rule with no failure behind it is cost
without benefit — `PROVENANCE.md` exists so that stays checkable.

## Layout

```
packs/<pack>/
  pack.json          detection hints and status — read by the skill, not by apc
  core.md            spliced into AGENTS.md          (core pack only)
  skill/<name>/      copied to .agents/skills/<name> (skill packs)
  SEED.md            the interview                   (empty packs)
  PROVENANCE.md      why each rule exists — never vendored
bin/apc              copy · symlink · hash · splice · diff. Nothing else.
docs/adr/            why it is built this way
CONTEXT.md           vocabulary
```

`bin/apc` knows nothing about rules, packs, or frameworks — it works off the
directory convention above. Adding a rule or a whole new stack never requires
editing it.

## Verbs

```
/agent-policy install     detect the stack, choose packs, write everything
/agent-policy update      pull a newer release, showing the diff first
/agent-policy check       verify installed files against the lockfile
/agent-policy capture     turn an observed failure into a rule, as a PR here
/agent-policy seed <pack> fill an empty pack from this repo's conventions
/agent-policy harden      report which rules the linter already enforces
```

Hand-edited a vendored file? `update` stops and offers three exits: keep it
(`fork`), take upstream, or `capture` it upstream so it stops being a fork.
Nothing is ever silently overwritten — [ADR-0007](docs/adr/0007-drift-stops-the-update.md).
