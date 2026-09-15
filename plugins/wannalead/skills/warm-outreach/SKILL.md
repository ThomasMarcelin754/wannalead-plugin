---
name: warm-outreach
description: Compose, humanize, and send one explicitly approved, context-personalized first touch through Wannalead guarded write endpoints.
---

# Warm outreach

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Compose a first touch from a real signal and the person's own context, then use
Wannalead's guarded write endpoints only after explicit approval.

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
candidate's complete contract, including write guards and idempotency, before
building a payload. Never rely on the endpoint names below or say a capability
is impossible until the live catalog, plausible candidates, deprecations, and
replacements have been checked.

## Durable context

Follow the progressive context rules from `wannalead-onboarding`. Before
drafting, use the confirmed user, company, ICP, and outreach-style context
available in the host. Refresh the recipient's relationship, conversation, and
authorized CRM exclusion state from their authoritative systems. Keep send and
reply state in Wannalead; do not maintain local prospect or messaging records.
Persist a style or ICP change only after the user confirms the proposed update.

## First-message patterns observed in our outreach

Use these for the first DM after a connection, not as connection-request notes.
They come from an August 2026 Wannalead journal snapshot: 10 of 86 unique first
DM recipients later replied and received an answer from the workflow. That is a
conservative, directional signal from a small non-random sample, not a reply-rate
guarantee.

The 10 reply-producing messages shared the same spine:

1. Open with one recent, verifiable signal specific to the recipient.
2. Connect it to one plausible operational consequence without asserting pain.
3. Ask one low-friction contrast question: is that part difficult now, or is it
   already handled?

They were 233–318 characters long, contained exactly one question, and never
mentioned the product. Treat that range as an observation, not a hard target.
Specificity matters more than filling a template or reaching a character count.

### Role or scope change

> Salut [Prénom],
>
> J'ai vu que tu venais de [changement concret] chez [Entreprise]. Avec
> [conséquence liée au nouveau périmètre], est-ce que [tension précise] devient
> plus compliqué, ou vous avez déjà un système propre ?
>
> Merci :)

### Recipient content or company signal

> Hi [First name],
>
> Your point about [specific idea from the signal] stuck with me. As
> [role-specific context], is [hard part] still harder than [adjacent task]?
>
> Thanks :)

These are structures, not reusable copy. Rebuild the wording from the person's
actual signal, match their language and register, and vary the opener across a
batch. Never ship brackets or repeat the same body with names swapped.

## Pipeline

1. Assemble context:
   - The trigger that surfaced the person.
   - `profiles.enrich` for current role and company.
   - `profiles.posts` for recent authored context.
   - `profiles.in-common` for shared connections.
2. Check `profiles.actions` before writing. Do not invite someone already
   connected or pending.
3. Choose a fitting first-message structure above and draft from a specific
   observed detail. If no concrete detail exists, stop or gather better context.
4. Apply the installed `humanizer` skill to the personalized draft. This is a
   required pre-send gate even when the user did not explicitly request it.
   Show the user the final humanized text and obtain approval for that exact
   version. If `humanizer` is unavailable, return the draft and do not send.
5. Validate the approved, humanized write with dry-run when available.
6. Send that exact text to one approved recipient with a unique idempotency key:

   ```bash
   wannalead call invitations.send \
     -idempotency-key <unique> \
     -body '{"profile_url":"<url>","custom_message":"<personalized-note>","confirm_send":true}'
   ```

   For an existing connection:

   ```bash
   wannalead call messages.send-profile \
     -idempotency-key <unique> \
     -body '{"profile_url":"<url>","text":"<personalized-message>","confirm_send":true}'
   ```

## Guardrails

- Require explicit user intent for every recipient.
- Never dry-run or send outbound copy that has not passed through `humanizer`.
- Approval applies to the exact humanized text; re-approve material rewrites.
- Require `linkedin:write`, `confirm_send=true`, and a unique idempotency key.
- Never reuse one message body across recipients or turn this into bulk send.
- Treat 403/409, rate-limit, restriction, or connection errors as stop signals.
- For a reproducible Wannalead product failure, capture the endpoint and
  `request_id`/`job_id`; report it with `feedback.send` only after the user
  approves the exact body, and never include secrets or raw private content.
- Never read cookies, browser state, or call LinkedIn directly.
