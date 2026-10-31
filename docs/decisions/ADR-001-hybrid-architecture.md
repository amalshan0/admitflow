# ADR-001: Python backend and Next.js frontend

Status: Accepted for the backend/frontend split; current boundaries are clarified by
ADR-012 and [ADR-018](ADR-018-product-context-consolidation.md).

## Decision and rationale

FastAPI/Python owns business rules, persistence, AI boundaries and background work.
Next.js/TypeScript owns the staff interface and uses authenticated Python APIs.
This keeps complex admissions/payment/tenant decisions in one domain implementation
while supporting an interactive accessible workspace.

The conversation engine is a plain async dispatcher with persisted per-turn context.
LangGraph/checkpointer proposals were replaced because routing already has a typed
boundary and the dashboard needs queryable conversation records; duplicating state
in another checkpoint store would add operational/debugging complexity.

The original alternatives were a full TypeScript application and server-rendered Python.
Neither is a current migration plan. Original WhatsApp timing, Tailwind and dark/glass
styling assumptions do not govern the active product. Telegram/OpenAI, plain CSS and
current workspace tokens are recorded in the active specification and engineering guides.

## Consequences

Python authorisation/validation remains authoritative even when a frontend action is
hidden. Preserve tenant identity through the API/session boundary. Run backend and
frontend gates for their changed surfaces; a frontend proxy is not its own business API.
