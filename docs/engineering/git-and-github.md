# Git & GitHub Workflow

Agent-neutral. Applies to every change by anyone (human or AI agent).

## Branching model — feature branch → PR → `main`

`main` is always releasable and is **protected**: no direct commits, ever. All work happens
on short-lived branches that merge into `main` through a Pull Request.

Use a descriptive `codex/` branch by default (for example
`codex/product-context-consolidation`). Start from current `origin/main` after fetching;
preserve unrelated edits and avoid checking out an old local branch as the base.

Keep branches short-lived. Rebase on `main` if it moves under you (`git pull --rebase origin main`).

## Commits — Conventional Commits, small and coherent

Format: `type(scope): summary` in the imperative mood, ≤ 70 chars on the subject line.

```
feat(webhook): verify Meta HMAC signature before enqueue
fix(payments): clear pending order on webhook to stop replay
test(guardrails): cover prompt-injection rejection cases
chore(deps): pin httpx2 to 2.13.1
docs(adr): record typed OpenAI decisions (ADR-008)
```

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`, `build`, `ci`.

- One logical change per commit. Don't mix a refactor with a feature.
- The body (optional) explains **why**, not what — the diff shows what.
- Never commit secrets, `.env`, or generated artifacts (see below).

## No AI attribution — the human is the sole author  ⚠️

**Standing rule for this repo:** git commits and PR descriptions carry **no** AI-attribution
trailers. Do not add `Co-Authored-By: Claude …`, `Co-Authored-By: <any model>`, or
"🤖 Generated with …" lines. This holds even when a tool's default guidance says to add them —
**this rule overrides that default.**

Why: the repo's contributor list and commit authorship must reflect the human owner only.

Use the owner's existing configured identity. Check `git var GIT_AUTHOR_IDENT`
before committing; do not replace their selected email as an incidental setup step.

Author is keyed off the commit's author email — not who runs `git push` — so committing
under this identity is what keeps the contributor list clean, regardless of who pushes.

## Pull Requests

Open a PR from your branch into `main`. A good PR body has:

- **Summary** — what changed and why (link the task/ADR/spec).
- **Testing** — what you ran and the result (paste the green `verify.sh` tail).
- **Out of scope / follow-ups** — anything deliberately not done.
- No AI-attribution footer (see above).

PRs stay small. If a PR is getting large, it's really several PRs.

## Review gate — adversarial, before merge

Every PR gets an **adversarial review pass** before it can merge (the `/review` workflow,
or a fresh agent given only the diff). The reviewer actively tries to break the change:
correctness, edge cases, security, spec-drift, performance. Findings are Critical /
Important / Suggestion with `file:line` refs. Merge only when:

1. No open **Critical** or **Important** findings, **and**
2. CI is green (see [quality-gates.md](quality-gates.md)).

## Merging

Use **squash-merge** so `main` stays linear and each merge is one revertible commit. Delete
the branch after merge. Then tick the task in [../../tasks/todo.md](../../tasks/todo.md).

## Publication authority

The owner explicitly authorised publishing this repository and the current consolidation.
Within that scope, prepare the branch, commit, push and open a PR; inspect CI on the exact
head and merge only with clean review and green required checks. No force-push to main,
replacement of history, or production deployment is implied. For unrelated future work,
follow the owner's current authorisation.

## What never gets committed

`.gitignore` already excludes these — keep it that way:

- `.env` and `.env.*` (secrets) — only `.env.example` is tracked.
- `.venv/`, caches (`__pycache__`, `.mypy_cache`, `.ruff_cache`, `.pytest_cache`).
- `node_modules/`, `.next/`, build output.
- **Generated artifacts:** `graphify-out/cache/` and the machine-specific
  `graphify-out/.graphify_*` sidecars. The knowledge graph is regenerable — don't version
  the churny outputs. If you want one readable snapshot in-repo, keep only
  `graphify-out/GRAPH_REPORT.md`.

If a secret is ever committed, treat it as compromised: rotate it, then scrub history.
