---
name: account-mapping
description: Map the buying committee and hiring signals at a named target account using Wannalead company and profile endpoints.
---

# Account mapping

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Map the buying committee at a named target account and read its hiring signal
so outreach can use shared account context.

## Required first-use gate

Before calling a business endpoint, invoke `wannalead-onboarding`. Its
`wannalead_call_endpoint` request for `me` must be the first Wannalead tool
call. Continue after identity and the required context are clear. Never let this task bypass the gate.

## Mandatory endpoint discovery

Before calling the API, run the complete discovery sequence:

```bash
wannalead endpoints
wannalead routes <filter>
wannalead endpoints <name> --examples --json-schema
wannalead openapi path <exact-/v1-route>
```

Scan the full catalog, search the domain and synonyms, then read every
candidate's complete contract before building a payload. Never rely on the
endpoint names below or say a capability is impossible until the live catalog,
plausible candidates, deprecations, and replacements have been checked.

## Durable context

Follow the progressive context rules from `wannalead-onboarding`. Before
querying, use the confirmed company and ICP context available in the host so
buyer, champion, evaluator, and exclusion criteria are explicit. Read
current CRM exclusions and prior contact state from their authoritative systems
when available. Return the committee as the task result or add it to an
explicitly authorized server-side campaign; do not maintain a local prospect
list. Treat one account's org chart as evidence, not a durable persona rule.

## Pipeline

1. Resolve the account with `companies.resolve` using a company URL or other
   accepted public company input.
2. Pass the resolved company to `companies.employees` with committee title
   keywords for the economic buyer, champion, and technical evaluator.
3. Read category-relevant hiring signals with `companies.jobs`.
4. Hydrate the committee shortlist with `profiles.enrich`.

Inspect these likely endpoint schemas before calling, plus any other candidates
found through the mandatory discovery step:

```bash
wannalead endpoints companies.resolve --examples --json-schema
wannalead endpoints companies.employees --examples --json-schema
wannalead endpoints companies.jobs --examples --json-schema
```

## Output

A named, enriched buying committee plus an account-level hiring-signal summary
that can support relevant outreach.

## Guardrails

- Map multiple stakeholders instead of depending on one contact.
- Treat hiring as a signal, not proof of budget or purchase intent.
- Keep employee page counts small and stop once the committee is covered.
- Stop on rate-limit or connection errors instead of retrying rapidly.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Never assemble LinkedIn IDs or call LinkedIn directly.
