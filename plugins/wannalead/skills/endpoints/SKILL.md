---
name: endpoints
description: Discover Wannalead endpoint semantics, request schemas, examples, permissions, and write guards through the live MCP catalog.
---

# Wannalead endpoints

Use the live MCP catalog instead of memorizing endpoint paths or request bodies.
Never invoke a local CLI from this plugin.

## First-use gate

Invoke `wannalead-onboarding` first. The identity bootstrap uses
`wannalead_call_endpoint` with `endpoint: "me"`. Continue only after the OAuth
workspace and LinkedIn identity are unambiguous.

## Discovery sequence

1. Call `wannalead_list_endpoints` without a filter and scan the full current
   catalog.
2. Search the user's domain and relevant synonyms with `filter`.
3. For each plausible endpoint, call the tool with the exact endpoint name,
   `include_schema: true`, and `include_examples: true`.
4. Read method, path, parameters, request schema, examples, responses,
   permissions, idempotency, pagination, deprecation, and replacement metadata.
5. Call the selected endpoint only after that inspection.

The live catalog overrides examples in skills. Do not conclude that a
capability is missing until the full catalog, synonyms, plausible candidates,
and replacements have been checked. If none fits, report the filters and
candidates inspected.

## Calls

Use `wannalead_call_endpoint` with the canonical endpoint name. Put path and
query values in `params`, JSON input in `body`, and a stable unique key in
`idempotency_key` when required. Prefer `wannalead_profile_resolve` for its
dedicated operation. Use `wannalead_create_job` only when no product alias is
available.

Opaque LinkedIn identifiers must be chained exactly as returned. Preserve
Unicode and percent encoding; never construct a slug from a display name.

## Async results

Use `wannalead_get_job` for returned `job_id` values and
`wannalead_cancel_job` only on explicit user request. A queued or running job is
pending. Keep the same job instead of creating another one.

For invitation prerequisite responses, poll `preflight_job_id`, then replay the
same request with the same idempotency key. Changing the target or message is a
different operation and requires a new approval and key.

## Writes and feedback

Write endpoints require explicit user intent, exact payload review, the
endpoint's confirmation field, and an idempotency key. Run dry-run or preview
first when available. Product feedback is also an external write: present its
exact body and obtain approval before calling `feedback.send`.

## Boundaries

Use only the public Wannalead catalog. Do not derive cookies, private routing,
gateway messages, raw provider requests, or direct LinkedIn calls.
