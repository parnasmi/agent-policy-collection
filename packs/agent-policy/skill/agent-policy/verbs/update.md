# update

Pull a newer Collection release, or add a pack to this install. Nothing is
written before the developer has seen what changes.

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

Clone per the shared setup. Compare `TAG` with `ref` in the lockfile. If equal
and no pack is being added, say so and stop — there is nothing to do.

## 3. Resolve the pack set

Start from `packs` in the lockfile — never re-detect. Add:

- every pack the developer named for this update;
- every pack that a pack in the set `requires` in the new release, repeated
  until nothing new arrives. A release can add a requirement, so read
  `requires` from `$DIR/packs/*/pack.json`, not from memory.

Add a pack only if its `status` is `populated`; an empty one gets the `seed`
offer instead, as in `install`.

## 4. Show what changes

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

`apc diff` covers only files already in the lockfile. Name each pack step 3
added, the skill it brings, and why it arrives: named, or required by which
pack.

Summarise in prose: rules added, rules changed, rules removed. A removed rule
matters most — say which one and that it will stop applying. A `REMOVED` file
may have moved to another pack: search `$DIR/packs` for its rule IDs, and call a
rule moved, not removed, when the new pack set carries it.

## 5. Apply

On confirmation, sync the pack set from step 3:

```sh
"$DIR/bin/apc" sync --source "$DIR" --target . \
  --ref "$TAG" --resolved "$SHA" <pack set from step 3>
```

Then `apc check` to confirm, and report the new ref.

## 6. Handling drift from step 1

For each drifted file, show three things — the local version, the version the
lockfile says was installed, and the incoming upstream version — then offer:

- **keep mine** — `apc fork <path>`. Update skips it from now on. Say plainly
  that upstream fixes to that file will no longer arrive.
- **take upstream** — leave it tracked; the sync overwrites it. The local edit is
  gone, so make sure the developer has seen it first.
- **capture** — run `/agent-policy capture` on the edit so it becomes an upstream
  rule and stops being a fork. Prefer this when the edit looks generally useful
  rather than project-specific.
