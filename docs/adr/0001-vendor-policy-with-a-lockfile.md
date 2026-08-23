---
status: accepted
---

# Vendor policy content into consuming projects with a lockfile

Policy content is copied into a consuming project's `.agents/`, committed to its
git history, and tracked in `.agents/agent-policy.lock.json` (Collection tag +
per-file hash). Rejected: fetching content at session start, because every
harness discovers skills by scanning the filesystem — a skill that is not on disk
does not exist. Also rejected: a git submodule, which puts a `--recursive` trap in
front of every teammate and CI job while still requiring the symlinks of ADR-0002.

## Considered options

A Claude Code **plugin marketplace** (`.claude-plugin/marketplace.json`) is the
best experience available for Claude Code specifically and needs nothing in the
project's git history. It was rejected as the primary mechanism because it is
Claude-Code-only: teammates on Codex, opencode, Antigravity, and Pi would need a
second, vendored code path, so the system would have two implementations that
drift. A marketplace may be added later as a convenience layer over the same
packs, never as the source of truth.

## Consequences

Rule changes arrive as reviewable diffs in a normal PR, which is the review gate
that lets updates track releases without ceremony. Vendored files are
upstream-owned; hand-editing one produces drift, handled per ADR-0007.
