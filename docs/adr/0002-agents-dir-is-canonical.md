---
status: accepted
---

# `.agents/` is canonical; Claude Code gets a mirrored copy

`.agents/` holds the one authoritative copy of every vendored file. Codex,
opencode, and Antigravity read `.agents/skills` natively. Claude Code reads only
`.claude/skills`, so `apc` mirrors each installed skill there as a **real copy**,
records both hashes in the lockfile, and writes a `.gitattributes` line marking
`.claude/skills/**` as `linguist-generated=true` so review tools collapse it.

Instructions follow the same shape: `AGENTS.md` is the real file and `CLAUDE.md`
contains `@AGENTS.md` plus any Claude-specific lines, which is the bridge
Anthropic documents.

`apc sync --symlinks` bridges with symlinks instead. It is correct only where
every contributor is on macOS or Linux.

## Considered options

Symlinks were the original decision and were reversed before release. Git stores
a symlink as a mode `120000` blob whose content is the target path. Git for
Windows defaults `core.symlinks` to false, and with it false the checkout writes
that blob as an **ordinary text file**. `.claude/skills/frontend-rules` becomes a
35-byte file rather than a directory, Claude Code finds no `SKILL.md`, and the
skill disappears from the list with no error. Most contributors to the repos this
is built for are on Windows and some use Claude Code, so the default had to be
the one that cannot fail silently.

Per-machine `core.symlinks=true` plus Developer Mode does work, but it must hold
on every machine, and a clone taken before it is enabled stays broken — git does
not re-materialise the links when the config later changes. That is a permanent
support cost, paid by the people least able to diagnose it.

## Consequences

Every rule file exists twice in a consuming project and appears twice in a diff
that touches rules. The `.gitattributes` marker keeps that collapsed in review,
and `apc check` compares the pair, reporting a mirror edit with a note to change
the `.agents/` copy instead.

`AGENTS.md` must be uppercase. A lowercase `agents.md` works on a default macOS
volume because APFS is case-insensitive, and silently matches nothing on a
case-sensitive volume or in Linux CI.

`apc check` detects a symlink that git materialised as text and says so
explicitly, so a repo installed with `--symlinks` still fails loudly rather than
silently on Windows.

`bin/apc` is `#!/bin/sh`. Windows contributors need Git Bash or WSL to run
`install`, `update`, or `check` — but not to *use* an installed project, since
everything vendored is plain files.
