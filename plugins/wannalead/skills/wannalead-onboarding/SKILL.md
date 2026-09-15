---
name: wannalead-onboarding
description: First-use gate for the Wannalead plugin; verify OAuth identity and gather only the commercial context needed for the requested workflow.
---

# Wannalead onboarding

Establish the authenticated workspace and the minimum confirmed commercial
context needed by later Wannalead skills. This skill does not authorize any
LinkedIn or external write.

## Identity bootstrap

The first Wannalead tool call in a new conversation or workspace must be
`wannalead_call_endpoint` with `endpoint: "me"`. Do not begin endpoint discovery
or a business operation first.

If authentication is missing, ask the user to reconnect Wannalead through the
host's plugin settings. Never ask for an API key or OAuth token. If the response
is `linkedin_connection_required`, direct the user to
`https://wannalead.co/extension/connect` and retry the same identity call after
they reconnect.

Verify that the returned workspace and LinkedIn identity are unambiguous. Ask a
separate identity question only when the returned state leaves a real choice.

## Portable context

Use confirmed context already available in the current conversation, project,
or host memory. Do not require `~/.wannalead`, a local CLI, or local Markdown
files: ChatGPT and Claude web may not expose a persistent filesystem.

If the host provides an explicitly selected writable project folder, context
may be maintained there only when the user wants persistent project files.
Otherwise keep the context in the conversation and return a concise summary the
user can reuse. Never block a requested read solely because durable storage is
unavailable.

Keep credentials, raw private messages, API responses, current prospect lists,
campaign state, and CRM records out of context notes.

## Progressive interview

Ask only what the requested workflow needs. Start with one focused question
covering the offer and initial ICP when those facts are missing. Then gather,
as needed:

1. The user's role and immediate commercial goal.
2. Company fit, buyer roles, triggers, geography, and exclusions.
3. Offer, proof, delivery constraints, and positioning boundaries.
4. Preferred language, tone, message length, and call to action.
5. The authoritative CRM or customer source and stable exclusion rules.

Ask one question at a time, or at most three tightly related questions when the
interface makes that easier. Accept `unknown` or `later`; do not trap the user
in onboarding or start a broad data crawl while waiting.

Label profile or company observations as observations and ask the user to
correct them. Never silently promote an inference into a durable ICP or style
rule.

## Completion

Once identity and the minimum context for the requested task are clear, show a
short synthesis and continue to the relevant operational skill. Before its
first business API call, use `wannalead_list_endpoints` to inspect the live
catalog and exact endpoint contract.

## Boundaries

- Onboarding never authorizes invitations, messages, reactions, posts,
  campaigns, feedback, CRM writes, or any other mutation.
- Do not ask for information already present and confirmed in the conversation.
- Do not infer nationality, language, buying intent, or sensitive traits.
- Do not expose OAuth tokens, API keys, LinkedIn cookies, or internal routing.
