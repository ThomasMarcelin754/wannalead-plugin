---
name: wannalead
description: Use Wannalead safely through the hosted MCP server for LinkedIn research, prospecting, campaigns, and explicitly approved outreach.
---

# Wannalead

Use the Wannalead MCP tools supplied by this plugin. Never install or invoke a
local Wannalead CLI, ask for a `wlk_` API key, read LinkedIn cookies, or call
LinkedIn directly.

## First-use gate

Invoke `wannalead-onboarding` before the first business operation in a new
conversation or workspace. Its first Wannalead call must use
`wannalead_call_endpoint` with `endpoint: "me"` so identity and workspace come
from the authenticated OAuth connection.

If the plugin is not authenticated, ask the user to reconnect Wannalead through
the host's plugin or connector settings. Never request an access token in chat.
If the API returns `linkedin_connection_required`, direct the user to
`https://wannalead.co/extension/connect`, then retry the same identity check.

## MCP tool map

- `wannalead_list_endpoints`: search the live endpoint catalog. Use an exact
  filter with `include_schema: true` and `include_examples: true` before a call.
- `wannalead_call_endpoint`: call a catalogued endpoint by canonical name or a
  relative `/v1/linkedin/*` path. Pass path/query values in `params` and JSON
  input in `body`.
- `wannalead_profile_resolve`: ergonomic helper for resolving one profile.
- `wannalead_get_job`: inspect an existing asynchronous job.
- `wannalead_cancel_job`: cancel a job only when the user requested it.
- `wannalead_create_job`: fallback for a generic job when no catalogued product
  alias is available.

Hosts may prefix tool names with the plugin or server namespace. Select the
tool whose unprefixed name matches the names above.

## Mandatory discovery

After the identity gate, call `wannalead_list_endpoints` and inspect the full
catalog. Search the relevant domain and synonyms. Before every business call,
call it again with an exact filter and include the schema and examples. Read
the complete returned contract, including permissions, write guards,
idempotency, pagination, deprecation, and replacement metadata.

The live catalog overrides remembered endpoint shapes and examples in bundled
skills. Do not claim a capability is unavailable until the complete catalog and
plausible synonyms have been checked.

## Calling endpoints

Prefer a dedicated MCP tool when one exists. Otherwise use
`wannalead_call_endpoint` with the endpoint's canonical catalog name. Preserve
opaque identifiers exactly as returned. Never reconstruct a LinkedIn profile
identifier from a display name or strip Unicode characters.

Calls can return data immediately or a `job_id`/`job_ids`. When a call remains
pending, keep and poll the returned job ID with `wannalead_get_job`; do not
create a duplicate operation. Treat a polling deadline as pending, not failed.

For invitation preflights, a response may contain `preflight_job_id` before an
invitation job exists. Poll that job, then replay the exact confirmed request
with the same idempotency key. Do not change the target or message during the
replay.

## Guarded writes

Never perform an invitation, message, reaction, comment, post, campaign launch,
feedback submission, cancellation, or other mutation unless the user explicitly
requested that exact action.

Before a LinkedIn write:

1. Inspect the exact endpoint schema.
2. Resolve the final recipients and payload.
3. Apply the bundled `humanizer` skill to outbound copy.
4. Show the exact final text, targets, timing, and action to the user.
5. Run a dry-run or preview when the endpoint supports it.
6. Obtain approval for the exact final operation.
7. Use a unique stable idempotency key and include the required confirmation
   field in the request body.

Humanization and a successful dry-run do not authorize the real write. Never
reuse one idempotency key for different operations, and never retry a rejected
or ambiguous write by creating a replacement job.

## Task skills

Use the bundled task skill matching the requested workflow:

- `account-mapping`
- `audience-mining`
- `engagement-prospecting`
- `job-change-signals`
- `icp-lookalikes`
- `campaign-automation`
- `warm-outreach`

These skills may retain CLI-shaped snippets as documentation of endpoint names
and payloads. Inside this plugin, those snippets are never shell commands.
Translate them through the MCP tool map above.

## Boundaries

- Use only public Wannalead MCP tools and catalogued public endpoints.
- Keep browser routing, cookies, raw provider payloads, gateway internals, and
  account diagnostics private.
- Stop on authentication, connection, billing, rate-limit, restriction, or
  idempotency-conflict errors. Preserve returned request and job identifiers.
- Do not claim an invitation, message, acceptance, post, or campaign action
  occurred until the returned product state confirms it.
