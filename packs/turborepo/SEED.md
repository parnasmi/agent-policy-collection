# Seeding the `turborepo` pack

Per ADR-0004 this pack does more than hold Turborepo rules: its router is how a
monorepo maps a workspace path to the rules that apply inside it, because a
single root install is the only layout that behaves identically across harnesses.

`/agent-policy seed turborepo` derives it from a real monorepo.

## What to read first

- `pnpm-workspace.yaml` (or the `workspaces` field) — the workspace globs
- `turbo.json` — the task graph, which tasks have `dependsOn`, what is cached,
  what the `outputs` are, and any task-level `env` declarations
- each workspace's `package.json` — its name, and its stack, from its dependencies
- the root `package.json` — which scripts delegate to `turbo` and which do not
- whether internal packages are consumed as source or as built output, and
  whether they use `exports` conditions

## What to ask

1. Which workspaces are applications and which are libraries — the distinction
   changes what rules apply and is not always inferable from the path.
2. Whether a package may import from another package directly or only through its
   public entry point.
3. Which tasks must be run through `turbo` rather than directly in a workspace,
   and which commands are the real ones for typecheck, lint, and test.
4. Where a new shared utility goes, and what the bar is for creating a new
   package rather than adding to an existing one.

## Candidate topics

`workspace-map.md` — **required**; the path-to-rules table that ADR-0004 depends
on. Then, as the interview supports them: `task-graph.md` ·
`package-boundaries.md` · `dependency-placement.md` · `commands.md`

## The workspace map

`workspace-map.md` is the load-bearing file. It gives the agent, for each
workspace, its stack and the rule files that apply — so a single root install can
still behave correctly in a repo where `apps/web` is Next.js and `apps/admin` is
Vite plus TanStack.

```md
| Workspace | Stack | Also read |
| --- | --- | --- |
| `apps/web` | Next.js App Router | `nextjs-rules` |
| `apps/admin` | Vite, TanStack Router + Query | `tanstack-rules` |
| `apps/mobile` | Expo / React Native | `frontend-rules` only — no DOM rules |
| `packages/ui` | library, no app shell | `frontend-rules`, plus package-boundaries |
```

## Output

`packs/turborepo/skill/turborepo-rules/SKILL.md` plus `rules/`. Provenance in
`packs/turborepo/PROVENANCE.md`; set `"status": "populated"`.
