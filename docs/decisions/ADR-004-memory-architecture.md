# ADR-004: Durable conversations and explicit facts

Status: Accepted persistence principle; current behaviour is specified in SPEC sections
4 and 6 and ADR-013–015.

## Decision

Persist conversation turns and outcomes in queryable institution-scoped records; store
validated facts and literal exploratory evidence separately. Reload bounded recent
chronological context and relevant facts per turn so a restart does not lose progress.
Staff inspect the same saved evidence. Encrypt sensitive content and keep corrections
explicit; updating age must not erase an unrelated education or interest statement.

Institution knowledge is a third layer: scoped reference passages/embeddings, separate
from applicant facts. Long-term storage does not mean sending the full transcript to a
model. Reset, retention and erasure have different scopes.

## Alternatives and open work

A graph checkpointer would duplicate the state needed by the workspace. External memory
providers would add an extra dependency/data transfer; none is selected. Rolling summaries
may be useful later but their threshold and correction-preservation acceptance are unset.
They are not a delivered substitute for literal evidence. Storage under our control does
not by itself establish complete privacy compliance or provider-side deletion.
