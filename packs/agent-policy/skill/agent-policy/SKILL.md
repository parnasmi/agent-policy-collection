---
name: agent-policy
description: Install, update, verify, and extend this project's agent policy from the agent-policy-collection. Run as `/agent-policy <verb>` where verb is install, update, check, capture, seed, or harden. Use when asked to set up agent rules, refresh them, add a pack, record a new rule, seed a stack pack, or audit which rules are mechanically enforced.
disable-model-invocation: true
---

# agent-policy

Maintenance for this project's vendored agent policy. Never run a verb the
developer did not ask for.

| Verb | Does | Read |
| --- | --- | --- |
| `install` | First-time setup: detect the stack, choose packs, write everything | [verbs/install.md](verbs/install.md) |
| `update` | Pull a newer Collection release or add a pack, showing what changes first | [verbs/update.md](verbs/update.md) |
| `check` | Verify installed files against the lockfile | [verbs/check.md](verbs/check.md) |
| `capture` | Turn an observed model failure into a rule, as a PR upstream | [verbs/capture.md](verbs/capture.md) |
| `seed` | Fill in an empty stack pack from this repository's conventions | [verbs/seed.md](verbs/seed.md) |
| `harden` | Report which rules the project's linter already enforces | [verbs/harden.md](verbs/harden.md) |

Read only the verb file you were asked for.

## Shared setup

**Find `apc`.** Prefer `.agents/bin/apc` — it ships with the install, so update
and check work without network. Fall back to `<clone>/bin/apc` when there is no
install yet. `apc` handles copying, symlinks, hashing, splicing, and diffing;
you handle every decision. Never do its job by hand — hashes computed in your
head are wrong, and a missed one makes drift detection lie.

**Find the Collection.** Read `origin` from `.agents/agent-policy.lock.json`. If
there is no lockfile, the developer supplies the URL. Clone it shallowly to a
temp directory, at the latest tag unless told otherwise:

```sh
DIR=$(mktemp -d)
git clone --quiet "$ORIGIN" "$DIR"
TAG=$(git -C "$DIR" describe --tags --abbrev=0 2>/dev/null || echo main)
git -C "$DIR" checkout --quiet "$TAG"
SHA=$(git -C "$DIR" rev-parse --short HEAD)
```

Delete the temp clone when the verb finishes.

## Rules for every verb

- **Never edit a vendored file in place.** Everything under `.agents/` with a
  lockfile entry belongs upstream. Project-specific rules go in `AGENTS.md`
  *outside* the `<!-- agent-policy:core -->` markers, which no verb ever touches.
- **Never hand-edit the lockfile.** Use `apc fork` and `apc unfork`.
- **Report what actually happened.** If a step failed or was skipped, say so.
  Do not report an install as complete because the command exited quietly.
