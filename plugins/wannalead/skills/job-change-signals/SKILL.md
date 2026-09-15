---
name: job-change-signals
description: Find recent buyer-persona job changes and produce a precise, fresh trigger list using Wannalead search and enrichment endpoints.
---

# Job-change signals

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Find people who recently moved into a role you sell to. Use Wannalead endpoints
only and prioritize recency and fit over volume.

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
querying, use the confirmed company and ICP context available in the host. Read current
CRM exclusions and prior contact state from their authoritative systems when
available. Keep the trigger list in the task result or server-side campaign
state, not in local Markdown. A job change is prospect evidence, not a durable
ICP rule; propose any context change to the user before recording it.

## Path A: announcement posts

1. Pull recent job-change announcements:

   ```bash
   wannalead call job-changes.search -body '{"keywords":"starting a new position","date_posted":"past-week"}'
   ```

2. Resolve each relevant post with `posts.resolve` to identify its author.
3. Filter authors to the buyer persona and ICP.
4. Hydrate survivors with `profiles.enrich`.
5. Optionally confirm account fit with `companies.enrich`.

Keep the announcement phrase as the search intent. Apply persona filtering
after retrieval so recruiter and general hiring noise can be removed.

## Path B: structured recent starts

1. Search the target persona with `search.people`, paging conservatively.
2. Enrich returned public identifiers with `profiles.enrich`.
3. Filter current positions by `startYear` and `startMonth` for the desired
   recency window.

LinkedIn-derived start dates may only have month-level precision. Do not claim
day-level recency when the response does not provide it.

## Output

A short weekly list of named new-role holders matching the persona and account
criteria, with the observed change signal and its available recency.

## Guardrails

- Prefer 20–50 precise matches over a wide export.
- Remove open-to-work, skills-update, and recruiter noise.
- Treat freshness as the value and avoid stale-trigger outreach.
- Stop on rate-limit or connection errors; never retry-loop.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Never assemble LinkedIn IDs or call LinkedIn directly.
