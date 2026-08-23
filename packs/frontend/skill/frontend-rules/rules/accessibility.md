# Accessibility

**[RULE_ACCESSIBILITY] — HIGH**
Never reduce accessibility that already exists.

For interactive UI: prefer semantic HTML; keep it keyboard-operable; give every
control an accessible name; associate labels with form controls; use `<button>`
for actions and `<a>` for navigation; keep focus behaviour sensible, especially
around dialogs and route changes.

Do not replace native accessible behaviour with a custom implementation unless
the native element genuinely cannot do the job. A `<div onClick>` is not a button:
it is not focusable, not keyboard-operable, and not announced.

*Enforced by:* `eslint-plugin-jsx-a11y` — partially. It catches missing alt text
and non-interactive handlers; it cannot see focus management or a name that is
technically present but meaningless.
