---
name: engagement-prospecting
description: Turn people engaging with relevant posts into a deduplicated, ICP-filtered warm prospect list through Wannalead endpoints.
---

# Engagement prospecting

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Turn people who engaged with a relevant post into an ICP-filtered warm list.
Use Wannalead endpoints only; pass user-facing post, company, profile, or
keyword inputs and let the API resolve execution details.

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
available. Keep the shortlist in the task result or server-side campaign state,
not in local Markdown. Treat the audience as evidence; propose any durable ICP
change to the user before recording it.

## Choose the entry point first

The post-first pipeline below works best when the ICP is defined by a topic
(a competitor, a category, a hashtag) and any engager can qualify. It is the
wrong start when the ICP names specific target accounts (e.g. "buyers at these
20 companies"): the engagers on a generic vendor/topic post are dominated by
that vendor's own employees and random topic followers, so named-account hit
rate is near zero.

When the ICP names specific accounts, go account-first instead:

1. Resolve each account with `companies.resolve`, then pull role-holders with
   `companies.employees` filtered by title keywords (e.g. procurement, energy,
   category, sourcing). This is the `account-mapping` path.
2. Verify recent topical engagement per person with `profiles.comments` — it
   returns the posts a member commented on (author + text + activityUrn), so
   keep only those who commented on on-topic content. Treat it as a warm
   signal, not a hard filter: senior buyers often comment little publicly, so
   an empty result does not disqualify a well-matched title.
3. Hydrate survivors with `profiles.enrich`.

Only fall back to the post-first pipeline for accounts with no engagement
signal, or when the ICP is topic-defined.

## Pipeline (post-first)

1. Find intent-bearing posts when no post is provided:

   ```bash
   wannalead call posts.search -body '{"keywords":"<category-or-competitor>"}'
   ```

2. Pull the relevant engagers from the selected post:
   - `posts.comments` for commenters.
   - `posts.reactions` for reactors.
   - `posts.reposters` for reposters.
3. Filter against the ICP: keep matching title, seniority, and company traits;
   exclude customers and already-contacted people.
4. Hydrate the survivors with `profiles.enrich`.

Each call may create a job. Use `--poll` when available or
`wannalead job -id <job_id>`.

## Output

A deduplicated, ICP-scored list of named people with the triggering post and
engagement type attached, ready for a personalized first touch.

## Guardrails

- Prioritize commenters, then reactors, then reposters for intent.
- Keep page counts conservative; a focused list beats a broad dump.
- Stop on rate-limit or connection errors instead of retrying rapidly.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Discovery only: use the `warm-outreach` skill for any send.
- Never assemble LinkedIn IDs or call LinkedIn directly.
