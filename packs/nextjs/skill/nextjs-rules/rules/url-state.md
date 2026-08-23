# URL state

**[RULE_URL_STATE] — HIGH**
State that describes *what the user is looking at* belongs in the URL: search
queries, filters, sorting, pagination, and a selected tab when the selection is
navigation-relevant.

The test is whether the state should survive a refresh, a shared link, or the
back button. If it should, the URL owns it.

Do not mirror URL state into React state and synchronise the two. That creates
two sources of truth that disagree during navigation, and it usually appears as
an effect watching `searchParams` — which is also a
`[RULE_EFFECT_DISCIPLINE]` violation.
