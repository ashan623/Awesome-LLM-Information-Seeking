# A5 — Counterfactual Evidence Necessity Supplement

## A5-20 Counterfactual Reasoning for Retrieval-Augmented Generation (CF-RAG)

- **Authors / year:** Huaiyu Qin, Chunyu Wei, Yueguo Chen, Yunhai Wang, 2026.
- **Venue / links:** ICLR 2026; [ICLR page](https://iclr.cc/virtual/2026/poster/10011109) · [OpenReview](https://openreview.net/forum?id=9U51rOnGko).
- **Role / contribution:** Inference-only / Boundary and diagnostic method.
- **Primary sub-direction:** A5.3 process-oriented evidence reliance; secondary A4.4 conflict/robustness.
- **Method:** CF-RAG generates counterfactual queries that probe alternative semantic dimensions, scores evidence by differential/selective support, and uses parallel arbitration to handle conflicting evidence. The paper explicitly describes itself as pure inference-time, with no fine-tuning or parameter update.
- **Evidence value:** CF-RAG is useful because it operationalizes a stronger notion than raw relevance: evidence should support the original question selectively rather than support related counterfactuals indiscriminately. Its distractor and conflict experiments provide a concrete counterfactual lens for evidence necessity and causal discriminability.
- **Capability isolation:** Medium for an external evidence-selection intervention; not evidence that the LLM learned process-level causal reliance through post-training. The intervention changes the evidence space and arbitration procedure, while the backbone remains frozen.
- **Generalization:** Reports robustness across multiple QA benchmarks and distractor/conflict conditions, but does not establish post-trained policy transfer across retrievers, sources, temporal shifts or deployment environments.
- **Boundary:** The paper demonstrates that counterfactual evidence evaluation can improve RAG robustness; it does not show that the generator learned to identify necessary evidence internally.
- **Survey placement:** Section 3 A5 after MIRAGE/FRANQ and before the conclusion on necessary-evidence tests. Use it as a diagnostic/inference-time boundary case, not as a post-training success.

## Synthesis

CF-RAG strengthens the distinction between **evidence-conditioned performance** and **learned evidence integration**:

> **A system can improve evidence discrimination by intervening on the external evidence space without changing the generator policy. This is valuable evidence for the evaluation protocol, but it cannot be counted as proof that post-training created process-level causal evidence reliance.**

This paper should be connected to the proposed `necessary-evidence leave-one-out` and `counterfactual evidence replacement` tests.

## Excluded candidate: EGR-FV

The candidate title **“EGR-FV: Evidence-Grounded Remixing for Debiased Fact Verification / Counterfactual Necessity-Guided Training”** was searched against ACL Anthology, arXiv and exact-title web queries during this round, but no formal, stable paper record was found. It is therefore **not added to the core catalog**. It can be reconsidered after the authors, venue, DOI or arXiv identifier are supplied.
