# Design review

Look only at what the diff adds or changes. For each hit, report the location,
what the unit now knows that is not its job, and the move from the file named.

- An edge — handler, controller, consumer, job, page, component — gained a
  rule, a calculation, a query, request state, or an outside call →
  [layers.md](layers.md).
- A new edge would need both a protocol test and a business-rule test →
  [layers.md](layers.md).
- A vendor SDK or a utility library is newly imported into an edge or a core
  function → [dependencies.md](dependencies.md).

Code the diff does not touch is out of scope: note a smell there in one line,
under its own heading, and propose no change to it.
