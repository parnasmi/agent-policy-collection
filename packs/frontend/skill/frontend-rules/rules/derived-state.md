# Derived state

**[RULE_DERIVED_STATE] — HIGH**
If a value can be computed from props, state, server data, or URL state, compute
it. Do not store it and keep it in sync.

```ts
// yes
const fullName = `${firstName} ${lastName}`;

// no
const [fullName, setFullName] = useState('');
useEffect(() => setFullName(`${firstName} ${lastName}`), [firstName, lastName]);
```

The second form has a render where `fullName` is stale, and a second source of
truth that can disagree with the first.

**[RULE_REACT_DATA_FLOW] — CRITICAL**
Keep state as close to where it is used as practical, and let it flow down. Do
not reach for global state when local state is enough, when the value can be
derived, or when it could live in a common parent.

Do not duplicate server data into a global client store. The data-fetching layer
already owns it, and the copy will drift.
