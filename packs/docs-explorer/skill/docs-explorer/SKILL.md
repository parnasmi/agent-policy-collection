---
name: docs-explorer
description: Fetch current documentation for any library, framework, SDK, API, CLI tool, or cloud service — React, Next.js, TanStack, Prisma, Tailwind and the rest included. Use before answering questions about library API syntax, configuration, version migration, setup, or library-specific debugging, even when the answer seems known, because training data lags releases. Not for refactoring, writing scripts, debugging business logic, code review, or general programming concepts.
---

# Docs explorer

Documentation lookups are token-expensive and mostly waste: you fetch several
pages to extract three sentences. Run the lookup in a separate context wherever
the harness allows it, and bring back only the answer.

## Step 1 — delegate if you can

Read [prompt.md](prompt.md). It contains the full lookup procedure. Then, by
harness:

| Harness | How |
| --- | --- |
| Claude Code | `Agent` tool, `subagent_type: "general-purpose"`, passing `prompt.md` plus the specific question |
| Codex | spawn a subagent with `prompt.md` as its instructions |
| opencode | delegate to a subagent with `prompt.md` as its instructions |
| Antigravity, Pi, anything else | no delegation — go to step 2 |

Give the delegate the **question**, not just a library name. "How do I configure
staleTime per query in TanStack Query v5" beats "TanStack Query".

Batch every library into a single delegation so the lookups run in parallel.

## Step 2 — if you cannot delegate

Follow `prompt.md` yourself, then report **only the distilled answer** — the API
shape, the specific code, the version constraint. Do not paste fetched pages into
the conversation. The whole point of this skill is that the raw documentation
does not end up in the main context.

## Always

- Say which version you looked up, and where the answer came from.
- If the docs contradict what you were about to write, the docs win — say so.
- If nothing authoritative was found, say that rather than filling the gap from
  memory. An unsourced answer presented as documentation is worse than no answer.
