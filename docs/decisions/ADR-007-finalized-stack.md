# ADR-007: Persistent application and worker foundation

Status: Partially superseded by ADR-008/011/012/018. Current decisions below replace
historical provider, hosting, identity-library, notification and visual-style mandates.

## Retained decisions and rationale

- Python/FastAPI and Next.js separate domain authority from the staff interface.
- PostgreSQL/pgvector holds queryable institution records and knowledge; SQLAlchemy/Alembic
  provide persistence/migrations. Redis/TaskIQ provides work delivery and coordination.
- A hand-rolled async dispatcher reloads state per turn. Typed decisions already route
  work; adding another graph/checkpoint store would duplicate queryable conversation state.
- Configured model roles and typed extraction keep provider details out of business rules.
- Always-available processing, persistent data, audit and recovery are operating needs;
  no hosting account is established by naming a vendor.

## Current choices

Telegram/OpenAI is active. No cross-provider fallback, self-hosted embedding/decision
model or training is required. Docling assets run offline for digital-document extraction.
Staff use password hashing and opaque server sessions implemented in Python; notifications
use authenticated WebSockets. Plain CSS and UI_THEME.md define the workspace presentation.
Sanitised model/usage/latency logs are required; external telemetry services are optional.

## Historical alternatives

Fly API/workers, Vercel UI, Neon PostgreSQL, Redis on Fly, R2 document storage, Resend email,
fastapi-users and SSE were considered. Earlier alternatives included Railway/Render/Koyeb,
Supabase and Upstash. They are not active deployment commitments or instructions to provision
services. Hosting/budget are selected during production planning; originals were never
proof that these services were deployed.

Gemini-first/OpenAI/Anthropic fallback, bge-m3 and System-1/Laya were replaced by the
OpenAI-only configured boundary. Preserve graceful outage and bounded retries without
restoring old services. See the active specification, status and production runbook.
