# AdmitFlow specification

[PRODUCT.md](PRODUCT.md) defines the institutional product. This specification defines
required behaviour; [STATUS.md](STATUS.md) and [verification](docs/verification/README.md)
identify implementation and acceptance evidence. Requirements do not imply delivery.
The active roadmap is [tasks/plan.md](tasks/plan.md). ADR-018 governs context consolidation.

## 1. Scope and authority

One multi-tenant admissions-engagement product serves institutions in Kerala. Students
and parents explore institution offerings, obtain grounded answers, share necessary
details and proceed through applications/payment/review when ready. Authorised staff
manage the relationship with its conversation evidence. Tenant separation applies even
when development uses one institution.

Telegram private chat is the active channel, OpenAI the hosted AI provider and English
the current adaptive-guidance language. Multilingual experience and self-service institution
onboarding remain product scope with unfinished acceptance. WhatsApp is disabled compatibility
code; no provider/vendor restoration is implied by the original vision.

Institutions control courses, intakes, fees, rules, reference content and operating settings.
They need not author a fixed dialogue script. Real admissions use authorised maintained
records. Public programme references and synthetic fixtures are separate operating modes.

## 2. Architecture and data ownership

| Layer | Current decision |
| --- | --- |
| API/domain | Python >=3.12, FastAPI, Pydantic, uv |
| Conversation | Plain async dispatcher; reload durable context per turn |
| Persistence | PostgreSQL, SQLAlchemy async, Alembic, pgvector |
| Jobs/coordination | Redis/TaskIQ, recoverable database inbox/outbox, separate serial knowledge worker |
| AI | OpenAI-only configured interpretation, extraction, guidance, quotation selection and embeddings; typed decisions and required passage reranking |
| Document extraction | Offline CPU Docling for digital PDF structure/tables; ordered DOCX and section-aware text; no OCR |
| Staff UI | Next.js/TypeScript/plain CSS; Python owns business logic and authorisation |
| Operation | Isolated Compose or documented native processes; production host unselected |

Model IDs, deadlines, retries and thresholds live in configuration. No training,
distillation, self-hosted decision model, graph framework, separate vector database or
extra hosted inference provider is planned. Schema-valid model output still needs domain
validation. Optional telemetry vendors must not introduce unapproved data transfers.

Tenant-scoped courses, batches, rules, discounts, leads, facts, messages, applications,
invoices, staff sessions, uploads, jobs and audit records are persisted. Institution/bot
identity determines authority server-side; client tenant IDs are never sufficient.

## 3. Telegram transport and identity

- One institution-owned bot per institution initially. Trusted bot configuration resolves
  the tenant; bot-scoped user/chat identity resolves the lead.
- Telegram IDs/usernames are not verified phones. Optional contacts are explicitly supplied;
  never fabricate phone values. Encrypt recoverable routing/content; use scoped keyed lookups.
- Support `/start`, text and signed callbacks in private chats. Handle unsupported media
  and group/channel messages explicitly. Bind callbacks to actor/chat/tenant; reject stale
  and forged choices. Acknowledge callbacks promptly.
- Initial operation uses one long-poll receiver per bot with a database receiver lock.
  Commit accepted updates/checkpoints durably and recover unpublished work. Webhooks need
  authenticated HTTPS and the same durable contract. Polling and webhooks cannot coexist.
- Deduplicate by bot/update identity, serialize per tenant-owned conversation and preserve
  reply order. Persist send outcomes; ambiguous timeouts are not blindly retried. Do not
  claim exactly-once external delivery or log bot-token URLs. Never start a second receiver
  for an existing bot against another data profile.

## 4. Adaptive student guidance

`/start` introduces the institution and opens English conversation or resumes the relevant
draft. Accurately disclose the operating context; no compulsory language menu or fixed
course-first interview. `/guide`, `/assessment` and `/courses` are shortcuts; deeper
exploration is optional and unscored.

Interpret the current turn before planning. Reuse explicit background, interests,
experiences, strengths, goals, values, constraints and dislikes. Keep literal evidence
notes encrypted and correctable; replace only explicitly corrected evidence and preserve
unrelated information. Academic facts are separate from exploratory notes.

Choose one useful next question, with contextual taps where they lower effort. Uncertain
learners can use everyday activities; lack of exposure is not dislike and low confidence
is not inability. Do not demand portfolio/project proof for a beginner course unless an
actual entry requirement needs it. Greetings, questions, refusals and unrelated turns
must not become form values or invented preferences.

Offer up to three supported matches with literal student/course evidence and clear reasons
and trade-offs. Schooling alone is not interest fit. Decided students can proceed without
optional exploration. Inspection/recommendation does not select a course; selection needs
explicit valid confirmation. Changes invalidate stale controls.

If no offering fits, acknowledge the gap and optionally suggest up to two general fields
to research elsewhere. Do not push unrelated courses or invent institutions, duration,
jobs, placement, salary or admission claims. Repeated uncertainty changes or simplifies
the next move. Questions, changes, pauses and corrections preserve useful context.

Exploring/comparing/decided are revisable support needs, not psychological types. Interest
themes are not aptitude, eligibility or personality scores. Use original contextual prompts;
never copy/re-score formal instruments or imply inherited validity. See [research and
limits](docs/engineering/ADAPTIVE_GUIDANCE_RESEARCH.md).

Student and parent contributions should be distinct; student preference takes priority.
Structured attribution remains unfinished. Future course-comparison metadata should separate
learning goals, teaching medium, discussion support and delivery format. Never infer
proficiency, schedules or unrecorded services. See context reconciliation for open choices.

## 5. Catalogue, intakes and eligibility

For authorised admissions offerings, show institution-owned details and available active
intakes. Explicitly choose among batches; inquiries do not reserve capacity. Revalidate
activity, rules and availability before consequential actions, including changes during
model requests. Public references can be explored without invented intakes or entry rules.

Configured rules have typed fields, operators and values: for example age `>=`, qualification
equality or membership in allowed values. Code evaluates them; AI extracts only supplied
facts. Ask relevant missing fields one at a time. Corrections trigger reevaluation. Refusal,
invalid values and missing data do not mean approval. Do not apply one Grade 12 rule to all
levels or approve a missing-rule course without explicit institutional policy.

Keep eligible/not-eligible/needs-information/needs-staff-review distinct with rule evidence.
Interest fit is not eligibility; eligibility is not admission, enrollment or a seat. AI
cannot override failure. Course changes invalidate course-bound answers/results while
preserving unrelated facts. Public listings do not authorise university applications or
payments. Development exceptions are isolated in [development modes](docs/engineering/DEVELOPMENT_MODES.md).

## 6. Memory, lifecycle and applications

Persist turns, processing/send outcomes, validated facts/corrections, selection, guidance
stage and pending questions. Load bounded recent context chronologically, not the oldest
limited slice. Persist before claiming success. Restarts must not lose progress or duplicate leads.

Lifecycle records observed progress rather than a mandatory sequence. Separate exploration,
academic eligibility, application state, staff ownership and payment status. The original
lifecycle includes new/discovering/qualifying/admission-ready/form-collecting/payment-pending/
under-review and staff-reviewed outcomes. Actual code/evidence define implemented transitions.
Payment alone never means enrolled. Numeric readiness calibration and rolling summaries
are not established features.

A valid institutional course/intake and confirmed entry rules permit draft collection.
Reuse explicit validated information; accept multiple fields/corrections and ask only what
is missing. Name, birthdate, guardian/contact, address and education may be required where
justified; no default collection of identity-proof numbers or inferred birthdate from age.
Final review and explicit submission are mandatory.

`/pause` preserves a draft; `/apply` resumes; `/cancel` explicitly discards it. Questions and
exploration can interrupt collection. Signed one-use `/reset` clears profile/facts/draft
and excludes earlier context from future prompts while preserving submitted applications,
payments, institutional contacts, reminder preferences and staff/safety ownership. Reset
is not erasure or a bypass of submitted-case controls.

Typing feedback is bounded and failure-tolerant. Simple greetings/form answers and clear
numbers answering the displayed field use code directly; free-form guidance retains typed
interpretation and refreshed catalogue checks.

## 7. Payments, discounts and review

Institutional fees come from stored course data in integer minor units. Unknown tuition
or other charges remain unknown; documents do not override authoritative prices. Staff
define discount type/value/validity/constraints. AI may suggest a configured code; code
validates it and computes the fee. No invented discounts, negotiation or model-calculated amounts.

Link creation requires the applicable application/eligibility state and explicit fee
confirmation. Persist invoice/provider IDs and replay guards. Failures must not announce
success or create uncontrolled duplicates. Verify Razorpay signatures and match link,
reference, currency and exact amount before settlement. Redirects/student assertions cannot
mark paid; duplicates do not repeat state/receipts. Preserve uncertain provider outcomes.

Payment leads to review, not automatic admission. Authorised staff approve/waitlist/reject
with reasons/audit and applicable capacity/state checks. Financial records and access
decisions remain separate; refunds require institution/provider handling. Sandbox keys and
live enablement are separate. Finish latest-flow test-card/webhook/handset acceptance before
live operation. Sample reference applications retain markers and never allocate university seats.

## 8. Institution knowledge

Administrators upload approved digital PDF, DOCX and UTF-8 TXT/Markdown/CSV, individually or
by nested folder. Preserve safe paths; reject unsupported/scanned inputs with actionable
errors. OCR is excluded. Bound bytes/pages/archive expansion/text/time/process resources;
[CONSTRAINTS.md](CONSTRAINTS.md) records defaults and authorities.

Encrypt originals, track recoverable queued/processing/ready/failed attempts and deduplicate
bytes per tenant. Claim atomically, publish chunks together and prevent publication after
deletion. Offline PDF layout/table parsing runs in a bounded child and serial queue,
protecting interactive capacity. Do not promise perfect extraction of every document.

Filter ready/approved tenant material by permitted shared/course scope before retrieval
and OpenAI reranking. Staff may apply explicit course filters. Resolve follow-ups from
bounded context, clarify ambiguity and translate non-ASCII queries without lowering the
relevance floor or changing numeric meaning. Validate complete candidate-ID permutations;
invalid ranking/outage retains safe baseline order.

Relevance alone is not support: Telegram factual answers need a validated exact quotation
or abstention. Student messages omit source footers; encrypted evidence and staff search
retain source IDs/path/passage. Fees/availability use structured records. Treat retrieved
content as untrusted data, never instructions. See ADR-011 and the ingestion guide.

## 9. Staff workspace

Provision institutions/staff manually initially. Institution code/email/password creates
expiring opaque server-side sessions with hashed tokens, HttpOnly/SameSite cookies,
throttled sign-in and origin/mutation checks. Administrators manage staff/content/settings;
agents act within permission. Self-registration, SSO and automated onboarding are future choices.

The workspace covers lead filters/search, conversation/evidence, applications/review,
payment monitoring, staff requests/takeover, reviewed co-pilot, courses/intakes/rules/discounts,
uploads/settings/accounts/privacy, analytics/exports, delivery budgets and integrations/
notifications. Provide responsive accessible loading/empty/error/unauthorised states. Current
visual choices are in [UI_THEME.md](docs/engineering/UI_THEME.md); older dark/glassmorphism
mandates are superseded.

Requests persist context. Acknowledge/resolve do not resume automation; explicit confirmation
does. Takeover suppresses direct AI sends; co-pilot suggestions require review/edit/send.
Maintain continuity when control returns. Never invent staff acceptance/contact/response times.
Routine uncertainty, unavailable facts and unsupported interests stay assistant-led;
explicit human requests, safety and staff-only/critical actions can require intervention.

## 10. Engagement, reporting and integrations

Follow-ups require explicit reminder consent and respect opt-out, office hours, delays/caps,
current state and staff ownership. `/followups` manages preferences; `/stop` revokes them.
Recheck at execution and recover safely. Original two-attempt/cold-lead ideas do not override
configured policy. Do not invent urgency. Unsolicited first contact needs a separate channel/
consent decision; the original outbound WhatsApp-template idea is not active Telegram capability.

Analytics report recorded lifecycle/application/payment counts and drop-off indicators,
not causal conversion gains. Tenant-scoped CSV/Excel exports retain formula-safety checks.
Integration endpoints need admin authority, SSRF protections, authenticated/signed events,
bounded retries and durable state. Notifications use authenticated tenant-scoped WebSocket
delivery with refresh recovery, rather than unauthenticated broadcasts.

## 11. Privacy, safety and production

Encrypt sensitive identifiers/messages/facts/forms and institution credentials. Audit access
and significant changes without copying private content into logs. Support inquiry erasure
and configurable conversation retention under documented institutional/financial/audit
policy. Reset, erasure, retention and external-provider deletion are distinct operations.

Typed validation, tenant filters/constraints, scoped limits and input/retrieval/output checks
reduce abuse, injection and unsupported claims. No classifier guarantees truth. Provider
refusal/outage/invalid schemas preserve state and offer safe recovery. Operational telemetry
excludes raw applicant content; optional vendors cannot bypass that boundary.

AI-processing disclosure/controls, minors/guardian handling, retention and provider processing
need explicit product/privacy decisions. Reminder consent does not establish all AI-processing
permissions. Encryption and `store=False` alone do not establish compliance.

Choose hosting/budget, secret storage, TLS/same-origin boundaries, protected data services,
backups/restores, monitoring, incident ownership and rollback before real operation. Local
container success is not deployment acceptance. No deployment without the owner's instruction.
See [security and production](docs/engineering/security-and-production.md).

## 12. Verification and release acceptance

Test-first for identity/tenant authority, rules, corrections, state, replay, payments,
privacy and guardrails. Default tests fake providers. Relevant integrations use disposable
PostgreSQL/pgvector and Redis; frontend checks exercise user actions. Live provider/handset
checks are bounded, separately authorised and record source/configuration, failures, usage
and limits. Never transfer another implementation's test count, paid link or retrieval
threshold as current proof.

Acceptance includes the persisted student-to-staff journey; valid/invalid eligibility;
grounded/unsupported answers; corrected/interrupted collection; explicit application/payment/
review; consent/ownership controls; duplicate/uncertain delivery; tenant-negative checks;
recovery and quality gates. Language fluency, student usefulness and institutional operating
readiness need their own evaluations. Current results and next work live in STATUS/tasks.
