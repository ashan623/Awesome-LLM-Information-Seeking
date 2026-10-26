# Round 10 — Targeted A1/A4/A5 Evidence Audit

## User-directed objective

Do not expand the survey boundary or collect a large new paper pool. Strengthen A1, A4, and A5 by matching claim strength to evidence strength, with priority A5 > A4 > A1.

## Search scope

Four candidate papers were searched and opened from formal sources:

1. Wan, Wallace, and Klein, **What Evidence Do Language Models Find Convincing?** ACL 2024; ACL Anthology and arXiv.
2. Cattan et al., **(D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs**; Google Research and arXiv:2506.08500.
3. Fadeeva et al., **Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation**; Findings ACL 2026; ACL Anthology and arXiv:2505.21072.
4. Abolghasemi et al., **Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models**; Findings ACL 2025; ACL Anthology.

## Inclusion decisions

- **Include A4-11:** direct counterevidence that query relevance is not the same as convincingness/source reliability; counterfactual sensitivity design is useful for A4.2/A4.4.
- **Include A4-12:** direct conflict taxonomy and desired-behavior benchmark; use as measurement/behavior evidence, not as post-training capability proof.
- **Include A5-18:** separates factuality from faithfulness and gives a diagnostic evaluation protocol; use as A5.3 measurement anchor, not as a generator policy-learning result.
- **Include A5-19:** counterfactual authorship metadata perturbation demonstrates attribution shortcut risk; use as A5.3 counterevidence, not as a general faithfulness benchmark.

## Existing-paper attribution corrections

- **Calibrate-Then-Act:** keep under A1 cost-aware decision learning, but explicitly record that its calibrated prior/answerability estimate is an external decision input. RL improvement does not alone establish intrinsic LLM calibration.
- **CONFACT:** keep under A4.2/A4.4 evaluation and source-aware fact-checking. It contains source credibility prediction and system-level methods, but should not be described as evidence that an LLM policy learned general source reliability through post-training.

## Deliverables

- `papers/a4-evidence-evaluation-third.md`
- `papers/a5-evidence-integration-third.md`
- `analysis/a1-a4-a5-claim-evidence-audit.md`
- This search log.

The next action is to add the four papers to README/catalog, update the A4/A5 synthesis links, and push the targeted audit. No broad search is opened.

## References

[1]: https://aclanthology.org/2024.acl-long.403/ "What Evidence Do Language Models Find Convincing?"
[2]: https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/ "(D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs"
[3]: https://aclanthology.org/2026.findings-acl.338/ "Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation"
[4]: https://aclanthology.org/2025.findings-acl.1087/ "Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models"
