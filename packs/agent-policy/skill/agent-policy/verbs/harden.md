# harden

Report which rules this project's tooling already enforces, and which are
advisory only. **Audit, never install.** Do not add dependencies, edit lint
config, or add hooks unless explicitly asked afterwards.

## 1. Collect the claims

Every rule that can be mechanically checked carries an `*Enforced by:*` line
naming the lint rules that check it:

```sh
grep -rn "Enforced by:" .agents/skills/ AGENTS.md
```

That is the whole registry. There is no separate mapping file to fall out of
date — a rule's enforcement claim lives with the rule.

## 2. Find out what is actually on

Read the project's real config, whichever it uses:

| Linter | Config |
| --- | --- |
| oxlint | `.oxlintrc.json` |
| ESLint flat | `eslint.config.{js,mjs,ts}` |
| ESLint legacy | `.eslintrc{,.json,.js,.cjs}` |
| Biome | `biome.json`, `biome.jsonc` |

Resolve what presets actually enable — `plugin:react-hooks/recommended` turns on
`rules-of-hooks` and `exhaustive-deps` without naming either. Where a preset's
contents are not certain, check them rather than assuming; `docs-explorer` is
there for this.

Also check `tsconfig.json` for `strict`, `noUncheckedIndexedAccess`, and
`exactOptionalPropertyTypes`, and note whether typecheck, lint, and test actually
run in CI. A rule enforced by a linter nobody runs is not enforced.

## 3. Report

```
Enforced
  ✓ RULE_TYPE_SAFETY      oxlint: no-explicit-any, ban-ts-comment
  ✓ RULE_HOOK_SAFETY      oxlint: rules-of-hooks, exhaustive-deps

Advisory only — no tooling covers these
  ✗ RULE_DEAD_CODE        unused vars covered; unreferenced
                          exports and files are not (knip would)
  ✗ RULE_ACCESSIBILITY    no jsx-a11y configured
  · RULE_EFFECT_DISCIPLINE  nothing can lint this — judgement only

Not run in CI
  ! typecheck is in package.json but not in the pipeline
```

Distinguish the three honestly. `✗` means tooling exists and is not configured.
`·` means no tool can check it and the prompt rule is the only thing there is —
which is the normal case for most of the interesting rules, and not a gap.

## 4. Do not overreach

Adding `jsx-a11y` to a mature codebase surfaces hundreds of existing violations.
Say what enabling something would cost before proposing it, and let the developer
decide. If they ask you to enable something, that is a separate task under the
ordinary rules — only what the task needs, no opportunistic cleanup.
