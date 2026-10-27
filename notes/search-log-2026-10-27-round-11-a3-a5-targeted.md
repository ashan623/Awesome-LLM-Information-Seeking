# Round 11 — Targeted A3/A5 Evidence Supplement

## Objective

根据用户提供的候选论文，只补强已有主线中的 A3 和 A5，不扩展 capability boundary，也不重新开启广泛 Search-Agent/RAG 文献搜集。

## Formal-source verification

1. **GISA: A Benchmark for General Information-Seeking Assistant** — verified on arXiv:2602.08543v2 and HTML. It provides 373 human-crafted queries, stable/live settings, four structured answer formats, and human search trajectories. Included as A3 evaluation evidence.
2. **SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration** — verified on arXiv:2607.15257v1 and HTML. It introduces Frontier Task, Evidence Graph, Coverage Map and Failure Memory, and reports system-level interventions for stalled search. Included as A3 boundary/system-state evidence.
3. **Counterfactual Reasoning for Retrieval-Augmented Generation (CF-RAG)** — verified on ICLR 2026 and OpenReview. It uses counterfactual query exploration and parallel arbitration at inference time and explicitly reports no training/fine-tuning. Included as A5 counterfactual evidence-evaluation boundary, not post-training evidence.
4. **EGR-FV** — exact-title searches did not produce an official ACL Anthology, arXiv, EMNLP or DOI record. Excluded from the core catalog pending stable metadata.

## Inclusion decisions

- Add **GISA** to A3 as `Evaluation`.
- Add **SearchOS-V1** to A3 as `Boundary`; use it to show that coverage-state management and middleware can improve outcomes without proving an updated LLM exploration policy.
- Add **CF-RAG** to A5 as `Inference-only / Boundary`; use it to sharpen the distinction between external evidence intervention and learned process-level evidence reliance.
- Do not add EGR-FV until formally verifiable.

## Scope-preserving conclusion

The additions strengthen existing claims rather than create new ones:

> A3 has stronger behavioral and system-level measurement of useful coverage, unresolved regions and failure recovery, while A5 has a sharper counterfactual evidence-necessity diagnostic. Neither closes the central transfer question for post-trained, environment-general information-seeking capabilities.

## References

- GISA: https://arxiv.org/abs/2602.08543v2
- SearchOS-V1: https://arxiv.org/abs/2607.15257v1
- CF-RAG: https://iclr.cc/virtual/2026/poster/10011109
- CF-RAG OpenReview: https://openreview.net/forum?id=9U51rOnGko
