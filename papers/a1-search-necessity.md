# A1 — Search Necessity and Epistemic Awareness

**Round 2, first sub-round.** This pool asks a narrow question:

> Can an LLM identify when its parametric knowledge is insufficient and decide whether external information is worth acquiring?

The pool deliberately excludes generic RAG papers and most Search-RL variants. The five core papers below are selected because they provide a distinct signal or evaluation protocol for retrieval necessity, knowledge boundaries, or self-aware uncertainty.

The follow-up sub-pool on the stricter question of whether information acquisition is worth its cost is maintained separately in [`a1-retrieval-utility.md`](./a1-retrieval-utility.md).

## Coding focus

For A1, distinguish four related but non-identical targets:

1. **Answerability-based:** whether the model can answer independently, including known versus unknown and answerable versus unanswerable.
2. **Uncertainty-based:** whether confidence, entropy, hidden states, or sampling variation indicate that external information may be needed.
3. **Complexity-based:** whether the task is complex enough to justify deeper or iterative retrieval; this is a useful routing baseline but is not automatically epistemic awareness.
4. **Utility-based:** whether retrieval is likely to improve the answer enough to justify its cost.
5. **Calibration:** whether the decision signal tracks actual answerability, downstream retrieval benefit, and behavior transfer.

A paper can improve retrieval triggering without proving that the LLM has a calibrated knowledge boundary. It can also improve final QA accuracy without learning any of these behaviors.

---

## Core papers

### A1-1. When to Retrieve: Teaching LLMs to Utilize Information Retrieval Effectively

- **Authors / year:** Tiziano Labruna, Jon Ander Campos, Gorka Azkune, RANLP 2025
- **Links:** [ACL Anthology](https://aclanthology.org/2025.ranlp-1.73/) · [code](https://github.com/tLabruna/Adapt-LLM) · [arXiv preprint](https://arxiv.org/abs/2404.19705)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A6 Adaptive Search Control
- **Training mechanism:** B1 Data / Trajectory Construction; B2 Behavior Learning
- **Learning target:** A two-stage LLM behavior: answer directly when its parametric knowledge is sufficient, otherwise emit a special `<RET>` token that invokes retrieval.
- **Supervision / reward:** Supervised labels are constructed from whether a base Llama-2 answer is correct without retrieval; incorrect examples are taught to emit `<RET>`, and retrieved-context examples are taught to answer from the context. No RL reward is used.
- **Training–capability alignment:** **Direct, but imperfect.** The `<RET>` target directly represents the claimed retrieval-necessity behavior. However, base-model answer correctness is only a proxy for true information need and does not encode uncertainty, expected information value, or retrieval cost.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Strong`; `Generalization Evidence: Limited`.
- **Generalization regime:** C5 Environment OOD and C6 Behavior Transfer are only weakly tested: training on NQ/SQuAD and testing on PopQA remains within open-domain factual QA and uses a similar retriever interface.
- **What it contributes:** It provides one of the clearest examples of explicit retrieval-necessity supervision. The model is evaluated not only by final answer accuracy but also by retrieval usage, differences between retrieved and non-retrieved answers, and behavior on a held-out QA dataset.
- **Main limitation:** The label conflates knowledge gaps with ordinary generation errors. It also uses a single Contriever passage and does not measure whether retrieval was actually useful enough to justify its cost. Cross-model, cross-domain, cross-tool, temporal, and budget transfer remain untested.
- **Research opportunity:** Replace answer-correctness pseudo-labels with counterfactual labels that measure `answer_without_retrieval`, `answer_with_retrieval`, evidence quality, and acquisition cost separately.

### A1-2. KBM: Delineating Knowledge Boundary for Adaptive Retrieval in Large Language Models

- **Authors / year:** Zhen Zhang et al., 2024 preprint; Findings of EMNLP 2025 version
- **Links:** [arXiv](https://arxiv.org/abs/2411.06207) · [Findings of EMNLP](https://aclanthology.org/2025.findings-emnlp.1188/)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A6 Adaptive Search Control
- **Training mechanism:** B1 Data / Trajectory Construction; B2 Behavior Learning
- **Learning target:** A Knowledge Boundary Model that predicts whether a target LLM knows an answer well enough to avoid retrieval.
- **Supervision / reward:** Synthetic or weak labels are built from sampled answer accuracy, response consistency, and entropy-like uncertainty signals. The model is SFT-trained to classify known versus unknown and route unknown cases to RAG.
- **Training–capability alignment:** **Partial.** The target is explicitly a knowledge-boundary decision, but the labels are generated by the model and an evaluator rather than by human or oracle counterfactual information value. The method does not directly train evidence evaluation or cost-sensitive retrieval utility.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Generalization regime:** Reports multiple QA settings, dynamic and long-tail knowledge, and use with different target LLMs. This is useful C2 Domain OOD / C5 Environment OOD / C6 Behavior Transfer evidence, but not full Tool OOD or broad task transfer.
- **What it contributes:** It separates the retrieval decision from the generator and makes the target model’s knowledge boundary an explicit prediction problem. It also contrasts accuracy-derived and uncertainty-derived labels rather than treating every confidence score as equivalent.
- **Main limitation:** The boundary model is target-model-dependent, and weak labels inherit errors from generation and evaluation. The experiments remain primarily QA- and Google-search-centric, without systematic tool, budget, temporal-environment, or evidence-quality tests.
- **Research opportunity:** Study whether one boundary model can transfer across model families, retrieval backends, domains, and changing corpora, while retaining calibration and cost sensitivity.

### A1-3. DRAGIN: Dynamic Retrieval Augmented Generation based on the Real-time Information Needs of Large Language Models

- **Authors / year:** Weihang Su et al., ACL 2024
- **Links:** [ACL Anthology](https://aclanthology.org/2024.acl-long.702/) · [code](https://github.com/oneal2000/DRAGIN) · [arXiv preprint](https://arxiv.org/abs/2403.10081)
- **Role:** Core
- **Contribution type:** Retriever Optimization / inference-time control
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A2 Information Need Formulation; A6 Adaptive Search Control
- **Training mechanism:** No parameter training; inference-time heuristic control based on token entropy, attention, and real-time information need.
- **Learning target:** Not directly trained. The method controls when to retrieve and how to form a query during generation.
- **Supervision / reward:** No SFT or RL reward. Retrieval decisions are made by the RIND signal; query formation uses attention-based context selection.
- **Training–capability alignment:** **Not Applicable as a training method.** The design directly operationalizes retrieval necessity, but it does not establish that an LLM learned a transferable epistemic behavior.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Limited`.
- **Generalization regime:** Mostly C1 In-distribution, with limited C2 Task OOD and C4 Tool OOD through several QA tasks, models, and retriever comparisons.
- **What it contributes:** It connects A1 retrieval necessity to A6 dynamic control and A2 query formation. The method asks whether the current generation state contains enough information and triggers retrieval only when a signal crosses a threshold.
- **Main limitation:** Thresholds and attention-based features are hand-designed, and the paper lacks ground-truth necessity labels, calibration analysis, behavior transfer, or cost-sensitive value-of-information evaluation. It also assumes access to model internals and a fixed Wikipedia/BM25-style environment.
- **Research opportunity:** Use DRAGIN as an interpretable baseline for learning a calibrated trigger, then compare learned triggers against internal-uncertainty, answer-counterfactual, and cost-aware signals.

### A1-4. SeaKR: Self-aware Knowledge Retrieval for Adaptive Retrieval Augmented Generation

- **Authors / year:** Zijun Yao et al., ACL 2025
- **Links:** [ACL Anthology](https://aclanthology.org/2025.acl-long.1312/) · [code](https://github.com/THU-KEG/SeaKR) · [arXiv preprint](https://arxiv.org/abs/2406.19215)
- **Role:** Core
- **Contribution type:** Retriever Optimization / inference-time control
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A4 Evidence Evaluation; A5 Evidence Integration; A6 Adaptive Search Control
- **Training mechanism:** No parameter update; inference-time self-aware uncertainty is extracted from model hidden states and multiple sampled generations.
- **Learning target:** Not directly trained. The signal controls retrieval triggering, passage reranking, and multi-step reasoning selection.
- **Supervision / reward:** No explicit labels or RL reward. A regularized hidden-state uncertainty signal is calibrated on a development set and used to trigger retrieval and choose evidence.
- **Training–capability alignment:** **Direct at the control-signal level, but not a learned capability.** The signal is designed to represent uncertainty, but it is not directly validated against oracle information value or trained to transfer across models and environments.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.
- **Generalization regime:** C1 In-distribution and limited C2 Task OOD across several QA datasets; no strong C3 Tool OOD, C4 Environment OOD, or C6 Behavior Transfer.
- **What it contributes:** It introduces internal representation uncertainty as an A1 signal distinct from question complexity, answer correctness pseudo-labels, and output-token entropy. The same signal is also used for evidence selection and reasoning-path choice.
- **Main limitation:** It requires access to open-model hidden states and multiple generations, and uses threshold tuning. The experiments focus on short-answer QA with a fixed Wikipedia/BM25-style environment; they do not establish cross-domain calibration, temporal robustness, or transfer of the uncertainty behavior itself.
- **Research opportunity:** Evaluate whether hidden-state uncertainty predicts *marginal retrieval benefit* rather than merely answer uncertainty, and whether this remains calibrated after changing the retriever, corpus, model, or search budget.

### A1-5. Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home

- **Authors / year:** Viktor Moskvoretskii et al., ACL 2025
- **Links:** [ACL Anthology](https://aclanthology.org/2025.acl-long.319/) · [code](https://github.com/s-nlp/AdaRAGUE) · [arXiv preprint](https://arxiv.org/html/2501.12835v2)
- **Role:** Supporting / evaluation
- **Contribution type:** Evaluation
- **Primary capability:** Not Directly Trained; evaluates A1 Epistemic Awareness
- **Secondary capabilities:** A6 Adaptive Search Control
- **Training mechanism:** No LLM post-training. It compares uncertainty estimators, adaptive-retrieval pipelines, and post-hoc routing models.
- **Learning target:** A retrieval-necessity evaluator or router, not an LLM search policy.
- **Supervision / reward:** No policy reward. In-Accuracy without external knowledge is used as an operational target for whether retrieval is necessary; thresholds and classifiers are evaluated using QA, self-knowledge, and efficiency metrics.
- **Training–capability alignment:** **Partial.** The evaluation target is close to retrieval necessity, but In-Accuracy is still a downstream proxy and does not measure expected information value or evidence quality.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Strong`; `Generalization Evidence: Medium`.
- **Generalization regime:** C1 In-distribution, C2 Task OOD / cross-dataset transfer, and limited C6 Behavior Transfer through self-knowledge and retrieval-behavior metrics. It does not provide strong Tool OOD or Environment OOD evidence.
- **What it contributes:** It provides a useful evaluation anchor: final QA accuracy and self-knowledge accuracy can diverge sharply. Simple uncertainty estimators can be competitive with complex pipelines while using fewer retrieval calls, and cross-dataset transfer can preserve QA performance while degrading self-knowledge.
- **Main limitation:** The study uses one model family, one Wikipedia corpus, one BM25/Elasticsearch backend, and six QA datasets. The post-hoc classifier does not prove that the LLM learned epistemic awareness, and In-Accuracy inherits generator and label confounds.
- **Research opportunity:** Turn this evaluation setup into a causal benchmark with oracle counterfactual retrieval benefit, evidence quality, cost, calibration, and behavior-transfer annotations.

---

## Excluded from the A1 core set

### Adaptive-RAG: Learning to Adapt Retrieval-Augmented Large Language Models through Question Complexity

This paper is useful as a supporting routing baseline, but it is not part of the A1 core set. Its central signal is question complexity, which routes between no retrieval, single-step retrieval, and iterative retrieval. It does not directly measure a model’s knowledge boundary, uncertainty, or expected value of information. It should be revisited only when comparing task-complexity routing against genuine epistemic-awareness signals.

---

## A1 synthesis

The five papers form two decision-signal families plus a separate evaluation track:

1. **Answerability / weak-label signals:** When to Retrieve and KBM use answer correctness, consistency, or evaluator-derived labels to approximate whether the model knows enough.
2. **Internal uncertainty signals:** DRAGIN and SeaKR use entropy, attention, hidden states, or multi-sample variation to trigger retrieval at inference time.
3. **Evaluation track, not a decision-signal family:** Adaptive Retrieval Without Self-Knowledge separates final answer quality from self-knowledge and retrieval efficiency, making it possible to test whether a trigger is actually calibrated. It should not be grouped with the two signal families above.

The shared conclusion is not that one trigger is best. It is that **retrieval necessity is currently represented by proxies**. Existing signals usually measure answerability, uncertainty, or task complexity, but rarely measure the counterfactual quantity the survey ultimately cares about:

> How much will acquiring external information improve the answer or decision, and is that improvement worth the retrieval cost?

The principal A1 gap is therefore a **training–capability alignment gap**. A useful next benchmark should provide counterfactual or expert labels for answer-without-retrieval, answer-with-retrieval, evidence quality, expected utility, and cost. It should separately report trigger calibration, query quality, evidence integration, behavior transfer, and final task performance.

The final utility-aware policy-learning extension is recorded in [`a1-utility-policy-learning.md`](./a1-utility-policy-learning.md). A1 is now closed for broad collection; the next search stage is A4 Evidence Evaluation. Further A1 work should be added only if it introduces ex-ante calibration, causal counterfactual utility estimation, or cross-environment cost transfer.
