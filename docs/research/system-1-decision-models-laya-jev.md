# Historical decision-model research and retained lessons

The earlier design considered specialised System-1 decision models (Laya/Jev) for bounded
intent, guardrail, readiness and escalation decisions, separate from user-facing text
generation. It proposed a shared `decide()` interface and a possible later distilled model.
This is historical architecture context, not an active model-selection or training plan.

[ADR-008](../decisions/ADR-008-system-1-decision-layer.md) now selects typed OpenAI decisions;
[ADR-012](../decisions/ADR-012-telegram-openai-mvp.md) selects the active provider/channel.
No training, distillation, self-hosted cutover or extra inference service is planned.
The original speed/calibration/vendor claims are not measurements for AdmitFlow.

## Lessons retained

- Separate interpretation from consequential business decisions.
- Expose bounded typed output behind one boundary; validate values after schema parsing.
- Keep model configuration replaceable without embedding vendor details in domain logic.
- Evaluate refusal/outage, invented IDs, language coverage and changes in a real decision
  path; valid JSON and a probability do not establish factual correctness or calibration.
- Treat readiness as a support need distinct from academic eligibility and payment state.

Current behaviour is in [SPEC.md](../../SPEC.md), [guidance research](../engineering/ADAPTIVE_GUIDANCE_RESEARCH.md)
and the [context reconciliation](../planning/CONTEXT_RECONCILIATION.md).
