# A5 — Targeted Supplement: Factuality, Faithfulness, and Attribution Shortcuts

This targeted supplement strengthens A5.3 without changing the A5 taxonomy. The central distinction is:

> **A factually correct answer is not necessarily faithful to the retrieved context, and an attributed answer is not necessarily causally dependent on the cited evidence.**

## Supplement papers

### A5-18. Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation

- **Authors / venue:** Ekaterina Fadeeva et al.; Findings of ACL 2026.
- **Links:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.338/) · [arXiv HTML](https://arxiv.org/html/2505.21072v3) · [arXiv](https://arxiv.org/abs/2505.21072)
- **Role:** Core / Evaluation diagnostic.
- **Primary sub-direction:** A5.3 Faithful Evidence Utilization.
- **Secondary sub-direction:** A4.1 Relevance & Support.
- **Question:** How should RAG outputs be fact-checked when factual correctness and support by the retrieved context are different properties?
- **Design:** FRANQ separates factuality from faithfulness. It first estimates whether a generated claim is entailed by the retrieved context, then conditions uncertainty quantification on that distinction. The paper introduces a long-form QA dataset annotated for both factuality and faithfulness, with automated labels and manual validation of difficult cases, and evaluates across multiple datasets, tasks, and LLMs.
- **Observed behavior:** The paper reports more accurate detection of factual errors in RAG outputs than existing uncertainty-quantification and hallucination-detection baselines. It also reports calibrated intermediate faithfulness estimates for partially grounded claims.
- **What it supports:** Factuality and context faithfulness must be measured separately. A model can be factually correct while not being supported by the retrieved context, or can be context-faithful while repeating an erroneous retrieved claim.
- **What it does not support:** FRANQ is a fact-checking and uncertainty-estimation method, not evidence that the generator learned causal evidence utilization. Faithfulness estimation remains an external diagnostic; it does not test necessary-evidence deletion, process-level attribution, or policy transfer.
- **Evidence vector:** `Method Effectiveness: Strong as a diagnostic`; `Capability Isolation: Medium`; `Generalization Evidence: Medium` across tasks/models, limited for causal behavior transfer.
- **Survey use:** Use as **partial mitigation** for A5 measurement and as evidence against conflating factual accuracy with evidence reliance.
- **Decision:** Include Core as an A5.3 diagnostic anchor, not as a post-training method.

### A5-19. Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models

- **Authors / venue:** Amin Abolghasemi et al.; Findings of ACL 2025.
- **Links:** [ACL Anthology](https://aclanthology.org/2025.findings-acl.1087/)
- **Role:** Core / Counterevidence and evaluation.
- **Primary sub-direction:** A5.3 Faithful Evidence Utilization.
- **Secondary sub-direction:** A4.2 Source Reliability / provenance cues.
- **Question:** Can source metadata, especially authorship information, change which documents a model attributes its answer to even when the underlying content is held constant?
- **Design:** The paper explicitly provides source authorship information and uses counterfactual evaluation to measure attribution sensitivity and bias across three LLMs. It compares human-written and AI-generated source metadata while studying attribution quality.
- **Observed behavior:** Adding authorship information changes attribution quality by 3–18% in the reported experiments, and models show bias toward explicit human authorship.
- **What it supports:** Citation or attribution is vulnerable to metadata shortcuts. A model may cite or prefer a source because of authorship cues rather than because the source content is more supportive or causally used.
- **What it does not support:** The paper does not measure whether the final answer is factually correct under the attribution change, nor does it prove causal process-level non-reliance. Its value is as a counterfactual warning against treating attribution quality as sufficient evidence of faithful integration.
- **Evidence vector:** `Method Effectiveness: Evaluation-only`; `Capability Isolation: Strong for metadata sensitivity`; `Generalization Evidence: Limited`.
- **Survey use:** Use as **counterevidence** to citation/attribution-as-faithfulness and as a concrete source-metadata perturbation protocol.
- **Decision:** Include Core as an A5.3 attribution-bias diagnostic anchor.

## Revised A5 synthesis

The A5 evidence should now be organized as a ladder rather than a single “faithfulness” score.

1. **Factuality:** Is the generated claim true?
2. **Output Reliance:** Does the answer change when relevant evidence is replaced?
3. **Attribution Sensitivity:** Does the cited source change for the right content reasons, rather than metadata cues?
4. **Compositional Reliance:** Does removing a necessary hop remove the corresponding conclusion?
5. **Process-level Reliance:** Does the intermediate reasoning causally depend on the evidence?

FRANQ strengthens the first measurement distinction: factuality and faithfulness are not interchangeable. Attribution Bias strengthens the second: even a citation can be driven by authorship metadata rather than content support. Together with ClashEval, Source-balancing, MIRAGE, RAFT, BRIEF, CARE, and TRACE-the-Evidence, these papers support a more precise conclusion:

> **Current methods can improve evidence-conditioned output behavior and can be evaluated with increasingly fine-grained diagnostics. However, factual correctness, context entailment, output sensitivity, and citation attribution remain weaker than necessary-evidence and process-level causal reliance. No current representative paper establishes that post-training produces a transferable evidence-integration policy that survives content, source, order, and environment interventions.**

## References

[1]: https://aclanthology.org/2026.findings-acl.338/ "Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation"
[2]: https://arxiv.org/html/2505.21072v3 "Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation"
[3]: https://aclanthology.org/2025.findings-acl.1087/ "Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models"


## Counterfactual Evidence Supplement

[Counterfactual Reasoning for Retrieval-Augmented Generation](./a5-counterfactual-evidence-supplement.md) is added as an inference-only counterfactual evidence-necessity boundary. It sharpens external evidence evaluation, but does not count as post-training evidence for process-level causal reliance.
