# Current product tasks

Follow [plan.md](plan.md), [PRODUCT.md](../PRODUCT.md), [SPEC.md](../SPEC.md) and
[STATUS.md](../STATUS.md). Task IDs 1–70 and their useful technical evidence are preserved
in [implementation history](../docs/verification/IMPLEMENTATION_HISTORY.md) and the
[verification index](../docs/verification/README.md). They are not reset or renumbered.

## Task 71: Consolidate the institutional product context

**Status:** Implementation and local verification complete. Owner-approved October 30,
2026. [PR #25](https://github.com/Shan091/admitflow/pull/25) records publication, CI and
merge status. The next product milestone is Task 72.

- [x] Define product purpose, current scope, document authority and remaining decisions.
- [x] Distil useful original/later decisions and acceptance cases with pinned provenance.
- [x] Remove temporary delivery assets and move technical evidence/tools to maintained paths.
- [x] Keep test data/sample admissions isolated; preserve runtime data and unrelated changes.
- [x] Reconcile active documentation, internal links and technical output paths.
- [x] Run the backend gate and appropriate frontend/documentation checks.
- [x] Complete an adversarial review and publish through PR #25; green final-head CI is required before squash merge.

**Scope:** Product context and repository maintenance, not new admissions functionality,
provider activation, database reset or production deployment. Preserve the pre-existing
`frontend/next-env.d.ts` edit. Existing development-only record markers/safeguards remain.

**Fresh verification:** Backend lint/format/strict typing passed; 323 tests passed,
131 deselected, two existing AsyncMock warnings. Frontend lint/production build passed;
227 internal documentation links passed. See the [Task 71 record](../docs/verification/CONTEXT_CONSOLIDATION_RESULTS.json).
Earlier integration/provider/handset results remain historical evidence.

**November 1 synchronization verification:** The full backend gate passes again
(323 tests passed, 131 deselected, two existing warnings; 131 strictly typed source files).
Frontend lint/production build pass, 220 relative Markdown file links resolve, and all
18 relocated historical JSON records remain byte-identical. Adversarial self-review
confirmed the changes preserve runtime behaviour, migrations and development safeguards;
no critical or important findings remain. PR #25 must pass checks on its updated head
before squash merge.

## Task 72: Establish authorised institution pilot data

- [ ] Confirm institution authority, maintained course/intake/rule/fee records and knowledge rights.
- [ ] Define staff ownership, review/capacity/refund policies and operating responsibilities.
- [ ] Separate public references and synthetic/sample records from live institutional offerings.

## Task 73: Finish application and payment acceptance

- [ ] Exercise latest-source sandbox card/link/authenticated confirmation/Telegram receipt/review.
- [ ] Record failures, replays and uncertain delivery without conflating simulated or older payments.
- [ ] Define live-mode acceptance and reconciliation procedures before enabling real payments.

## Task 74: Settle product and privacy controls

- [ ] Decide AI-processing disclosure/opt-out separately from follow-up consent.
- [ ] Specify structured parent/student attribution and correction/authority boundaries.
- [ ] Set minors/guardian, retention/deletion and provider-processing policies for the pilot.
- [ ] Track and test required implementation changes; do not mark policy complete from documentation.

## Task 75: Verify comparison and language quality

- [ ] Specify learning goals, teaching medium, discussion support and delivery-format metadata.
- [ ] Verify only recorded facts influence comparison; unknowns and no-match paths stay honest.
- [ ] Evaluate realistic student conversations and full Hindi/Hinglish/Malayalam experience with fluent reviewers.

## Task 76: Prepare production operation

- [ ] Select budget/host and establish secrets/TLS/data isolation/monitoring/incident ownership.
- [ ] Validate backups/restores, migrations, recovery and release/rollback procedures.
- [ ] Disable legacy sample admission mode for pilot operation without erasing financial records.
- [ ] Deploy only after the owner's instruction and record actual health/handset acceptance.

## Task 77: Validate adoption and onboarding

- [ ] Measure pilot usefulness, repeated work, buyer demand and willingness to pay.
- [ ] Choose pricing and self-service onboarding scope from evidence.
