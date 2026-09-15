---
name: icp-lookalikes
description: Turn strong seed customers or buyer profiles into scored lookalike accounts and matched decision-makers using Wannalead.
---

# ICP lookalikes

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Describe the ICP by strong examples, generate lookalike accounts or people,
then surface the matching buying personas. Use Wannalead endpoints only.

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

Follow the progressive context rules from `wannalead-onboarding`. Use the
confirmed company and named ICP context available in the host before choosing seeds.
Read current CRM exclusions from the authoritative CRM when available. Keep
lookalike results in the task result or server-side campaign state, not in
local Markdown. Preserve the existing ICP and propose new criteria to the user
before recording a durable context change.

## Pipeline

1. Choose the best seed path:
   - Account seed: resolve 3–5 strong customers with `companies.resolve`, then
     call `companies.lookalikes`.
   - Person seed: call `profiles.lookalikes` for a strong champion, then use
     `profiles.enrich` to map results to employers.
   - Filtered ICP: call `companies.search` with explicit category, geography,
     industry, size, and hiring criteria.
2. Rank candidate companies by ICP fit before hydrating people.
3. Surface decision-makers with `people.search` and company/title criteria.
4. Use the `account-mapping` skill when the full committee is needed.
5. Hydrate the final shortlist with `profiles.enrich`.

Example filtered search:

```bash
wannalead call companies.search -body '{"keywords":"<category>","company_hq_geo":"<geo>","industry_company_vertical":"<industry>","company_size":["51-200"],"has_jobs":true}'
```

## Output

A scored account list with matched decision-makers and the seed or criteria
that explain each match.

## Guardrails

- Seed quality matters more than seed volume.
- Score accounts before pulling people.
- Use `companies.lookalikes` for account seeds and `profiles.lookalikes` for a
  person seed; do not conflate the two.
- Stop on rate-limit or connection errors instead of retrying rapidly.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Never assemble LinkedIn IDs or call LinkedIn directly.
