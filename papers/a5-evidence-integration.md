# A5 — Evidence Integration

The second batch focuses on A5.2/A5.3 and is documented in [`a5-evidence-integration-second.md`](./a5-evidence-integration-second.md). Together, the two batches form the current eleven-paper A5.1–A5.3 map.

## Boundary with A4

A4 asks whether acquired evidence is relevant, supportive, sufficient, credible, or contradictory. A5 asks whether the model **faithfully integrates** evidence that has already been acquired and evaluated into reasoning, beliefs, generation, and decisions.

The first A5 batch uses three distinctions:

- **Evidence Access:** the evidence appears in the context.

- **Evidence Utilization:** the output changes appropriately because of the evidence.

- **Belief Update:** the model revises an internal answer or state when new evidence warrants revision, while resisting false or unreliable evidence.

Context presence, citation format, and final-answer improvement are not sufficient evidence of A5 capability acquisition.

## First-round scope

This round covers A5.1–A5.3:

- **A5.1 Knowledge Integration & Belief Revision:** correcting wrong parametric knowledge without blindly accepting false external evidence.

- **A5.2 Multi-evidence Composition:** combining multiple passages, hops, or partial facts into a valid inference.

- **A5.3 Faithful Evidence Utilization:** ensuring that the reasoning and conclusion genuinely depend on the supplied evidence.

A5.4 **Persistent Information Integration & Memory Updating** is deferred to the dedicated Memory and State Update stage. Its scope is learned selective writing, revision, forgetting, and reuse across interactions, not a general survey of all agent-memory architectures. The first six-paper A5.4 pool is documented in [`a5.4-memory-updating.md`](./a5.4-memory-updating.md).

Every entry records the following A5-specific fields:

- **Integration Target:** Answer, Reasoning Chain, Belief, Memory, or Decision.

- **Evidence Reliance:** split into `Output Reliance` (does the answer change appropriately after key evidence is replaced?), `Compositional Reliance` (does deleting a necessary hop remove the corresponding basis?), and `Process-level Reliance` (does the reasoning process itself depend on the key evidence rather than producing a plausible post-hoc explanation?).

- **Parametric–External Balance:** whether the model can correct a wrong prior, reject false evidence, and explain conflicts rather than following either source mechanically.

---

## A5.1 Knowledge Integration and Belief Revision

### A5-1. ClashEval: Quantifying the Tug-of-War between an LLM’s Internal Prior and External Evidence

- **Authors / year:** Kevin Wu, Eric Wu, James Zou, 2024; NeurIPS 2024 Datasets and Benchmarks Track.

- **Links:** [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2024/hash/3aa291abc426d7a29fb08418c1244177-Abstract-Datasets_and_Benchmarks_Track.html) · [arXiv](https://arxiv.org/abs/2404.10198) · [code](https://github.com/kevinwu23/StanfordClashEval)

- **Role / contribution:** Core / Evaluation

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capability:** A5.3 answer-level evidence reliance

- **Integration target:** Answer

- **Training mechanism:** Evaluation-only. A token-probability calibration heuristic is applied at inference time; no SFT, DPO, RL, or joint-decoding training is performed.

- **Evidence reliance:** Strongest direct answer-level probe in this batch. The answer-bearing fact in context is replaced with true, subtly false, strongly false, or absurd values, so the benchmark measures whether the answer changes appropriately. It does not test key-evidence deletion, evidence order, irrelevant-evidence invariance, or causal reasoning-chain reliance.

- **Parametric–external balance:** Explicit and bidirectional. It tests correct prior / false context and wrong prior / correct context separately. It measures answer switching or retention, but not an explicit belief state or conflict explanation.

- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Strong`; `Generalization Evidence: Medium`.

- **Main finding:** Models can revise a wrong prior when context is correct, but also frequently adopt false retrieved facts when their own prior is correct. Calibration improves answer arbitration at inference time but trades lower context bias for higher prior bias.

- **Main limitation:** Mostly single-fact QA; no multi-hop composition, reasoning-chain faithfulness, realistic source quality, learned intervention, or held-out behavior transfer.

- **Decision:** Include Core as a measurement anchor, not as post-training evidence.

### A5-2. FaithfulRAG: Fact-Level Conflict Modeling for Context-Faithful Retrieval-Augmented Generation

- **Authors / year:** Qinggang Zhang et al., ACL 2025.

- **Links:** [ACL Anthology](https://aclanthology.org/2025.acl-long.1062/) · [arXiv](https://arxiv.org/abs/2506.08938) · [code](https://github.com/XMUDeepLIT/Faithful-RAG)

- **Role / contribution:** Core / Inference-time Method

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capability:** A5.3 Faithful Evidence Utilization

- **Integration target:** Answer and Reasoning Chain

- **Training mechanism:** Inference-time self-fact mining, embedding-based context alignment, conflict reconciliation, and step-by-step prompting. The base model parameters are unchanged.

- **Evidence reliance:** Medium at answer level. The evaluation includes entity/fact replacement, golden versus negative contexts, and irrelevant/unanswerable cases. It lacks key-passage deletion, order manipulation, matched causal interventions, and independent chain-level faithfulness.

- **Parametric–external balance:** Medium but non-orthogonal. The method externalizes parametric self-facts and reports over-confidence versus incorrect-match errors, but it does not provide a fully controlled parametric-right/external-false versus parametric-wrong/external-true protocol.

- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.

- **Main finding:** Structured conflict handling improves answer accuracy and reduces some over-confidence and incorrect-match errors across FaithEval, RealtimeQA, MuSiQue, and SQuAD. This demonstrates useful inference-time integration, not learned or persistent belief revision.

- **Main limitation:** Output-level ACC, Memorization Ratio, and heuristic error categories do not establish causal reasoning dependence on evidence. Cross-domain, tool, budget, temporal, and source-quality transfer are not tested.

- **Decision:** Include Core as an inference-time A5.1/A5.3 anchor.

### A5-3. Belief-R: Belief Revision: The Adaptability of Large Language Models Reasoning

- **Authors / year:** Bryan Wilie et al., 2024; EMNLP 2024.

- **Links:** [ACL Anthology](https://aclanthology.org/2024.emnlp-main.586/) · [arXiv](https://arxiv.org/abs/2406.19764) · [code](https://github.com/HLTCHKUST/belief-revision)

- **Role / contribution:** Evaluation / Supporting

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capability:** A5.3 output-level evidence utilization

- **Integration target:** Belief, operationalized as final Answer

- **Training mechanism:** Evaluation-only. Direct prompting, CoT, and Plan-and-Solve are compared; model parameters, beliefs, and memory are not trained.

- **Evidence reliance:** Medium but narrow. Delta-R holds two premises fixed and adds a third premise labelled for update or maintenance. BU-Acc, BM-Acc, and BREU separate change from stability. There is no evidence replacement, deletion, order manipulation, false external evidence, or reasoning-chain intervention.

- **Parametric–external balance:** Not tested. Synthetic premises are task facts rather than controlled conflicts with known parametric knowledge.

- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.

- **Main finding:** Models often fail to update when new conditions warrant revision or lose maintenance accuracy when they update too aggressively. CoT and Plan-and-Solve yield only marginal, model-dependent improvements.

- **Main limitation:** Synthetic ATOMIC-derived commonsense patterns confound logical competence, knowledge recall, and instruction following. Final multiple-choice accuracy cannot establish faithful evidence use or real-world belief revision.

- **Decision:** Include Supporting as a belief-revision diagnostic, not as a learned integration method. This paper is **Belief-R**, distinct from **BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression**, which is a separate A5.2 candidate for the second batch.

## A5.2 Multi-evidence Composition and A5.3 Faithful Utilization

### A5-4. GuarantRAG: Guaranteeing Knowledge Integration with Joint Decoding for Retrieval-Augmented Generation

- **Authors / year:** Zhengyi Zhao et al., ACL 2026.

- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.192/) · [arXiv](https://arxiv.org/abs/2604.08046) · [research page](https://research.cuhk.edu.hk/en/publications/guaranteeing-knowledge-integration-with-joint-decoding-for-retrie/)

- **Role / contribution:** Supporting / Capability Learning

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capabilities:** A5.2 Multi-evidence Composition; A5.3 Faithful Evidence Utilization

- **Integration target:** Answer and Reasoning Chain

- **Training mechanism:** Contrastive DPO trains a Refer-Answer evidence model; auxiliary supervised/distilled training supports retrieval decisions; segment-level hidden-state joint decoding fuses retrieved evidence with the base generator.

- **Evidence reliance:** Limited-to-Medium. The method injects retrieved segments and tests irrelevant-document noise, but does not perform controlled key-evidence deletion/replacement, order variation, answer-without-evidence counterfactuals, or chain-level causal attribution.

- **Parametric–external balance:** Limited and asymmetric. Inner-Answer and Refer-Answer explicitly represent parametric and retrieved content, but DPO prefers the retrieved answer when they differ. The evaluation does not test false external evidence against a correct prior, so calibrated revision is not established.

- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.

- **Main finding:** Across NQ, TruthfulQA, Wizard-of-Wikipedia, HotpotQA, and ELI5, the method reports better answer metrics, lower hallucination, higher entity precision, and maintained structure across several retrievers and model scales. These results support improved evidence-conditioned generation and composition, but not causal faithful utilization.

- **Main limitation:** The DPO negative treats the parametric Inner-Answer as rejected even when it may be correct. The method may encourage external obedience or evidence copying. It has no public implementation link and no persistent belief or memory update.

- **Decision:** Include Supporting; do not treat as a definitive A5.3 verifier.

### A5-5. Efficient Prior-Guided Reasoning for Robust Retrieval-Augmented Generation under Conflicts

- **Authors / year:** Xiaowei Yuan et al., ACL 2026.

- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1013/) · [paper PDF](https://aclanthology.org/2026.acl-long.1013.pdf)

- **Role / contribution:** Core / Capability Learning

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capability:** A5.2 Multi-evidence Composition

- **Integration target:** Answer and Reasoning Chain

- **Training mechanism:** GRPO/RLVR response-policy optimization with answer-F1 reward, plus Bernoulli-gated prior dropout that transfers explicit prior-guided behavior into one-turn inference.

- **Evidence reliance:** Medium for conflict/noise robustness. The study varies noisy or conflicting passages, retrieval depth, no-gold retrieval, all-incorrect contexts, and multi-document settings. It lacks leave-one-out key deletion, order swaps, irrelevant-stability tests, attribution, and step-level evidence-use rewards.

- **Parametric–external balance:** Medium and partial. Gold-document cases expose harmful prior bias, while all-incorrect retrieval tests reliance on the prior. There is no systematic parametric-wrong/external-true counterfactual.

- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.

- **Main finding:** The trained policy is more robust to noisy or conflicting retrieval and can preserve answers under all-incorrect contexts. It also shows that explicit two-turn prior-guided reasoning remains stronger than one-turn latent reasoning on harder multi-hop tasks.

- **Main limitation:** The reward is format multiplied by final answer F1. Improvements may come from answer shortcuts or stronger parametric priors rather than faithful evidence use. There are no key-evidence interventions, source attribution, or public code link.

- **Decision:** Include Core as an early A5.1 post-training anchor, with limited A5.3 evidence.

### A5-6. How Large Language Models Balance Internal Knowledge with User and Document Assertions

- **Authors / year:** Shuowei Li, Haoxin Li, Wenda Chu, Yi Fang, Findings of ACL 2026.

- **Links:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.1267/) · [arXiv](https://arxiv.org/abs/2604.22193) · [code](https://github.com/shuowl/llm-source-balancing)

- **Role / contribution:** Core / Evaluation plus a secondary capability-learning intervention

- **Primary capability:** A5.1 Knowledge Integration & Belief Revision

- **Secondary capability:** A5.3 answer-level evidence reliance

- **Integration target:** Answer

- **Training mechanism:** A broad 27-model source-balancing probe, followed by mixed LoRA SFT on correct, incorrect, agreeing, conflicting, and order-varied assertions.

- **Evidence reliance:** Strongest answer/probability-level protocol in this batch together with ClashEval. It varies assertion correctness, source presence, source combination, source identity, and user-first/document-first order. It does not test irrelevant-evidence invariance or reasoning-chain causal reliance.

- **Parametric–external balance:** Explicitly measured with PAR+ (maintain a correct parametric answer against a false external assertion) and SDR+ (adopt a correct external assertion when the parametric answer is wrong). Mixed SFT improves both on the two fine-tuned models, but performance remains imperfect.

- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Strong`; `Generalization Evidence: Limited`.

- **Main finding:** Baseline models are mostly impressionable: they often accept helpful corrections but also accept harmful assertions. Mixed SFT makes the tested models more selective, but the task remains answer-level and synthetic.

- **Main limitation:** English multiple-choice CSQA and GSM8K-style assertions do not establish persistent belief revision, realistic retrieved-document integration, open-ended reasoning, or held-out target-behavior transfer.

- **Decision:** Include Core as a controlled A5.1 measurement and limited post-training anchor.

---

## First-batch synthesis

### A5.1 dominates the current evidence

All six papers touch A5.1, while only GuarantRAG and Efficient Prior-Guided Reasoning substantially address A5.2 through multi-document fusion or conflict handling. A5.3 is usually measured indirectly through answer accuracy, hallucination, entity precision, output probability, or robustness to noise. No paper in this batch provides a complete reasoning-level causal protocol.

### Controlled evidence changes are uneven

ClashEval and the source-balancing study provide the clearest Output Reliance probes. They manipulate the correctness of answer-bearing assertions and separately measure whether models should switch or retain an answer. Efficient Prior-Guided Reasoning adds noisy, conflicting, no-gold, and all-incorrect contexts. FaithfulRAG includes fact replacement and irrelevant contexts. GuarantRAG mainly tests irrelevant-document noise. Belief-R tests update versus maintenance after an additional premise. The second batch shows that Compositional Reliance and Process-level Reliance remain substantially less directly measured.

However, none combines all of the following in one controlled protocol: key-evidence deletion or leave-one-out, irrelevant-evidence invariance, evidence order variation, false external evidence, source-quality variation, and reasoning-level attribution. The current evidence therefore supports output sensitivity, not full causal faithfulness.

### Training evidence is narrower than method claims

Three papers perform genuine parameter-updating interventions:

- **GuarantRAG** trains an evidence model, retrieval-decision component, and fusion mechanism, but its DPO preference for retrieved content is asymmetric and may encourage external obedience.

- **Efficient Prior-Guided Reasoning** trains a response policy with GRPO/RLVR and prior dropout, but its answer-F1 reward cannot distinguish faithful integration from parametric shortcuts.

- **The source-balancing study** uses mixed LoRA SFT and improves answer-level acceptance of correct external assertions and rejection of incorrect ones, but does not supervise reasoning or attribution.

ClashEval, FaithfulRAG, and Belief-R demonstrate useful inference-time integration or evaluation behavior, not learned capability acquisition.

### Preliminary conclusion, not a field-wide claim

Within the selected A5 batches, **direct measurement evidence is richer for answer-level parametric–external conflict than for learned, reasoning-level, transferable evidence utilization**. This describes the evidence available in the selected representative papers; it is not a ranking of the models' actual capability strength and not a definitive conclusion about the entire field.

The three questions for the next batch are:

1. **Measurement:** Does the evaluation distinguish evidence following, evidence-based reasoning, and genuine belief revision?

1. **Learning:** Do SFT, DPO, or RL signals supervise evidence-dependent reasoning, or only final answers and source preferences?

1. **Generalization:** Does learned integration survive source replacement, evidence order changes, new domains, new tools, temporal change, and misleading external evidence?

A5.4 Persistent Information Integration & Memory Updating remains outside these A5.1–A5.3 batches. It should be studied with explicit memory-write correctness, state consistency, retrieval-time reuse, future utility attribution, and stale-memory correction tests.
