# Product and engineering constraints

This is the durable constraint ledger. [PRODUCT.md](PRODUCT.md) defines purpose;
[SPEC.md](SPEC.md) defines behaviour; [quality gates](docs/engineering/quality-gates.md)
define verification. Configuration remains authoritative for tunable limits.

## Product boundaries

- Student fit before enrollment pressure; no invented offerings, outcomes or availability.
- Natural, adaptive conversation; avoid repeated questions and preserve unrelated facts.
- Interest evidence is separate from academic eligibility; no psychometric score or diagnosis.
- Code owns tenant authority, eligibility, selection, fees/discounts and consequential state.
- Institutional records require explicit review/confirmation; payment is not admission.
- Public references cannot become authorised admissions merely by changing labels.
- Record decisions persistently here or in an ADR; no undocumented cross-repository dependency.
- English guidance, Telegram and OpenAI are current choices; other languages need verification.
- Preserve unrelated edits, existing data, credentials and migration history.
- Do not deploy publicly without the owner's instruction.

## Enforced limits and where they live

| Area | Current boundary | Authority |
| --- | --- | --- |
| Knowledge upload | Default 10 MiB/file, 25 files and 30 MiB/request, 100 PDF pages, 400,000 extracted characters, 180-second indexing attempt | `backend/src/core/settings.py`, `backend/src/knowledge/limits.py` and [ingestion guide](docs/engineering/DOCUMENT_INGESTION.md) |
| Guidance output | At most three course matches, four optional answer choices and two external research fields | `backend/src/engine/guidance_contracts.py` |
| Reranking | At most 12 eligible candidates; preserve the configured similarity floor and validate the full permutation | [ADR-011](docs/decisions/ADR-011-advanced-rag-strategy.md) and retrieval/reranking code |
| Model calls | Configured deadlines/retries; completion output bounded to 200–2,000 tokens | `backend/src/engine/llm_factory.py` and settings |
| Payments | Integer minor units, stored currency/reference matching, authenticated confirmation and replay protection | `backend/src/payments/` |
| Verification | Backend lint, format, strict typing and default tests before a PR; additional checks by changed surface | `backend/scripts/verify.sh` and quality gates |

No universal coverage percentage, student-outcome target, production latency SLA or
approved exception waiver has been set. Record a threshold's owner, reason, enforcement
and review/expiry date when one is adopted; do not present old measurements as promises.
