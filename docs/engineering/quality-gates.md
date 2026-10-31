# Quality Gates — Definition of Done

A change is **done** when it passes the gate for the tier it touches. The gate is tiered so a
docs typo isn't held to the same bar as the payment webhook — but the baseline is
mandatory for everything.

## The baseline gate (every change)

Run from `backend/`:

```bash
bash scripts/verify.sh
```

Which runs, and must all pass:

```bash
uv run ruff check .            # lint
uv run ruff format --check .   # formatting
uv run mypy                    # type check, --strict, over src/
uv run pytest                  # tests
```

Current command results belong in [tasks/todo.md](../../tasks/todo.md).
Dated historical results remain in [verification](../verification/README.md); do not use
an old example test count as evidence for the current checkout.

Baseline also requires:
- New behavior has tests (see [testing.md](testing.md)).
- No secrets, PII, or `.env` in the diff or logs.
- The task checkbox in [../../tasks/todo.md](../../tasks/todo.md) is updated.
- An adversarial review pass is clean (see [git-and-github.md](git-and-github.md)).

## The tiers — add gates by what the change touches

| If the change touches… | Add these gates on top of baseline |
|---|---|
| **DB schema, engine dispatcher, RAG retrieval, mutex/dedup** | Integration tests against real Postgres (pgvector) + Redis; Alembic migration applies cleanly up **and** down. |
| **Any LLM call** | Shared-provider redacted model/usage/latency telemetry present (ADR-012); extraction uses a typed structured-output schema (no free-text parsing); model IDs read from settings, not hard-coded. |
| **Payments (Razorpay)** | Replay protection tested (webhook clears the pending order / `payment_link_sent` flag); amounts come from the DB, never the LLM; HMAC verified. |
| **Any external surface (webhook, HTTP endpoint)** | Signature/HMAC verified; input validated with Pydantic; authn/authz checked; unauthenticated exposure is called out explicitly. |
| **Anything handling PII** | Application-level encryption on the field; access is audit-logged; tenant scoping (`tenant_id`) enforced. See [security-and-production.md](security-and-production.md). |

If you're unsure which tier applies, it's the higher one.

## CI mirrors this exactly

[../../.github/workflows/ci.yml](../../.github/workflows/ci.yml) runs the identical baseline
steps on every push to `main` and every PR. A local pass does not guarantee the same result on another platform. A PR merges only
when CI is green **and** the review is clean. CI is the enforcement; `verify.sh` is the fast
local copy so you never wait on CI to learn you forgot to format.

Inspect the required GitHub checks on the current PR head; do not infer their result from a local pass.

## Fast iteration vs the gate

While coding, run the narrow thing (`uv run pytest -k …`, `uv run ruff check src/foo.py`).
Before you open the PR, run the **whole** `verify.sh` once — CI runs everything, so partial
local runs are how green branches still fail CI.

## The enforced-numbers ledger

[../../CONSTRAINTS.md](../../CONSTRAINTS.md) is the place to record hard, enforced thresholds
(coverage floors, latency budgets, exception waivers with owners + expiry) as they're set.
This document describes the *process*; CONSTRAINTS.md holds the *numbers*.
