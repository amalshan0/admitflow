# ADR-003: Shared database with tenant isolation

Status: Accepted; channel identity updated by ADR-012.

## Decision

Use shared PostgreSQL tables with tenant columns on tenant-scoped records, scoped
constraints and explicit tenant filtering in every relevant query. Institution-owned
Telegram bot configuration resolves student authority; authenticated server sessions
resolve staff authority. A client-supplied tenant ID or username is not sufficient.

Prevent cross-tenant references in identity, course/batch/rule, document, lead,
application/payment and integration paths. Review with negative tests across two tenants.
A missing tenant constraint/filter is a correctness and data-isolation failure.

## Alternatives and consequences

Schema-per-tenant offers a different isolation boundary but complicates migrations at
scale. Database-per-tenant adds provisioning/cost/operations. Neither is required by the
current product. Application/query discipline remains mandatory with shared tables.
The original Meta phone-number mapping is disabled compatibility behaviour, not the
active source of Telegram or staff authority.
