---
status: accepted
---

# Two tiers: an inlined core block and lazily-read router skills

`AGENTS.md` has no import mechanism — Codex, opencode, and Antigravity read it as
flat text, and only Claude Code's `CLAUDE.md` expands `@path`. The bytes in
`AGENTS.md` therefore *are* the always-on budget, and splitting it across files is
not possible. Policy is split by what the two mechanisms can actually do:

- **Core pack** — inlined into `AGENTS.md` inside `<!-- agent-policy:core … -->`
  markers, rewritten verbatim on update, byte-identical in every consuming
  project. Contains only rules that fire on every diff and have no trigger word to
  match on: the restraint rules, the evidence rules, and the exit checks.
- **Stack packs** — router skills. `SKILL.md` is a short "touching X → read Y"
  table; the rules live in sibling files read on demand. Contains the craft rules,
  which have a real trigger a model can recognise.

Rejected: severity as the tiering criterion (drags Next.js CRITICALs into a Vite
SPA's always-on context while leaving restraint rules like `REUSE_EXISTING` out),
and a per-project composed core block (makes "what changed upstream" stop being a
single diff).

## Consequences

Everything outside the markers in `AGENTS.md` is project-owned and never touched
by `update`, which is what makes the marker boundary load-bearing.

A rule in a skill only applies when the skill fires. Any rule that must hold
unconditionally has to be in core, whatever its severity label.
