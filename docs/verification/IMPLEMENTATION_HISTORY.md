# Historical implementation evidence

These task records describe bounded checks on their original source/configuration through
Task 70. They preserve implementation reasons and limitations, not today's runtime state.
Current scope/status is in [STATUS.md](../../STATUS.md); current work is in
[tasks/todo.md](../../tasks/todo.md). Source history remains recoverable at the predecessor
commit recorded in the [consolidation manifest](../planning/context-sources.json).

Tasks 1–42 retain their original identities. Tasks 43–60 established Telegram identities,
durable inbox/outbox, bounded OpenAI interpretation, conversation memory, staff knowledge
and inquiry/request controls. Their technical results remain in the verification JSON.
Tasks 61–65 and 69–70 extend guidance, continuity, reference data, UI and the isolated
sample-application path. Retired documentation-only tasks 66–68 are not active product work.
Paths in original fingerprints and evidence can refer to the predecessor tree. Relocation
does not rerun a test or change its source fingerprint.

## Foundation and institutional implementation (Tasks 1–42)

These are retained technical records from the original product roadmap. Current
priorities and release requirements are in plan/todo and the product specification.

## Phase 1: Foundation

### Task 1: Initialize Python project, FastAPI, settings, and Docker

**Description:** Create the `backend/` directory, `requirements.txt`, FastAPI app entry, settings singleton, and Dockerfile.
**Acceptance criteria:**

- [X] FastAPI boots on localhost:8000
- [X] `/docs` shows Swagger UI
- [X] Settings loaded from `.env`
  **Verification:** `uvicorn src.main:app --reload` starts successfully
  **Dependencies:** None
  **Estimated scope:** Small

### Task 2: PostgreSQL schema (SQLAlchemy + Alembic)

**Description:** Define all core SQLAlchemy models: Institution, StaffUser, Course, Batch, QualificationRule, DiscountCode, Lead, LeadFact, Message, Application, PaymentOrder, AuditLog. Every table has `tenant_id`.
**Acceptance criteria:**

- [X] All models defined with proper relationships
- [X] Alembic migration runs cleanly
- [X] `tenant_id` on every tenant-scoped table
  **Verification:** `alembic upgrade head` succeeds
  **Dependencies:** Task 1
  **Estimated scope:** Large

### Task 3: TaskIQ + Redis broker setup

**Description:** Configure TaskIQ with Redis as the message broker for background processing.
**Acceptance criteria:**

- [X] TaskIQ broker boots successfully
- [X] A dummy task can be enqueued and processed
  **Verification:** Trigger a test task from a `/debug` endpoint
  **Dependencies:** Task 1
  **Estimated scope:** Small

### Task 4: PII encryption utilities and audit log infrastructure

**Description:** Build `security.py` (encrypt/decrypt PII fields) and the audit log write utility.
**Acceptance criteria:**

- [X] Phone numbers and names can be encrypted/decrypted transparently
- [X] `write_audit_log()` function writes to the `audit_log` table
  **Verification:** Unit tests for encryption roundtrip and audit log writes
  **Dependencies:** Task 2
  **Estimated scope:** Small

---

## Phase 2: WhatsApp Ingress

> **Phase 1 → Phase 2 carry-over (from the foundation readiness audit).**
> Phase 1 (Tasks 1–4) is complete and green — no defects. These are *forward-looking
> prerequisites* the Phase 2 webhook needs; none existed yet at end of Phase 1. Do them
> as the opening moves of the tasks noted, not as afterthoughts:
>
> 1. **Shared async Redis client/helper** — a `redis.asyncio` client for app code (the
>    TaskIQ broker's internal Redis is separate). Needed by **Task 5** (`SETNX` dedup) and
>    **Task 7** (Lua CAS mutex). The `redis[hiredis]` dep is already installed; this is
>    net-new code, no mutex/dedup foundation exists today.
> 2. **Meta webhook config in `Settings` + `.env.example`** — `meta_app_secret` (HMAC body
>    verify), `whatsapp_verify_token` (GET challenge), Meta access token, and Graph API
>    version. None exist yet. Owned by **Task 5** (and reused by Task 6 sending).
> 3. **E.164 phone-normalization helper feeding `hash_phone`** — deliberately deferred from
>    Task 4 to its first caller. Un-normalized input (`+91 98765…` vs `919876…`) defeats the
>    `(tenant_id, phone_hash)` unique constraint and creates duplicate leads. Owned by
>    **Task 5** (lead creation / lookup by phone).
> 4. **Decision: UNIQUE/partial index on `messages.wa_message_id`?** Today it is indexed but
>    *not* UNIQUE — dedup is Redis-`SETNX`-only by design (ADR-002). Make a conscious call in
>    **Task 5** on whether to add a DB-level backstop for when the Redis layer is bypassed.
>
> Naming note: acceptance text below says "`audit_log`"; the real table is `audit_logs`
> (code + migration agree) — cosmetic, no change needed.

### Task 5: Meta WhatsApp webhook (HMAC + dedup + enqueue)

**Description:** Build `GET` (challenge verification) and `POST` (HMAC verify body, Redis SETNX dedup on `message.id`, extract payload, enqueue to TaskIQ).
**Acceptance criteria:**

- [X] `GET /api/webhook` passes Meta challenge
- [X] `POST /api/webhook` returns 200 OK immediately
- [X] Duplicate `message.id` silently dropped
- [X] Valid messages enqueued to TaskIQ
  **Verification:** Test with ngrok + Meta test tool
  **Dependencies:** Task 3
  **Estimated scope:** Medium

### Task 6: WhatsApp message sending service

**Description:** Build `whatsapp.py` service with functions to send: text, interactive buttons, interactive lists, and template messages via Meta Cloud API.
**Acceptance criteria:**

- [X] `send_text()`, `send_buttons()`, `send_list()`, `send_template()` all work
  **Verification:** Send a test message to a verified number
  **Dependencies:** Task 1
  **Estimated scope:** Medium

### Task 7: Redis distributed mutex

**Description:** Implement `redis_mutex(key)` async context manager using Redis Lua CAS for owner-safe release, with keepalive TTL extension.
**Acceptance criteria:**

- [X] Only one worker processes a message per phone number at a time
- [X] Lock auto-releases after TTL if worker crashes
  **Verification:** Unit test with simulated concurrent access
  **Dependencies:** Task 3
  **Estimated scope:** Small

---

## Phase 3: AI Core (Engine + RAG)

### Task 8: Engine state definition and lifecycle

**Description:** Define the conversation state schema (a plain dataclass/Pydantic model loaded per turn, not a graph state) including: `lead_status`, `conversation_history`, `current_course`, `extracted_facts`, `pending_order`, `applied_discount`, `co_pilot_mode`, `consult_stage`. Build `engine/state.py` (lifecycle states + allowed transitions) and the `engine/dispatcher.py` skeleton (`handle_turn()`).
**Acceptance criteria:**

- [X] State schema defined with all required fields
- [X] Lead lifecycle transitions enforced in code
  **Verification:** Unit test state transitions
  **Dependencies:** Task 2
  **Estimated scope:** Medium

### Task 9: Multi-provider LLM factory with fallback

**Description:** Build `llm_factory.py` that tries Gemini → OpenAI → Anthropic with automatic fallback on API errors (chain per ADR-007; model IDs config-driven).
**Acceptance criteria:**

- [X] Primary provider used when healthy
- [X] Automatic fallback to next provider on error
- [X] All providers return the same interface
  **Verification:** Mock primary provider failure, verify fallback triggers
  **Dependencies:** Task 1
  **Estimated scope:** Medium

### Task 10: Input guardrail node

**Description:** First node in the graph. Detects prompt injection, profanity, and abuse patterns. Issues canned refusal and halts processing.
**Acceptance criteria:**

- [X] Injection attempts detected and blocked
- [X] Profanity detected and blocked
- [X] Clean messages pass through unchanged
  **Verification:** Unit tests with adversarial inputs
  **Dependencies:** Task 8
  **Estimated scope:** Medium

### Task 11: Triage node (intent detection)

**Description:** Routes incoming messages to the appropriate downstream node: Discovery, RAG, Qualification, Handoff.
**Acceptance criteria:**

- [X] Correct routing for course exploration, knowledge questions, qualification answers, and human requests
  **Verification:** Unit tests with representative message types
  **Dependencies:** Task 8, 9
  **Estimated scope:** Medium

### Task 12: Discovery node

**Description:** Presents available courses as WhatsApp interactive lists/buttons. Handles course selection and batch routing.
**Acceptance criteria:**

- [X] Courses fetched from DB filtered by `tenant_id`
- [X] Only batches with `seats_remaining > 0` shown
- [X] Tap-based selection updates lead state to `discovering`
  **Verification:** Integration test with seeded course data
  **Dependencies:** Task 8, 6
  **Estimated scope:** Medium

### Task 13: RAG pipeline

**Description:** Build document ingestion (chunk, embed, store in pgvector), hybrid search (dense + lexical with RRF), and RAG chunk sanitizer.
**Acceptance criteria:**

- [X] Documents chunked and embedded per tenant
- [X] Search returns relevant chunks above confidence threshold
- [X] Chunks sanitized before injection
  **Verification:** Golden question retrieval test (≥80% hit rate)
  **Dependencies:** Task 2
  **Estimated scope:** Large

### Task 14: RAG node

**Description:** Graph node that retrieves from the knowledge base and generates an answer. Fail-closed below confidence threshold.
**Acceptance criteria:**

- [X] Answers knowledge questions from institution documents
- [X] Says "I don't have that information" below threshold (no hallucination)
  **Verification:** Test with institution FAQ documents
  **Dependencies:** Task 13, 9
  **Estimated scope:** Medium

### Task 15: Qualification node

**Description:** Asks the user questions to extract field values, then verifies against structured course rules in code. Extraction uses native structured outputs (JSON-schema / function calling), not free-text parsing — extracted fields feed deterministic rule eval (ADR-007 LLM efficiency).
**Acceptance criteria:**

- [X] AI asks the right questions to fill rule fields
- [X] Field values extracted via structured output into a typed schema (no regex/free-text parsing)
- [X] Code evaluates field/operator/value rules deterministically
- [X] Lead marked `admission_ready` only when ALL rules pass
  **Verification:** Unit tests for rule engine + integration test with mocked LLM
  **Dependencies:** Task 8, 9, 12
  **Estimated scope:** Large

### Task 16: Output guardrail

**Description:** Validates AI response before sending to WhatsApp. Blocks hallucinated fees, made-up course names, PII leakage, and ensures WhatsApp-safe formatting.
**Acceptance criteria:**

- [X] Monetary figures not from DB are blocked
- [X] Course names not in the DB are flagged
- [X] WhatsApp formatting applied correctly
  **Verification:** Unit tests with adversarial AI outputs
  **Dependencies:** Task 8
  **Estimated scope:** Medium

### Task 17: Wire webhook → TaskIQ → engine dispatcher → WhatsApp

**Disposition:** Historical WhatsApp acceptance is superseded by ADR-012 and verified
Telegram Tasks 44-58. The unchecked device criteria below are retained history, not
a request to implement another channel.

**Description:** Connect all the pieces: TaskIQ worker picks up the message, runs it through the full `handle_turn()` dispatcher (decide → dispatch → generate → guard), and sends the response.
**Acceptance criteria:**

- [ ] Full loop works end-to-end
- [ ] Episodic memory (`messages` table) persists conversation between turns; recent turns reloaded each message
- [ ] Semantic memory (`lead_facts`) updated after each turn
  **Verification:** End-to-end test on a real WhatsApp device
  **Dependencies:** Task 5, 6, 7, 8–16
  **Estimated scope:** Medium

---

## Phase 4: Advanced Bot Features

### Task 18: Application form collection node

**Description:** Collect structured application data through explicit Telegram prompts.
**Acceptance criteria:**

- [X] Full name, DOB, parent/guardian, address and previous school collected after an explicit start; cancel and review/submit are supported.
- [X] Form saved encrypted in the application table; new DOB/school values are not written to legacy plaintext columns.
- [X] Lead moves through form_collecting to payment_pending (or under_review for a zero-fee course).
  **Evidence:** Database form journey proves invalid-DOB rejection, stale-submit nonce rejection,
  explicit submission, replay protection, encryption, no seat reservation and application-state
  preservation through later qualification/course requests and human resume. Live transport
  for the earlier inquiry is separate; no new handset application submission is claimed.
  **Verification:** Integration test collecting a full application
  **Dependencies:** Task 15, 17
  **Estimated scope:** Medium

### Task 19: Razorpay integration

**Description:** Build discount code validation, payment link creation (raw httpx), HMAC webhook verification, and in-chat confirmation.
**Acceptance criteria:**

- [X] Invoice and provider request use DB-sourced integer paise, with validated discounts.
- [X] Fee preview and an expiring, nonce-bound explicit tap precede link creation.
- [X] Immutable invoice, atomic job claims and outbound dedup prevent duplicate creation/sends; uncertain attempts are not blindly repeated.
- [X] Raw-body HMAC, full captured amount/reference checks and replay-safe events update payment status.
- [X] Verified settlement queues one encrypted Telegram confirmation; a full discount is WAIVED, not a fabricated PAID payment.
- [X] Staff monitoring and administrator reconciliation recover only an existing exact-reference link; lookup alone cannot confirm payment.
- [X] Known pre-creation configuration/rate failures can retry the same bounded invoice; missing or ambiguous links cannot authorize retry.
- [ ] Complete the actual Razorpay test-mode link/card/webhook/Telegram journey.
  **Verification:** Provider contracts, fee-change reconfirmation, concurrent creation,
  waiver, signed event tampering/replay, tenant/role boundaries and reconciliation are
  automated. Browser fixture checks cover explicit staff confirmation, unresolved state,
  inquiry navigation and mobile/desktop rendering. The user needs to create a Razorpay
  test account; no real provider payment is claimed. Live payments remain disabled.
  **Dependencies:** Task 17, 6
  **Estimated scope:** Large

### Staff takeover and reviewed co-pilot evidence (Tasks 20 and 29)

The staff dashboard can pause bot replies, compose a manual Telegram reply, generate
an encrypted OpenAI draft, inspect its reference lookup, edit/approve/ignore it, and
resume AI. Draft generation never sends a message. Approval checks the latest saved
student event, staff revision, expiry, reviewed flag and active tenant-owned routing.
Exact retries reuse one durable outbox entry. Resume preserves facts and includes only
confirmed delivered replies in AI context. Office-hours and urgent-support copy is
reviewed in all four supported language variants.

The backend gate passes **175 tests**, lint/format and strict typing over **86 source
files**; two pre-existing RAG AsyncMock warnings remain. The final PostgreSQL run
passes **40 cases** (the Redis-only case is skipped there); the explicit Redis check
passes separately, for **41 integration cases**. These checks cover takeover, scoped access, pause/resume, draft expiry/replay, provider failures,
concurrent staff edits and cancellation during provider I/O. Frontend lint/build and
all **4 browser scenarios** pass, including mobile/desktop layout and manual-send retries.
Live OpenAI generated a 237-character draft from the saved Telegram inquiry; it was
ignored and AI resumed. Student facts were preserved and **zero outbound messages were
added**. This does not establish handset receipt of a new staff-authored reply.

Adversarial review reproduced and fixed stale ORM state overwriting a concurrent staff
revision, and cancelled draft generations overwriting the staff disposition. Default
tests now disable implicit developer dotenv loading, including LiteLLM's dotenv import;
explicit fixture configuration remains supported. No local secrets are moved or exposed.
Published through fresh repository PR #2 after both backend and frontend CI passed.
The published ancestry contains only newly created commits; prior repository history
remains local. The remaining handset/payment checks stay open.

### Task 20: Human handoff + co-pilot mode

**Description:** Handoff node for escalation. Co-pilot mode: AI generates suggested replies visible in dashboard.
**Acceptance criteria:**

- [X] Escalate clear frustration, explicit human requests and urgent safety signals; ordinary questions/refusals remain in the normal journey.
- [X] Co-pilot generates encrypted drafts without sending; approval requires explicit review, fresh context and an active takeover.
- [X] "Resume AI" restores the bot with confirmed staff replies and saved student facts; pending staff sends must finish first.
- [X] Institution-local office hours affect escalation wording without promising immediate support.
  **Verification:** Integration test with escalation triggers
  **Dependencies:** Task 17
  **Estimated scope:** Large

### Task 21: Cold lead re-engagement worker

**Description:** Scheduled TaskIQ task that finds stale leads and sends AI-generated, context-aware follow-up hooks.
**Acceptance criteria:**

- [X] Identify opted-in stale inquiries using the configured delay; new messages, opt-out and staff takeover invalidate pending outreach.
- [X] Generate a context-aware multilingual reminder with a continue button; no invented urgency, deadlines, prices or admission promises.
- [X] At most two reminder delivery opportunities; a confirmed second reminder marks a still-inactive early-stage inquiry `cold`.
  **Verification:** The feature-group gate passes lint/format, strict typing over **92
  source files** and **186 default tests** (two pre-existing RAG AsyncMock warnings).
  The full PostgreSQL suite passes **60 cases** and two real Redis cases pass separately:
  **62 integration cases** total. Sixteen follow-up database scenarios cover explicit signed
  consent, opt-out during takeover, fresh-state checks, queued inbound messages, office
  hours/unknown hours, inactive courses, global disable, provider ambiguity, expired leases,
  concurrent jobs and the two-reminder cap. Five unit cases cover eligibility and generated
  copy refusal/provider-failure fallbacks.

Live OpenAI generation plus independent typed safety checks pass in English, Hindi,
Hinglish and Malayalam (**8 successful structured calls**, no neutral fallbacks).
No Telegram reminder was sent. Migration `e31b0847c629` is applied locally; existing
inquiries remain opted out. The API and worker were restarted with the follow-up task
module, the scheduler's one-shot sweep queued zero jobs, its continuous producer is
running, and authenticated saved-inquiry reads/readiness pass.

Post-review regression reproduced queued reminders remaining executable after consent
fields were rolled back. Downgrade now cancels queued reminders and preserves in-flight
uncertainty; the focused rollback/up-down test passes after the fix. Scoped adversarial
review covered consent defaults/signatures, source freshness, lock release/leases, send
opportunity counting, safe provider fallback, tenant boundaries and rollback. No open
important/critical finding remains. Publication follows the separate PR/CI workflow.
Reminder copy has agent language review; handset-delivered reminders and native-speaker
certification are not claimed. Task 26 analytics and the remaining roadmap stay open.
**Dependencies:** Task 17, 6
**Estimated scope:** Medium

### Task 22: Telegram delivery limits and queuing

**Description:** Track institution send volume, honor Telegram retry delays and queue messages when a configured send budget is reached. ADR-012 supersedes the old Meta tier requirements.
**Acceptance criteria:**

- [X] Conversation replies and standalone deliveries serialize claims per lead and preserve order in both directions; uncertain delivery is not automatically repeated.
- [X] Shared Redis reservations count send attempts per institution-local day; confirmed-delivery totals come from durable database records.
- [X] Workspace warning at 80% usage; authenticated counts, queued totals, reset time and administrator budget control.
- [X] Both bot and standalone replies stay queued at the cap; zero pauses new claims, raising the cap wakes deferred work, and production budget outages defer safely.
  **Verification:** The backend gate passes **181 tests**, lint/format and strict typing
  across **87 source files**. The full PostgreSQL suite passes **44 cases**; two Redis-only
  cases pass separately (**46 integration cases** total). Six budget unit cases cover local
  midnight/DST, replay, pause and unavailable budget storage. Four budget database cases
  cover both delivery queues, concurrent claims, scoped settings, exact provider retry delay,
  migration up/down and preservation. Rate-limited replies remain queued beyond three
  attempts and wait the full delay measured from rejection. Final review reproduced a lost-statistics-write
  regression; the four budget database cases pass after making confirmed counts DB-owned.
  The real Redis test admits exactly seven of 100 competing reservations, replays without
  spending again and proves tenant isolation. Failed/uncertain attempts consume budget.

Frontend lint/build and **all five browser scenarios** pass, including budget pause/change,
unavailable statistics, existing staff/payment flows and live OpenAI knowledge search.
Desktop/mobile screenshots show no overflow; the UI detector reports no findings. The
local database is at `9c5027e2d614`; API/worker/frontend were restarted on current sources,
and authenticated budget/readiness reads pass. No synthetic outbound event or unsolicited
Telegram message was added for this validation. Counts track attempts from activation;
they are an operational send cap, not a provider billing guarantee. Production requires
shared persistent Redis; the in-process development fallback is explicitly limited.

Adversarial review covered atomic caps, ambiguous sends, crash/replay reservation safety,
zero-limit revocation, repeated provider rate limits, tenant/admin boundaries, browser-write protection, local days and
queue recovery. No open important/critical finding remains in this slice. Publication
follows the separate feature-branch/PR/CI gate; GitHub records its result. Task 21
consent-aware follow-ups is next.
**Dependencies:** Task 6
**Estimated scope:** Small

### Task 23: Langfuse tracing integration (optional, not selected)

**Disposition:** ADR-012 replaces the mandatory vendor requirement with shared-provider
redacted model/usage/latency telemetry. That baseline is implemented and verified.
The following vendor-specific acceptance is an optional future integration, not
unfinished required release work; raw prompt/content logging is not permitted.

**Description:** Add `@observe()` decorators to all LLM-calling functions. Configure Langfuse client in `tracing.py`.
**Acceptance criteria:**

- [ ] Every LLM call visible in Langfuse with input/output/latency/cost
- [ ] Graph execution path traceable per conversation
  **Verification:** Run a conversation and verify traces appear in Langfuse dashboard
  **Dependencies:** Task 17
  **Estimated scope:** Small

---

## Phase 5: Dashboard Frontend (Next.js)

### Task 24: Initialize the Next.js staff workspace

**Description:** The current SPEC uses the existing responsive staff design and plain CSS; the archived dark/glass/Tailwind baseline is superseded.
**Acceptance criteria:**

- [X] Next.js app runs on localhost:3000; lint/build and browser navigation pass.
- [X] Current staff-design tokens, responsive layouts and visible focus styles are in place; no dark/glass redesign is claimed.
  **Verification:** Visual inspection of a test page
  **Dependencies:** None
  **Estimated scope:** Small

### Task 25: Authentication (email + password)

**Description:** Login with expiring opaque database sessions and administrator/staff roles under the current SPEC; JWT storage from the archived baseline is superseded.
**Acceptance criteria:**

- [X] Provisioned staff accounts can log in; default Overview and unauthorized access are verified.
- [x] Administrators create staff, manage roles/active status and reset initial passwords. Private first-login password setup is mandatory; role/reset/deactivation revokes sessions. No email invitation delivery or public self-registration is claimed.
  **Evidence:** Three PostgreSQL account tests cover setup, rotation, tenant isolation, revocation and concurrent last-admin protection; browser setup flow passes.
- [X] Hashed opaque sessions use expiring HttpOnly cookies; passwords are salted hashes.
- [X] Route guards enforce tenant scope and administrator writes versus staff reads.
  **Verification:** Login flow + unauthorized access blocked
  **Dependencies:** Task 24, Task 2 (backend auth endpoints)
  **Estimated scope:** Medium

### Task 26: Dashboard home (funnel analytics)

**Current execution:** Implemented on `codex/institution-reporting`, based on fresh
`origin/main` after PR #5 (`91f4370`). Real ORM transitions append same-transaction
`lead.stage_changed` audit records; current counts and known stage reach remain separate.
Older transitions are not invented. All aggregates use one repeatable-read snapshot,
creation-cohort date bounds and session-derived tenant scope. Application/payment
records supply verified conversions; paid and waived remain distinct. The Overview
home preserves access to knowledge, inquiries, staff requests, management and payments.
**Description:** Main dashboard page with funnel visualization and time-series lead charts.
**Acceptance criteria:**

- [X] Lifecycle funnel separates current states from observed reach, with explicit inquiry-cohort denominators; application, fee-satisfaction and enrollment conversions have real numerators/denominators.
- [X] Institution-local daily/weekly inquiry series, zero-filled gaps, accessible exact counts and bounded date filters.
  **Verification:** The group gate passes lint/format, strict typing over **93 source
  files** and **193 default tests**, with two existing RAG AsyncMock warnings. The full
  PostgreSQL suite passes **65 cases**, and two real Redis cases pass separately:
  **67 integration cases** total. Seven analytics unit cases cover local/DST boundaries,
  Monday buckets, zero/invalid denominators, date limits and calendar extremes. Five
  real-database cases cover scoped counts, corrections/history, empty/weekly data,
  tenant/session controls, transactional/no-op/rollback auditing and consistent metrics
  under a concurrent arrival.

Frontend lint/build and **all six browser scenarios** pass. The new overview case
covers filter changes, chart/denominator content and error/empty recovery. Real login
now confirms the Overview data before navigating to live upload/search and the saved
Telegram inquiry. Desktop/mobile inspection passes after a scoped override of the
legacy table minimum width; two bounded visual rounds are complete, with captures
under ignored `frontend/playwright-report/analytics-*.png`. Normal browser runs keep
layout assertions and make captures opt-in with `CAPTURE_UI=1`. The UI detector reports
no findings. API/worker/frontend run the current sources; live authenticated analytics
reports two saved demo inquiries and one applicant, with matching current-state and
series totals and healthy readiness. No history was backfilled and no Telegram message
was sent for analytics validation.

Scoped adversarial review covered tenant/session isolation, conversion subset rules,
concurrent snapshots, audit rollback/no-op behavior, date limits and data freshness
labels. No open important/critical finding remains in this slice. Publication follows
the feature-branch/PR/CI workflow; remaining CRM/exports/operations work stays open.
**Dependencies:** Task 25
**Estimated scope:** Medium

### Task 27: Leads CRM page

**Description:** Table/grid of all leads with status filters, search, and click-to-view detail.
**Acceptance criteria:**

- [X] Leads fetched from FastAPI, filtered by tenant
- [X] Status indicators (color-coded by lifecycle state), all twelve states available as filters
- [X] Known-name word-prefix search and exact international phone lookup; private POST body, audited access
  **Evidence:** Four real PostgreSQL cases cover tenant boundaries, role access, Unicode
  prefixes, encrypted-name rename/removal, idempotent legacy backfill, bounded 50-row
  pagination and exports. The browser scenario verifies the private query body, all status
  options, name display and 390/1440px layouts. No phone is inferred from Telegram.
  Migration `a82df107e436` applies/reverses; local backfill processed both saved inquiries.
  **Verification:** Load with seeded leads
  **Dependencies:** Task 25
  **Estimated scope:** Medium

### Task 28: Course & batch management

**Description:** CRUD pages for courses, batches, discount codes, and qualification rules builder.
**Acceptance criteria:**

- [X] Create/edit/archive courses and batches; archiving preserves existing inquiry references.
- [X] Qualification rules builder with typed field/operator/value validation.
- [X] Batch capacity and available seats visible; invalid capacity rejected.
- [X] Administrator discount-code create/edit/deactivate, percentage/fixed values, usage limits and immutable usage counts.
  **Evidence:** Real database management tests cover roles, tenant boundaries, invalid values
  and duplicate-code conflicts. Actual Chrome creation/archive, rules, fees and seat values
  pass at desktop/mobile widths. The permanent catalog/discount browser test passes,
  including INR-to-paise conversion and deactivation. Settings and application screens
  inherit the existing staff design; detector checks report no findings.
  **Verification:** Create a course with rules, verify in DB
  **Dependencies:** Task 25
  **Estimated scope:** Large

### Task 29: Chat takeover + co-pilot UI

**Description:** Messaging interface showing chat history, AI suggested replies, and manual send.
**Acceptance criteria:**

- [X] Unified student/bot/standalone-delivery history with payment labels, timestamps and delivery states; stable tenant-scoped cursor pagination includes equal-time messages.
- [X] AI-suggested replies support review, edit, approve and ignore; stale/expired drafts cannot be approved.
- [X] Manual text input queues a tenant-scoped Telegram reply with an exact-text confirmation and replay-safe send reference.
- [X] "Take Over" and "Resume AI" buttons, confirmations and bot-pause status.
  **Verification:** PostgreSQL/API and browser scenarios cover takeover, manual-send retry, reviewed drafts and resume. The explicitly authorized staff-authored test delivery is saved as sent and the user confirmed handset receipt; automation was restored.
- [x] Confirm one explicitly authorized staff reply on the Telegram handset.
  **Dependencies:** Task 25, 20
  **Estimated scope:** Large

### Task 29.1: Student answer presentation

**Description:** Show only the grounded answer in Telegram, as requested after the PDF handset check.
**Acceptance criteria:**

- [x] Student FAQ replies omit source filenames and citation footers.
- [x] Exact-quote support, relevance, course/tenant scoping and abstention remain enforced.
- [x] Document/chunk IDs, source path and passage survive encrypted reply persistence; transport sends only text/buttons.
- [x] Staff document search retains its sources; legacy queued replies remain readable.

**Verification:** Test-first transport regression failed on the previous reply contract,
then passed with internal typed evidence. The full backend gate passes **252 tests**,
lint/format and strict typing over **117 source files**. **16 PostgreSQL journey,
contextual-retrieval and staff-operation cases** pass. The multilingual live FAQ
rerun passes **24/24** with metadata-based source scoring and no visible footer.
A read-only live PDF answer contains Lotus Hall with its internal source; no additional
Telegram message was sent. Native API and Telegram worker were restarted. Existing
saved messages retain their original text; staff search still displays sources.
Adversarial self-review covered evidence identity/tenant scope, old queued payloads,
transport allowlisting, abstention and synthetic-data publication; no open important
or critical finding. This source slice does not revalidate the old Compose image.

### Task 30: Document management (RAG)

**Description:** Upload/delete institution documents for the RAG knowledge base.
**Acceptance criteria:**

- [X] File upload triggers durable backend ingestion (verified with an isolated worker invocation).
- [X] Document list shows indexed files and failure/retry states.
- [X] Delete removes chunks from pgvector; an active worker cannot republish a deleted upload.
- [x] Upload a PDF and verify a grounded answer through the live Telegram bot.
  **Verification:** See current execution evidence above. API/database and fixture-backed UI
  checks are complete locally. A retained synthetic counselling PDF indexed through
  the real worker; the Lotus Hall answer is saved as sent and user-confirmed.
  See `docs/verification/LIVE_HANDSET_RESULTS.json`; the later footer change was verified
  read-only against OpenAI without another handset send.
  **Dependencies:** Task 13; provisioning/authentication slice of Task 54.
  **Estimated scope:** Medium

### Task 31: Application submissions view

**Description:** Table of submitted application forms with detail view and manual review actions (Approve, Waitlist, Reject).
**Acceptance criteria:**

- [X] Submitted encrypted form fields displayed through audited tenant-scoped staff APIs.
- [X] Application links to the saved inquiry and conversation.
- [X] Explicit administrator approve/waitlist/reject controls; unpaid approval is blocked,
  final decisions cannot be overwritten, and approval allocates one available seat atomically.
  **Evidence:** Two simultaneous approvals compete for one seat: one succeeds and one receives
  409; repeating the winner does not allocate another seat. Actual staff-browser validation
  uses a labelled synthetic seeded form, blocks unpaid approval, saves waitlist, checks
  desktop/mobile and opens its inquiry. No Telegram updates were fabricated for that fixture.
  **Verification:** Submit an application via bot, verify it appears in dashboard
  **Dependencies:** Task 25, 18
  **Estimated scope:** Small

### Task 32: Data export + webhook config

**Description:** CSV/Excel export for leads, applications, payments. Outbound webhook URL configuration.
**Acceptance criteria:**

- [X] Administrator download buttons generate CSV/Excel for inquiries, applications and payments
- [X] Administrator webhook destination and signing-secret rotation configurable per institution
- [X] Transactional minimal events dispatched promptly through a signed, leased, bounded-retry outbox
  **Verification:** Export data + configure a test webhook URL
  **Export evidence:** Tenant-scoped, CSRF-protected, audited downloads pass real database
  checks for all three datasets in both formats. CSV formula-like text is neutralized;
  Excel stores text cells explicitly. Amounts retain database minor units, phone strings
  retain country codes, ciphertext/search indexes are excluded, and exceeding the configured
  row/cell limit fails explicitly. Actual local authenticated CSV downloads returned two
  inquiries, one application and zero payments; browser download and mobile layout pass.
  **Webhook evidence:** Six PostgreSQL cases cover source rollback, endpoint roles and
  encrypted secrets, tenant scope, concurrent claims, retry/recovery/caps, configuration
  cancellation and per-user socket receipts. Twenty security cases cover public-address
  checks, mixed DNS answers, IP pinning with TLS hostname verification, signatures,
  non-followed redirects and secret-safe logs. The browser verifies destination settings,
  secret handling and delivery status at 390/1440px. Delivery uses a simulated receiver;
  no real external webhook is configured or contacted locally.
  **Dependencies:** Task 25
  **Estimated scope:** Medium

### Task 33: Real-time WebSocket notifications

**Description:** Notification bell with live badge count for new events.
**Acceptance criteria:**

- [X] Authenticated same-origin WebSocket established on dashboard load, with reconnect and HTTP recovery
- [X] Badge reflects committed eligible-inquiry, failed/uncertain Telegram delivery, co-pilot draft and confirmed-payment events
  **Verification:** A real authenticated browser connection through Next.js received a
  committed synthetic eligibility event and persisted its read receipt. Source Telegram
  delivery tests verify one attention event for an ambiguous send. Mocked browser events
  verify all four notification categories; database tests prove cross-tenant exclusion,
  separate staff receipts, session revocation and unread backlogs beyond 50. The temporary
  local inquiry was removed afterward; no Telegram message or real webhook was sent.
  All nine browser scenarios pass, alongside 220 default tests/105 strictly typed source
  files and 75 PostgreSQL plus two Redis cases across full/focused runs. Desktop/mobile
  visual QA completed in two bounded rounds; no further cosmetic pass is needed.
  **Dependencies:** Task 25
  **Estimated scope:** Medium

### Task 34: Institution settings

**Description:** Settings page for business hours, re-engagement timing, and general config.
**Acceptance criteria:**

- [X] Business hours and IANA timezone configurable with opening/closing validation.
- [X] Re-engagement delay and optional retention period configurable by administrators.
- [x] Configured timezone/business hours affect follow-ups and escalation; tenant retention controls history cleanup, verified with boundary and inactivity cases.
  **Verification:** Update settings, verify they affect bot behavior
  **Dependencies:** Task 25
  **Estimated scope:** Small

---

## Phase 6: Production Hardening

### Task 35: Sentry error monitoring (optional, not selected)

**Disposition:** ADR-012 selects sanitized operational logs and inspectable durable
failure state without requiring an extra vendor. The Sentry-specific acceptance
below is optional future work; unredacted full applicant context is not permitted.

**Description:** Integrate Sentry SDK in both FastAPI and TaskIQ worker.
**Acceptance criteria:**

- [ ] Unhandled exceptions reported to Sentry with full context
  **Verification:** Trigger a test error, verify it appears in Sentry
  **Dependencies:** Task 17
  **Estimated scope:** Small

### Task 36: Explicit inquiry erasure

**Description:** Administrator-only, CSRF-protected `DELETE /api/staff/leads/{id}` with matching `confirm_id`; the dashboard requires typing the inquiry reference before confirmation.
**Acceptance criteria:**

- [x] Tenant-owned inquiry identity, facts, memories, messages, inbox/outbox, applications, payment records and linked integration deliveries/receipts are deleted through the linked data graph
- [x] Subject-linked audit details are scrubbed; a receipt retains actor/reason without the deleted inquiry reference or student content
- [x] Active sends, payment creation and webhook delivery block erasure; lead locking serializes outbound claims against deletion
  **Verification:** Five privacy PostgreSQL cases plus browser mock and actual synthetic inquiry deletion; nonadministrator and cross-tenant requests fail. No real inquiry was deleted. This is database erasure, not provider/backups deletion or withdrawal; enrolled seats are not released.
  **Dependencies:** Task 4, 25

### Task 37: Conversation-history retention

**Description:** Scheduler expires inactive conversation history and saved dialogue facts at each institution's optional retention period. It preserves the inquiry/contact profile, submitted applications and financial records. Full inquiry erasure remains explicit Task 36.
**Acceptance criteria:**

- [x] History, memory, dialogue facts and associated conversation delivery/integration data expire after the latest created/inbound/outbound timestamp crosses the UTC cutoff
- [x] Configurable per tenant; blank disables cleanup. Active/queued inbound work is skipped, and an expiry marker makes repeated sweeps idempotent
- [x] Pending reminders/consent and queued payment creation from expired dialogue are invalidated; confirmed payments and business records remain
- [x] Retention runs independently of queue startup/dispatch failures
  **Verification:** Artificially aged PostgreSQL data, cutoff/activity/tenant boundaries, preserved financial graph and scheduler outage tests. The local one-shot removed zero histories. No actual data removal is claimed.
  **Dependencies:** Task 4, 34, 36

### Task 38: Full local Docker Compose runtime

**Description:** One isolated local profile for PostgreSQL/pgvector, persistent Redis, migration/synthetic-seed bootstrap, API, all registered workers, scheduler and standalone Next.js. Telegram polling is explicitly enabled through the `telegram` profile; it is not started against an existing bot by default. No extra telemetry vendor is required by ADR-012.
**Acceptance criteria:**

- [x] Private configuration generator creates fresh keys without reading live credentials or overwriting an existing file
- [x] `docker compose up --build -d --wait` builds and starts the core services from a fresh isolated database; startup waits for migrations/data readiness
- [x] API/frontend run as nonroot users; data services have no host ports and UI/API bind only to loopback
- [x] Actual container login/proxy, authenticated WebSocket, data readiness, Redis/TaskIQ result and scheduler checks pass
- [x] Stop/start preserves the synthetic credential and successful access; seed reruns preserve existing accounts
- [x] Receiver profile validates and its image CLI loads; live container polling remains deliberately unclaimed because the existing native receiver owns the demo bot
  **Verification:** The original nine-check restart pass is extended by a real dedicated knowledge-worker roundtrip. The document slice passes ten restart checks before its platform-only typing correction and nine current smoke checks afterward; `docs/verification/COMPOSE_RESULTS.json` records results. Backend gate passes 234 tests/111 strictly typed source files; nine affected PostgreSQL journey/account cases pass. Linux image builds and migrations succeed; the existing native data/bot runtime is untouched.
  **Runbook:** `docs/engineering/COMPOSE_RUNTIME.md`; bootstrap config from `src.scripts.configure_compose` first.
  **Dependencies:** Platform/application migrations and Task 25

### Task 39: Production deployment

**Description:** Deploy the verified application to a selected public host with TLS, correct same-origin WebSocket proxying, protected persistent data, backups and a secret store. The user deferred choosing hosting until remaining build tasks are finished. The earlier Vercel/Fly/Neon list is a proposal, not provisioned infrastructure; optional observability vendors are not dependencies under ADR-012.
**Acceptance criteria:**

- [ ] Frontend accessible on production URL
- [ ] Backend API accessible on production URL
- [ ] Webhooks configured with production URLs
  **Verification:** End-to-end test on production
  **Dependencies:** All previous tasks
  **Estimated scope:** Medium

---

## Phase 3–4 addition (see ADR-008)

### Task 40: Typed OpenAI decision layer

**Description:** Use the existing OpenAI-only provider boundary through `src/decision/decide()` for typed interpretation and extraction. No model training or additional decision service. Eligibility, readiness state, money and authorization remain code-owned. Model output is not a calibrated probability.
**Acceptance criteria:**

- [x] Shared typed interface validates structured OpenAI responses and rejects refusal/malformed output
- [x] Active Telegram intent/escalation routing and eligibility extraction use `decide()`; deterministic safety and explicit staff-request shortcuts remain
- [x] Input/output safety classifiers and compatibility triage use the same decision boundary
- [x] Configured OpenAI roles, bounded provider calls and safe failure behavior retained
  **Verification:** Decision, triage and safety unit checks; PostgreSQL Telegram journey checks. Exact-quote generation remains behind the existing provider factory. No AI readiness score or calibration claim.
  **Dependencies:** Task 9, 10, 11, 15

Task 41 was removed by user decision. No distillation, fine-tuning, self-hosted decision backend or training-label collection is planned.

### Task 42: Advanced RAG Strategy (ADR-011)

**Description:** Implement the advanced RAG pipeline to handle complex document parsing, latency-optimized reranking, and contextual query rewriting.
This epic is broken down into sub-tasks for focused implementation.

### Task 42.1: Complex Document Parsing & Semantic Chunking

**Description:** Integrate `docling` (or `unstructured`) to parse uploaded PDFs into Markdown, preserving tables and structure. Replace the naive sliding window chunker in `rag.py` with a semantic, Markdown-header-aware chunker.
**Acceptance criteria:**

- [x] Document ingestion pipeline uses `docling`/`unstructured` for PDFs
- [x] Tables and multi-column layouts are preserved in Markdown
- [x] Semantic chunking implemented (retains parent-child headers)
  **Verification:**
- [x] Unit tests uploading a complex PDF and verifying structural chunks

**Evidence:** The real two-page digital PDF passes authenticated upload, offline Docling
layout/table parsing and atomic PostgreSQL chunk publication. Both programmes retain
row/amount associations, heading ancestry, column text and the fee caveat. DOCX block
order, table-row chunking and child-process timeout/cancellation/memory boundaries pass.
The live dedicated Redis worker -> Docling -> OpenAI embeddings -> authenticated search
check passes all four checks and removes its own temporary upload; see
[DOCUMENT_PIPELINE_RESULTS.json](DOCUMENT_PIPELINE_RESULTS.json).
No Telegram message or OCR was used. Native setup, the serial ingestion worker and
limits are documented in [DOCUMENT_INGESTION.md](../engineering/DOCUMENT_INGESTION.md).

  **Dependencies:** Task 13
  **Files likely touched:** `backend/pyproject.toml`, `backend/src/core/rag.py`
  **Estimated scope:** Medium

### Task 42.2: Contextual query resolution and course-scoped evidence

**Description:** Resolve follow-up references using typed OpenAI decisions over six recent completed/sent turns (at most 1,000 characters each). Standalone questions keep the existing translation/ASCII fast path. Unresolved references ask for clarification instead of searching. Model output never changes the student's course selection or invents an authoritative course ID.
**Acceptance criteria:**

- [x] OpenAI query resolution uses bounded recent conversation; ambiguous references produce reviewed clarification copy in all four languages
- [x] Rewrites preserve original meaning and cannot introduce unsupported numeric values; Unicode digits and Indian grouping/decimal formatting compare by value
- [x] Explicit active catalog names override the selected course for the lookup, including comparisons; application fees still come only from the matching database rows
- [x] Administrators assign an indexed reference to a course or shared institution information; course ownership, tenant scope and browser-write checks are enforced
- [x] Student retrieval filters approved document metadata before ranking; staff may explicitly search a course plus shared references
- [x] Staff scope/search controls support loading, unavailable course, failure recovery and desktop/mobile layouts
  **Verification:** Backend gate 240 tests/113 strict source files; full PostgreSQL suite 86 passed plus two actual Redis tests. Twelve browser scenarios pass, including scope save/search/failure recovery; desktop/mobile QA completed. Live query-resolution evaluation initially passed 7/8 (ambiguous singular reference became a plural request); the clarified prompt passes 8/8. Source/abstention rerun passes 24/24. No student message was sent and no live document scope was changed.
  **Evidence:** `tests/test_retrieval_query.py`, `tests/integration/test_contextual_retrieval.py`, `frontend/tests/knowledge-scope.e2e.ts`, `docs/verification/CONTEXTUAL_RETRIEVAL_RESULTS.json`.
  **Dependencies:** Task 13, 40, 42.3

### Task 42.3: Required OpenAI passage reranking

**Description:** Rerank the approved, relevance-filtered candidate passages in the active Telegram FAQ and staff document-search paths using the existing OpenAI configuration. This is current release work, independent of contextual rewriting. No additional provider or local model service.
**Acceptance criteria:**

- [x] Reranking runs for two or more eligible candidates before selecting the top five
- [x] At most 12 tenant-owned candidates; bounded question/passage text and configurable timeout
- [x] Accept only a complete permutation of original candidate IDs; preserve original content and source references
- [x] Retain relevance floor, approved-upload scope, injection filtering, exact-quote verification and unsupported-question abstention
- [x] Timeout, refusal, provider failure or invalid ranking falls back to the filtered retrieval order
- [x] Unit checks cover ordering, tenant isolation, injected passages, malformed rankings and outage fallback; PostgreSQL document/journey checks pass
  **Verification:** `tests/test_decision_reranking.py`, document workspace and Telegram journey integration tests. Live synthetic checks pass **4/4 ranking cases** and **24/24 FAQ source/abstention cases** across the four languages; results are recorded in `docs/verification/RERANKING_RESULTS.json` and `EVALUATION_RESULTS.json`. No broad fluency or production latency claim.
  **Dependencies:** Task 13; does not wait for Task 42.2
  **Files:** `src/decision/`, `src/knowledge/reranking.py`, `src/knowledge/api.py`, `src/engine/journey.py`

---


### Task 61: Adaptive, student-first conversation

**User decisions:** Institution applicants only; adaptive conversation with optional
further exploration; English for now; student fit takes priority over enrollment.
**Acceptance criteria:**

- [x] Primary sources and assessment licensing/limitations documented.
- [x] Contextual questions follow the student's evidence rather than a fixed question order.
- [x] Decided, comparing and exploring applicants receive different support; corrections,
  refusal, uncertain/conflicting interests and resumed conversations are respected.
- [x] Profile evidence and tentative interest themes persist encrypted and stay separate
  from code-owned academic eligibility; no diagnostic/aptitude/percentage-fit claims.
- [x] Existing/newly discovered institution courses are presented with grounded reasons,
  checked availability and eligibility uncertainty; no unsuitable-course pressure.
- [x] Missing desired offerings are honestly acknowledged; external fields are explicitly
  exploratory and do not claim verified colleges, fees, recognition or job outcomes.
- [x] Optional deeper exploration and requested official assessment resources work;
  ordinary turns use text and no compulsory language/quiz menu.
- [x] Text and signed selections, FAQ interruptions, human/safety requests, consent,
  application locks, tenant isolation and provider-failure recovery are covered.
- [x] Authorized staff can inspect the saved guidance evidence and classification.
- [x] Full applicable gates, realistic OpenAI scenarios and adversarial review pass;
  native runtime is refreshed.
- [x] Publish through a reviewed protected-main PR with green exact-head CI
  ([PR #15](https://github.com/Shan091/admitflow/pull/15)).

**Verification:** Backend gate: 280 default tests, lint/format and strict typing over
120 source files. Full PostgreSQL: 99 passed; the two Redis-only cases pass separately.
Frontend lint/build and 14 browser scenarios pass across the mock/local runs, with
all four real login/catalog/upload/account/WebSocket cases passing after restoring
the stopped API. The guidance panel was inspected at 1440/390 px; no overflow or
mechanical design findings. Live OpenAI: 13/13 independent scenarios and 3/3 multi-turn
conversations; no Telegram message was sent by these verifiers.

Test-first checks exposed language-menu onboarding, unsafe academic-number extraction,
stale source identity after a concurrent course closure, and a validated correction
lost when reply generation failed. The completed flow separates interpretation from
planning, refreshes catalog entities, preserves unrelated notes and recomputes selected
course eligibility even when the reply fails. Code-owned confirmation wording and
cleared pending choices prevent an unrelated yes after failed deeper exploration
from selecting a course. Self-review covered tenant/explicit-choice
boundaries, submitted applications, pause/consent, profile privacy and unsupported
claims, with no open important/critical finding. The native API/default worker were
reloaded; the receiver and serial knowledge worker listen and the scheduler runs.

**Evidence:** `docs/engineering/ADAPTIVE_GUIDANCE_RESEARCH.md`, ADR-013,
`docs/verification/GUIDANCE_EVALUATION_RESULTS.json`, `GUIDANCE_RELEASE_RESULTS.json`,
`tests/test_guidance.py`, `tests/integration/test_adaptive_guidance.py`,
`tests/integration/test_guidance_transitions.py`, `frontend/tests/guidance.e2e.ts`.
This is exploratory guidance, not psychometric validation or real-student outcome
evidence. No new adaptive-guidance handset receipt or upgraded running Compose image
is claimed. Public deployment remains deferred by the owner.

### Task 62: Natural collection, reset and response latency

The pasted messages were separate examples, not a report of conflicting replies.
The requested work is personalised conversation, less repetitive copy, automatic reuse
of explicit details, a fresh testing context, typing feedback and lower response latency.

- [x] Collect only missing application details, reuse verified explicit information,
  support interruptions/pauses/corrections, and require review before submission.
- [x] Reduce unnecessary menus and repeated technical notices.
- [x] Confirm `/reset`, exclude earlier history, invalidate old course/application controls and preserve
  submitted business records, consent and staff ownership.
- [x] Bound best-effort typing and keep failures outside the durable reply path.
- [x] Reduce model/SQL overhead; record measured latency and call counts.
- [x] Pass regression, real database, live provider and full quality gates; complete
  adversarial review with no open critical/important findings.
- [x] Publish through protected-main CI ([PR #16](https://github.com/Shan091/admitflow/pull/16)).

**Evidence:** 293 default tests and strict typing over 124 source files; 107 distinct
PostgreSQL cases verified across the full run and focused assertion/regression rechecks,
plus two dedicated Redis checks. The full run's remaining failure was an old English
copy assertion; its refreshed case and all nine final focused regressions pass.
Live OpenAI: 13 independent guidance cases, three multi-turn dialogues and five
collection cases pass. The three collection model turns took 1,078–2,663 ms (median
1,297 ms) locally; greetings/simple form answers and active numeric eligibility answers
need no model. This is a small local sample, not a production latency percentile.

Adversarial checks cover repeated numeric questions after punctuation, omitted literal
marks, direct named-course confirmation, unavailable courses, inferred DOB, literal
detail extraction, reset replay/history boundaries, retained institutional contact data,
staff ownership/consent, one/two-query context loading and typing cancellation/failure.
See ADR-014, `CONVERSATION_EVALUATION_RESULTS.json` and
`CONVERSATION_RELEASE_RESULTS.json`. No new handset observation or public deployment
is claimed; submitted applications keep their existing review controls.

### Task 63: Guidance should feel supportive, not like an interview

- [x] Document primary-source techniques and their implementation mapping in
  `docs/engineering/ADAPTIVE_GUIDANCE_RESEARCH.md` and ADR-015; adapt to each student
  without turning background, interests and prerequisites into a universal sequence.
The owner tested tech → web development → some experience → one project → no technologies.
The assistant kept demanding details instead of helping, and showed no answer choices.

- [x] Accept short/uncertain answers without technical project or portfolio demands.
- [x] Offer contextual optional choices when they reduce effort, with free text always available.
- [x] Prevent repeated open-ended discovery loops and preserve honest course availability.
- [x] Validate signed/one-use options, replay and reset boundaries, and no implicit course selection.
- [x] Verify unit/database/live short-answer journeys and quality gate; document evidence.
- [x] Publish through the reviewed protected-main PR/CI gate
  ([PR #17](https://github.com/Shan091/admitflow/pull/17)).

Task 63 local evidence: **305 default tests**, strict typing across **126 source files**,
**115 PostgreSQL cases** and **two separate Redis checks** pass. Live OpenAI: the existing
**13 scenarios / three dialogues**, plus **three supportive dialogues** using the production
composer (interest-first, background-first with an actual contextual tap, and an absent
specific field) pass. Eight focused interaction cases cover literal tapped evidence,
replay/stale context, reset, no implicit course selection, appropriate prerequisite
answers and progress-sensitive discovery. See `SUPPORTIVE_GUIDANCE_RESULTS.json`,
`SUPPORTIVE_GUIDANCE_RELEASE.json`, the research document and ADR-015. Research does
not establish psychometric validity or real-student outcomes. Local API/common worker
were reloaded; readiness and UI HTTP checks pass. No public deployment or Telegram
message was sent for this change.

### Task 64: Necessary staff intervention and unavailable payments

- [x] Remove routine guidance/catalogue staff buttons; retain necessary review and explicit help.
- [x] Show unavailable online payments honestly without creating a payable quote or staff handoff.
- [x] Verify uncertainty, eligibility, explicit handoff and payment regressions; document results.

Task 64 evidence: **306 default tests**, strict typing across **126 source files** and
lint/format pass. Focused integration runs pass **25 conversation/reset cases**,
**20 final guidance/payment cases**, and **six final copy/journey cases** (overlapping
runs, not additive counts). Explicit `/human`, failed-rule review, payment confirmation,
receipts and webhook controls remain verified. No live-provider or handset test was
repeated for this control/copy change. Demo Razorpay credentials remain absent and
live payments disabled. Local API/worker/receiver and dashboard were started after
finding the prior processes stopped; readiness, listener and UI HTTP checks pass.
Existing staff requests were preserved. Resolve the test request and use **Resume
automation → Confirm resume**, then `/reset` for a fresh test. No charge or public
deployment was performed. See `docs/verification/NECESSARY_HANDOFF_RELEASE.json`.

### Task 65: Razorpay placeholders and JAIN Kochi reference reset

- [x] Owner selected Kochi and requested removal of all current dummy data.
- [x] Private SerpAPI key saved and used for discovery; official source pages fetched.
- [x] Razorpay key ID/secret placeholders added, webhook secret prepared, live mode disabled.
- [x] Publish 64 programme options as references, with sources and unverified admissions details.
- [x] Remove both institutions' old dummy activity, fake catalogue and documents; retain one
  clearly labelled JAIN Kochi public-information demo with existing admin/bot configuration.
- [x] Index 65 tenant/course-scoped knowledge documents and preserve internal source evidence.
- [x] Block reference applications/payments and unknown-fee-as-zero displays; prevent fake reseeding.
- [x] Verify code, real PostgreSQL reset/rollback, official-reference guidance and desktop/mobile UI.
- [x] Owner-supplied Razorpay test credentials verified read-only and saved encrypted.
- [ ] Actual test payment/card/webhook receipt awaits authorized public test ingress.

Local evidence: **313 default tests**, strict typing across **127 source files**, lint/format;
**121 PostgreSQL cases** pass (two Redis-specific checks skipped in that run). Six final focused
reset/reference/application/payment cases pass. Frontend lint/build and a real authenticated
reference-catalogue test pass at 1440 and 390 px; the mechanical UI scan reports no findings.
Three synthetic OpenAI checks pass: graduate AI guidance, beginner undergraduate computing and
an indexed campus answer. Temporary leads were rolled back, with no Telegram test send.

Reset verification: **one institution, 64 reference options, 65 ready uploads/chunks, zero leads,
applications, staff requests, payments, batches and old synthetic campus-text matches**. Bot
polling position and admin access survive; tenant budget counters are zero. API/worker/receiver,
knowledge worker, scheduler and dashboard run as hidden local processes. No public deployment
or payment was performed. Programme fees, current intake, availability and admission approval
are not verified by public listing. See ADR-016, `JAIN_KOCHI_VALIDATION.json` and
`JAIN_KOCHI_RELEASE.json`. The separate Compose database was not reset.

Task 65 final regression record: 13 distinct synthetic guidance cases pass across the
11/13 sweep and a successful two-case follow-up; all three multi-turn dialogues pass.
The original two provider/typed-decision failure outcomes remain recorded in
`docs/verification/GUIDANCE_EVALUATION_RESULTS.json`; this is not a single all-pass sweep.

Task 65 payment follow-up: the read-only Razorpay test API probe passed; encrypted
institution configuration matches the private environment. Zero payment orders or
links were created. Actual card/webhook receipt is unverified; live mode is disabled.

## Task 69: Warm neutral UI and wordmark cleanup

- [x] Replace green brand accents and surfaces with Claude-inspired ivory, charcoal and terracotta roles.
- [x] Remove the trailing logo dot from login and workspace navigation.
- [x] Keep controls, charts, focus, selection and status colors consistent and accessible.
- [x] Verify desktop/mobile login and workspace views, frontend checks and the repository gate.
- [x] Correct the reference-catalogue list's unknown fee label; prove the regression before/after.

Task 69 evidence: eight desktop/mobile browser checks pass, contrast and overflow checks
pass, frontend lint/build pass, the backend gate passes 313 tests with 127 strictly typed
source files, and the extended reference-catalogue UI test passes. Mechanical scans report
zero findings. Local frontend refreshed; no public deployment or Telegram send. See
`docs/engineering/UI_THEME.md` for palette and verification scope.

## Task 70: Course selection through demo payment and staff approval

- [x] Confirm demo/test mode for the imported catalogue; preserve reference provenance.
- [x] Configure all 64 reference courses at the owner-selected INR 3,000 demo fee.
- [x] Start a labelled, reviewable demo application after confirmed course selection.
- [x] Restrict demo invoices/provider operations to test credentials and development mode.
- [x] Verify payment before staff approval without creating or consuming university seats.
- [x] Validate normal/reference flows, replay/tenant safety, UI labels and local activation.

Task 70 evidence: the baseline gate passes 323 tests with 131 strictly typed source
files; PostgreSQL integration checks and migration up/down pass (129 passed initially; the two
Redis checks also pass when rerun against a disposable Redis instance), with 13 focused demo/reference/webhook/staff-payment cases rerun.
Four browser checks cover desktop/mobile demo review, normal payment lookup and
reference fee labels. All 64 actual local courses were exercised through draft,
explicit submit, fee preview and immutable test invoice; temporary rows rolled back.
A real Razorpay test link was created, read and cancelled. All 64 courses are active;
the owner approved restoring the archived Communication Design option. Existing
records are preserved. Local services and data-store readiness pass. Actual card
completion and new Telegram handset receipts remain unverified; no public deployment.
See ADR-017 and docs/verification/DEMO_APPLICATIONS_VALIDATION.json.

Task 70 adversarial review checked disabled/production mode, tenant boundaries,
provider references and captured amounts, replay, unpaid/waived approval and seat
preservation. The staff provider-lookup path now rechecks demo mode and records
its request; no open critical or important findings remain.

Final post-review follow-up: all 11 demo/staff-payment integration checks pass;
API/data stores and frontend are healthy after the interrupted local session resumed.
