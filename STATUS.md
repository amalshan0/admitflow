# Product status

Updated November 1, 2026. This repository is the maintained institutional product.
Product requirements are in [PRODUCT.md](PRODUCT.md) and [SPEC.md](SPEC.md).
This is an implementation/evidence summary, not a claim of production readiness.

## Current foundation

| Capability | Implementation and evidence | Remaining limits |
| --- | --- | --- |
| Student channel | Institution-owned Telegram bot mapping, durable polling checkpoints, encrypted inbox/outbox, signed callbacks, ordered replies and uncertain-send handling | Latest full application/payment flow still needs handset acceptance; legacy WhatsApp is disabled |
| Adaptive guidance | English OpenAI interpretation, literal evidence notes, contextual questions/options, up to three evidence-backed matches, corrections and optional deeper exploration | Research informs design; real-student usefulness and outcomes are unmeasured |
| Eligibility and selection | Code-owned configured rules, explicit course/intake confirmation, missing/failed/review outcomes and stale-control rejection | Public reference courses have no verified admission rules, seats or intakes |
| Knowledge | Authenticated file/folder upload, bounded offline digital-PDF/DOCX/text extraction, recoverable serial ingestion, tenant/course-scoped pgvector retrieval, OpenAI reranking and quotation/abstention checks | OCR is excluded; evaluated cases do not prove universal extraction or language quality |
| Applications and review | Validated collection, corrections, review/submit controls, encrypted form storage, staff approve/waitlist/reject actions | Real institution policies and full operational acceptance remain prerequisites |
| Payments and discounts | Database-owned amounts, code-validated discounts, Razorpay links, signed webhook matching, durable confirmation/replay controls and staff monitoring | Real test-card/latest-flow handset acceptance and live-mode operations remain open |
| Staff assistance | Request queue, acknowledge/resolve/explicit resume, takeover and reviewed co-pilot suggestions | No response-time promise without an institution's staffing configuration |
| Institutional workspace | Staff accounts, role/session controls, CRM/search, catalogue/intake/rule/document management and institution settings | Multi-tenant customer onboarding is manual; self-service onboarding is future work |
| Engagement and reporting | Consent-aware reminders, office hours, delivery budgets, analytics, CSV/Excel exports, signed outbound webhooks and notifications | No measured conversion gains or commercial adoption |
| Privacy and operations | Sensitive-field encryption, access/action audit, inquiry erasure, conversation retention and isolated full-stack local packaging | Production privacy/age policy, recovery, operational monitoring and pilot acceptance remain release gates |

Implementation files and dated checks are indexed in [the verification record](docs/verification/README.md)
and [implementation history](docs/verification/IMPLEMENTATION_HISTORY.md). Old test counts
belong to their tested source, not automatically to the latest checkout.

## Development data is separate from the product

The recorded local workspace has 64 public JAIN Kochi programme references and 65 sourced
reference documents. The snapshot is for independent information and development;
it does not establish institution representation, verified prices, intakes or authority
to admit. Current source also retains an explicitly enabled, development-only sample
application/test-payment path (ADR-017), with persistent markers. The recorded sample
fee was INR 3,000 and is not a product price or university fee.

This consolidation changes documentation and technical report locations. It does not
reset local data, change ignored configuration, activate providers, grant real admission
or make sample records production offerings. Use [development modes](docs/engineering/DEVELOPMENT_MODES.md).

## Remaining product work

1. Establish an authorised pilot institution's catalogue, intakes, academic rules, fees,
   review policy, staff responsibilities and approved reference material.
2. Validate the actual end-to-end application/payment/confirmation/staff-review journey
   using sandbox payments before enabling live payment operations.
3. Complete production privacy, minors/guardian, retention, deletion and provider-processing
   policies; decide AI-processing controls separately from reminder consent.
4. Specify and verify structured student/parent attribution and course-comparison metadata.
5. Evaluate supported languages with fluent speakers and realistic course-guidance cases.
6. Choose hosting/budget and prove backups/restores, monitoring, incident handling, secrets,
   release/rollback and institutional operating procedures before a production pilot.
7. Validate buyer demand, pricing and usefulness; plan self-service onboarding from evidence.

Work is ordered in [tasks/plan.md](tasks/plan.md). Open choices and inherited decisions
are recorded in [context reconciliation](docs/planning/CONTEXT_RECONCILIATION.md).

## Current consolidation verification

Task 71 is verified and published through [PR #25](https://github.com/Shan091/admitflow/pull/25).
GitHub records its final CI and merge state. Backend lint, formatting and
strict typing passed with 323 passing default tests (131 deselected, two existing warnings).
Frontend lint/production build and 227 internal documentation links passed. See the
[Task 71 record](docs/verification/CONTEXT_CONSOLIDATION_RESULTS.json) and
[tasks/todo.md](tasks/todo.md). No running service or provider has been revalidated
merely by updating these documents.

Repository synchronization was reverified November 1, 2026: the backend gate again
passed 323 tests (131 deselected, two existing warnings), frontend lint/production build
passed, and 220 relative Markdown file links resolved. All 18 relocated historical
JSON records remain byte-identical. Task 71 implementation is complete; PR #25 records
CI and merge status. The next product milestone is Task 72.
