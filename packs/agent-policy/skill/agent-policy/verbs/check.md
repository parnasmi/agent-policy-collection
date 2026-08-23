# check

```sh
.agents/bin/apc check
```

Report the four states in plain language rather than pasting the output:

- **tracked** — matches the lockfile. Nothing to do.
- **drift** — the file was hand-edited. Explain that `update` will refuse to
  touch it until it is resolved, and offer the three exits from
  [update](update.md#5-handling-drift-from-step-1).
- **forked** — deliberately kept local. Name each one and note that upstream
  changes to it are not arriving.
- **missing** — a vendored file was deleted. Ask whether to restore it (re-run
  the sync) or drop the pack.

`check` exits non-zero when there is drift or something is missing, so it works
in a pre-commit hook or CI step. Suggest that only if the developer asks — do not
add hooks unprompted.

Nothing is written by this verb. If asked to "fix" what check reports, the fix is
`update`, `fork`, or `unfork` — never editing the lockfile.
