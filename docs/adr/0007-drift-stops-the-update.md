---
status: accepted
---

# Drift stops the update and offers three exits

A vendored file whose hash no longer matches its lockfile entry is drift. `check`
and `update` refuse to touch it and present a three-way view — the local version,
the installed upstream version, and the new upstream version — with three exits:
keep the local version and mark it `forked` in the lockfile so `update` skips it
from then on; take upstream and discard the edit; or run `capture` to turn the
edit into a PR against the Collection so it stops being a fork.

Silent overwrite was rejected because a good local edit disappears with no path to
keep it. Auto-forking on edit was rejected because divergence becomes invisible —
upstream fixes a bug in a rule and the project never hears about it.

## Consequences

The third exit is the point: it is the mechanism by which a local edit made under
real pressure becomes an upstream rule, which is the same provenance path as
`capture`.
