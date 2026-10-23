# A4 — Second Batch: Authority, Sufficiency, and Conflict

This second batch fills the main coverage gaps from the first A4 anchor pool. It does not change the four-layer taxonomy; it adds representative evaluation evidence for A4.2, A4.3, and A4.4.

## B1. AuthorityBench: Benchmarking LLM Authority Perception for Reliable Retrieval-Augmented Generation

- **Authors / year:** Zhihui Yao, Hengran Zhang, Keping Bi, 2026
- **Links:** [arXiv](https://arxiv.org/abs/2603.25092) · [code](https://github.com/Trustworthy-Information-Access/AuthorityBench)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary sub-direction:** A4.2 Source Reliability
- **Secondary sub-direction:** Authority comparison for domains and entities
- **Judgment target:** Source/domain authority, entity authority, and URL-level authority in pointwise, pairwise, and listwise ranking.
- **Ground truth quality:** Mixed but mostly weak proxies. Domain labels use PageRank-like scores; entity labels use Wikipedia/Wikidata sitelink popularity. RAGAuth contains author-curated questions and documents but does not report independent expert adjudication of authority.
- **Controlled perturbation:** Partial. It varies authority gaps, list composition, input format, label granularity, and webpage text. It does not systematically swap the same content across sources, control factual correctness independently, randomize order as a robustness factor, or test temporal changes.
- **Training mechanism:** Evaluation-only zero-shot judging; no SFT, RL, post-training, or reward optimization.
- **Learning target:** None. It evaluates prompted authority scoring/ranking and downstream URL filtering.
- **Evidence signal:** Domain/entity authority and URL-level source scores, not claim truth or evidence support.
- **A4 versus A5:** Authority ranking is A4.2. RAGAuth filtering and final QA are downstream evidence selection/use, not learned A5 behavior.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Observed behavior:** Models can recover some authority signal, often recognizing `.gov`/`.edu` and popular entities, but webpage text can hurt ranking.
- **Main limitation:** The benchmark may measure popularity, centrality, brand familiarity, or TLD bias rather than principled source reliability. Authority is not factual correctness.
- **Reward-hacking / shortcut risk:** Moderate-to-High. The authors observe over-scoring of familiar domains and social platforms; same-content source swaps and popularity controls are missing.
- **Decision:** Include Core as an A4.2 measurement anchor, not a training method.

## B2. Resolving Conflicting Evidence in Automated Fact-Checking: A Study on Retrieval-Augmented LLMs

- **Authors / year:** Ziyu Ge, Yuhao Wu, Daniel Wai Kit Chin, Roy Ka-Wei Lee, Rui Cao, IJCAI 2025
- **Links:** [IJCAI proceedings](https://www.ijcai.org/proceedings/2025/1073) · [arXiv](https://arxiv.org/abs/2505.17762) · [code](https://github.com/zoeyyes/CONFACT)
- **Role:** Core / Evaluation and method study
- **Contribution type:** Evaluation
- **Primary sub-direction:** A4.4 Conflict & Robustness
- **Secondary sub-direction:** A4.2 Source Reliability
- **Judgment target:** Claim veracity under support/refute evidence and different source-credibility conditions.
- **Ground truth quality:** Mixed. HumC uses human annotation and third-party adjudication for stance and source credibility; GT-MB uses MBFC-style expert source labels; some conflict construction and stance aggregation use GPT-4.
- **Controlled perturbation:** Limited. It filters naturally retrieved evidence for support/refute conflict, compares source filtering, credibility weighting, and source-background prompting, but does not factorially control conflict ratio, source replacement, evidence order, temporal change, or truth content.
- **Training mechanism:** Inference-time prompting, source filtering, credibility weighting, and source-background augmentation. An auxiliary credibility predictor is trained, but the claim verifier is not post-trained with SFT or RL.
- **Learning target:** Source-credibility prediction only; the final claim verdict is produced by a frozen LLM.
- **Evidence signal:** Separately collected source-credibility and support/refute signals, combined during fact-checking.
- **A4 versus A5:** Source credibility and support/refute are A4 variables. Final Yes/No claim accuracy is downstream fact-checking performance and cannot replace separate conflict-detection or conflict-attribution metrics.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Limited`; `Generalization Evidence: Limited`.
- **Observed behavior:** Source-aware prompting can improve some final verdicts, but effects are model- and split-dependent; credibility and relevance are not interchangeable.
- **Main limitation:** Conflict detection, source attribution, and conflict resolution are not separately scored. Static source labels, GPT-generated components, and same-distribution evaluation limit conclusions.
- **Reward-hacking / shortcut risk:** No RL reward hacking is tested. Risks include majority-source bias, popularity/authority bias, stale MBFC labels, and filtering away useful counter-evidence.
- **Decision:** Include Core as an A4.2/A4.4 evaluation anchor, with limited evidence of learned behavior.

## B3. Sufficient Context: A New Lens on Retrieval Augmented Generation Systems

- **Authors / year:** Hailey Joren, Jianyi Zhang, Chun-Sung Ferng, Da-Cheng Juan, Ankur Taly, Cyrus Rashtchian, ICLR 2025
- **Links:** [arXiv v3](https://arxiv.org/abs/2411.06037v3) · [Google Research](https://research.google/pubs/sufficient-context-a-new-lens-on-retrieval-augmented-generation-systems/) · [code](https://github.com/hljoren/sufficientcontext)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary sub-direction:** A4.3 Evidence Sufficiency
- **Judgment target:** Evidence Set / Query–Context pair. The label asks whether the context alone contains enough information for a plausible, definitive answer, without requiring a pre-specified gold answer.
- **Ground truth quality:** Stronger than title-coverage proxies but still mixed. A manually judged set of 115 query-context pairs provides independent human/expert sufficiency labels; larger-scale labels come from prompted Gemini or FLAMe autoraters. There are no minimal evidence-set or span-level gold annotations.
- **Controlled perturbation:** Limited. It compares natural sufficient/insufficient contexts, context length, multiple datasets, and retrieval settings, but lacks systematic order shuffling, noise injection, retrieval-recall curves, missing-evidence ablations, or answer-leakage controls.
- **Training mechanism:** Prompted autorating and a selective abstention controller; auxiliary LoRA experiments teach `I don’t know` behavior but do not train an evidence evaluator or verifier directly.
- **Learning target:** Binary query-context sufficiency and selective answer/abstain risk, not claim-level support or evidence extraction.
- **Evidence signal:** Set-level sufficiency independent of a known gold answer, which separates insufficient context from sufficient context that the generator fails to use.
- **A4 versus A5:** Sufficiency is A4.3. The finding that a model fails despite sufficient context diagnoses A5 evidence integration, but the paper does not train that integration ability.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Observed behavior:** Autoraters inspect implicit assumptions and multi-hop requirements; generators still hallucinate in both insufficient-context and sufficient-context conditions.
- **Main limitation:** Only 115 independently human-labeled pairs support the strongest ground-truth claim; the large-scale labels are model-generated, and minimal evidence sets, evidence chains, and sufficiency degrees are not annotated.
- **Reward-hacking / shortcut risk:** Medium. Main sufficiency judgment is not RL, but answer-string and evaluator-based shortcuts remain possible; order, noise, and leakage robustness are not systematically audited.
- **Decision:** Include Core as the strongest second A4.3 measurement anchor and as a clear A4/A5 diagnostic separator.

## B4. Benchmarking LLM’s Capability in Reasoning over Conflicting Web References (ConfRAG)

- **Authors / year:** Yizhen Yuan, Rui Kong, Dongze Li, Yuanchun Li, Yunxin Liu, ACL 2026
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.11/) · [code](https://github.com/XaiverYuan/ConfRAG)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary sub-direction:** A4.4 Conflict & Robustness
- **Secondary sub-directions:** A4.1 Relevance & Support; A4.2 Source Reliability
- **Judgment target:** Organization and coverage of conflicting Web viewpoints: Answer Clustering, Answer Coverage, and Reason Coverage. It is not a factual-truth or final-answer benchmark.
- **Ground truth quality:** Human-verified and adjudicated after GPT-assisted webpage answer/reason extraction. The benchmark reports high inter-annotator agreement, but gold labels represent source-expressed viewpoints rather than independently verified world truth.
- **Controlled perturbation:** Weak. The data contains naturally occurring conflicts and samples subsets of retrieved pages, but does not systematically vary conflict ratio, source order, source replacement, temporal state, noise, or fabricated citations.
- **Training mechanism:** Evaluation-only; zero-shot models, GPT-assisted construction, and human annotation. No SFT, RL, verifier training, or conflict-resolution policy.
- **Learning target:** None. Models are tested on viewpoint clustering and source-indexed reason coverage.
- **Evidence signal:** NMI for clustering and keyword/substring matching for answer/reason coverage; these measure structural organization, not source truth or resolution.
- **A4 versus A5:** The benchmark is A4.4 because it tests organization under conflict. Conflict detection and attribution are only indirect, and conflict resolution is outside scope. Answer/reason coverage must not be treated as factual correctness or A5 evidence integration.
- **Evidence vector:** `Method Effectiveness: Medium`; `Capability Isolation: Strong`; `Generalization Evidence: Limited`.
- **Observed behavior:** Models merge or omit viewpoints, miss reason coverage, and show errors even when output format is valid; strong syntax does not imply reliable conflict reasoning.
- **Main limitation:** The benchmark leaks the number of gold clusters, uses static Web snapshots, and relies on keyword matching. It cannot distinguish real conflict from reasonable disagreement or incomplete evidence with a dedicated label, and it does not rank source credibility or resolve conflicts.
- **Reward-hacking / shortcut risk:** Medium-to-High. Keyword overlap, known cluster count, and answer-match gating may reward surface matching rather than genuine conflict understanding.
- **Decision:** Include Core as an A4.4 measurement anchor, not as a conflict-resolution method.

---

## Second-batch synthesis

The second batch changes the first-batch diagnosis in one important way: A4.2 is not a literature void. It was a coverage void in the initial pool. AuthorityBench directly measures authority perception, and CONFACT explicitly combines source credibility with support/refute evidence. However, neither provides strong evidence that an LLM has learned a generalizable source-reliability capability through post-training.

The second batch also strengthens A4.3 and A4.4 measurement. Sufficient Context provides an independently human-labeled query–context sufficiency set, although it is small and pair-level. ConfRAG provides a realistic multi-source Web conflict benchmark with human-adjudicated viewpoint clusters and reason coverage, but it measures organization and coverage rather than conflict resolution or truth selection.

## Final A4 map after two batches

| Sub-direction | Representative papers | Evaluation status | Post-training status |
|---|---|---|---|
| **A4.1 Relevance & Support** | CRAG, RAGTruth, RAGChecker, RLSeek, EAPO | Stronger claim/span diagnostics exist | RLSeek and EAPO provide early narrow training evidence; generalization limited |
| **A4.2 Source Reliability** | AuthorityBench, CONFACT | Direct authority/credibility evaluation exists | Direct LLM post-training evidence remains limited; source bias and factual correctness are not fully separated |
| **A4.3 Evidence Sufficiency** | S2G-RAG, Sufficient Context, RAGChecker | Set-level sufficiency and missing evidence are measurable | S2G-RAG provides process supervision; independent set-level ground truth remains small |
| **A4.4 Conflict & Robustness** | RAGTruth, CONFACT, ConfRAG, RAGChecker | Conflict, viewpoint organization, and noise diagnostics exist | Direct conflict-verification post-training and robust resolution remain limited |

The three final A4 questions are:

1. **Measurement:** Do evidence-quality labels distinguish relevance, support, sufficiency, source credibility, and conflict, or do they collapse into final-answer proxies?
2. **Learning:** Do SFT/RL methods teach evidence judgment itself, or do they rely on external evaluators, rules, prompts, or outcome rewards?
3. **Generalization:** Does the learned behavior resist source replacement, evidence reordering, time shift, conflict changes, and verifier/reward gaming?

A4 can now be closed as a compact ten-paper map for the current survey stage. The defensible conclusion is not that evidence evaluation is unsolved in general, but that **measurement is ahead of post-training and cross-environment behavior transfer**, especially for source reliability and conflict robustness.

The next capability axis is A5 Evidence Integration: whether an LLM can correctly use evidence that has already been judged relevant, credible, sufficient, and non-contradictory.
