# Documentation map

Read [PRODUCT.md](../PRODUCT.md), [SPEC.md](../SPEC.md), [STATUS.md](../STATUS.md), then
[the plan](../tasks/plan.md) and [current tasks](../tasks/todo.md). This repository contains
the maintained product context; other repositories are not prerequisites.

| Question | Authoritative document |
| --- | --- |
| What are we building and for whom? | [Product definition](../PRODUCT.md) |
| How should it behave? | [Specification](../SPEC.md) |
| What exists, what is verified, and what remains? | [Status](../STATUS.md), [verification index](verification/README.md) |
| What is next? | [Plan](../tasks/plan.md), [task list](../tasks/todo.md) |
| Which decisions supersede earlier assumptions? | [Architecture decisions](decisions/), especially ADR-018 |
| What context was consolidated? | [Decision reconciliation](planning/CONTEXT_RECONCILIATION.md) and its source manifest |
| How do we work? | [Development lifecycle](engineering/development-lifecycle.md), [constraints](../CONSTRAINTS.md) |
| How do I run the product locally? | [Native runtime](engineering/LOCAL_RUNTIME.md), [isolated Compose](engineering/COMPOSE_RUNTIME.md) |
| Which data/modes are development-only? | [Development modes](engineering/DEVELOPMENT_MODES.md) |
| Where are source and model terms recorded? | [Data/model provenance](DATA_PROVENANCE.md) |

Product intent is not implementation evidence. Source/tests determine implemented behaviour;
dated verification records describe a particular source/configuration. Historical records
and partially superseded ADRs do not override the current specification or newer decisions.
Update the relevant entries when behaviour changes instead of creating another parallel roadmap.
