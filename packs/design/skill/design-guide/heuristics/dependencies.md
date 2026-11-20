# Dependencies

## SDKs and libraries sit behind a module you own

**Signal** — a vendor SDK (payments, SMS, email, storage, analytics) or a
utility library is imported into a handler, a core function, or a component;
provider field names, plugin set-up, or credentials sit next to business logic
or markup.

**Ask** — to change this unit, would someone need to know which provider or
library is behind it?

**Move** — one small module owns the import, the configuration, and the
provider's vocabulary, and exports functions named in the application's terms.
Everything else calls those functions, so a provider change or an SDK upgrade is
an edit to one file. Plain exported functions are enough. Reuse a wrapper the
project already has — `[RULE_REUSE_EXISTING]`.

```ts
// gateways/sms.ts — the only file that knows the provider
import twilio from 'twilio';

const client = twilio(config.sms.accountSid, config.sms.authToken);

export function sendSms({ to, text }: { to: string; text: string }) {
  return client.messages.create({ to, from: config.sms.sender, body: text });
}
```

In UI code the move is the same at a smaller scale: `formatTimeAgo(iso, now)`
in a formatting module owns the date library and its plugins, and the component
calls the helper.

**Not when** — the module is itself the wrapper or helper for that library; the
library is built for the layer you are in (a UI kit in a component, the HTTP
framework in a handler, a validation library at the edge); or it is a
language-level utility the project already uses directly everywhere.
