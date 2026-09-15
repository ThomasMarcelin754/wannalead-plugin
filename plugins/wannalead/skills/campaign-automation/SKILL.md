---
name: campaign-automation
description: Prepare, freely customize, approve, launch, and monitor LinkedIn campaigns through Wannalead, from the standard preset to fully custom sequences.
---

# LinkedIn campaign automation

## Plugin transport

Use the bundled Wannalead MCP tools, never a local CLI. Treat every
`wannalead ...` snippet below as endpoint and payload documentation, then map
it with `wannalead_list_endpoints` for discovery,
`wannalead_call_endpoint` for product calls, and `wannalead_get_job` for async
results. Use confirmed context available in the host; do not require local
`~/.wannalead` files.

Build invitation-only campaigns or invitation flows that wait for acceptance
before sending a separately personalized message. A visible profile visit and
relationship-check step are optional product behavior, not invitation safety
prerequisites. Use only Wannalead's public API through the bundled MCP tools.

## Required first-use gate

Before calling a business endpoint, invoke `wannalead-onboarding`. Its
`wannalead_call_endpoint` request for `me` must be the first Wannalead tool
call. Continue after identity and the required context are clear. Never let campaign setup or OpenAPI discovery
bypass this gate.

## User freedom is a product invariant

The standard flow is an optional convenience, never the campaign contract's
limit. A user may add or remove steps, set each delay, add several follow-ups,
choose invitation or message behavior, branch on observed events, use A/B
variants, and handle relationship states differently.

When proposing a campaign, state in one short sentence that the suggested
sequence is editable and name the main dimensions the user can change. Do not
make the user discover the custom builder by objecting to a preset.

## Always refresh the live OpenAPI contract

After onboarding, treat live OpenAPI as the source of truth. Before building a
preview or mutation, run:

```bash
wannalead endpoints
wannalead routes campaigns
wannalead endpoints <name> --examples --json-schema
wannalead openapi path <exact-/v1-route>
```

Inspect every endpoint the chosen workflow will call and read its full method,
schema, parameters, responses, guards, idempotency, deprecation, and
replacement metadata. Do this even when an example appears below. Never claim
that a campaign shape is unavailable until the complete catalog has been
searched with campaign and sequence synonyms and every plausible endpoint has
been inspected.

## Choose the campaign mode

- Use the standard two-call workflow for relationship check, invitation when
  needed, acceptance monitoring, first message, and one no-response follow-up.
  Every recipient's copy and the timings remain configurable.
- Use the granular custom builder for multiple follow-ups, invitation-only
  campaigns, profile visits, A/B variants, custom branches, per-step delays, or
  different relationship handling. Never reduce the user's requested graph to
  fit the standard preset.

## Durable context

Follow the progressive context rules from `wannalead-onboarding`. Before
selecting targets or drafting copy, use the confirmed user, company, ICP, and
outreach-style context available in the host. Refresh CRM exclusions, relationships,
conversations, campaign targets, and activity from their authoritative systems.
Wannalead owns campaign and recipient state; do not mirror it in local
Markdown. Creation, launch, delivery, or one reply does not justify changing
durable context. Propose any update and persist it only after user confirmation.

## Start from the user's decisions

Before creating the campaign, establish:

- The exact recipients, using profile URLs or identifiers returned by
  Wannalead. Never reconstruct or ASCII-normalize a profile identifier.
- The campaign objective and outreach angle.
- Whether the user explicitly wants a visible profile visit or a delay before
  the invitation. Do not add either by default.
- The delay between observed acceptance and the message, for example 2, 12, or
  24 hours.
- Whether existing first-degree connections should proceed directly to the
  message branch. Never try to invite them again.
- Any execution window, date, and timezone the user cares about.

Do not silently choose a material timing or recipient. If the user gives a
relative time such as "tomorrow", state the resulting timing before approval.

Common millisecond values:

| Delay | Milliseconds |
| --- | ---: |
| Now | `0` |
| 2 hours | `7200000` |
| 12 hours | `43200000` |
| 24 hours | `86400000` |
| 48 hours | `172800000` |

## Verify access and inspect the chosen live contract

Preview requires read access. Activation and custom campaign mutations require
a paid workspace, `linkedin:write`, and a connected LinkedIn account. For the
standard flow inspect:

```bash
wannalead endpoints automation.campaigns.preview --examples --json-schema
wannalead endpoints automation.campaigns.activate --examples --json-schema
```

For a custom graph inspect at least:

```bash
wannalead me
wannalead endpoints automation.campaigns.create --examples --json-schema
wannalead endpoints automation.campaigns.sequences.create --examples --json-schema
wannalead endpoints automation.campaigns.reviews --examples --json-schema
```

Also inspect every additional endpoint immediately before first use. On
`linkedin_connection_required`, direct the user to
`https://wannalead.co/extension/connect`.
On `campaigns_not_in_trial`, explain that campaign automation is unavailable on
the trial. Never attempt to change billing or permissions.

## Reuse a compatible campaign before creating one

Before previewing or creating a new campaign, list the workspace campaigns and
inspect plausible matches with `automation.campaigns.get`,
`automation.campaigns.sequences.list`, and
`automation.campaigns.targets.list`. Prefer extending an existing campaign when
all of the following hold:

- its `intake_state` is `open`, and its status is `running` or a compatible
  draft that is already intended for the same work;
- its sender, objective, audience, and operational purpose fit the new targets;
- its active sequence has the requested timing, branches, and message steps;
- the new targets are not already present in that campaign.

Do not infer sequence compatibility from the campaign name, active version, or
root step alone. Reuse only when the exact active step graph is known from the
current workflow or exposed by current public reads. If the public contract
does not expose enough detail to verify it, treat compatibility as unknown.

Do not reuse a canary, E2E, or unrelated one-off or migration campaign merely
to reduce the campaign count. A paused campaign requires its explicit resume
flow; do not reuse completed, archived, or canceled campaigns. If no compatible
campaign exists, create a new one and briefly state why.

Add targets to a compatible campaign through
`automation.campaigns.imports.stage` followed by
`automation.campaigns.imports.commit`. The commit binds new targets to the
campaign's active sequence version and root step. Then create and freeze their
per-target drafts and review/schedule those target IDs; do not launch an
already-running campaign again.

If reuse would require publishing a different active sequence, state the
impact and obtain explicit approval before changing it. Existing scheduled
targets retain their assigned sequence version; newly committed targets use the
active version at commit time.

An existing LinkedIn invitation cannot be attached to a campaign by its
invitation URN. For already-pending invitations, use a bounded
`relationship.check` chain that routes `relationship.pending` to a later check
and `relationship.first_degree` to the approved message. Omit any
`relationship.not_connected` to `invitations.send` branch unless the user has
explicitly approved sending a replacement invitation.

## Personalize before launch

For each recipient:

1. Use `profiles.enrich` for current role and company. Use `profiles.posts` or
   another relevant public Wannalead read when it supplies evidence for the
   requested angle.
2. Draft one message from verified recipient context and the user's angle. Do
   not assert a pain point, familiarity, or intent that the evidence does not
   support. Do not reuse the same body with only the name changed.
3. Apply the installed `humanizer` skill. This is required before approval.
4. Show the user the recipient, evidence, angle, and exact final message. Get
   explicit approval of the complete recipient-to-message mapping and campaign
   timing. A material rewrite requires new approval.

The campaign invitation is sent without a note. The personalized text belongs
to the message step after acceptance, not to the invitation.

## Standard two-call workflow

When no compatible campaign exists and the requested sequence matches the
standard preset, preview it without side effects:

```json
{
  "name": "Paris architects",
  "acceptance_window_days": 30,
  "first_message_delay_hours": 24,
  "follow_up_delay_days": 4,
  "targets": [
    {
      "public_identifier": "alice",
      "first_message": "<approved first message>",
      "follow_up": "<approved follow-up>"
    }
  ]
}
```

```bash
wannalead call automation.campaigns.preview -body '<campaign-json>'
```

Show the returned recipients, exclusions, exact messages, and timing. After
exact approval, repeat the unchanged campaign fields with the returned token:

```bash
wannalead call automation.campaigns.activate \
  -idempotency-key <stable-campaign-key> \
  -body '<same-campaign-json-plus-preview_token-and-confirm-true>'
```

Activation requires `linkedin:write`, `preview_token`, `confirm: true`, and a
stable idempotency key. Do not report success until it returns `running` and
every eligible target is scheduled.

## Custom campaign builder

Use the granular sections below when extending a compatible campaign, when the
user requests a custom graph, or when the live catalog does not expose both
standard endpoints.

### Create or extend the campaign

Every mutation needs a unique, stable idempotency key. Reuse a key only to
replay the identical operation and body.

When no compatible campaign exists, create a campaign shell with the approved
recipients:

```bash
wannalead call automation.campaigns.create \
  -idempotency-key <campaign-create-key> \
  -body '{"name":"<campaign-name>","targets":[{"profile_url":"<profile-url-1>"},{"profile_url":"<profile-url-2>"}]}'
```

When extending an existing campaign, or importing a larger list, use
`automation.campaigns.imports.stage` followed by
`automation.campaigns.imports.commit`; do not skip the commit. Then list the
targets to obtain their stable target IDs:

```bash
wannalead call automation.campaigns.targets.list \
  -param campaign_id=<campaign-id>
```

### Publish the sequence

For an invitation-only campaign, publish `invitations.send` directly as the
root. The server reuses a cached stable identity when available, resolves a
missing identity as an internal prerequisite, and always reruns the fresh
`profile.actions` safety check immediately before dispatch. First-degree,
pending, or unavailable targets terminate as non-billable skips.

```json
{
  "source": "manual_edit",
  "root_step_key": "invite",
  "publish": true,
  "steps": [
    {
      "step_key": "invite",
      "type": "invitations.send",
      "order": 0,
      "delay_ms": 0,
      "endpoint_key": "invitations.send",
      "review_policy": "required"
    }
  ]
}
```

For a campaign that should message existing first-degree connections without
inviting anyone, make `relationship.check` the root and route only
`relationship.first_degree` to the message. A target that is not currently a
first-degree connection has no matching branch and completes without an
invitation or message.

```json
{
  "source": "manual_edit",
  "root_step_key": "relationship",
  "publish": true,
  "steps": [
    {
      "step_key": "relationship",
      "type": "relationship.check",
      "order": 0,
      "delay_ms": 0,
      "endpoint_key": "profile.actions",
      "review_policy": "none",
      "branch_edges": {
        "relationship.first_degree": {
          "stepKey": "first_message",
          "delayMs": 0
        }
      }
    },
    {
      "step_key": "first_message",
      "type": "messages.send_profile",
      "order": 1,
      "delay_ms": 0,
      "endpoint_key": "messages.send_profile",
      "review_policy": "required"
    }
  ]
}
```

For a mixed audience, start with `relationship.check`, route
`relationship.first_degree` to the message and
`relationship.not_connected` to `invitations.send`. Give the invitation an
`invitation.sent` branch to a separate `profile.actions` acceptance check, then
route that check's `relationship.first_degree` outcome to the same message.
This prevents duplicate invitations while still allowing new connections to
receive the approved message after acceptance.

When the campaign must message a target after acceptance, add an
`invitation.sent` branch from `invite` to a `profile.actions` acceptance check,
then branch `relationship.first_degree` to the approved message after the
chosen delay. Acceptance is observed asynchronously, so that delay is measured
from the observed acceptance event, not from the recipient's exact click time.

Add a visible `profile.full_unary` step only when the user explicitly requests
a profile visit or a delay anchored to that visit. Keep the longer visible
`profile.full_unary -> profile.actions -> invitations.send` graph only when the
workflow itself needs to route existing first-degree connections to a separate
branch. Do not add these reads merely to make an invitation safe; the server
owns identity resolution and the just-in-time relationship preflight.

The outer request accepts snake_case, but each object inside `branch_edges`
uses `stepKey` and `delayMs` in camelCase. Pass timing values as JSON numbers.
Create and publish the sequence with:

```bash
wannalead call automation.campaigns.sequences.create \
  -param campaign_id=<campaign-id> \
  -idempotency-key <sequence-create-key> \
  -body '<sequence-json>'
```

### Freeze approved messages when the sequence sends messages

Skip this section for invitation-only campaigns. When the active sequence
includes `first_message`, optionally persist the facts and personalization
rationale with `automation.targets.context`. Then create and approve a message
draft for every target using the exact user-approved text:

```bash
wannalead call automation.message-drafts.create \
  -idempotency-key <draft-create-key> \
  -body '{"campaign_id":"<campaign-id>","target_id":"<target-id>","step_key":"first_message","proposed_text":"<approved-message>"}'

wannalead call automation.message-drafts.approve \
  -param draft_id=<draft-id> \
  -idempotency-key <draft-approve-key> \
  -body '{"edited_text":"<same-approved-message>"}'
```

Do not approve a generated draft before comparing it byte-for-byte with the
text the user approved.

### Validate, launch, and schedule

Validate while the campaign is still a draft. Resolve every blocking issue
before launch and report warnings to the user:

```bash
wannalead call automation.campaigns.validate \
  -param campaign_id=<campaign-id> \
  -idempotency-key <validation-key> \
  -body '{}'
```

Immediately before launch, restate the campaign name, exact recipients,
timings, and exact messages. Launch only after explicit user intent:

```bash
wannalead call automation.campaigns.launch \
  -param campaign_id=<campaign-id> \
  -idempotency-key <launch-key> \
  -body '{}'
```

Launch changes the campaign to `running`, but it does not approve or schedule
the targets. Schedule the approved target IDs against the published sequence:

```bash
wannalead call automation.campaigns.reviews \
  -param campaign_id=<campaign-id> \
  -idempotency-key <review-and-schedule-key> \
  -body '{"decision":"approve","target_ids":["<target-id-1>","<target-id-2>"],"sequence_version":<version>,"schedule":true}'
```

Use `not_before_at` in epoch milliseconds when the whole selected target set
must not start before an absolute time. More than 100 targets require the
confirmation-token flow returned by the review endpoint.

## Verify and monitor

Do not report success from `launch` alone. Confirm both `running` status and
scheduled targets, then use these read endpoints for follow-up:

```bash
wannalead call automation.campaigns.get -param campaign_id=<campaign-id>
wannalead call automation.campaigns.summary -param campaign_id=<campaign-id>
wannalead call automation.campaigns.targets.list -param campaign_id=<campaign-id>
wannalead call automation.campaigns.activities.list -param campaign_id=<campaign-id>
```

Report per-target progress using product states and activity events. A future
scheduled time is normal waiting, not a failure. Do not manually retry a
pending step or create a replacement campaign.

`automation.campaigns.get` maps to
`GET /v1/linkedin/automation/campaigns/{campaign_id}`.

For lifecycle changes, inspect and use the public endpoint matching the user's
explicit request:

- Campaign: `automation.campaigns.pause`, `resume-preview`, `resume`, `cancel`,
  or `archive`. A paused campaign resumes through preview and confirmation, not
  by calling launch again. There is no campaign-level `stop`: use `cancel` for
  the campaign or `automation.targets.stop` for one target.
- Target: `automation.targets.pause`, `resume`, `stop`, `skip-next`, `opt-out`,
  `unreview`, `unschedule`, or `delete`.

## Boundaries

- Never expose or depend on infrastructure, workers, browser routing, cookies,
  private endpoints, operational local files, or direct LinkedIn calls. The
  five files defined by `wannalead-onboarding` are the only local context this
  workflow may use.
- Never change billing, permissions, account binding, or connection state to
  make a campaign pass validation.
- Stop on authentication, connection, billing, rate-limit, restriction, or
  idempotency-conflict errors. Preserve returned `request_id`, `campaign_id`,
  `target_id`, and `job_id` values for safe continuation.
- Never claim that an invitation, acceptance, or message occurred until the
  campaign activity confirms it.
