# Function signatures and constants

**[RULE_NO_HARDCODED_VALUES] — MEDIUM**
Magic numbers, URLs, timeouts, storage keys, query keys, and feature strings get
named constants at an appropriate scope — module-level for anything used more
than once, and colocated with what they belong to rather than piled into a shared
`constants.ts`.

Do not extract a constant that is used exactly once, in the file that defines it,
where the literal is already self-explanatory. `setTimeout(fn, 300)` inside a
debounce helper needs no `DEBOUNCE_DELAY_MS`. A constants file full of
single-use names is its own kind of mess.

The test: would someone changing this value need to find every other place it
appears? If yes, name it.

See also `[RULE_NAMED_PARAMS]` in `AGENTS.md`, which governs how new functions
take their arguments.
