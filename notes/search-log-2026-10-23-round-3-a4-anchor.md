# Search Log — Round 3 — A4 Evidence Evaluation Anchor Batch

## Boundary

A4 is restricted to evidence-quality judgment before evidence is used:

- A4.1 Relevance & Support
- A4.2 Source Reliability
- A4.3 Evidence Sufficiency
- A4.4 Conflict & Robustness

A5 covers using evaluated evidence to update reasoning, beliefs, memory, generation, or decisions. Final QA improvement, context utilization, and answer generation are not automatically A4.

## Six verified anchors

- **CRAG** — A4.1 relevance evaluator and corrective routing; not a general evidence verifier.
- **S2G-RAG** — A4.3 structured sufficiency and gap judging with context-only process supervision.
- **RAGTruth** — A4.1/A4.4 human span-level unsupported and contradictory claim resource with detector baselines.
- **RAGChecker** — A4.1 evaluation framework with claim support/entailment, claim recall, context precision, faithfulness, and noise sensitivity.
- **RLSeek** — A4.1 post-training anchor for source-grounded verification, quotation fidelity, and support/refutation reasoning.
- **EAPO** — A4.1 mixed training anchor that uses evidence utility in GRPO and reward-model co-evolution; its evidence score is coupled to answer correctness.

## First-batch synthesis

The first batch provides four non-equivalent evidence signals: query-document relevance, claim support/contradiction, set-level sufficiency and gaps, and query-relative evidence utility. This confirms that A4 should not be treated as a single “faithfulness” category.

The strongest learned behaviors are narrow: corrective relevance routing, sufficient/insufficient plus missing-gap prediction, source-anchored claim verification, quotation-faithfulness behavior, and evidence extraction. None of the training methods yet establishes a general evidence evaluator that simultaneously handles source credibility, set completeness, multi-source conflict, and adversarial evidence.

The chain Evidence Quality Signal → Evaluation/Training → Verification Behavior → Generalization is only partially complete. RAGTruth and RAGChecker provide measurement resources; S2G-RAG, RLSeek, and EAPO provide early training evidence. Generalization and reward/verifier robustness remain limited.

## Confirmed missing coverage

A4.2 is nearly empty in this batch: no core paper directly learns source authority, reliability, freshness, provenance, or traceability. A4.3 still lacks independent human set-level sufficiency truth; gold-title coverage is only a proxy. A4.4 lacks systematic multi-source contradiction attribution, evidence priority, fabricated citations, and misleading/noisy evidence stress tests.

Relevance and support also need controlled separation: topic relevance, claim entailment, partial support, low-quality support, and unsupported but semantically related passages should be evaluated separately.

## Next batch boundary

The next batch should add only gap-focused papers for source reliability, human-verified evidence-set sufficiency, multi-source conflict, controlled relevance/support separation, direct OOD judge transfer, and verifier/reward hacking. Stop after the total A4 map reaches 8–12 papers. Generic hallucination and fact-checking work remains out of scope unless it directly measures or trains one of the four A4 behaviors.
