# Agent Policy Collection

A public repository of LLM development rules, skills, and subagents for frontend
and full-stack projects, vendored into consuming projects by an agent and kept in
sync with a lockfile. Harness-agnostic: Claude Code, Codex, opencode, Antigravity,
and Pi all read the same installed content.

## Language

### The two repositories

**Collection**:
This repository — the upstream source of all rules, packs, skills, and subagents.
_Avoid_: library, registry, template repo

**Consuming project**:
A frontend or full-stack repository that has had policy content installed into it.
_Avoid_: target repo, client project, downstream

### Units of content

**Pack**:
The smallest independently installable unit of policy. A pack is the core pack, a
stack pack, or a universal pack, and is selected as a whole — never partially.
_Avoid_: module, bundle, plugin, preset

**Core pack**:
The single always-on pack. Its content is inlined into the consuming project's
`AGENTS.md` and is byte-identical in every consuming project.
_Avoid_: base rules, global rules

**Stack pack**:
A pack installed only when the consuming project uses that stack. The roster is
`frontend`, `nextjs`, `tanstack`, `fsd`, `turborepo`. Each ships as a router
skill.
_Avoid_: domain pack, framework pack, optional pack

**Universal pack**:
A pack installed in every consuming project whatever its stack, because its
content holds in any language — unless the developer declines it. A universal
pack that another installed pack requires cannot be declined. The roster is
`general`, `design`, `docs-explorer`. Each ships as a skill.
_Avoid_: global pack, default pack

**Rule**:
One named, bracketed directive with a severity, e.g. `[RULE_NAME_STABILITY]`.
The atom of policy; rules are authored once and never duplicated across packs.

**Router skill**:
A skill whose `SKILL.md` is a short "touching X → read Y" table rather than the
rules themselves. The rules live in sibling files the agent reads on demand.
_Avoid_: index skill, dispatcher

**Guide skill**:
A router skill whose sibling files hold heuristics instead of rules — each a
signal, a question, a move, and when not to — with no rule IDs and no
severities. It informs judgement and obliges nothing; anything an agent must do
is a rule. Its sources are recorded in the pack's `PROVENANCE.md` in place of
observed failures.
_Avoid_: best-practice skill, handbook

### Installation

**Vendoring**:
Copying pack content from the Collection into a consuming project's `.agents/`
and committing it. The only supported install mechanism.
_Avoid_: linking, fetching, syncing (as a noun)

**Core block**:
The marker-delimited region of a consuming project's `AGENTS.md` that holds the
core pack. Rewritten verbatim on update; everything outside it is project-owned
and never touched.
_Avoid_: managed section, generated block

**Lockfile**:
`.agents/agent-policy.lock.json` in the consuming project. Records, per vendored
file, the Collection ref it came from and its content hash.

**Drift**:
A vendored file whose current hash no longer matches its lockfile entry — i.e.
someone hand-edited content the updater owns.
_Avoid_: divergence, conflict

**Harness**:
An agent CLI or IDE that reads the installed content. Currently Claude Code,
Codex, opencode, Antigravity, Pi.
_Avoid_: tool, client, agent (which means the model, not the program)

**Fork**:
A vendored file the developer has deliberately kept in its edited state. Recorded
in the lockfile so update skips it, and distinct from drift, which is unresolved.
_Avoid_: override, patch, local change

**Provenance**:
The concrete model failure a rule was written in response to — date, project, and
what went wrong. Lives in the Collection only and is never vendored.
_Avoid_: rationale, history

**Verb**:
One operation of the `agent-policy` skill: `install`, `update`, `check`,
`capture`, `seed`, or `harden`.
_Avoid_: command, subcommand
