---
status: accepted
---

# Monorepos get a single root install

In a turborepo, policy is installed once at the repo root — one `AGENTS.md`, one
`.agents/`, one lockfile — and per-app variation is expressed inside the root
packs, via a `turborepo-rules` router that maps workspace path to the rule files
that apply there. Per-package installs were rejected because harness discovery is
asymmetric and would make the result depend on which directory a teammate happened
to start their agent in.

## Considered options

Two facts rule out putting policy below the root:

- **Nested `AGENTS.md` files shadow rather than merge.** The agents.md standard is
  "nearest wins", and opencode implements exactly that — an `apps/web/AGENTS.md`
  *replaces* the root one, core block included. Claude Code and Codex merge
  instead, so the same layout behaves differently per harness.
- **Skill discovery only reliably walks upward.** Codex and opencode scan `$CWD`
  and its ancestors. Claude Code additionally discovers `.claude/skills/` *below*
  cwd as `apps/web:skill-name`. So a package-level skill is invisible to Codex and
  opencode from the repo root.

Replicating the core block into every workspace's `AGENTS.md` would be correct
under shadowing, but lands the core rules in context two or more times in the
harnesses that merge, and multiplies the update surface by the workspace count.

## Consequences

A package may still carry its own `AGENTS.md` for humans and for sessions started
inside it. It is project-owned and must never contain the core block.
