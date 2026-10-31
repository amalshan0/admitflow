# Product context reconciliation

Owner decision, October 30, 2026: maintain the actual institutional product here.
The original product foundation supplied the intent; later implementations supplied
useful conversation and integration lessons. Those lessons are distilled below, so no
other repository is required. Temporary delivery instructions/assets are not imported.
The [source manifest](context-sources.json) records commits, original paths, hashes and
local destinations. It is provenance, not another roadmap or an instruction to open old sources.

## Original product intent

| Original context | Current disposition and local authority |
| --- | --- |
| Reusable multi-tenant product for Kerala coaching centres, colleges and training institutes | Retained in [PRODUCT.md](../../PRODUCT.md); manual provisioning now, self-service onboarding later |
| Full inquiry-to-enrollment relationship, memory and readiness-aware support | Retained; [SPEC sections 4–6](../../SPEC.md) separate support, eligibility, application/payment and review |
| Institution owns content, not a fixed conversation script | Retained; staff controls catalogue/rules/knowledge/settings |
| Tap-first understanding | Updated to adaptive free text with contextual taps; ADR-013–015 |
| One WhatsApp number per institution and Meta routing | Superseded for active operation by Telegram bot mapping, ADR-012; compatibility disabled |
| Multilingual English/Hindi/Hinglish/Malayalam | Product ambition retained; English guidance active; complete language acceptance remains Task 75 |
| Structured qualification fields/operators and code-owned eligibility | Retained with missing/failed/review outcomes; interest fit never approves eligibility |
| Batches, capacity and availability | Retained for authorised offerings; explicit selection and revalidation; references never fabricate intakes |
| Cold-lead follow-up after configurable inactivity, originally at most two attempts | Retained as consent-aware engagement; actual settings/ownership/office-hour/cap checks govern, not a restored hard-coded sequence |
| Lead-source outbound WhatsApp templates | Not active Telegram functionality; unsolicited outreach needs a separate channel/consent decision |
| Application details and manual review | Retained, with minimal necessary fields, explicit submission and separate staff outcomes; no default ID-proof-number collection |
| Razorpay, database fees, active discounts and under-review after payment | Retained; matching/replay controls and explicit continuation; sandbox/live are separate |
| Per-institution uploads, retrieval and fail-closed answers | Retained and extended with durable ingestion, scoped pgvector, OpenAI reranking and exact supporting quotations |
| Human escalation and co-pilot | Retained with explicit takeover/resume and reviewed sends; ordinary uncertainty remains assistant-led under later decisions |
| Office hours and staff response | Retained as configured operations; no unconfigured SLA or automatic promise of staff response |
| Admin/agent roles, catalogue/documents/settings, CRM, analytics and CSV/Excel/integrations | Retained; [STATUS.md](../../STATUS.md) identifies implementation/limits |
| Authentication with fastapi-users, SSE, dark glassmorphism | Replaced by implemented password/server-session auth, authenticated WebSockets and warm-neutral plain CSS |
| Fly/Vercel/Neon/R2 and Gemini/OpenAI/Anthropic chain | Historical proposals, not requirements; current hosting unselected and OpenAI-only inference, ADR-007/012 |
| System-1/Laya readiness scoring and future distillation | Superseded by typed OpenAI decisions, no training/self-hosted cutover; ADR-008 |
| Episodic/fact/knowledge memory | Retained; bounded context and exact corrections. Rolling summaries remain an open tuning decision |
| PII encryption/audit, erasure, configurable retention | Retained; production policy/age/provider review remains Task 74. Encryption is not a compliance assurance |

## Useful later conversation decisions

| Context from later feature briefs | Disposition, acceptance example or gap |
| --- | --- |
| Known course, known field, unsure and browse/question entry paths | Intent retained without a compulsory menu. A named verified course bypasses optional exploration; ambiguity asks for clarification |
| Student preference before parent preference | Retained requirement. Current prompts distinguish family expectations but notes have no structured speaker field; Task 74 must specify/test attribution. A parent correction must not silently overwrite student facts |
| Attributed corrections and preservation | Retained intent: changing age preserves education/interests; changing a course invalidates its eligibility, not unrelated preferences. Structured other-speaker isolation remains unverified |
| Repeated uncertainty, non-informative turns and interruptions | Retained in ADR-014/015 and guidance policy. A second “not sure” simplifies the next move; a greeting is not a qualification answer; an FAQ does not lose pending progress |
| Explainable matches and inspect-before-commit | Retained. Evidence-backed cards/details do not select a course; changed facts invalidate old controls. No compulsory fixed confirmation interview is restored |
| Optional learning-goal tags | Record as catalogue-owned metadata candidate; no current dedicated field or validated goal-tag filter is claimed. Task 75 |
| Teaching medium versus discussion support | Preserve the distinction. Never infer proficiency or promise support from a language preference; dedicated current metadata/filter is unfinished. Task 75 |
| Delivery mode versus schedule/accessibility | Preserve the distinction. Online/hybrid/in-person does not prove a timetable or accommodation; metadata/acceptance remains Task 75 |
| One optional narrowing question per exact fact set | Historical implementation policy, not a universal product limit. Current adaptive policy asks only consequential questions and handles stalled conversation; ADR-015 |
| Qualitative readiness separate from lifecycle/payment | Retained conceptual boundary; current exploring/comparing/decided guidance stages are not a validated numeric readiness score |
| Non-undergraduate prerequisite self-report unlocking a sample fee | Historical gate, not current institutional policy. Real eligibility uses institution rules; ADR-017 is a separate development-only reference exception |
| Processing notice, default-on AI and opt-out/re-enable/button-only fallback | An explicit outstanding product decision. Reminder consent and `store=False` do not establish this feature. Task 74 must settle intended controls and test no provider calls after any adopted opt-out |
| No names/Telegram IDs in bounded AI context | Preserve data minimisation intent. Current model-history/application-extraction paths require their own field-level review; do not import an old “no personal data sent” claim |
| Full-message/identifier Langfuse debugging | Not active policy; current telemetry is sanitised. No permission to export raw applicant content is imported. External copies would need separate retention/deletion controls |
| Whole-demo reset preserving catalogue | Not a current product feature claim. `/reset`, inquiry erasure, retention and one-off development catalogue replacement have different semantics |
| SQLite/NumPy FAQ thresholds 0.58/0.05 and 27 cases | Historical retriever-specific calibration only. Do not transfer to current pgvector/document/reranking thresholds or acceptance |
| Earlier INR 500 paid sandbox link and live bot checks | Do not transfer as current acceptance. Latest-source INR 3,000 sample flow still lacks recorded completed test-card/handset acceptance |
| Single-process SQLite, webhook-only Telegram, simple staff credential, illustrative catalogue | Omitted as implementation choices; current PostgreSQL/TaskIQ/Next.js/receiver/session foundation remains |

## Implementation lessons retained as acceptance

- Revalidate catalogue state after model latency; a closing course cannot remain selectable.
- Apply accepted corrections before planning; failure to generate a reply must not restore stale facts.
- A stale recommendation/detail/payment control cannot act on a different course/profile/tenant.
- Incomplete course facts remain unknown; generic descriptions cannot prove a specific curriculum.
- Hard gates use the selected course's actual rules, never a universal undergraduate assumption.
- False, unsure or missing prerequisite answers cannot masquerade as verified eligibility.
- Store current-message quotations literally, with atomic notes and targeted replacement.
- Staff acknowledgment, resolution and resumption are distinct; an existing pause is preserved.
- Test schema-valid but invented IDs/values, refusal/outage, duplicate delivery and uncertain sends.
- Verify retrieval against expected sources and unsupported/ambiguous cases using the same
  decision path as production; a top-ranked passage alone is not support.
- Keep live-model, database, provider, browser and handset evidence separate. A new UI/path
  does not inherit an older paid-link or test-count claim.

## Document authority and unresolved choices

PRODUCT defines purpose; SPEC defines requirements; STATUS and verification identify
evidence; plan/todo define work. ADR-018 governs this consolidation and newer ADRs override
conflicting older assumptions. No raw external instruction file is an active dependency.

Open choices include authorised pilot data/policies, operating budget/host, AI-processing
controls, minors/guardian and retention policy, structured parent attribution, comparison
metadata, language quality, pricing and self-service onboarding. Tasks 72–77 keep these
visible. Consolidation does not invent answers or implement those features.
