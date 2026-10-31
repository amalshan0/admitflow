# Institutional product implementation plan

[PRODUCT.md](../PRODUCT.md) defines purpose; [SPEC.md](../SPEC.md) defines behaviour;
[STATUS.md](../STATUS.md) distinguishes implementation from acceptance. This is the active
roadmap. Historical task IDs and dated technical results are retained in
[implementation history](../docs/verification/IMPLEMENTATION_HISTORY.md).

## Delivery checkpoint: Task 71

Task 71 implements the consolidation of the original product intent and useful later
design decisions into this repository. Temporary delivery materials were removed,
active documents reconciled and technical evidence/tools relocated. Unrelated edits,
runtime data, financial markers, migration history and existing safeguards were preserved.
Acceptance and local verification are in [todo.md](todo.md);
[PR #25](https://github.com/Shan091/admitflow/pull/25) records CI and merge status.
The next product milestone is Task 72, authorised institution pilot configuration.

## Existing foundation

The Python/Next.js platform includes Telegram delivery, adaptive English guidance,
persistent memory, deterministic eligibility, document ingestion/retrieval/reranking,
applications/review, payment integration, co-pilot, institutional controls, consent-aware
follow-ups, reporting/integrations and privacy controls. Local automated evidence does
not establish production readiness or full real-user/provider acceptance.

## Next milestones

| Order | Work | Exit evidence |
| --- | --- | --- |
| 1 | One maintained product context (Task 71) | Canonical docs, local decision coverage, working links/tools, reviewed checks and GitHub publication |
| 2 | Authorised institutional pilot configuration (Task 72) | Owner-approved institution, offerings/intakes/rules/fees, knowledge rights, review policy and operating responsibilities |
| 3 | Remaining application/payment acceptance (Task 73) | Latest-source sandbox card/link/webhook/Telegram/staff-review evidence; errors/replays covered; no live enablement implied |
| 4 | Product/privacy choices (Task 74) | Explicit AI-processing controls, student/parent attribution, minors/guardian, retention and provider-processing policy; implemented gaps tracked |
| 5 | Course-comparison and language quality (Task 75) | Institution-owned metadata and relevant edge cases; fluent-speaker/realistic guidance review; no inferred aptitude or outcomes |
| 6 | Production operation (Task 76) | Selected budget/host, secrets/TLS, backup/restore, monitoring/alerts, incident ownership and release/rollback rehearsal |
| 7 | Institutional adoption (Task 77) | Pilot usefulness/demand/pricing evidence and a justified self-service onboarding plan |

Existing implementation is extended rather than replaced. Do not introduce parallel
roadmaps, copy another implementation, or activate old provider/vendor choices.

## Dependencies and operating decisions

Real admissions depend on authorised institutional records, not public programme listings.
Production/live payment enablement depends on acceptance and operating readiness. The
legacy sample-application exception remains development-only and must be retired or
disabled for a pilot while preserving existing marked records. Structured parent attribution
and dedicated course-comparison attributes are requirements to specify, not delivered claims.

Telegram/OpenAI and the existing data/worker/frontend foundation remain selected. English
is active; Hindi/Hinglish/Malayalam need verified complete experience. Provisioning is
manual; hosting and operating budget remain unselected. No public deployment until the
owner instructs it. Keep paused/staff-owned conversations, opt-outs and provider uncertainty
intact. Use synthetic data for bounded verification.
