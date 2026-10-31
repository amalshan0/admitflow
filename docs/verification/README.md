# Technical verification records

These files preserve bounded implementation checks. They were moved without rewriting
recorded JSON results. They are evidence for the source/configuration they describe,
not a fresh pass on the current checkout, operational readiness or student-outcome proof.
Current scope is in [STATUS.md](../../STATUS.md) and fresh work in [tasks/todo.md](../../tasks/todo.md).

| Area | Records |
| --- | --- |
| Product context consolidation | [CONTEXT_CONSOLIDATION_RESULTS.json](CONTEXT_CONSOLIDATION_RESULTS.json); fresh local checks for Task 71 |
| Code/configuration fingerprint | [BUILD_MANIFEST.json](BUILD_MANIFEST.json); original paths/hashes intentionally retained |
| Local packaging | [COMPOSE_RESULTS.json](COMPOSE_RESULTS.json) |
| Ingestion/parser | [DOCUMENT_PIPELINE_RESULTS.json](DOCUMENT_PIPELINE_RESULTS.json), [DOCUMENT_CONTAINER_RESULTS.json](DOCUMENT_CONTAINER_RESULTS.json), [review](DOCUMENT_REVIEW.md) |
| Retrieval/reranking | [CONTEXTUAL_RETRIEVAL_RESULTS.json](CONTEXTUAL_RETRIEVAL_RESULTS.json), [RERANKING_RESULTS.json](RERANKING_RESULTS.json), [EVALUATION_RESULTS.json](EVALUATION_RESULTS.json) |
| Guidance | [GUIDANCE_RELEASE_RESULTS.json](GUIDANCE_RELEASE_RESULTS.json), [GUIDANCE_EVALUATION_RESULTS.json](GUIDANCE_EVALUATION_RESULTS.json) |
| Continuity | [CONVERSATION_RELEASE_RESULTS.json](CONVERSATION_RELEASE_RESULTS.json), [CONVERSATION_EVALUATION_RESULTS.json](CONVERSATION_EVALUATION_RESULTS.json) |
| Supportive interaction | [SUPPORTIVE_GUIDANCE_RELEASE.json](SUPPORTIVE_GUIDANCE_RELEASE.json), [SUPPORTIVE_GUIDANCE_RESULTS.json](SUPPORTIVE_GUIDANCE_RESULTS.json), [NECESSARY_HANDOFF_RELEASE.json](NECESSARY_HANDOFF_RELEASE.json) |
| Public reference dataset | [JAIN_KOCHI_RELEASE.json](JAIN_KOCHI_RELEASE.json), [JAIN_KOCHI_VALIDATION.json](JAIN_KOCHI_VALIDATION.json) |
| Legacy sample application | [DEMO_APPLICATIONS_VALIDATION.json](DEMO_APPLICATIONS_VALIDATION.json); development-only, not real admission |
| Actual handset checks | [LIVE_HANDSET_RESULTS.json](LIVE_HANDSET_RESULTS.json); scope is the recorded earlier flow |
| Language | [LANGUAGE_REVIEW.md](LANGUAGE_REVIEW.md); historical agent review is not fluent-speaker approval |
| Task-level reasoning | [IMPLEMENTATION_HISTORY.md](IMPLEMENTATION_HISTORY.md) |

Original code fingerprints can name files that have moved or been removed. Keep those
fingerprints intact; regenerating one is a new measurement. Provider probes, simulated
settlement, old handset checks and sandbox card completion are distinct evidence types.
Do not combine separate reports into an invented all-pass current sweep.
