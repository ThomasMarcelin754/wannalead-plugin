---
name: humanizer
description: Humanize drafts and user-facing prose by matching the author's voice and removing canonical AI-writing patterns. Must be used before any Wannalead invitation, DM, follow-up, or outreach draft is approved or sent.
---

# Humanizer

Rewrite text so it sounds like its author, not a generic assistant. Use this
skill for campaign messages, outreach, posts, emails, articles, documentation,
and other user-facing prose. Preserve the original meaning, facts, language,
level of detail, and call to action.

## Required Wannalead pre-send gate

Apply this skill to every outbound Wannalead message after personalization and
before dry-run, approval, or send. This requirement applies even when the user
did not explicitly ask to humanize the message.

Within a Wannalead workflow, use the user-confirmed outreach style available in
the current host context as the primary voice guide. Do not change that context
from one draft, send, or reply; propose any durable style change and persist it
only after user confirmation.

- The user must approve the exact humanized text that will be sent.
- A previous approval of materially different copy is not approval of the
  rewrite.
- If this skill is unavailable, return the draft and ask the user to install or
  update the Wannalead skills. Do not send.
- Humanizing copy does not authorize a send. The write workflow still requires
  explicit approval, write scope, confirmation, and a unique idempotency key.

## Workflow

1. Read the source text and any writing sample first.
2. Identify the purpose, audience, voice, and concrete details that must survive.
3. Draft a complete rewrite. Replace weak phrasing instead of deleting ideas.
4. Audit the draft against the patterns below. Look for clusters rather than
   treating one isolated word or punctuation mark as proof of AI writing.
   Check language integrity too: preserve expected accents and diacritics, and
   reject text that was transliterated to ASCII for transport convenience.
5. Revise again, read it aloud, and return the final version.

When this skill is a gate inside another workflow, return the final copy to that
workflow and keep the audit internal unless the user asks for it. When the user
asks for a writing diagnosis, briefly name the patterns found before the final
rewrite.

## Voice calibration

When a writing sample is available, match its sentence length, vocabulary,
punctuation, paragraph openings, transitions, recurring phrases, and level of
formality. Do not polish away natural habits merely because they are informal.

Without a sample, use plain language and varied sentence lengths. Add opinions,
humor, first person, or unresolved tension only when the format and source voice
call for them. Technical, legal, and reference writing should usually remain
neutral.

## Canonical AI-writing patterns

Rewrite these patterns when they appear. Preserve quoted text, proper names,
and intentional examples.

### Inflated or generic content

- Significance inflation: `pivotal`, `crucial`, `testament`, `underscores`,
  `shaping the landscape`, or claims that an ordinary fact reflects a broader
  movement without evidence.
- Promotional language: generic praise, superlatives, `vibrant`, `renowned`,
  `groundbreaking`, `breathtaking`, `rich tapestry`, or `boasts`.
- Superficial analysis: trailing `-ing` clauses such as `highlighting`,
  `showcasing`, `ensuring`, or `reflecting` that add no concrete fact.
- Vague authority: `experts say`, `industry reports`, or `observers note`
  without a named, relevant source.
- Notability padding: lists of media mentions, follower counts, or claims of
  prominence that do not explain why the cited coverage matters.
- Speculative gap-filling: invented familiarity, motives, biography, pain
  points, or claims that unavailable information proves someone is private.
- Knowledge-cutoff disclaimers and paragraphs about missing information. State
  what is known, name the source or uncertainty, and stop there.
- Formulaic `Challenges`, `Legacy`, or `Future outlook` sections that offer
  generic obstacles and optimism instead of concrete facts.
- Generic conclusions and future-looking optimism that do not add a concrete
  next fact.

Prefer specific actions, dates, observations, and attributed claims. Never make
the copy feel personal by inventing context.

### Formulaic language

- AI vocabulary clusters such as `additionally`, `delve`, `enhance`, `foster`,
  `intricate`, `interplay`, `pivotal`, `showcase`, `tapestry`, and `underscore`.
  Do not replace a legitimate isolated use mechanically.
- Elaborate substitutes for `is`, `are`, or `has`, such as `serves as`,
  `stands as`, `represents`, `features`, and `offers`.
- Negative parallelisms such as `not just X, but Y`, plus clipped endings such
  as `no guessing` or `no wasted motion`.
- Forced groups of three, false `from X to Y` ranges, and synonym cycling used
  only to avoid repeating the clearest noun.
- Filler and hedging such as `in order to`, `it is important to note`, `could
  potentially`, and `at this point in time`.
- Persuasive-authority tropes such as `the real question`, `at its core`,
  `fundamentally`, or `what really matters` when the next sentence is ordinary.
- Fake-candid openers such as `Honestly?`, `Here's the thing`, `Let's be
  honest`, or `Real talk` when they manufacture intimacy rather than match the
  author's voice.
- Servile agreement and automatic praise such as `Great question`, `You're
  absolutely right`, or `That's an excellent point` when the substance does not
  require it.
- Chatbot artifacts such as `I hope this helps`, `Would you like me to`, `Let
  me know`, or offers to continue that accidentally remain inside the copy.

Prefer simple verbs, active subjects, and the author's normal vocabulary.

### Structure and rhythm

- Tutorial signposting such as `Let's dive in`, `Let's break this down`,
  `Here's what you need to know`, and `Without further ado`.
- Repeated headings, bold labels, decorative emoji, and summaries that restate
  the preceding text.
- Several short fragments arranged as manufactured drama or punchlines.
- Aphorism formulas such as `X is the language of Y`, `X is the currency of Y`,
  or `not a tool but a mirror` when a concrete statement would be clearer.
- Uniform paragraph size and sentence rhythm.
- Passive constructions or subjectless fragments that hide the actor, such as
  `No configuration needed` or `The result is preserved automatically`, when a
  direct subject and verb would be clearer.
- Documentation that narrates a recent diff instead of describing how the
  current system works, except in changelogs and migration notes.
- Em dashes and en dashes in the final rewrite. Use a period, comma, colon,
  parentheses, or a restructured sentence instead.

## Preserve human signals

Do not flatten details that already make the text sound authored:

- unusual and verifiable specifics;
- mixed feelings or uncertainty that genuinely exists in the source;
- natural changes in sentence length;
- genuine asides, self-corrections, slang, and in-jokes;
- a deliberate first-person point of view;
- an author's existing punctuation or vocabulary when it is not part of a
  broader cluster of AI-writing tells.

Perfect grammar, formal vocabulary, one transition word, one emphatic sentence,
or one unusual punctuation mark is not enough to justify a rewrite by itself.

## Campaign message mode

Use this mode only for a one-to-one campaign or outreach message when the user
explicitly asks for a casual, imperfect voice. A real recipient signal must
still provide the personalization. Never use formatting, a smiley, or a typo to
make reused generic copy appear personal.

- Split the message into two or three short blocks separated by a blank line.
- In an existing conversation, reopen naturally and vary the greeting across a
  batch. Avoid unnatural constructions such as `hi back`.
- Use at most one simple textual smiley when it fits the sender's existing
  voice. Do not add decorative emoji.
- Add at most one plausible imperfection when the requested voice calls for it:
  a swapped adjacent letter, one doubled character, one missing terminal
  period on a statement, or one or two missing accents in ordinary low-risk
  words. The one-or-two limit applies to the whole message.
- Treat a request for `2-3 mistakes` as applying to the whole small batch by
  default, not to every message. Leave some messages clean.
- Keep pricing, security, data-handling, procurement, access, onboarding, and
  factual or technical messages clean unless the established conversation is
  clearly casual and the imperfection cannot affect meaning.
- Never alter a person's name, company, product, URL, date, number, factual
  claim, quoted text, question mark, or call to action.
- Normalize display styling copied from profiles. Do not repeat all-caps names
  merely because a profile displays them that way; preserve real acronyms.
- Preserve Unicode and all other correct accents throughout the message. Never
  strip diacritics globally or convert prose to ASCII. Only campaign mode may
  omit one or two accents deliberately; shell commands and JSON payloads must
  encode the resulting text as UTF-8 instead of rewriting it.
- Vary or omit imperfections across recipients. Remove any one that attracts
  attention when read aloud.

## Outreach closing question

End an outreach message on one specific question placed on its own line, with
nothing after it before the sign-off.

- Tie it to the recipient signal or the concrete problem raised in the message.
- Make it answerable in one sentence or less.
- On a first touch, ask about the recipient's reality, not for a meeting or a
  block of time.
- Avoid vague closers such as `thoughts?`, `what do you think?`, or `curious for
  your feedback`.
- Ask one thing. Do not stack questions.

## Guardrails

- Use only details supplied by the user or returned by Wannalead.
- Do not infer nationality or language from a profile. Use explicit profile,
  content, conversation, or user-provided signals.
- Do not reuse the same body across recipients. Personalize the substance, not
  only the greeting.
- Never weaken factual accuracy or conceal uncertainty to make prose smoother.
- More than two missing accents in a message that normally uses them fails the
  pre-send review. Restore the extras before presenting the copy for approval.
- Return outbound copy for approval. This skill edits text; it never sends it.
