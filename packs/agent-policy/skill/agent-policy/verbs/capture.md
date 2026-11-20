# capture

Turn an observed model failure into a rule in the Collection, without leaving the
project you are in. This is the verb that keeps rules grounded in real failures
rather than in what sounds sensible.

## 1. Establish the failure concretely

You need the actual case, not a paraphrase. Get it from what is in front of you —
the diff, the file, the conversation. If the failure is not in the current
session, ask for a concrete instance. **Do not write a rule from a hypothetical.**
A rule with no observed failure behind it is a rule nobody needed, and the
Collection already has enough words competing for context.

Establish: what the model did, what it should have done, and why the existing
rules did not prevent it.

That last question decides everything. Often the answer is that a rule *does*
cover it and was in a skill that never fired — in which case the fix is a better
`description` on that skill, or moving the rule to core. Say so instead of adding
a near-duplicate.

## 2. Place it

**Tier**, per ADR-0003 (two-tier policy):

- **core** if it fires on every diff and has no trigger word — a restraint,
  evidence, or verification rule. Core is expensive: every line is in context in
  every session in every project. The bar is high.
- **skill** if there is a recognisable trigger — "I am writing an effect", "I am
  adding a test". This is where almost everything belongs.

**Pack**: `general` for craft that holds in any stack; `frontend` for React
craft; `nextjs`, `tanstack`, `fsd`, `turborepo` for stack-specific; `core` for
universal.

**Severity**: CRITICAL if violating it breaks correctness, security, or a
contract. HIGH if it reliably produces worse code. MEDIUM for preference.

## 3. Write it

Match the existing shape exactly — `**[RULE_NAME] — SEVERITY**`, then two to five
lines of prose. Read a neighbouring rule file first and follow it.

Three things that separate a rule that works from one that gets ignored:

- **Say what to do, not only what to avoid.** A rule that only forbids leaves the
  model to invent a replacement.
- **Write the exemptions in.** An unqualified rule gets over-applied, which is
  how `[RULE_NAMED_PARAMS]` would have started rewriting callbacks.
- **Add `*Enforced by:*`** naming the lint rule, if one exists. `harden` reads
  those lines directly, so this is what makes the audit work.

If it belongs in a new topic file, add a row to the pack's `SKILL.md` router
table too — a rule file nothing points at will never be read.

## 4. Open the PR

```sh
DIR=$(mktemp -d); git clone --quiet "$ORIGIN" "$DIR"
cd "$DIR" && git checkout -b "rule/<slug>"
# write the rule file and the router row
# append the observed failure to packs/<pack>/PROVENANCE.md
git add -A && git commit -m "Add [RULE_X] to <pack>"
gh pr create --title "Add [RULE_X] to <pack>" --body "<the observed failure>"
```

The PROVENANCE entry is not optional. It is what lets a future maintainer decide
whether a rule still earns its place — models change, and a rule written for a
failure that no longer happens is pure cost.

If `gh` is unavailable, push the branch and print the compare URL. If there is no
remote, write the files into the clone and tell the developer where it is.

## 5. Afterwards

The rule is not in this project yet — it lands on the next `/agent-policy update`
after the PR merges and a release is tagged. Say that explicitly so nobody
expects it to be active now.
