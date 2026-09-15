---
name: audience-mining
description: Convert event, group, or company-page audiences into deduplicated and ICP-filtered prospect lists using Wannalead.
---

# Audience mining

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Turn a relevant event, group, or company-page audience into an ICP-filtered
list. Treat audience membership as an affinity signal, not automatic intent.

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
selecting a source, use the confirmed company, ICP, persona, and audience
hypotheses available in the host. Read current CRM exclusions and prior
contact state from their authoritative systems when available. Keep the
deduplicated shortlist in the task result or server-side campaign state, not in
local Markdown. Propose proven audience learnings to the user before recording
them; never promote one low-yield or untested source automatically.

## Pipeline

1. Select the audience source:
   - `events.attendees` for an event audience.
   - `groups.members` for a topical group.
   - `companies.followers` for a company-page audience after resolving the
     company with `companies.resolve`.
2. Filter by title, seniority, company fit, and exclusions.
3. Deduplicate across sources.
4. Hydrate the survivors with `profiles.enrich`.

Inspect the exact source endpoint before calling:

```bash
wannalead endpoints events.attendees --examples --json-schema
wannalead endpoints groups.members --examples --json-schema
wannalead endpoints companies.followers --examples --json-schema
```

## Output

A deduplicated, ICP-filtered list of named people with the source audience
attached to each record.

## Guardrails

- Pair affinity with a stronger second signal before high-effort outreach.
- Cap page counts and score before hydrating large audiences.
- Exclude customers and already-contacted people.
- Stop on rate-limit or connection errors instead of retrying rapidly.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Discovery only: use the `warm-outreach` skill for any send.
