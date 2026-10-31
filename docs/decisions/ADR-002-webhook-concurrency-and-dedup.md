# ADR-002: Durable intake, concurrency and duplicate handling

Status: Accepted principle; Telegram runtime supersedes the original WhatsApp-only
implementation assumptions under ADR-012.

## Decision

Accept external work durably before acknowledging it, deduplicate scoped provider
identities, and serialize each tenant-owned conversation. Keep slow AI/provider work
out of external request handlers. Redis/TaskIQ delivers jobs; database state supports
recovery when job publication fails.

Telegram polling persists accepted events with checkpoints and holds a receiver lock.
Workers serialize on the tenant-owned lead and commit state/replies atomically. The
outbox preserves send order and records uncertain delivery rather than blindly resending.
Callbacks bind actor/chat/tenant and current selection. Webhooks need authenticated
HTTPS and equivalent durable acceptance.

## Historical alternatives and consequences

The original Meta route used Redis SETNX message dedup and a per-phone mutex. Those
helpers remain compatibility code; they do not replace active bot/update/chat scoping,
durable inbox recovery or database ownership checks. Synchronous model work in a webhook
was rejected because provider deadlines and retries can otherwise duplicate/loss work.
No external exactly-once delivery claim is made. Test replays, concurrent updates,
publication failures, lock ownership, crash recovery and uncertain sends.
