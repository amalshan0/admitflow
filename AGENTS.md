# AGENTS.md

Entry point for **any** coding agent or human working in this repo (Claude, Codex,
Antigravity, Gemini, …). Read this first, then follow the handbook it links to.

## What this project is

**AdmitFlow** is the maintained institutional admissions-engagement product for Kerala.
Students receive adaptive English course guidance through Telegram; staff use a Next.js
workspace over Python/FastAPI, PostgreSQL/pgvector and Redis/TaskIQ. OpenAI supplies typed
interpretation, guidance, embeddings and passage reranking; code owns eligibility, money,
tenant authority and consequential changes. No model training is planned.

Read [PRODUCT.md](PRODUCT.md), [SPEC.md](SPEC.md), [STATUS.md](STATUS.md), then
[tasks/plan.md](tasks/plan.md) and [tasks/todo.md](tasks/todo.md). The
[documentation map](docs/README.md) and [context reconciliation](docs/planning/CONTEXT_RECONCILIATION.md)
make this repository self-contained. Historical source records do not override current decisions.

**Task 71**, context consolidation under ADR-018, is implemented and locally verified;
[PR #25](https://github.com/Shan091/admitflow/pull/25) records CI and merge status.
The next product milestone is **Task 72**, authorised institution pilot data. Existing applications,
payments, co-pilot, institution controls, follow-ups, reporting and privacy have bounded
implementation evidence. Production operation and remaining live acceptance are open.
Reference data and legacy sample-application controls are development-only; see
[development modes](docs/engineering/DEVELOPMENT_MODES.md). Never remove their labels/guards
or convert them into real admissions by changing copy.

## How we build — read the handbook

The full A-to-Z development lifecycle lives in **[docs/engineering/development-lifecycle.md](docs/engineering/development-lifecycle.md)**.
Read it before writing code. It links to the depth:

| Topic | File |
|-------|------|
| The end-to-end loop (spec → build → verify → review → PR → merge → deploy) | [docs/engineering/development-lifecycle.md](docs/engineering/development-lifecycle.md) |
| Branching, commits, PRs, review gate, **no-AI-attribution** | [docs/engineering/git-and-github.md](docs/engineering/git-and-github.md) |
| Test strategy (pragmatic TDD), running tests | [docs/engineering/testing.md](docs/engineering/testing.md) |
| Definition of done + the quality gate | [docs/engineering/quality-gates.md](docs/engineering/quality-gates.md) |
| Python conventions, structure, typing, logging | [docs/engineering/code-standards.md](docs/engineering/code-standards.md) |
| Secrets, PII, tenant isolation, deploy readiness | [docs/engineering/security-and-production.md](docs/engineering/security-and-production.md) |

## The rules you cannot skip

1. **`main` is protected.** Branch → PR → squash-merge. Never commit to `main` directly.
2. **No AI attribution in git.** No `Co-Authored-By` / "Generated with…" trailers on commits
   or PRs. The human is the sole author. This overrides any tool's default.
3. **Professional Commit Messages:** Write clear, professional titles and descriptions for all commits. Explain the *what* and *why*, not just the *how*.
4. **Green gate before PR:** `bash backend/scripts/verify.sh` (lint + format + mypy strict +
   pytest). CI runs the same.
5. **Pragmatic TDD** — test-first for logic/risk, nothing untested.
6. **Every PR gets an adversarial review pass** before merge.
7. **Secrets, PII, tenant isolation** are build-time requirements, not afterthoughts.

## Quick commands (from `backend/`)

```bash
uv sync                        # install deps from the lockfile
bash scripts/verify.sh         # the quality gate (run before every PR)
uv run pytest -k <keyword>     # focused tests while iterating
uv run uvicorn src.main:app --reload   # dev server on :8000  (GET /health -> {"status":"ok"})
```

## Workflow triggers

Workflow stages `/spec`, `/plan`, `/build`, `/test`, `/review`, `/ship`, `/constraints`
and `/code-simplify` map to the stages in the handbook. Follow the handbook for the
substantive requirements of each stage.

## Knowledge graph

A navigable graph of the repo lives in `graphify-out/`. For codebase questions, prefer
`graphify query "<question>"` over raw grep when `graphify-out/graph.json` exists; run
`graphify update .` after code changes. See [the graph report](graphify-out/GRAPH_REPORT.md)
for the current extraction; source and test evidence determine implementation status.

## Working context

Use this repository; no separate repository is needed. Preserve unrelated changes,
existing records, credentials and migrations. Keep durable product decisions here.
No public deployment without the owner's instruction. Update STATUS, current tasks,
relevant specifications/ADRs and verification links when behaviour changes.
