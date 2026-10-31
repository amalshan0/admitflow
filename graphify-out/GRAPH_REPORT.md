# Graph Report - admitflow  (2026-10-30)

## Corpus Check
- cluster-only mode — file stats not available

## Summary
- 2884 nodes · 8975 edges · 146 communities (119 shown, 27 thin omitted)
- Extraction: 94% EXTRACTED · 6% INFERRED · 0% AMBIGUOUS · INFERRED: 548 edges (avg confidence: 0.91)
- Token cost: 0 input · 0 output

## Graph Freshness
- Scope: refreshed source structure plus current document headings and explicit links.
- Base commit below includes the Task 71 working-tree changes; it is not a clean-commit fingerprint.
- Prior inferred code relationships are navigation hints; verify claims against source/tests.
- Built from commit: `e195fc9d`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- Security & Production Readiness
- Current product tasks
- AdmitFlow Backend
- AdmitFlow specification
- Reply
- budget.py
- test_demo_applications.py
- test_integrations_notifications.py
- RedisMutex
- broker.py
- Historical implementation evidence
- transport.py
- WhatsAppSender
- core/rag.py
- test_schema_metadata.py
- evaluate_faq.py
- Settings
- admissions.tsx
- retrieval.py
- GuidanceMemory
- analytics
- route
- Adaptive course guidance: evidence and implementation
- test_scheduler_independence.py
- Phase 3–4 addition (see ADR-008)
- Code Standards
- ExtractionError
- TestClient
- run_postman_check.cjs
- update
- sqlalchemy
- leads.py
- crm.py
- management.py
- test_migrations.py
- live
- test_structured_documents.py
- AdmitFlow
- alembic
- main.py
- tests/test_conversation_readiness.py
- ADR-012: Telegram and OpenAI for the first complete admissions MVP
- Phase 5: Dashboard Frontend (Next.js)
- signin
- StaffUser
- test_knowledge_workspace.py
- ADR-002: Durable intake, concurrency and duplicate handling
- Product and engineering constraints
- normalize_e164
- TelegramBot
- model_validator
- ADR-009: Application-Layer PII Encryption and Audit Logging
- check_input_safety
- staff/applications.py
- detect_intent
- accept_update
- PaymentOrder
- ADR-018: One maintained institutional product
- LeadStatus
- Phase 4: Advanced Bot Features
- identity_database
- exports.py
- ADR-006: Code-Owned Qualification and Payments
- compilerOptions
- auth.py
- integration/test_followups.py
- log_delivery_status
- api.py
- tests/test_telegram_identity.py
- pydantic
- Language review packet
- 20261008_add_knowledge_models_for_rag_c470f1ebc811.py
- guidance.py
- management.tsx
- GuidanceDecision
- json
- verify.sh
- db/__init__.py
- src/__init__.py
- tasks/__init__.py
- analytics.tsx
- whatsapp/__init__.py
- integration/__init__.py
- admitflow-backend
- scope.py
- download
- DocumentChunk
- reindex_batch
- test_payment_and_webhook_claims_wait_for_erasure_lock
- build_structured_pdf.py
- package.json
- erase_inquiry
- @playwright/test
- ADR-004: Durable conversations and explicit facts
- ADR-010: Keep routine uncertainty with the assistant
- process_upload
- next-env.d.ts
- ADR-014: Natural collection, context reset and responsive replies
- CRM search and institution downloads
- layout.tsx
- Staff access and privacy operations
- scripts
- Typed OpenAI decisions and required reranking
- copilot.py
- test_pdf_parser_boundary.py
- enums.py
- models/__init__.py
- Contextual retrieval and reference scope
- This is NOT the Next.js you know
- devDependencies
- applications.tsx
- dependencies
- encrypt_pii
- outreach/followups.py
- hashlib
- knowledge/processing.py
- handle_turn
- payments.tsx
- asyncio
- Knowledge
- consent.py
- env.py
- timeline.py
- test_staff_operations.py
- page.tsx
- test_shared_budget_preserves_queues_and_settings_wake_deliveries
- RuleOperator
- get_settings
- workspace
- telegram/service.py
- notifications.tsx
- operations.py
- frontend_next_types_root_params_d
- frontend_next_types_routes_d
- staff/payments.py
- index_known_name
- RazorpayClient
- write_audit_log
- test_worker_broker.py

## God Nodes (most connected - your core abstractions)
1. `encrypt_pii()` - 118 edges
2. `Reply` - 107 edges
3. `write_audit_log()` - 103 edges
4. `LeadStatus` - 102 edges
5. `get_settings()` - 102 edges
6. `TelegramBot` - 100 edges
7. `Lead` - 98 edges
8. `Course` - 91 edges
9. `decrypt_pii()` - 88 edges
10. `route()` - 82 edges

## Surprising Connections (you probably didn't know these)
- `Security and operations` --references--> `Security & Production Readiness`  [EXTRACTED]
  README.md → docs/engineering/security-and-production.md
- `1. Understand & spec  (`/spec`)` --references--> `AdmitFlow specification`  [EXTRACTED]
  docs/engineering/development-lifecycle.md → SPEC.md
- `8. CI → merge` --references--> `Current product tasks`  [EXTRACTED]
  docs/engineering/development-lifecycle.md → tasks/todo.md
- `Merging` --references--> `Current product tasks`  [EXTRACTED]
  docs/engineering/git-and-github.md → tasks/todo.md
- `test_old_guidance_memory_remains_loadable()` --uses--> `GuidanceMemory`  [INFERRED]
  backend/tests/test_guidance_interaction.py → backend/src/engine/guidance_contracts.py

## Import Cycles
- None detected.

## Communities (146 total, 27 thin omitted)

### Community 0 - "Security & Production Readiness"
Cohesion: 0.07
Nodes (44): How we build — read the handbook, 11. Privacy, safety and production, AdmitFlow — Development Lifecycle, The loop, Reference docs, Ground rules (non-negotiable), Stages, 1. Understand & spec  (`/spec`) (+36 more)

### Community 1 - "Current product tasks"
Cohesion: 0.08
Nodes (42): The rules you cannot skip, Quick commands (from `backend/`), What this project is, Workflow triggers, Knowledge graph, Working context, AdmitFlow product definition, Customers and problem (+34 more)

### Community 2 - "AdmitFlow Backend"
Cohesion: 0.07
Nodes (33): Getting started, Prerequisites, Option 1: Isolated Docker Compose workspace, Option 2: Native development, Configuration, AdmitFlow Backend, Local development, Telegram admissions runtime (+25 more)

### Community 3 - "AdmitFlow specification"
Cohesion: 0.07
Nodes (30): AdmitFlow specification, 5. Catalogue, intakes and eligibility, 6. Memory, lifecycle and applications, 7. Payments, discounts and review, 9. Staff workspace, 10. Engagement, reporting and integrations, 12. Verification and release acceptance, 2. Architecture and data ownership (+22 more)

### Community 4 - "Reply"
Cohesion: 0.08
Nodes (58): Lead, A prospective applicant with optional phone contact and channel identities.…, decide(), Any, Validate structured model output; refusal/malformed output is never a decision.…, action(), application_action(), collect_application() (+50 more)

### Community 5 - "budget.py"
Cohesion: 0.09
Nodes (34): get_redis(), Redis, RuntimeError, Shared async Redis client for application code. Carry-over prerequisite #1: the…, Raised when Redis is needed but ``REDIS_URL`` is unset in settings., Return the process-wide async Redis client singleton. The client is created…, RedisNotConfiguredError, day_window() (+26 more)

### Community 6 - "test_demo_applications.py"
Cohesion: 0.19
Nodes (22): discount_action(), order_reply(), payment_action(), preview(), Any, AsyncSession, UUID, Code-owned fee previews and explicit, conversation-bound payment requests. (+14 more)

### Community 7 - "test_integrations_notifications.py"
Cohesion: 0.09
Nodes (22): deliver(), UUID, Leased at-least-once webhook delivery with stable recipient deduplication IDs., Durable, minimal institution events and signed outbound integrations., Staff authentication and institution-scoped access., task, Integration queue messages contain identifiers only., send_webhook() (+14 more)

### Community 8 - "RedisMutex"
Cohesion: 0.08
Nodes (25): MutexAcquireError, Redis, RuntimeError, Background loop that extends the TTL while the lock is held., Raised when the mutex cannot be acquired after all retries., Async context-manager mutex backed by Redis ``SET NX``. Parameters: redis: An…, Attempt to acquire the lock, retrying up to ``retry_count`` times., Release the lock using the Lua CAS script (owner-safe). (+17 more)

### Community 9 - "broker.py"
Cohesion: 0.24
Nodes (8): AsyncBroker, _build_broker(), TaskIQ broker singleton. In production (and local dev with Redis running) the…, Construct the right broker based on the current settings. This is a plain…, A real Redis worker must stay listening while its queue is idle., test_idle_queue_listener_receives_a_later_message(), taskiq, taskiq_redis

### Community 10 - "Historical implementation evidence"
Cohesion: 0.07
Nodes (30): Historical implementation evidence, Task 6: WhatsApp message sending service, Task 7: Redis distributed mutex, Phase 3: AI Core (Engine + RAG), Task 8: Engine state definition and lifecycle, Task 9: Multi-provider LLM factory with fallback, Task 10: Input guardrail node, Foundation and institutional implementation (Tasks 1–42) (+22 more)

### Community 11 - "transport.py"
Cohesion: 0.09
Nodes (32): destination(), post_event(), PrivateTransportLogs, protect_logs(), public_ip(), LogRecord, HTTPS delivery pins a public resolved address while retaining TLS host…, resolve_public() (+24 more)

### Community 12 - "WhatsAppSender"
Cohesion: 0.08
Nodes (23): Any, RuntimeError, WhatsApp message sending service (Task 6). Uses httpx2 to call the Meta Graph…, Send an interactive list message. `sections` is a list of section dicts…, Send a pre-approved template message., Raised when the Meta Cloud API returns an error response., Service for sending messages via Meta WhatsApp Cloud API., Post the payload to Meta and extract the created message ID. (+15 more)

### Community 13 - "core/rag.py"
Cohesion: 0.10
Nodes (26): chunk_text(), embed_text(), ingest_document(), Any, AsyncSession, UUID, RAG pipeline core logic (Task 13). Handles ingestion (chunking, embedding),…, Sanitizer to prevent RAG poisoning. Blocks chunks that contain obvious prompt… (+18 more)

### Community 14 - "test_schema_metadata.py"
Cohesion: 0.11
Nodes (13): Any, Record persisted ORM transitions atomically; never infer an earlier stage., record_stage_changes(), AuditLog, One recorded significant action (state change, payment, deletion, takeover).…, capture_event(), Connection, Mapper (+5 more)

### Community 15 - "evaluate_faq.py"
Cohesion: 0.22
Nodes (14): GroundingSource, Channel-neutral input and validated plain-text reply boundary., Internal evidence persisted in encrypted replies, excluded from channel…, Case, cases(), evaluate(), main(), BaseModel (+6 more)

### Community 16 - "Settings"
Cohesion: 0.08
Nodes (25): ASGIApp, Environment-backed application configuration., Settings, Reject oversized multipart streams before they can fill temporary storage., UploadBodyLimit, Tests for the settings singleton., test_defaults_boot_without_env(), test_dotenv_is_explicit_in_tests() (+17 more)

### Community 17 - "admissions.tsx"
Cohesion: 0.10
Nodes (19): Admissions(), Detail, Human, Inquiry, labels, Request, RuleEvidence, statusLabel() (+11 more)

### Community 18 - "retrieval.py"
Cohesion: 0.10
Nodes (24): contextual_query(), numeric_values(), BaseModel, Bounded query resolution for approved evidence; never turn queries into facts., Resolve pronouns/slang using bounded recent turns; uncertainty asks the…, ResolvedQuestion, retrieval_query(), RetrievalQuery (+16 more)

### Community 19 - "GuidanceMemory"
Cohesion: 0.12
Nodes (44): CourseMatch, GuidanceMemory, InterestTheme, Note, ProfileUpdate, BaseModel, Bounded exploratory guidance; evidence notes are not psychometric measurements., StrictModel (+36 more)

### Community 20 - "analytics"
Cohesion: 0.14
Nodes (21): alias, analytics(), bucket_dates(), count(), percent(), Period, period_window(), Any (+13 more)

### Community 21 - "route"
Cohesion: 0.07
Nodes (74): Event, GuidanceInterpretation, recent_history(), route(), model_history(), Any, AsyncSession, Any (+66 more)

### Community 22 - "Adaptive course guidance: evidence and implementation"
Cohesion: 0.12
Nodes (17): How a conversation works, 4. Adaptive student guidance, ADR-013: Adaptive, student-first course guidance, Decision, Consequences, Context, ADR-015: Supportive guidance and contextual answer controls, Decision (+9 more)

### Community 23 - "test_scheduler_independence.py"
Cohesion: 0.33
Nodes (4): Local operational commands with secrets entered through protected prompts., parametrize, A queue outage must not suppress a configured database privacy sweep., test_queue_failure_does_not_skip_retention()

### Community 24 - "Phase 3–4 addition (see ADR-008)"
Cohesion: 0.12
Nodes (16): Outbound integrations and live notifications, Institution document ingestion, Processing and evidence, Verification, PDF setup and worker, Phase 3–4 addition (see ADR-008), Task 40: Typed OpenAI decision layer, Task 42: Advanced RAG Strategy (ADR-011) (+8 more)

### Community 25 - "Code Standards"
Cohesion: 0.12
Nodes (16): ADR-007: Persistent application and worker foundation, Current choices, Historical alternatives, Retained decisions and rationale, Code Standards, Typing — `mypy --strict`, no escapes, Configuration — settings, not literals, Async & I/O (+8 more)

### Community 26 - "ExtractionError"
Cohesion: 0.13
Nodes (27): extract_file(), ExtractionError, ValueError, Bounded extraction of institution reference documents; never execute uploads., A safe, public error code rather than parser output containing document text., validate_path(), convert_pdf(), main() (+19 more)

### Community 27 - "TestClient"
Cohesion: 0.16
Nodes (8): Any, TestClient, Statuses (read/delivered) have no 'messages' array., GET /api/webhook challenge verification., POST /api/webhook payload processing., sign_payload(), TestWebhookProcessing, TestWebhookVerification

### Community 28 - "run_postman_check.cjs"
Cohesion: 0.20
Nodes (8): collection, config, crypto, fs, keys, newman, path, root

### Community 29 - "update"
Cohesion: 0.10
Nodes (21): private_send(), normalize_update(), protect_logs(), Any, LogRecord, TaskIQ changes log levels; redact before either HTTP client or root handlers…, Ephemeral feedback never delays, retries or fails the durable conversation., pulse() (+13 more)

### Community 30 - "sqlalchemy"
Cohesion: 0.12
Nodes (28): Audit-log write helper. :func:`write_audit_log` is the one path significant…, Declarative base, metadata naming convention, and shared column mixins. Every…, Application form submissions and their payment orders., Append-only audit trail (SPEC §12 / security-and-production.md)., Course catalog: courses, batches, qualification rules, discount codes., Encrypted durable non-conversation deliveries and payment-event deduplication., Institution (the tenant root) and its staff users., RAG knowledge base models (pgvector). (+20 more)

### Community 31 - "leads.py"
Cohesion: 0.23
Nodes (18): lead_summary(), Any, course_options(), get_lead(), human_requests(), lead_detail(), leads(), Any (+10 more)

### Community 32 - "crm.py"
Cohesion: 0.12
Nodes (27): Core cross-cutting concerns: settings, security, schemas, guardrails, tracing., name_tokens(), normalized_phone(), UUID, query_tokens(), Tenant-keyed blind name prefixes; plaintext remains in encrypted columns only., token(), words() (+19 more)

### Community 33 - "management.py"
Cohesion: 0.18
Nodes (35): require_admin(), archive_course(), archive_discount(), BatchInput, catalog(), catalog_detail(), course_view(), CourseInput (+27 more)

### Community 34 - "test_migrations.py"
Cohesion: 0.20
Nodes (14): _alembic_config(), monkeypatch_session(), pg_url(), MonkeyPatch, Migration + schema integration tests against real Postgres. Spins up…, Start Postgres, point settings at it, yield the asyncpg URL., The tier requirement: alembic upgrade head then downgrade base, both clean., test_migration_applies_up_and_down() (+6 more)

### Community 35 - "live"
Cohesion: 0.19
Nodes (13): Acknowledge, current(), live(), Any, AsyncSession, BaseModel, Db, get (+5 more)

### Community 36 - "test_structured_documents.py"
Cohesion: 0.19
Nodes (14): blocks(), Markdown sections and table rows stay together within bounded evidence chunks., Keep section ancestry; repeat table headers instead of breaking row…, semantic_chunks(), table_parts(), table_start(), text_parts(), parametrize (+6 more)

### Community 37 - "AdmitFlow"
Cohesion: 0.14
Nodes (14): AdmitFlow, How AdmitFlow uses AI, The problem, Knowledge ingestion and retrieval, Tech stack, The idea, Student commands, Development data and production boundaries (+6 more)

### Community 38 - "alembic"
Cohesion: 0.08
Nodes (13): alembic, columns(), Column, upgrade(), Column, DateTime, _timestamps(), upgrade() (+5 more)

### Community 39 - "main.py"
Cohesion: 0.10
Nodes (17): recovery_loop(), create_app(), ready(), lifespan(), FastAPI, FastAPI application entry point. Boots the API, manages the TaskIQ broker…, Start / stop the TaskIQ broker alongside the FastAPI process. The…, Application factory — builds and configures the FastAPI instance. (+9 more)

### Community 40 - "tests/test_conversation_readiness.py"
Cohesion: 0.22
Nodes (14): Detail, BaseModel, validated_details(), ApplicationTurn, BaseModel, test_multiple_details_and_corrections_fill_only_evidenced_fields(), interpret(), interpret() (+6 more)

### Community 41 - "ADR-012: Telegram and OpenAI for the first complete admissions MVP"
Cohesion: 0.15
Nodes (13): Configuration, ADR-008: Typed OpenAI decision layer, Consequences and verification, Decision, ADR-012: Telegram and OpenAI for the first complete admissions MVP, Decision, Supersession and preserved decisions, Context (+5 more)

### Community 42 - "Phase 5: Dashboard Frontend (Next.js)"
Cohesion: 0.15
Nodes (13): Phase 5: Dashboard Frontend (Next.js), Task 24: Initialize the Next.js staff workspace, Task 25: Authentication (email + password), Task 26: Dashboard home (funnel analytics), Task 27: Leads CRM page, Task 28: Course & batch management, Task 29: Chat takeover + co-pilot UI, Task 29.1: Student answer presentation (+5 more)

### Community 43 - "signin"
Cohesion: 0.24
Nodes (11): test_local_cohort_counts_conversions_and_observed_stages(), test_tenant_is_derived_from_session_and_period_is_bounded(), test_zero_filled_weekly_buckets_and_empty_rates(), Any, Real database management: administrator writes, role/tenant boundaries and…, test_catalog_writes_validate_rules_capacity_and_ownership(), test_discount_management_rejects_invalid_values_duplicates_and_foreign_writes(), test_settings_validate_timezone_hours_and_tenant_scope() (+3 more)

### Community 44 - "StaffUser"
Cohesion: 0.11
Nodes (36): Dashboard access role (SPEC — admin vs agent)., StaffRole, A dashboard user (admin or agent) belonging to one institution., StaffUser, main(), provision(), Manually provision an institution administrator without printing credentials., main() (+28 more)

### Community 45 - "test_knowledge_workspace.py"
Cohesion: 0.10
Nodes (20): alembic_config, Explicit, institution-scoped development demos never authorise real admissions., Application-layer PII protection: symmetric encryption + deterministic hashing.…, Application settings singleton. Loads configuration from the environment (and…, Institution document extraction, durable ingestion and retrieval., Bounded login throttling; shared Redis when configured, development fallback., isolated_local_login_windows(), MonkeyPatch (+12 more)

### Community 46 - "ADR-002: Durable intake, concurrency and duplicate handling"
Cohesion: 0.14
Nodes (12): ADR-002: Durable intake, concurrency and duplicate handling, Historical alternatives and consequences, Decision, ADR-003: Shared database with tenant isolation, Alternatives and consequences, Decision, ADR-005: Input, retrieval and output validation, Alternatives and limits (+4 more)

### Community 47 - "Product and engineering constraints"
Cohesion: 0.20
Nodes (10): Product and engineering constraints, Enforced limits and where they live, Product boundaries, 8. Institution knowledge, ADR-011: Required OpenAI passage reranking, Separate retrieval work, Verification and tradeoffs, Decision (+2 more)

### Community 48 - "normalize_e164"
Cohesion: 0.14
Nodes (10): InvalidPhoneError, normalize_e164(), ValueError, E.164 phone-number normalization. Carry-over prerequisite #3: un-normalized…, Raised when a phone string cannot be normalized to E.164., Normalize *raw* into canonical E.164 (``+<digits>``). Strips whitespace,…, Tests for E.164 phone normalization (carry-over prerequisite #3). The…, normalize_e164 → canonical E.164 string. (+2 more)

### Community 49 - "TelegramBot"
Cohesion: 0.07
Nodes (47): Batch, Course, A program a lead can be qualified for and pay to apply to., A dated intake of a course with finite seats (SPEC §7.5)., Trusted bot-to-institution mapping; token is application-encrypted., TelegramBot, seed(), seed() (+39 more)

### Community 51 - "ADR-009: Application-Layer PII Encryption and Audit Logging"
Cohesion: 0.22
Nodes (9): ADR-009: Application-Layer PII Encryption and Audit Logging, Decision, Status, Alternatives Considered, Rely on the provider's at-rest encryption only, Full envelope encryption (per-tenant data keys wrapped by a master key), Deterministic encryption of the phone number (so it is both stored and searchable), Context (+1 more)

### Community 52 - "check_input_safety"
Cohesion: 0.09
Nodes (33): apply_whatsapp_formatting(), check_input_safety(), check_output_safety(), _deterministic_check(), InputGuardrailResult, LLMGuardrailResponse, LLMOutputGuardrailResponse, OutputGuardrailResult (+25 more)

### Community 53 - "staff/applications.py"
Cohesion: 0.36
Nodes (10): applications(), detail(), owned(), Any, Db, get, post, UUID (+2 more)

### Community 54 - "detect_intent"
Cohesion: 0.20
Nodes (15): detect_intent(), Classify the user's intent to route to the correct node. Design Decision: If a…, asyncio, Test routing for course discovery., Test routing for knowledge questions., Test routing for qualification answers., Test routing for human handoff., Test routing for empty messages without calling LLM defaults to discovery. (+7 more)

### Community 55 - "accept_update"
Cohesion: 0.18
Nodes (12): hash_telegram_identifier(), UUID, Hash an identifier in its tenant/bot/type domain using the lookup secret. This…, accept_update(), Any, Commit the encrypted inbox and offset in the same transaction, before…, identity(), MonkeyPatch (+4 more)

### Community 56 - "PaymentOrder"
Cohesion: 0.16
Nodes (17): PaymentOrder, A Razorpay order for an application fee (SPEC §7.10). Amount is DB-sourced…, OutboundMessage, PaymentEvent, LinkEntity, LinkEnvelope, PaidPayload, PaymentEntity (+9 more)

### Community 57 - "ADR-018: One maintained institutional product"
Cohesion: 0.25
Nodes (8): ADR-001: Python backend and Next.js frontend, Consequences, Decision and rationale, ADR-018: One maintained institutional product, Decision, Supersession, Verification, Context

### Community 58 - "LeadStatus"
Cohesion: 0.06
Nodes (56): LeadStatus, Lead lifecycle states (SPEC §7.3). Where a lead *stands*, not a fixed track —…, _flush_engine_state(), AsyncSession, Engine Dispatcher (Task 8 & 17). This module contains the main entry point for…, Flush mutations back to DB records., present_courses(), AsyncSession (+48 more)

### Community 59 - "Phase 4: Advanced Bot Features"
Cohesion: 0.25
Nodes (8): Phase 4: Advanced Bot Features, Task 18: Application form collection node, Task 19: Razorpay integration, Staff takeover and reviewed co-pilot evidence (Tasks 20 and 29), Task 20: Human handoff + co-pilot mode, Task 21: Cold lead re-engagement worker, Task 22: Telegram delivery limits and queuing, Task 23: Langfuse tracing integration (optional, not selected)

### Community 60 - "identity_database"
Cohesion: 0.43
Nodes (7): identity_database(), Use a disposable pgvector database; never mutate the demo database., test_downgrade_preserves_legacy_rows_and_refuses_to_destroy_telegram_data(), insert_rows(), inspect_and_clean(), remove_test_bot(), verify_legacy()

### Community 61 - "exports.py"
Cohesion: 0.19
Nodes (14): clean(), Audited institution exports, with explicit limits and spreadsheet-safe text., render_csv(), render_xlsx(), crm_data(), Real database CRM lookup, index maintenance, export roles and isolation., test_downloads_roles_datasets_limits_and_tenants(), test_search_scope_validation_and_audit() (+6 more)

### Community 62 - "ADR-006: Code-Owned Qualification and Payments"
Cohesion: 0.29
Nodes (7): ADR-006: Code-Owned Qualification and Payments, Decision, Alternatives Considered, Prompt-Based Qualification, Consequences, Status, Context

### Community 63 - "compilerOptions"
Cohesion: 0.11
Nodes (17): compilerOptions, allowJs, esModuleInterop, incremental, isolatedModules, jsx, lib, module (+9 more)

### Community 64 - "auth.py"
Cohesion: 0.16
Nodes (25): change_password(), issue_session(), Login, logout(), me(), PasswordChange, AsyncSession, BaseModel (+17 more)

### Community 65 - "integration/test_followups.py"
Cohesion: 0.11
Nodes (27): age(), callback(), outreach(), parametrize, Real database consent, reminder leases, freshness, delivery and the two-message…, seed(), test_budget_pause_does_not_spend_a_reminder_and_stop_cancels_it(), exercise() (+19 more)

### Community 66 - "log_delivery_status"
Cohesion: 0.40
Nodes (5): log_delivery_status(), Request, Response, Observe only authenticated status events, without recipient or error text., middleware

### Community 67 - "api.py"
Cohesion: 0.12
Nodes (31): encrypt_bytes(), Encrypt uploaded source bytes before persistence, without lossy text decoding., assign_scope(), documents(), get_upload(), Any, BaseModel, Db (+23 more)

### Community 69 - "pydantic"
Cohesion: 0.07
Nodes (36): Typed decisions through the existing OpenAI-only provider boundary., generate_completion(), Any, Generate a completion with automatic provider fallback. Tries the fast model,…, Input Guardrail Node (Task 10). Detects prompt injection, profanity, and abuse…, BaseModel, Triage Node (Task 11). Routes incoming messages to the appropriate downstream…, Structured output expected from the LLM triage classifier. (+28 more)

### Community 70 - "Language review packet"
Cohesion: 0.40
Nodes (5): Language review packet, Current templates, Live exercise, Reminder consent and outreach copy, Reference clarification

### Community 73 - "guidance.py"
Cohesion: 0.12
Nodes (42): QualificationRule, A structured eligibility rule evaluated in code, not by the LLM (ADR-006).…, academic_number_supported(), catalog(), catalog_status(), commit_guidance(), guide(), interpret_turn() (+34 more)

### Community 76 - "management.tsx"
Cohesion: 0.09
Nodes (16): blank, Discount, Discounts(), Batch, Course, empty, Management(), operators (+8 more)

### Community 78 - "GuidanceDecision"
Cohesion: 0.13
Nodes (29): checked_text(), GuidanceDecision, add_response_controls(), answer_labels(), compose_reply(), consume_answer(), needs_progress(), Any (+21 more)

### Community 79 - "json"
Cohesion: 0.11
Nodes (23): main(), execute(), Programme, PublicCatalogue, AsyncSession, BaseModel, field_validator, model_validator (+15 more)

### Community 84 - "analytics.tsx"
Cohesion: 0.28
Nodes (7): Analytics(), Conversion, labels, frontend_app_analytics_module, rate(), Report, shortDate()

### Community 92 - "download"
Cohesion: 0.22
Nodes (10): download(), export_rows(), ExportRequest, Db, post, Response, text(), old() (+2 more)

### Community 93 - "DocumentChunk"
Cohesion: 0.17
Nodes (22): DocumentChunk, A chunk of a document, with its embedding for vector search., UUID, Reorder only original, safe tenant candidates; never invent text or sources.…, rerank(), evaluate(), main(), chunks() (+14 more)

### Community 94 - "reindex_batch"
Cohesion: 0.40
Nodes (6): main(), AsyncSession, UUID, reindex_batch(), test_index_rename_clear_and_legacy_backfill(), exercise()

### Community 96 - "build_structured_pdf.py"
Cohesion: 0.29
Nodes (5): Rebuild the committed, fictional digital-PDF fixture with reportlab., reportlab_lib, reportlab_lib_pagesizes, reportlab_pdfgen_canvas, reportlab_platypus

### Community 97 - "package.json"
Cohesion: 0.22
Nodes (8): name, private, version, react-dom, @types/node, @types/react, @types/react-dom, typescript

### Community 98 - "erase_inquiry"
Cohesion: 0.27
Nodes (10): erase(), erase_inquiry(), EraseRequest, in_flight(), AsyncSession, BaseModel, Db, delete (+2 more)

### Community 99 - "@playwright/test"
Cohesion: 0.18
Nodes (5): ref_node_child_process, ref_node_crypto, ref_node_fs, ref_node_path, @playwright/test

### Community 100 - "ADR-004: Durable conversations and explicit facts"
Cohesion: 0.67
Nodes (3): ADR-004: Durable conversations and explicit facts, Alternatives and open work, Decision

### Community 101 - "ADR-010: Keep routine uncertainty with the assistant"
Cohesion: 0.67
Nodes (3): ADR-010: Keep routine uncertainty with the assistant, Historical implementation and verification, Decision

### Community 102 - "process_upload"
Cohesion: 0.07
Nodes (35): decrypt_bytes(), Authenticate and decrypt source bytes for an authorized ingestion task., process_upload(), Claim once, process outside the transaction, then publish only the current…, AmbiguousCourseReference, AsyncSession, Course, ValueError (+27 more)

### Community 103 - "next-env.d.ts"
Cohesion: 0.50
Nodes (3): frontend_next_dev_types_root_params_d, frontend_next_dev_types_routes_d, NOTE: This file should not be edited

### Community 104 - "ADR-014: Natural collection, context reset and responsive replies"
Cohesion: 0.67
Nodes (3): ADR-014: Natural collection, context reset and responsive replies, Evidence and limits, Decision

### Community 106 - "layout.tsx"
Cohesion: 0.25
Nodes (5): frontend_app_globals, metadata, config, @fontsource-variable/dm-sans, next

### Community 108 - "scripts"
Cohesion: 0.29
Nodes (7): scripts, build, dev, lint, start, test:e2e:live, typecheck

### Community 110 - "copilot.py"
Cohesion: 0.15
Nodes (20): content(), decide(), DraftContent, DraftDecision, DraftRequest, generate(), latest(), Any (+12 more)

### Community 111 - "test_pdf_parser_boundary.py"
Cohesion: 0.15
Nodes (13): kill_descendants(), parse_pdf(), Include Windows venv redirector children and any parser subprocesses., resident_memory(), watch_memory(), Process, parametrize, Untrusted PDF parsing is bounded, offline and separate from admission workers. (+5 more)

### Community 112 - "enums.py"
Cohesion: 0.11
Nodes (20): ApplicationStatus, DiscountType, Enumerated types used across the schema. These are Python ``str`` enums mapped…, How a discount code reduces the fee., Application review status (SPEC §7.9 / lifecycle)., Code-owned application fees and authenticated Razorpay payment processing., process_payment(), Explicit fee requests, immutable invoices and recoverable payment-link creation. (+12 more)

### Community 113 - "models/__init__.py"
Cohesion: 0.11
Nodes (48): Base, IdMixin, Declarative base shared by every AdmitFlow model., UUID primary key with an application-side default. UUIDs avoid cross-tenant…, Server-side created/updated timestamps., Row-level tenant scoping (ADR-003). Mixed into every tenant-scoped table., TenantMixin, TimestampMixin (+40 more)

### Community 116 - "devDependencies"
Cohesion: 0.33
Nodes (6): devDependencies, @playwright/test, @types/node, @types/react, @types/react-dom, typescript

### Community 117 - "applications.tsx"
Cohesion: 0.25
Nodes (5): Applications(), Detail, fields, statuses, Summary

### Community 118 - "dependencies"
Cohesion: 0.40
Nodes (5): dependencies, @fontsource-variable/dm-sans, next, react, react-dom

### Community 120 - "encrypt_pii"
Cohesion: 0.08
Nodes (39): decrypt_pii(), encrypt_pii(), _fernet(), Encrypt a PII string, returning a url-safe base64 Fernet token. Fernet is…, Decrypt a token produced by :func:`encrypt_pii`. Raises…, test_profile_and_facts_survive_encryption_without_selecting_a_course(), exercise(), plan() (+31 more)

### Community 121 - "outreach/followups.py"
Cohesion: 0.09
Nodes (36): office_open(), datetime, Deterministic clear support signals and institution-local office hours., load_memory(), activity(), after_send(), before_send(), Check (+28 more)

### Community 123 - "hashlib"
Cohesion: 0.07
Nodes (31): Expose only the existing Meta webhook through the temporary demo tunnel., main(), prepare(), Path, Download only local PDF layout/table assets; no OCR, generation or document…, Private Telegram identity lookup without storing raw identifiers in indexes., get, post (+23 more)

### Community 125 - "knowledge/processing.py"
Cohesion: 0.22
Nodes (7): UUID, Durable upload claims, bounded embedding and atomic publication of ready…, ingest_upload(), task, TaskIQ entry point for institution document indexing., Document jobs must not occupy the interactive admissions queue., litellm

### Community 126 - "handle_turn"
Cohesion: 0.13
Nodes (16): handle_turn(), Any, Process a single turn of conversation., ping(), process_inbound_message(), Any, task, Background tasks for message processing. For now this module holds only the… (+8 more)

### Community 128 - "payments.tsx"
Cohesion: 0.32
Nodes (6): label(), money(), outcomes, Payment, Payments(), statuses

### Community 129 - "asyncio"
Cohesion: 0.05
Nodes (37): asyncio, compose(), Verify the isolated local Compose stack without external API calls or sends., seed_credentials(), verify(), Bounded synthetic PDF upload/queue/OpenAI/search check; removes its own upload., Redis distributed mutex with owner-safe release and keepalive (Task 7). Ensures…, Bounded OpenAI-only model roles; provider details never escape in errors. (+29 more)

### Community 131 - "Knowledge"
Cohesion: 0.44
Nodes (9): refresh(), api(), Knowledge(), action(), assignScope(), login(), search(), upload() (+1 more)

### Community 132 - "consent.py"
Cohesion: 0.19
Nodes (16): button(), submitted_reply(), Any, AsyncSession, reset_reply(), cancel_queued(), note_inbound(), preference_reply() (+8 more)

### Community 133 - "env.py"
Cohesion: 0.22
Nodes (9): do_run_migrations(), Connection, Alembic migration environment (async). Reads the database URL from application…, Emit SQL to stdout without a live DB connection., Run migrations against a live database via an async engine., run_migrations_offline(), run_migrations_online(), logging_config (+1 more)

### Community 134 - "timeline.py"
Cohesion: 0.22
Nodes (8): Cursor, BaseModel, datetime, field_validator, Bounded chronological transcript across inbound, replies and standalone…, event_text(), AsyncSession, base64

### Community 136 - "test_staff_operations.py"
Cohesion: 0.15
Nodes (18): inbound(), prepare(), Any, MonkeyPatch, parametrize, UUID, Staff takeover pauses bot replies; explicit sends are scoped, durable and…, test_clear_escalation_records_request_outside_hours_without_a_model() (+10 more)

### Community 137 - "page.tsx"
Cohesion: 0.12
Nodes (13): DeliveryBudget(), Usage, Exports(), Configuration, Integrations(), Request, Doc, errors (+5 more)

### Community 140 - "RuleOperator"
Cohesion: 0.27
Nodes (13): Qualification-rule comparison operators (SPEC §7.4). Evaluated…, RuleOperator, _coerce_values(), evaluate_rule(), Any, Evaluate a qualification rule deterministically. Args: operator: The…, test_evaluate_rule_in_list(), test_evaluate_rule_missing_extracted_value() (+5 more)

### Community 141 - "get_settings"
Cohesion: 0.07
Nodes (54): argparse, async_sessionmaker, AsyncEngine, get_settings(), Return the process-wide :class:`Settings` singleton., DatabaseNotConfiguredError, get_engine(), get_session() (+46 more)

### Community 145 - "workspace"
Cohesion: 0.08
Nodes (31): PaymentLink, BaseModel, field_validator, workspace(), Any, A student must confirm a fresh code-owned fee quote; retries cannot create…, test_fee_changes_require_another_tap_and_legacy_paid_orders_are_not_charged_again(), exercise() (+23 more)

### Community 146 - "telegram/service.py"
Cohesion: 0.09
Nodes (34): is_reset_event(), task, Encrypted outbound-message delivery jobs carry only internal identifiers., send_outbound_message(), process_telegram_event(), task, send_telegram_reply(), RuntimeError (+26 more)

### Community 147 - "notifications.tsx"
Cohesion: 0.33
Nodes (4): labels, Notice, Notifications(), Snapshot

### Community 149 - "operations.py"
Cohesion: 0.30
Nodes (11): active_request(), Any, AsyncSession, Db, post, UUID, Explicit staff takeover and replay-safe manual sends through the durable outbox., resume() (+3 more)

### Community 157 - "staff/payments.py"
Cohesion: 0.14
Nodes (27): demo_fee(), AsyncSession, Course, EncryptionKeyNotConfiguredError, RuntimeError, Raised when a PII helper is used but its key is unset in settings., PaymentStatus, Payment order lifecycle (SPEC §7.10). (+19 more)

### Community 161 - "index_known_name"
Cohesion: 0.40
Nodes (5): index_known_name(), Connection, Mapper, Keep index updates atomic with encrypted names, including name removal., listens_for

### Community 173 - "RazorpayClient"
Cohesion: 0.08
Nodes (34): AsyncBaseTransport, PaidEvent, Credentials, PaymentProviderError, Exception, Bounded Razorpay requests; ambiguous link creation is never blindly retried., Read an existing exact reference; absence never authorizes another creation., Read only: verify the exact paid link and its captured, unrefunded test payment. (+26 more)

### Community 177 - "write_audit_log"
Cohesion: 0.20
Nodes (11): AsyncSession, UUID, Append one entry to ``audit_logs`` on ``session``; return it flushed, not…, write_audit_log(), Any, get, Response, users() (+3 more)

### Community 183 - "test_worker_broker.py"
Cohesion: 0.25
Nodes (7): Tests for the TaskIQ broker setup and task execution (Task 3). All tests run…, When REDIS_URL is unset the factory should yield an InMemoryBroker., The ping task should run and return the expected payload., Ping without an explicit payload should default to 'pong'., test_broker_is_in_memory_without_redis_url(), test_ping_task_default_payload(), test_ping_task_returns_result()

## Knowledge Gaps
- **305 isolated node(s):** `Detail`, `Summary`, `Payment`, `Usage`, `Configuration` (+300 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 1121 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **27 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Work-memory lessons

**Preferred sources** — corroborated by past sessions; start here.
- `answer_question()` (3× useful, score=2.99774848)
- `GroundedQuote` (3× useful, score=2.99774848)
- `WebhookDelivery` (2× useful, score=1.995671698)
- `Admissions()` (2× useful, score=1.995521132)

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `get_settings()` connect `get_settings` to `asyncio`, `Reply`, `budget.py`, `env.py`, `consent.py`, `broker.py`, `WhatsAppSender`, `core/rag.py`, `evaluate_faq.py`, `Settings`, `route`, `staff/payments.py`, `sqlalchemy`, `crm.py`, `test_migrations.py`, `live`, `main.py`, `StaffUser`, `test_knowledge_workspace.py`, `accept_update`, `exports.py`, `auth.py`, `api.py`, `pydantic`, `json`, `download`, `DocumentChunk`, `process_upload`, `test_pdf_parser_boundary.py`, `enums.py`, `encrypt_pii`, `outreach/followups.py`, `hashlib`, `knowledge/processing.py`, `handle_turn`?**
  _High betweenness centrality (0.037) - this node is a cross-community bridge._
- **Why does `encrypt_pii()` connect `encrypt_pii` to `Reply`, `test_demo_applications.py`, `test_staff_operations.py`, `transport.py`, `get_settings`, `workspace`, `telegram/service.py`, `operations.py`, `route`, `staff/payments.py`, `sqlalchemy`, `leads.py`, `test_knowledge_workspace.py`, `TelegramBot`, `accept_update`, `PaymentOrder`, `LeadStatus`, `identity_database`, `exports.py`, `integration/test_followups.py`, `json`, `reindex_batch`, `copilot.py`, `enums.py`, `models/__init__.py`, `outreach/followups.py`, `handle_turn`?**
  _High betweenness centrality (0.031) - this node is a cross-community bridge._
- **Why does `write_audit_log()` connect `write_audit_log` to `Reply`, `consent.py`, `test_demo_applications.py`, `test_integrations_notifications.py`, `transport.py`, `get_settings`, `test_schema_metadata.py`, `telegram/service.py`, `analytics`, `operations.py`, `staff/payments.py`, `sqlalchemy`, `leads.py`, `management.py`, `live`, `StaffUser`, `staff/applications.py`, `PaymentOrder`, `exports.py`, `auth.py`, `api.py`, `guidance.py`, `json`, `download`, `erase_inquiry`, `process_upload`, `copilot.py`, `enums.py`, `models/__init__.py`, `outreach/followups.py`, `knowledge/processing.py`?**
  _High betweenness centrality (0.025) - this node is a cross-community bridge._
- **Are the 23 inferred relationships involving `Reply` (e.g. with `advance_application()` and `request_staff()`) actually correct?**
  _`Reply` has 23 INFERRED edges - model-reasoned connections that need verification._
- **Are the 61 inferred relationships involving `LeadStatus` (e.g. with `record_stage_changes()` and `Lead`) actually correct?**
  _`LeadStatus` has 61 INFERRED edges - model-reasoned connections that need verification._
- **What connects `Detail`, `Summary`, `Payment` to the rest of the system?**
  _305 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Security & Production Readiness` be split into smaller, more focused modules?**
  _Cohesion score 0.06659619450317125 - nodes in this community are weakly interconnected._