# Layers

Whatever the framework calls them, code plays one of three roles. These are
roles, not names — use the project's names and places for them.

| Role | Back end | Front end | Holds |
| --- | --- | --- | --- |
| **Edge** | route handler, controller, message consumer, job | page, component | protocol: read and validate input, call the core, turn its result or error into a response or markup |
| **Core** | service, use-case function | hook, model, plain module | decisions: rules, calculations, which steps run in which order |
| **Gateway** | repository, API client, SDK wrapper | API client | I/O: storage and outside services, translated into the core's shapes |

Calculations in the core take their inputs as arguments — the time, a record —
so they stay pure and testable without the I/O around them.

## New logic gets its own unit

**Signal** — the change adds a rule, a calculation, a lookup, or an outside
call, and the quickest place to type it is the handler or component already
open.

**Ask** — to change this rule later, would someone have to read request
parsing, status codes, or markup to find it?

**Move** — write it as a new function in the core, with its I/O in a gateway,
and call it from the edge. The edge's existing lines stay; it gains a call and,
if the new function can fail, a mapping from that failure to the response. Logic
the edge already held stays there too, even where it breaks these heuristics:
moving it is a refactor the task did not ask for, so mention it instead.

```ts
// core — orders/pricing.ts
export async function amountToCharge({ order, discountCode }: { order: Order; discountCode?: string }) {
  if (!discountCode) return order.totalCents;
  const discount = await discounts.findActiveByCode(discountCode);
  if (!discount) throw new InvalidDiscountError(discountCode);
  return Math.max(0, order.totalCents - discount.amountCents);
}

// edge — the handler gains one call; its catch maps InvalidDiscountError to 400
const amountCents = await amountToCharge({ order, discountCode: body.discountCode });
```

**Not when** — the addition is protocol itself: a status code, a header, a
prop, a branch of markup, or one expression that only means something at that
edge.

## A new edge only translates

**Signal** — you are writing a new handler, controller, consumer, or page, and
its body is growing past "parse, call, respond": queries, SDK calls, business
values computed inline.

**Ask** — would testing it take both a protocol test (status codes, markup) and
a business-rule test?

**Move** — shape it as: validate input → call one core function → map the
result and the core's typed errors to output. The core function sequences the
gateways; the edge never calls an SDK.

**Not when** — plain create, read, or update with no decision in it: the edge
may call the gateway directly. A core function earns its place by holding a
decision, not by forwarding a call.

## Components render; hooks decide

**Signal** — a component gains request state (loading, error, retry), a fetch,
or reshaping of server data.

**Ask** — does the markup need to know where the data comes from or when it
reloads?

**Move** — put fetching, request state, and reshaping in a hook, or in the
project's data layer, that returns exactly what the markup uses: the data, a
status, and actions such as `retry`. The component turns that into markup and
calls the actions from its event handlers. When the task already rewrites a
component's fetching, moving it into the hook is part of that change. The hook
uses the project's data-fetching strategy — `[RULE_DATA_FETCHING]` in
frontend-rules — rather than a new one.

```tsx
export default function ReportPage() {
  const { report, status, retry } = useCurrentReport();
  if (status === 'error') return <LoadError onRetry={retry} />;
  if (!report) return <Spinner />;
  return <ReportTable report={report} />;
}
```

**Not when** — state that exists only in the UI — open or closed, a draft
input, hover — stays in the component.
