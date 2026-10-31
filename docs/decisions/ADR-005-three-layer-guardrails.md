# ADR-005: Input, retrieval and output validation

Status: Accepted principle; typed OpenAI decisions and current dialogue policy are in
ADR-008/011/012/015.

## Decision

Treat student messages, catalogue descriptions and uploaded passages as untrusted data.
Validate input, filter suspicious/ineligible retrieved content before model use, and
check outputs before persistence/sending. Structured schemas bound interpretation but
never authorise a tenant, course, price, payment or admission change.

Factual answers need approved scoped evidence and supporting-quotation validation or
abstention. Application/registration amounts come from structured records. Reject invented
IDs, unsupported course matches, invalid permutations and stale actions. Refusal/outage
preserves state and gives a safe recovery rather than enabling another unconfigured provider.

## Alternatives and limits

System-prompt-only safety was rejected; it cannot enforce business invariants. A classifier
alone is not a truth guarantee either. The historical System-1 noun/score implementation
proposal is superseded. Automatic escalation for every gap is also superseded: normal
uncertainty stays assistant-led; explicit requests/safety/staff authority can require a person.
Test hostile uploads/messages, numerical claims, schema-valid invented values and failure paths.
