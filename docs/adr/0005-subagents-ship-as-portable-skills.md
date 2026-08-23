---
status: accepted
---

# Portable subagents ship as skills that delegate

Subagents have three incompatible formats — Claude Code uses markdown with YAML
frontmatter in `.claude/agents/`, Codex uses TOML in `.codex/agents/`, opencode
uses markdown with a different schema in `.opencode/agents/`, and the Antigravity
and Pi formats are undocumented. Rather than maintain emitters for formats that
change independently and guessing at two of five, a subagent ships as a skill —
the one artifact all five harnesses read. `SKILL.md` carries the trigger and the
delegation instruction; `prompt.md` beside it carries the agent's actual
instructions. Where the harness has subagents the skill spawns one with
`prompt.md`; where it does not, the work runs inline and only the distilled answer
is reported.

## Consequences

`DocsExplorer` becomes the `docs-explorer` skill. Context isolation is preserved
where the harness supports it and degrades to an inline lookup where it does not,
rather than being unavailable.

Delegation is prompt-level, so it is advisory. A harness-native subagent file can
still be added later for a specific harness without changing the source of truth.
