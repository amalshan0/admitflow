# AdmitFlow product definition

AdmitFlow is an AI-native admissions-engagement platform for educational institutions.
It gives students a personalised conversation and staff one workspace for the relationship
from first inquiry through application, payment and institutional review.

This document governs product purpose and boundaries. [SPEC.md](SPEC.md) defines behaviour;
[STATUS.md](STATUS.md) distinguishes implemented work from remaining acceptance.

## Customers and problem

The intended buyers are coaching centres, colleges and training institutes in Kerala.
Students and parents are the people seeking information and guidance. Institutions face
repeated questions, scattered inquiries, lost context and inconsistent follow-up. Students
may know only familiar course names or struggle to connect institutional information to
their interests, background and practical constraints.

AdmitFlow is one multi-tenant product, rather than a separately built service for each
customer. Institutions manage their own offerings, rules and reference material. Initial
provisioning is manual; self-service onboarding is a longer-term product direction.
Pricing, willingness to pay, customer demand, conversion gains and staff time savings
remain unvalidated. No customers, revenue or measured student outcomes are claimed.

## Student experience

- Understand before recommending. Use explicit interests, study background, experiences,
  goals, values and relevant constraints. Ask only what helps the next decision.
- Adapt to readiness. A decided applicant can proceed directly; someone comparing options
  receives trade-offs; an uncertain student gets accessible activity examples and optional
  deeper exploration. These are revisable support needs, not permanent student labels.
- Use natural conversation with contextual taps where they reduce effort. No compulsory
  quiz, language menu, fixed interview or universal sales funnel.
- Explain suitable institution courses with evidence and introduce relevant unfamiliar
  options. Let the student inspect, compare, correct, pause and choose explicitly.
- Put student fit first. If the institution has no suitable course, acknowledge the gap.
  General fields to research elsewhere may be labelled as such; never invent external
  institutions, offers, placements, earnings, accreditation or admission promises.
- Remember useful context across turns and sessions. Corrections preserve unrelated facts;
  interruptions and questions do not force a restart. Durable memory does not mean sending
  an entire historical transcript to the model on every turn.
- Check actual academic requirements separately from interest fit. Explain unknowns and
  code-evaluated outcomes without turning eligibility into an admission or seat guarantee.
- Collect only the necessary application details, reuse validated information and require
  explicit review/submission. Prices and payment actions come from institutional records.

Guidance is scoped to institution applicants and offerings. Career-guidance techniques
inform exploration; the assistant is not a general career-placement service or validated
aptitude, personality or psychological assessment.

## Institution experience

Institutions control content and business rules, rather than prescribing a conversational
script: courses, intakes, seats, fees, eligibility criteria, discounts, approved knowledge,
office hours and follow-up preferences. Staff inspect inquiries, conversation evidence,
application/payment state and next actions in the same tenant-scoped workspace.

Staff can review applications, manage knowledge/catalogue/settings, take over a chat,
review or edit co-pilot suggestions, resume automation explicitly, and use analytics,
exports and integrations. Consent-aware follow-ups support inactive inquiries without
invented urgency. Role permissions, audit records, deletion and retention are part of the
product, not optional additions after a pilot.

When a parent contributes, the student's own preference should remain distinct and take
priority. Structured speaker attribution is a remaining product requirement; current
guidance prompts alone do not establish that it is implemented.

## Boundaries of AI and application code

AI interprets messages, extracts explicitly stated facts into typed schemas, chooses useful
guidance questions, drafts grounded replies and reranks retrieved passages. Code validates
facts and owns tenant authority, catalogue identities, eligibility, fees/discounts,
selection, application/payment transitions and staff permissions.

Institution material and student messages are untrusted data. Answers need supporting
evidence or an honest abstention. Routine uncertainty, unavailable facts and out-of-catalog
interests stay with the assistant. Explicit human requests, safety and staff-only matters
can require intervention with the conversation context; a queued request does not mean a
person has responded and must not imply an unconfigured response time.

## Current choices and future scope

Telegram is the active student channel, OpenAI is the hosted AI provider, and English is
the current adaptive-guidance language. English, Hindi, Hinglish and Malayalam remain the
language ambition, subject to full translation and fluent-speaker evaluation. Legacy
WhatsApp compatibility is disabled by default; restoring it is a separate product and
operational decision.

The foundation remains Python/FastAPI, PostgreSQL/pgvector, Redis/TaskIQ, a plain async
conversation dispatcher and a Next.js staff workspace. No model training, self-hosted
decision model or additional hosted AI provider is planned. Production hosting is
unselected and deployment requires the owner's instruction.

Real admissions require an authorised institution's maintained offerings, intakes, rules
and fees. Public programme references are information, not permission to accept university
applications. Development-only sample applications and test payments are isolated tools,
described in [development data and modes](docs/engineering/DEVELOPMENT_MODES.md).

The original intent and later useful decisions are reconciled locally in
[context consolidation](docs/planning/CONTEXT_RECONCILIATION.md). No other repository is
required to understand this product.
