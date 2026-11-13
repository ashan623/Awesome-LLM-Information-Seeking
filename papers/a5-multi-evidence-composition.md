# A5 — Multi-Evidence Composition and Faithful Utilization

This note focuses on two complementary directions:

- **A5.2 Multi-evidence Composition:** whether the model can combine multiple passages, hops, or partial facts into a valid inference;
- **A5.3 Faithful Evidence Utilization:** whether the reasoning and conclusion genuinely depend on the evidence rather than merely improving final answer metrics or producing a plausible explanation afterward.

The paper **Belief-R: Belief Revision: The Adaptability of Large Language Models Reasoning** must not be confused with **BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression**. They are different papers; BRIEF is included below as an A5.2 anchor.

## Three-level Evidence Reliance framework

Each paper discussed here is coded at three levels:

- **Output Reliance:** after replacing key evidence, does the final answer change appropriately?
- **Compositional Reliance:** after deleting a necessary hop or passage, does the corresponding conclusion lose its basis?
- **Process-level Reliance:** does the intermediate reasoning actually depend on the key evidence rather than producing a plausible post-hoc rationale?

Input perturbations can establish behavioral sensitivity, but output changes alone do not prove causal faithfulness of the internal reasoning chain.

---

## B1. RAFT: Adapting Language Model to Domain Specific RAG

- **Authors / year:** Tianjun Zhang, Shishir G. Patil, Naman Jain, Sheng Shen, Matei Zaharia, Ion Stoica, Joseph E. Gonzalez, 2024.
- **Venue / links:** COLM 2024; [OpenReview](https://openreview.net/forum?id=rzQGHXNReU) · [arXiv](https://arxiv.org/abs/2403.10131) · [code](https://github.com/ShishirPatil/gorilla/tree/main/raft)
- **Role / contribution:** Core / Capability Learning
- **Primary sub-direction:** A5.2 Multi-evidence Composition
- **Secondary sub-direction:** A5.3 Faithful Evidence Utilization
- **Integration target:** Answer and Reasoning Chain
- **Training mechanism:** SFT of Llama 2 7B on golden/oracle documents, distractors, and quote-bearing CoT-style answer targets. A fraction of examples omits the golden document to encourage domain knowledge use when context is incomplete.
- **Training–capability alignment:** Partial. The targets encode distractor filtering, quote use, and multi-document answering, but next-token SFT over generated CoT does not supervise necessary-hop composition or causal process faithfulness.
- **Output Reliance:** Limited and indirect. Answer/API gains and distractor-count robustness suggest useful context processing, but there is no matched key-evidence substitution or deletion test.
- **Compositional Reliance:** Limited. HotpotQA and golden-document settings include multiple documents and partial facts, but no necessary-hop leave-one-out experiment tests whether a corresponding conclusion fails.
- **Process-level Reliance:** Not Tested. Quote spans and CoT are surface targets without independent citation correctness, internal attribution, or chain-level causal evaluation.
- **Controlled perturbation:** Distractors and retrieved-document count are varied. Key evidence deletion, evidence substitution, irrelevant-evidence invariance, evidence order, source replacement, temporal shift, and conflict evidence are not tested at evaluation time.
- **Parametric–External Balance:** Limited and asymmetric. Gold-absent training encourages domain memorization, but the work does not test correction of a wrong prior or resistance to false external evidence.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Limited`.
- **Main finding:** RAFT is a genuine post-training anchor for evidence-conditioned domain RAG and multi-document output construction. It does not yet show that a learned reasoning process faithfully depends on particular evidence.
- **Main limitation:** Generated CoT can be post-hoc or teacher-forced, and gold-absent data can improve memorization rather than external evidence use. Held-out behavior transfer and process-level attribution are absent.
- **Decision:** Include Core.
- **Metadata note:** RAFT is ambiguous. This entry is the RAG paper; a separate EMNLP 2024 paper titled “RAFT: Realistic Attacks to Fool Text Detectors” is not part of A5.

## B2. BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression

- **Authors / year:** Yuankai Li, Jia-Chen Gu, Di Wu, Kai-Wei Chang, Nanyun Peng, 2025.
- **Venue / links:** Findings of NAACL 2025; [ACL Anthology](https://aclanthology.org/2025.findings-naacl.301/) · [arXiv](https://arxiv.org/abs/2410.15277) · [code](https://github.com/JasonForJoy/BRIEF/tree/main/BRIEF)
- **Role / contribution:** Core / Capability Learning
- **Primary sub-direction:** A5.2 Multi-evidence Composition
- **Secondary sub-direction:** A5.3 Faithful Evidence Utilization
- **Integration target:** Answer through a compressed evidence summary; no explicit reasoning-chain faithfulness is evaluated.
- **Training mechanism:** SFT of a T5-large compressor. Synthetic multi-hop examples require different documents to provide helpful propositions for different hops. The downstream reader remains frozen.
- **Training–capability alignment:** Partial for A5.2. The target explicitly represents multi-document evidence composition, but proposition selection is based on frozen-reader answer likelihood rather than faithful reasoning or causal attribution.
- **Output Reliance:** Limited / not directly tested. The paper compares no-document, compressed-document, and full-document conditions, but does not delete or replace a known key passage and test the appropriate answer change.
- **Compositional Reliance:** Limited. The training construction requires cross-document hop coverage, and the method compresses multiple propositions into a summary. There is no necessary-hop deletion or leave-one-out test.
- **Process-level Reliance:** Not Tested. The compressor and reader produce a summary and answer without chain-of-thought, attribution, or process-level causal evaluation.
- **Controlled perturbation:** Top-5 to top-25 context size, compression budget, and multiple readers are tested. The work does not systematically control evidence order, conflict, source replacement, temporal shift, or noise ratio.
- **Parametric–External Balance:** Not Tested. There is no parametric-wrong/external-true versus parametric-right/external-false condition.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.
- **Main finding:** BRIEF learns to select and concatenate query-relevant propositions from multiple documents, achieving strong compression and useful downstream multi-hop QA performance. This establishes efficient evidence-conditioned composition behavior, not process-level faithful utilization.
- **Main limitation:** The reader-likelihood objective may learn reader-specific shortcuts. Results are mainly Wikipedia-based and do not establish conflict handling, source reliability, temporal transfer, or causal evidence dependence.
- **Decision:** Include Core.

## B3. CARE: Improving Context Fidelity via Native Retrieval-Augmented Reasoning

- **Authors / year:** Suyuchen Wang, Jinlin Wang, Xinyu Wang, Shiqi Li, Xiangru Tang, Sirui Hong, Xiao-Wen Chang, Chenglin Wu, Bang Liu, 2025.
- **Venue / links:** EMNLP 2025 main conference; [ACL Anthology](https://aclanthology.org/2025.emnlp-main.1075/) · [arXiv](https://arxiv.org/abs/2509.13683) · [code](https://github.com/FoundationAgents/CARE)
- **Role / contribution:** Core / Capability Learning
- **Primary sub-direction:** A5.3 Faithful Evidence Utilization
- **Secondary sub-direction:** A5.2 Multi-evidence Composition
- **Integration target:** Reasoning Chain and Answer
- **Training mechanism:** LoRA SFT on reasoning chains containing explicit `<retrieval>` spans, followed by GRPO/RL curriculum training. The reward is `0.7 R_acc + 0.1 R_fmt + 0.2 R_ret`, where `R_acc` is final-answer token F1, `R_fmt` checks structure, and `R_ret` checks whether emitted spans occur in the context.
- **Training–capability alignment:** Partial. Explicit span supervision and context-match reward are closer to A5.3 than final-answer-only training, but the dominant answer reward and loose span-existence check do not supervise evidence necessity, entailment, or causal process reliance.
- **Output Reliance:** Limited-to-Medium. Counterfactual context evaluation suggests greater output conditioning on supplied text, but there is no matched key-fact replacement or deletion protocol.
- **Compositional Reliance:** Limited. CARE interleaves retrieved spans in multi-hop reasoning and improves HotpotQA, 2WikiMQA, and MuSiQue, but does not delete necessary hops or test factorial evidence dependency.
- **Process-level Reliance:** Limited, not established. Retrieval markers and context-match rewards make the process more evidence-oriented, but there is no intermediate causal attribution or post-hoc rationale control.
- **Controlled perturbation:** CofCA supplies counterfactual contexts that conflict with parametric knowledge. Key evidence deletion, substitution, irrelevant-evidence invariance, evidence order, controlled noise, source replacement, and temporal change are not systematically tested.
- **Parametric–External Balance:** Limited. CofCA tests adherence to counterfactual context, but not both directions of parametric–external conflict or calibrated belief revision.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.
- **Main finding:** CARE is a genuine SFT+RL method for learned evidence-conditioned reasoning behavior. It improves output accuracy and context matching, but the evidence does not prove process-level faithful utilization.
- **Main limitation:** The retrieval reward can be satisfied by a span that merely appears in context. Because answer F1 dominates the reward, the policy may optimize final correctness or surface evidence display rather than necessary evidence use.
- **Decision:** Include Core.

## B4. MIRAGE: Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation

- **Authors / year:** Jirui Qi, Gabriele Sarti, Raquel Fernández, Arianna Bisazza, 2024.
- **Venue / links:** EMNLP 2024 main conference; [ACL Anthology](https://aclanthology.org/2024.emnlp-main.347/) · [arXiv](https://arxiv.org/abs/2406.13663) · [code](https://github.com/Betswish/MIRAGE)
- **Role / contribution:** Core / Inference-time Method and Evaluation
- **Primary sub-direction:** A5.3 Faithful Evidence Utilization
- **Integration target:** Answer
- **Training mechanism:** No parameter update. CTI compares predictions with and without context; CCI uses gradient-based contrastive feature attribution to connect answer tokens to context tokens/documents. Human attribution examples calibrate thresholds but do not train the generator.
- **Training–capability alignment:** Not Applicable for post-training. The diagnostic signal is directly related to context influence but is not converted into an evidence-use learning objective.
- **Output Reliance:** Limited. CTI identifies context-sensitive answer tokens, but does not re-generate answers after deleting or replacing key documents.
- **Compositional Reliance:** Not Tested. The study does not test multi-hop composition or necessary evidence removal.
- **Process-level Reliance:** Medium but not proof of faithfulness. CCI is closer to the generation process than NLI or self-citation, yet remains post-hoc saliency and lacks causal chain validation.
- **Controlled perturbation:** Full-context versus no-context prediction comparisons and context-sensitivity analyses are used. Key deletion, substitution, irrelevant invariance, multi-evidence dependency, order shifts, source replacement, temporal change, and conflict are not systematically tested.
- **Parametric–External Balance:** Not Tested. MIRAGE does not test correction of wrong parametric knowledge or resistance to false external evidence.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Main finding:** MIRAGE provides a useful internal attribution diagnostic for identifying which documents influence answer tokens, with human-annotated and proxy evaluations. It does not train a new A5 capability and does not establish multi-evidence composition.
- **Main limitation:** Gradient and contrastive attribution depends on modeling assumptions and thresholding. TRUE NLI is used as a proxy in part of the evaluation, and no necessary-evidence counterfactual re-generation is performed.
- **Decision:** Include Core as an A5.3 diagnostic anchor, not a post-training method.

## B5. TRACE the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation

- **Authors / year:** Jinyuan Fang, Zaiqiao Meng, Craig MacDonald, 2024.
- **Venue / links:** Findings of EMNLP 2024; [ACL Anthology](https://aclanthology.org/2024.findings-emnlp.496/) · [arXiv](https://arxiv.org/abs/2406.11460) · [code](https://github.com/jyfang6/trace)
- **Role / contribution:** Core / Inference-time Method
- **Primary sub-direction:** A5.2 Multi-evidence Composition
- **Secondary sub-direction:** A5.3 Faithful Evidence Utilization
- **Integration target:** Reasoning Chain
- **Training mechanism:** No parameter update. Frozen models construct document-grounded triples, rank and select them, and use beam search to compose 2–4-hop chains before answer generation.
- **Training–capability alignment:** Not Applicable for post-training. The prompt and chain construction directly target A5.2 composition, but no learned policy or process reward is introduced.
- **Output Reliance:** Medium behavioral reliance. Removing the constructor, KG generator, selector, or chain representation lowers QA performance, but no single key fact is replaced or deleted in a matched re-generation test.
- **Compositional Reliance:** Medium but indirect. Autoregressive chains of linked triples and constructor-versus-top-T ablations provide the strongest composition behavior in this set, but necessary-hop leave-one-out is absent.
- **Process-level Reliance:** Not Established. The chain is generated before the answer and is structurally grounded in retrieved documents, but there is no independent chain-quality annotation, internal attribution, causal chain intervention, or answer-without-chain counterfactual.
- **Controlled perturbation:** Natural document error rates, all-document versus selected-chain contexts, and component substitutions are examined. Key deletion, evidence substitution, irrelevant invariance, order shift, conflict, source replacement, and temporal change are not tested.
- **Parametric–External Balance:** Not Tested. No bidirectional parametric–external conflict or belief revision protocol is included.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Main finding:** TRACE explicitly composes multi-hop evidence chains and improves QA over all-document RAG across three Wikipedia multi-hop datasets. It is the strongest indirect A5.2 behavior anchor in this set, but not proof of faithful process-level reasoning.
- **Main limitation:** Results are ultimately judged mainly by EM/F1, use Wikipedia evidence, and lack independent chain correctness, cross-domain transfer, order robustness, conflict handling, and necessary-hop interventions.
- **Decision:** Include Core.

---

## Synthesis

### Strict necessary-evidence tests remain absent

None of the five papers completes the strongest proposed intervention: delete or replace one necessary piece of evidence, re-run reasoning, and verify that the corresponding conclusion fails or changes appropriately. RAFT varies distractors and document counts; BRIEF varies compression and context size; CARE uses counterfactual contexts; MIRAGE compares context-sensitive predictions; TRACE performs chain and component ablations. These are useful, but none is a matched necessary-evidence leave-one-out protocol.

### Composition evidence is behavioral and indirect

TRACE provides the strongest indirect composition evidence because it explicitly constructs 2–4-hop linked chains and component replacement degrades QA. BRIEF represents cross-document hop coverage in synthetic training targets. RAFT and CARE show multi-document or multi-hop answer behavior in the presence of distractors or context conflicts. However, no paper isolates whether every required hop is necessary for the final conclusion.

### Training signals remain only partially aligned

RAFT and BRIEF use SFT to teach evidence selection, quote-bearing answers, proposition compression, and cross-document coverage. CARE combines supporting-fact SFT with a retrieval-span reward and GRPO. These signals are closer to A5.2/A5.3 than a final-answer-only objective, but they still do not directly supervise necessary evidence, step-level entailment, conflict handling, or causal process reliance.

MIRAGE and TRACE are valuable for diagnosis and inference-time evidence organization, but neither updates parameters. MIRAGE’s internal attribution is closer to the generation process than NLI or self-citation, yet gradient/contrastive saliency remains post-hoc and cannot by itself establish faithful internal reasoning.

### Evidence Reliance hierarchy

Across this set, the evidence is generally strongest at **Output Reliance**, weaker at **Compositional Reliance**, and weakest at **Process-level Reliance**. CARE supplies limited process-oriented training signals; MIRAGE supplies a diagnostic signal related to model internals; TRACE supplies a structured intermediate chain. None provides a complete causal process-faithfulness test.

### Generalization remains incomplete

RAFT provides multi-domain and distractor variation; BRIEF tests compression budgets and reader transfer; CARE tests multiple tasks, models, and limited parametric-conflict conditions; MIRAGE tests several tasks and models; TRACE tests several readers over Wikipedia multi-hop QA. None systematically establishes stable evidence dependence across domain, document order, controlled noise ratio, source replacement, temporal change, and bidirectional parametric–external conflict.

## A5.1–A5.3 Evidence Summary

Across the A5.1–A5.3 evidence map, the six belief-revision studies, five composition/use studies, and two diagnostics in [`a5-faithfulness-and-attribution.md`](./a5-faithfulness-and-attribution.md) support the following boundary:

> The current representative literature does not yet provide strict evidence that post-training teaches necessary multi-evidence dependence or process-level faithful evidence utilization that survives controlled perturbations and environment changes.

This does **not** mean that models cannot use evidence, or that existing methods provide no value. The positive evidence supports useful evidence-conditioned answer behavior, multi-document composition heuristics, structured reasoning chains, context-fidelity training, and increasingly precise factuality/faithfulness and attribution diagnostics. The unresolved issue is whether these behaviors are causal, calibrated, content-based, and transferable rather than benchmark-specific, metadata-driven, or reward-specific.

A5.4 Cross-turn Information Persistence & State Updating is a bounded subdirection within A5 Evidence Integration. The six-paper pool is documented in [`a5.4-memory-updating.md`](./a5.4-memory-updating.md). The targeted factuality/faithfulness and attribution-bias supplement is documented in [`a5-faithfulness-and-attribution.md`](./a5-faithfulness-and-attribution.md).
