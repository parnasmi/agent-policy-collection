# update

Pull a newer Collection release. Nothing is written before the developer has
seen what changes.

## 1. Check for drift first

```sh
.agents/bin/apc check
```

Any `DRIFT` line means someone hand-edited a file the updater owns. **Stop.** Do
not sync. Handle every drifted file per ADR-0007 (drift stops the update)
before continuing.

`MISSING` means a vendored file was deleted. Ask whether it should come back or
be dropped from the pack set.

`FORKED` is fine — `apc` preserves those automatically.

## 2. Resolve the new release

Clone per the shared setup. Compare `TAG` with `ref` in the lockfile. If equal,
say so and stop — there is nothing to do.

## 3. Show what changes

```sh
.agents/bin/apc diff --source "$DIR"
```

That lists which files differ. Then show the developer *what* differs, not just
that it does:

```sh
diff -u ".agents/skills/frontend-rules/rules/effects.md" \
        "$DIR/packs/frontend/skill/frontend-rules/rules/effects.md"
```

Also diff the core block, since it lands in `AGENTS.md` and is the part that is
always in context:

```sh
diff -u <(sed -n '/<!-- agent-policy:core/,/<!-- \/agent-policy:core -->/p' AGENTS.md \
          | sed '1d;$d') \
        "$DIR/packs/core/core.md"
```

Summarise in prose: rules added, rules changed, rules removed. A removed rule
matters most — say which one and that it will stop applying.

## 4. Apply

On confirmation, sync the **same pack set the lockfile already records** — read
`packs` from the lockfile rather than re-detecting. Adding a pack is a separate,
explicit request.

```sh
"$DIR/bin/apc" sync --source "$DIR" --target . \
  --ref "$TAG" --resolved "$SHA" $(packs from lockfile)
```

Then `apc check` to confirm, and report the new ref.

## 5. Handling drift from step 1

For each drifted file, show three things — the local version, the version the
lockfile says was installed, and the incoming upstream version — then offer:

- **keep mine** — `apc fork <path>`. Update skips it from now on. Say plainly
  that upstream fixes to that file will no longer arrive.
- **take upstream** — leave it tracked; the sync overwrites it. The local edit is
  gone, so make sure the developer has seen it first.
- **capture** — run `/agent-policy capture` on the edit so it becomes an upstream
  rule and stops being a fork. Prefer this when the edit looks generally useful
  rather than project-specific.
