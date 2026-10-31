# ADR-018: One maintained institutional product

Status: Accepted by the owner, October 30, 2026. Implementation/publication is Task 71.

## Context

The original institutional product acquired separate implementations with narrower goals.
Useful product intent, conversational edge cases and verification lessons became scattered.
The owner now directs all maintained product context into this repository and removes
temporary delivery materials. Keeping old instructions active would reintroduce conflicting
provider choices, simplified architectures and unverified feature claims.

## Decision

- Maintain [PRODUCT.md](../../PRODUCT.md), [SPEC.md](../../SPEC.md), [STATUS.md](../../STATUS.md),
  the plan/tasks, engineering guides and ADRs as one coherent product record.
- Preserve substantive product decisions locally with provenance and explicit dispositions
  in [context reconciliation](../planning/CONTEXT_RECONCILIATION.md). Do not import a second
  implementation, parallel roadmap or raw external instructions.
- Remove temporary presentation, submission, schedule and recording materials. Preserve
  useful technical checks under `docs/verification` and reusable tools under engineering.
  Prior versions remain recoverable in Git history.
- Retain current Telegram/OpenAI/data/worker/frontend decisions. Historical WhatsApp,
  model-training, hosting-vendor and visual-style choices do not automatically return.
- Distinguish product requirements, implementation, dated verification and open acceptance.
  Do not call implemented features future work or use an old test result as current proof.
- Keep development data/test modes explicitly isolated. No reference listing becomes an
  authorised admissions offering, and no sample fee becomes a production fee.
- Preserve database records, financial markers, migrations, runtime secrets and unrelated edits.
  Production deployment remains separately authorised.

## Supersession

This ADR replaces temporary scope and handoff instructions. ADR-012 remains authoritative
for the active channel/provider; ADR-013–015 for adaptive guidance; ADR-016–017 describe
reference-data/development boundaries rather than the commercial product's definition.
Earlier ADRs remain useful only where their decisions are not superseded.

## Verification

Task 71 checks source-context coverage, internal links, removed-material references,
technical report paths, the required code gate and the publication workflow. Removal
of outdated materials is not evidence of production deployment or new feature delivery.
