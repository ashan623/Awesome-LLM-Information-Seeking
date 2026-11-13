# A4 — Evidence Evaluation

The A4 map is complemented by dedicated analyses of evidence sufficiency/conflict in [`a4-evidence-sufficiency-and-conflict.md`](./a4-evidence-sufficiency-and-conflict.md) and source reliability/convincingness in [`a4-source-reliability-and-conflict.md`](./a4-source-reliability-and-conflict.md). Together, these papers cover the measurement and learning evidence without treating evaluation resources as proof of learned judgment.

## Boundary with A5

A4 asks whether acquired evidence is worth trusting **before it is used**:

- Does the evidence support the question or a specific claim?
- Is the source credible, authoritative, current, and traceable?
- Is the evidence set sufficient, or is something missing?
- Are there contradictions, noise, misleading passages, or forged citations?

A5 asks whether the model correctly uses evaluated evidence to update reasoning, beliefs, memory, generation, or decisions. Final answer improvement alone is not evidence of A4.

## Four sub-directions

- **A4.1 Relevance & Support:** relevance to the task is not the same as entailment of a claim.
- **A4.2 Source Reliability:** credibility, authority, freshness, provenance, and traceability.
- **A4.3 Evidence Sufficiency:** set-level completeness, missing evidence, bridge facts, and stopping.
- **A4.4 Conflict & Robustness:** contradiction, noise, misleading evidence, fabricated citations, and adversarial contexts.

This selective paper set is not an exhaustive collection of hallucination, fact-checking, or RAG-evaluation work.

---

## Anchor papers

### A4-1. Corrective Retrieval Augmented Generation

- **Authors / year:** Shi-Qi Yan, Jia-Chen Gu, Yun Zhu, Zhen-Hua Ling, 2024; ICLR 2025 withdrawn submission
- **Links:** [arXiv](https://arxiv.org/abs/2401.15884) · [code](https://github.com/HuskyInSalt/CRAG)
- **Role:** Core / supporting anchor
- **Contribution type:** Retriever Optimization
- **Primary sub-direction:** A4.1 Relevance & Support
- **Training mechanism:** B1 supervised retrieval-evaluator construction
- **Learning target:** A T5-based evaluator that scores query–document relevance and routes evidence as correct, incorrect, or ambiguous.
- **Evidence-quality signal:** Weak relevance labels based on golden-title tracking and random negatives; no claim-level support, source authority, sufficiency, or contradiction labels.
- **A4 versus A5:** The evaluator is A4.1. Knowledge refinement, web search fallback, and final QA generation are downstream A5 or control behavior.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Learned behavior:** Relevance gating and corrective routing, not general verification.
- **Main limitation:** PopQA-derived relevance labels and fixed threshold routing may encourage dataset shortcuts. Cross-domain results are mostly final-task performance; source reliability, set sufficiency, conflicts, and adversarial evidence are not tested.
- **Reward-hacking risk:** Low-to-Medium and not directly tested; a relevance evaluator may learn title or lexical proxies.
- **Decision:** Include Core as an A4.1 relevance-gating anchor, not as a general evidence-verification method.

### A4-2. S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA

- **Authors / year:** Minghan Li et al., ACL 2026
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1185/) · [arXiv](https://arxiv.org/abs/2604.23783) · [code](https://github.com/nianaaa/S2G-RAG)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary sub-direction:** A4.3 Evidence Sufficiency
- **Secondary sub-directions:** A4.1 Relevance & Support; A4.4 Conflict & Robustness
- **Training mechanism:** B1 data construction and B2 teacher-distilled process supervision through LoRA SFT.
- **Learning target:** A context-only judge that predicts sufficient/insufficient and outputs structured missing evidence or information gaps.
- **Evidence-quality signal:** Sufficiency and gap annotations over intermediate evidence contexts; gold-supporting-title coverage is used as a noisy auxiliary proxy, not as the entire judge input.
- **A4 versus A5:** The sufficiency/gap judgment is A4.3. Query generation, evidence accumulation, extraction, and final answer generation are downstream search/control or A5 behaviors.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Learned behavior:** Continue searching when evidence is insufficient, identify missing bridge entities/attributes/relations, and stop only when the accumulated context is judged sufficient.
- **Main limitation:** Sufficiency truth is still largely tied to gold-title coverage and Wikipedia multi-hop QA. There is no independent human set-level completeness truth, source reliability, temporal change, or explicit multi-source contradiction evaluation.
- **Reward-hacking risk:** Low-to-Medium and not directly tested; teacher imitation may learn title-coverage shortcuts or become overly conservative.
- **Decision:** Include Core as the strongest A4.3 anchor.

### A4-3. RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models

- **Authors / year:** Cheng Niu et al., Findings of ACL: EMNLP 2023; ACL 2024 long-paper version
- **Links:** [ACL Anthology](https://aclanthology.org/2024.acl-long.585/) · [arXiv](https://arxiv.org/abs/2401.00396) · [code](https://github.com/particlemedia/ragtruth)
- **Role:** Evaluation / resource
- **Contribution type:** Evaluation
- **Primary sub-directions:** A4.1 Relevance & Support; A4.4 Conflict & Robustness
- **Training mechanism:** Benchmark construction plus supervised hallucination-detector baseline.
- **Learning target:** A detector that marks unsupported or contradictory response spans relative to a provided context.
- **Evidence-quality signal:** Human annotations of context-grounded conflict and unsupported/baseless information at response-span level.
- **A4 versus A5:** Span-level support/contradiction detection is A4.1/A4.4. Candidate-response selection and final hallucination reduction are downstream output control, not evidence integration.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Learned behavior:** Identify unsupported or conflicting output spans, but not source authority, evidence sufficiency, or multi-source conflict attribution.
- **Main limitation:** The provided context is treated as the reference truth. Random in-distribution splits do not establish cross-source, cross-domain, temporal, tool, or adversarial robustness.
- **Reward-hacking risk:** Low-to-Medium and not directly tested; detectors may learn annotation or formatting shortcuts and candidate selection may optimize predicted span counts.
- **Decision:** Include Core as a support/contradiction annotation and diagnostic anchor.

### A4-4. RAGChecker: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation

- **Authors / year:** Dongyu Ru et al., NeurIPS 2024
- **Links:** [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2024/hash/27245589131d17368cccdfa990cbf16e-Abstract-Datasets_and_Benchmarks_Track.html) · [arXiv](https://arxiv.org/abs/2408.08067) · [code](https://github.com/amazon-science/RAGChecker)
- **Role:** Evaluation
- **Contribution type:** Evaluation
- **Primary sub-direction:** A4.1 Relevance & Support
- **Secondary sub-directions:** A4.3 Evidence Sufficiency; A4.4 Conflict & Robustness
- **Training mechanism:** Evaluation-only; claim extraction and entailment checking are performed at evaluation time.
- **Learning target:** No trained policy. It decomposes RAG quality into claim–chunk entailment, claim recall, context precision, faithfulness, and noise sensitivity.
- **Evidence-quality signal:** Direct claim-level support/entailment and context completeness/precision metrics, plus sensitivity to relevant and irrelevant noise.
- **A4 versus A5:** Claim support and evidence metrics are A4. The framework diagnoses response faithfulness but does not train whether the model should update beliefs or use the evidence correctly.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Strong`; `Generalization Evidence: Medium`.
- **Learned behavior:** None; this is an evaluation and diagnostic framework, not evidence-evaluation capability training.
- **Main limitation:** Source credibility, authority, freshness, and provenance are not evaluated. Neutral versus contradiction is not fully separated, and several metrics depend on a large LLM checker and generated or filtered references.
- **Reward-hacking risk:** Medium and not empirically tested; optimizing the metric could encourage checker-specific claim wording, citation style, or avoidance of detectable contradictions.
- **Decision:** Include Core as a fine-grained evaluation anchor, not as a learned A4 method.

### A4-5. RLSeek: Evidence-Grounded Reasoning for RAG Hallucination Detection

- **Authors / year:** Zhaoheng Huang et al., ACL 2026
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1492/) · [code](https://github.com/WaldenRUC/RLSeek)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary sub-direction:** A4.1 Relevance & Support
- **Training mechanism:** B3 reward design and B4 GRPO.
- **Learning target:** A policy that locates source spans, quotes them faithfully, and explains whether they support or refute a claim or response span.
- **Evidence-quality signal:** Span-F1 for hallucination detection, quotation-faithfulness/LCS penalties, and claim-support explanation constraints.
- **A4 versus A5:** Source-grounded support and citation fidelity are A4. The model is not shown to update beliefs or integrate evidence into a new decision, so it should not be treated as A5.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Learned behavior:** Stepwise evidence-seeking verification with source-anchored quotations and support/refutation explanations.
- **Main limitation:** The grounding source is supplied in advance, and retrieval quality is outside the scope. There is no source reliability, set-level sufficiency, multi-source contradiction, adversarial context, or strong cross-tool evaluation.
- **Reward-hacking risk:** Medium; LCS and quote-format rewards may favor copying or formatting without genuine claim support. The paper does not provide held-out verifier or adversarial checker-gaming tests.
- **Decision:** Include Core as the first direct A4.1 post-training anchor.

### A4-6. Evidence-Augmented Policy Optimization with Reward Co-Evolution for Long-Context Reasoning

- **Authors / year:** Xin Guan et al., ACL 2026; arXiv v2
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1404/) · [arXiv](https://arxiv.org/abs/2601.10306)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary sub-direction:** A4.1 Relevance & Support
- **Secondary sub-direction:** A4.3 Evidence Sufficiency
- **Training mechanism:** B4 GRPO with an evidence-quality reward and B6 reward-model co-evolution.
- **Learning target:** Explicit evidence extraction and citation before reasoning, with a reward model that scores the utility of evidence for the answer.
- **Evidence-quality signal:** A 1–5 evidence utility score over evidence segments/sets, combined with answer and format rewards; human support judgments are reported as an auxiliary audit.
- **A4 versus A5:** Evidence utility and support judgment are A4. The final answer reward and long-context reasoning improvement are A5/downstream utility and cannot alone prove A4.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Learned behavior:** Extract and cite evidence that supports an answer, reducing unsupported guesses and incorrect citations.
- **Main limitation:** Reward-model co-evolution and answer-correctness coupling create outcome leakage. The evidence reward may reflect query overlap, formatting, or answer familiarity rather than independent support. Cross-source authority, conflict, freshness, and adversarial verification are not tested.
- **Reward-hacking risk:** Medium-High and not directly stress-tested; the policy may optimize the verifier’s surface cues or citation format.
- **Decision:** Include Core, but below S2G-RAG and RLSeek as a pure A4 anchor because the evidence reward is coupled to final outcome reward.

---

## Evidence synthesis

The six papers establish four different evidence-quality signals:

1. **Relevance routing:** CRAG scores query–document relevance and routes corrective retrieval.
2. **Claim support and contradiction:** RAGTruth and RAGChecker provide span/claim-level unsupported, entailment, faithfulness, and noise diagnostics; RLSeek turns support/refutation and citation fidelity into a learned policy behavior.
3. **Set-level sufficiency and gaps:** S2G-RAG explicitly judges whether the accumulated context is sufficient and what information is missing.
4. **Evidence utility:** EAPO optimizes an evidence utility reward inside GRPO, but the signal is coupled to answer correctness and a learned reward model.

The current evidence chain is therefore:

> **Evidence Quality Signal → Evaluation or Training → Verification Behavior → Generalization**

The evidence-quality signals are partly developed, but the concepts must remain separate. Relevance is not support. Support is not sufficiency. A source can be relevant without entailing a claim; multiple individually weak passages can jointly form sufficient evidence; and a supported passage can still come from an unreliable or outdated source.

The core learning papers provide limited direct A4.2 coverage and lack independent human ground truth for set-level sufficiency, multi-source conflict attribution, source provenance, temporal freshness, and adversarial or fabricated evidence. Complementary studies measure authority, pair-level sufficiency, Web conflict organization, counterfactual convincingness, and typed conflicts. Learned methods still show narrow verification behaviors and weak cross-domain/tool/time transfer.

## Cross-paper synthesis

The A4 collection now has a compact twelve-paper map with representative measurement anchors for all four sub-directions. A4.1 has the strongest training evidence; A4.2 and A4.4 now have stronger counterfactual and typed-conflict evaluation resources but limited direct post-training evidence; A4.3 has both process supervision and independent pair-level sufficiency measurement, but limited set-level ground truth. Further work would be most informative when it tests controlled perturbations, held-out verifier robustness, or direct cross-environment post-training transfer.

The strongest bounded conclusion is:

> Measurement has moved beyond relevance toward convincingness, source cues, sufficiency, and typed conflict handling. This does not yet establish a generalizable post-trained policy that separates relevance from reliability or resolves unseen conflicts under source and time shifts.
