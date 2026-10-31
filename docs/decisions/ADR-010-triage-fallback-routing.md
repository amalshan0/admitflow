# ADR-010: Keep routine uncertainty with the assistant

Status: Accepted intent; current routing supersedes the original blanket discovery fallback
under ADR-012/014/015.

## Decision

Greetings, unclear replies, unsupported media, out-of-catalog interests, missing facts and
retrieval abstention should not create routine staff work. Acknowledge the limit and offer
one useful contextual next step while preserving unrelated facts and pending progress.
Do not interpret an empty or unrelated turn as an answer to an academic/application field.

Explicit requests for a person, safety and necessary institutional/critical actions can
require a durable staff request. Ordinary uncertainty or frustration is not permission
to promise a counsellor response, reset the conversation or abandon an existing staff hold.

## Historical implementation and verification

The legacy classifier defaulted empty/unclear or failed structured output to discovery.
That protected staff from spam but could loop/restart a current collection. The active
journey uses state-aware handling; tests must cover uncertainty, interruption, unsupported
content, provider failure and explicit human requests without losing corrections or consent.
