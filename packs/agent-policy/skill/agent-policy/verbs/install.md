# install

First-time setup. Detect, confirm once, write. Do not interview the developer
about things the repository can answer.

## 1. Refuse if already installed

If `.agents/agent-policy.lock.json` exists, stop. This is `update`'s job — say so
and offer it. Re-running install would discard fork markers.

## 2. Detect

Read, do not ask:

| Signal | Source |
| --- | --- |
| React, Next.js, TanStack, Expo, versions | `package.json` dependencies |
| Monorepo shape | `turbo.json`, `pnpm-workspace.yaml`, `workspaces` |
| FSD | `src/{shared,entities,features,widgets}` — a hint only, confirm it |
| Linter | `.oxlintrc.json`, `eslint.config.*`, `.eslintrc*`, `biome.json` |
| Test runner | `vitest.config.*`, `jest.config.*`, `playwright.config.*` |
| Real commands | `package.json` `scripts` — the actual names, not assumed ones |
| Existing agent config | `AGENTS.md`, `CLAUDE.md`, `.agents/`, `.claude/` |

In a monorepo, detect per workspace as well as at the root. Per ADR-0004 the
install still goes at the root; the per-workspace stacks are what the
`turborepo` pack's workspace map will need.

## 3. Choose packs

Read `packs/*/pack.json` in the clone. Each has a `detect` block and a `status`.

- `agent-policy` and `core` always.
- A pack whose `detect` matches and whose `status` is `populated`: install it.
- A pack whose `detect` matches and whose `status` is `empty`: **do not install
  it.** An empty skill is noise. Report it as detected-but-unseeded and offer
  `/agent-policy seed <pack>` after install.
- Honour `requires`. Installing `nextjs` without `frontend` is incoherent.

## 4. Confirm — once

Print exactly this shape, then wait for a single yes:

```
Detected
  React 19.1, TanStack Router 1.x + Query 5.x
  FSD layers: shared, entities, features, widgets  (confirm?)
  pnpm + turbo, oxlint, vitest
  commands: pnpm typecheck · pnpm lint · pnpm test

Will install
  core            → AGENTS.md core block
  frontend        → .agents/skills/frontend-rules
  docs-explorer   → .agents/skills/docs-explorer
  agent-policy    → .agents/skills/agent-policy

Detected but not seeded (nothing installed)
  tanstack, fsd, turborepo   → /agent-policy seed <pack>

Will touch
  AGENTS.md    append core block, existing content untouched
  CLAUDE.md    create with @AGENTS.md
  .agents/     new
  .claude/skills/*   mirror for Claude Code
  .gitattributes     marks the mirror generated

Proceed?
```

Ask a real question only where detection was genuinely ambiguous — an empty
repo, or a layer tree you cannot classify. Never ask what `package.json` answers.

## 5. Write

```sh
"$DIR/bin/apc" sync --source "$DIR" --target . \
  --ref "$TAG" --resolved "$SHA" --origin "$ORIGIN" \
  agent-policy core frontend docs-explorer
```

`$ORIGIN` is the URL this Collection was cloned from — use the **HTTPS** form,
`https://github.com/parnasmi/agent-policy-collection.git`, not the SSH one. It is
written into the lockfile and is what `update` re-clones from on every machine,
including those of teammates who have no SSH key configured. Getting it wrong
bakes a URL nobody else can reach into the project.

The mirror into `.claude/skills` is real copies by default. Pass `--symlinks`
only if every contributor to this repo is on macOS or Linux — on Windows git
checks a committed symlink out as a text file unless `core.symlinks` is enabled
on that machine, and the skill then silently vanishes from Claude Code. When in
doubt, keep the default; `apc check` verifies the pair.

If `AGENTS.md` is lowercase (`agents.md`), rename it with `git mv` first. It
works on a case-insensitive macOS volume and silently matches nothing on Linux
CI, so Codex and opencode would find no instructions at all.

## 6. Report

State what was written, then:

- name any unseeded pack that was detected, and the `seed` command for it;
- offer `/agent-policy harden` to find which rules the linter already covers;
- if the repo has a git remote, say the change is ready to review as a normal
  diff. Do not commit unless asked.
