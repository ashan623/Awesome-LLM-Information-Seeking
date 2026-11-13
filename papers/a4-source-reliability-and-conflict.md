# A4 — Targeted Supplement: Convincingness, Conflict Types, and Source Cues

This targeted supplement does not expand the A4 taxonomy. It strengthens the argument that **relevance, support, source credibility, conflict type, and appropriate conflict handling are distinct judgment targets**.

## Supplement papers

### A4-11. What Evidence Do Language Models Find Convincing?

- **Authors / venue:** Alexander Wan, Eric Wallace, Dan Klein; ACL 2024.
- **Links:** [ACL Anthology](https://aclanthology.org/2024.acl-long.403/) · [arXiv](https://arxiv.org/abs/2402.11782)
- **Role:** Core / Evaluation and counterevidence.
- **Primary sub-direction:** A4.2 Source Reliability.
- **Secondary sub-direction:** A4.4 Conflict & Robustness.
- **Question:** When real-world documents disagree on a controversial question, which document features affect an LLM's judgment of what is convincing?
- **Design:** ConflictingQA pairs controversial queries with real-world evidence documents that vary in quantitative facts, argument styles, authority appeals, scientific references, tone, and yes/no answers. Sensitivity and counterfactual analyses vary text features while holding the broader query/evidence setting fixed.
- **Observed behavior:** Models rely heavily on query–website relevance but largely ignore stylistic features that human readers consider important, such as scientific references and neutral tone.
- **What it supports:** Relevance is not equivalent to credibility or human-aligned convincingness. This is direct evaluation evidence for separating A4.1 relevance from A4.2 reliability.
- **What it does not support:** It is not evidence that post-training teaches a general source-reliability policy. The study is primarily a model-behavior and counterfactual evaluation, not an SFT/RL intervention with held-out source, domain, or temporal transfer.
- **Evidence vector:** `Method Effectiveness: Not a policy method`; `Capability Isolation: Medium-to-Strong`; `Generalization Evidence: Limited`.
- **Survey use:** Use as **counterevidence** against interpreting retrieval relevance, answer accuracy, or citation presence as reliable evidence judgment.
- **Decision:** Include Core as an A4.2/A4.4 evaluation anchor.

### A4-12. (D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs

- **Authors / venue:** Arie Cattan et al.; Google Research, 2025; arXiv:2506.08500.
- **Links:** [Google Research](https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/) · [arXiv](https://arxiv.org/abs/2506.08500)
- **Role:** Core / Evaluation and behavior study.
- **Primary sub-direction:** A4.4 Conflict & Robustness.
- **Secondary sub-direction:** A4.2 Source Reliability.
- **Question:** Do different knowledge-conflict types require different model behavior, rather than one generic “follow the retrieved context” rule?
- **Design:** The paper proposes a taxonomy of conflict types and desired behavior for each type, constructs the CONFLICTS benchmark with expert conflict annotations in realistic RAG instances, and evaluates whether explicitly informing the model of the conflict category improves response quality and appropriateness.
- **Observed behavior:** Conflict-aware prompting improves the quality and appropriateness of responses, but substantial errors remain.
- **What it supports:** Conflict resolution is not reducible to factuality, relevance, or majority voting. The appropriate action depends on the conflict type and should be evaluated as a behavior target.
- **What it does not support:** Conflict-category prompting is not evidence that the LLM learned a transferable conflict-resolution policy through post-training. The method does not establish robustness under source replacement, time shift, unseen conflict types, or a no-category-information deployment setting.
- **Evidence vector:** `Method Effectiveness: Medium-to-Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Survey use:** Use as a **partial mitigation** for A4.4 measurement and as a counterexample to generic evidence-quality scoring; keep the learning claim narrow.
- **Decision:** Include Core as an A4.4 evaluation anchor; do not classify it as direct LLM policy post-training.

## Revised A4 synthesis

The two papers sharpen the A4 argument in two ways.

First, **relevance is not reliability**. CRAG and retrieval-quality evaluators can identify query–document relevance, but ConflictingQA shows that relevance can dominate features that humans treat as credibility or convincingness. Therefore, a high relevance score or improved retrieval recall cannot be used as evidence that the model learned source-reliability judgment.

Second, **conflict is a typed decision problem**. CONFACT and ConfRAG provide conflict-related evaluation resources, while (D)RAGged Into a Conflict explicitly maps conflict categories to desired responses. This strengthens the measurement side of A4.4, but the post-training side remains narrow: current methods show local support/verification or conflict-aware prompting, not a source-robust policy that transfers across conflict types and information environments.

The updated A4 conclusion is:

> **The literature increasingly measures evidence judgment beyond relevance, including convincingness, source cues, sufficiency, and typed conflict handling. However, direct post-training evidence remains narrow: models can be trained or prompted to perform local verification behaviors, but a generalizable policy that separates relevance from reliability and adapts to unseen conflict conditions is not established.**

## References

[1]: https://aclanthology.org/2024.acl-long.403/ "What Evidence Do Language Models Find Convincing?"
[2]: https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/ "(D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs"
[3]: https://arxiv.org/abs/2506.08500 "DRAGged into Conflicts: Detecting and Addressing Conflicting Sources in Search-Augmented LLMs"
