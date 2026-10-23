# A1 Final Sub-pool — Utility-aware Policy Learning

This is the final A1 verification sub-pool. It tests whether existing work has already moved beyond answerability and uncertainty proxies toward training search or tool-use policies with observed utility, information gain, counterfactual margins, or explicit costs.

The conclusion is deliberately conservative:

> Utility-aware policy learning already exists. The unresolved question is whether an LLM can predict and calibrate the value of information **before acting**, then transfer that decision across models, tools, corpora, tasks, environments, and costs.

## Core papers

### P1. s3: You Don’t Need That Much Data to Train a Search Agent via RL

- **Authors / year:** Pengcheng Jiang et al., EMNLP 2025
- **Links:** [ACL Anthology](https://aclanthology.org/2025.emnlp-main.1095/) · [arXiv](https://arxiv.org/abs/2505.14146) · [code](https://github.com/pat-jj/s3)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary relation to A1:** Utility-aware search policy learning; secondary A6 Adaptive Search Control
- **Training mechanism:** PPO-based RL updates a searcher while the answer generator is frozen.
- **Learning target:** Multi-turn query generation, evidence selection, and search completion.
- **Reward:** Gain Beyond RAG (GBR), approximately `GenAcc(searcher) − GenAcc(naive RAG)`.
- **Counterfactual utility:** Medium. The same problem is compared against a naive-RAG baseline, but the signal is not a full action-level potential-outcome estimate.
- **Cost awareness:** Limited. Turn, retrieval, and token budgets are controlled or reported, but cost is not part of the GBR objective.
- **Ex-ante versus ex-post:** Primarily ex-post. GBR is available only after the complete search trajectory and frozen generator outcome are observed.
- **Training–capability alignment:** Partial. The signal is more aligned with downstream search utility than final-answer reward, but it does not directly train prediction of retrieval necessity before tool invocation.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Medium`.
- **Generalization:** Task OOD and Domain OOD through held-out general QA and medical QA, plus changes in corpus and frozen generator. Tool OOD and direct Behavior Transfer are not established.
- **Key finding:** A small-data searcher trained with relative downstream gain can outperform larger-data search-training baselines across several QA and medical settings.
- **Limitation:** The reward depends on the generator and judge, is binary, omits explicit retrieval cost, and compares against one naive-RAG baseline. It does not estimate expected information value before acting.

### P2. Optimizing Agentic Reasoning with Retrieval via Synthetic Semantic Information Gain Reward

- **Authors / year:** Senkang Hu et al., 2026; arXiv v3, ICML 2026 acceptance noted on arXiv
- **Links:** [arXiv v3](https://arxiv.org/abs/2602.00845v3) · [HTML](https://arxiv.org/html/2602.00845v3) · [code](https://github.com/dl-m9/InfoReasoner)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary relation to A1:** Information-gain reward learning; secondary A6 Adaptive Search Control
- **Training mechanism:** GRPO updates the search-and-reasoning LLM policy.
- **Learning target:** Whether to search, how to formulate queries, and how to continue or stop multi-turn retrieval.
- **Reward:** A practical semantic information-gain signal based on the change in probability of the gold semantic answer class after evidence is added, combined with final answer reward.
- **Counterfactual utility:** Medium. The retrieval-free context is used as a baseline, but the practical reward is an observed posterior shift rather than a matched action-level expected benefit.
- **Cost awareness:** Limited. Search-turn efficiency and training overhead are reported, but no explicit latency, token, API, or monetary cost enters the policy objective.
- **Ex-ante versus ex-post:** Ex-post for the optimized signal. The paper motivates expected information gain theoretically, but the trained reward is computed after observing the retrieved evidence and posterior change.
- **Training–capability alignment:** Partial. It is more fine-grained than final-answer reward, but it still uses the gold answer and does not directly train a calibrated pre-retrieval value predictor.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.
- **Generalization:** Several QA datasets, MATH500 with Python execution, WebDetective cached web search, and multiple model scales. Behavior Transfer of retrieval-necessity prediction is not directly measured.
- **Key finding:** Semantic information gain can improve search-and-reasoning performance and reduce the number of turns needed to reach a baseline peak.
- **Limitation:** The reward depends on NLI clustering, sampled model outputs, and the gold answer; confident wrong beliefs can receive misleading signals. There is no cost-calibrated trigger or direct ex-ante EIG evaluation.

### P3. CoBRA: Learning Tool-Use Boundaries via Counterfactual Margins

- **Authors / year:** Wenhao Zou et al., 2026; arXiv v1, EMNLP 2026 submission
- **Links:** [arXiv](https://arxiv.org/abs/2609.00967) · [HTML](https://arxiv.org/html/2609.00967v1)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary relation to A1:** Tool-use boundary learning; secondary A6 Adaptive Search Control
- **Training mechanism:** Boundary-Aware SFT followed by counterfactual-margin RL.
- **Learning target:** An LLM policy that chooses between internal knowledge and external tool use for the same query.
- **Reward:** Cost-adjusted utility difference between paired internal and external expert trajectories; task correctness is combined with a tool-call cost.
- **Counterfactual utility:** Strong operationally. The same query is evaluated through paired no-tool and tool branches. However, the main setup relies on single deterministic branch rollouts, so it is not yet a full expected-potential-outcome estimator.
- **Cost awareness:** Strong relative to the other seed methods. Tool-call count enters the utility and the paper reports accuracy–cost trade-offs.
- **Ex-ante versus ex-post:** The policy acts ex ante, but its training labels are ex-post realized margins from executed internal/external branches.
- **Training–capability alignment:** Direct for tool-use boundary learning, but partial for ex-ante information-value estimation because the reward depends on post-hoc branch outcomes and expert quality.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Strong`; `Generalization Evidence: Medium`.
- **Generalization:** In-distribution and Task/Domain OOD QA, plus an initial vertical music-agent setting. Cross-model, cross-tool, retriever, temporal-corpus, and cost-distribution transfer are not systematically tested.
- **Key finding:** The method reduces over-calling on internal-favored questions and under-calling on external-favored questions while improving accuracy–tool-use trade-offs.
- **Limitation:** The counterfactual margin is still an observed branch difference. It may mix expert quality, retrieval quality, judge noise, and policy-induced trajectory differences; the cost model is mainly tool-call count.

### P4. Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents

- **Authors / year:** Wenxuan Ding, Nicholas Tomlin, Greg Durrett, 2026; arXiv v3
- **Links:** [arXiv v3](https://arxiv.org/abs/2602.16699v3) · [HTML](https://arxiv.org/html/2602.16699v3) · [code](https://github.com/NSF-Simons-CosmicAI-Institute/CalibrateThenAct)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary relation to A1:** Cost-aware decision learning; secondary A6 Adaptive Search Control
- **Training mechanism:** Prompt-conditioned decision making and GRPO with an explicit answerability/calibration prior.
- **Learning target:** Retrieve-versus-answer, verify-versus-commit, and test/execute/submit action policies.
- **Reward:** Task reward discounted by a cost parameter; a separate prior estimator supplies an estimate of answerability or potential success.
- **Counterfactual utility:** Medium. The framework compares alternative actions before acting and reports oracle-policy match, but does not learn a complete per-instance expected retrieval-benefit model.
- **Cost awareness:** Strong. Costs are varied across QA and file-interaction settings, and behavior is evaluated across cost conditions.
- **Ex-ante versus ex-post:** More ex-ante than most seed methods because the policy is conditioned on calibrated priors and costs before action; nevertheless, those priors are external components and not automatically learned by the LLM itself.
- **Training–capability alignment:** Partial. Cost and prior information are explicit in the decision interface, but terminal task rewards do not assign credit to individual search actions.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Strong`; `Generalization Evidence: Medium`.
- **Generalization:** Multiple controlled tasks and cost settings, with limited Behavior Transfer across action domains. No systematic cross-model, cross-tool, corpus, or real-environment transfer.
- **Key finding:** Explicit calibrated priors can improve cost-aware exploration; ordinary RL does not necessarily discover the correct uncertainty–cost trade-off by itself.
- **Limitation:** The prior estimator is external, tasks are controlled, and the method does not establish that the LLM learned a transferable intrinsic value-of-information representation.

### P5. IKEA: Reinforced Internal-External Knowledge Synergistic Reasoning

- **Authors / year:** Ziyang Huang et al., 2025; arXiv v1; later ICLR 2026 withdrawn submission version
- **Links:** [arXiv](https://arxiv.org/abs/2505.07596) · [code](https://github.com/hzy312/knowledge-r1) · [OpenReview submission](https://openreview.net/forum?id=IEDYctsolA)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary relation to A1:** Knowledge-boundary-aware tool-use policy learning; secondary A6 Adaptive Search Control
- **Training mechanism:** GRPO with knowledge-boundary-aware data construction and a terminal reward combining answer correctness with retrieval count.
- **Learning target:** Whether to rely on internal knowledge or invoke external retrieval, and how to stop.
- **Reward:** Exact-match answer reward plus a coarse retrieval-count penalty/bonus.
- **Counterfactual utility:** Limited. Direct, fixed-retrieval, and internal/external comparisons provide overall tool/no-tool contrasts, but not a per-state counterfactual margin or information-gain estimate.
- **Cost awareness:** Medium. Retrieval count is explicitly rewarded or penalized, but it is only a proxy for real latency, token, API, monetary, privacy, or safety costs.
- **Ex-ante versus ex-post:** Primarily ex-post. The policy receives terminal answer and retrieval-count feedback; easy/hard probes provide a knowledge-boundary prior but not calibrated per-action value.
- **Training–capability alignment:** Partial. The reward points toward “retrieve when internal knowledge is insufficient and avoid redundant calls,” but it cannot identify which retrieval action created value.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Limited`; `Generalization Evidence: Medium`.
- **Generalization:** NQ/HotpotQA training and QA OOD tests such as PopQA/2Wiki, plus model-size comparisons. Tool, corpus, temporal, and explicit behavior-transfer tests are absent.
- **Key finding:** The method improves answer accuracy while reducing retrieval calls compared with always-retrieve or search-only baselines.
- **Limitation:** The signal is an answer-plus-call-count proxy and uses a fixed Wikipedia/E5-style environment. It does not measure counterfactual retrieval value, calibrated uncertainty, or cost-sensitive transfer.

### P6. Self-Knowledge Guided Retrieval Augmentation for Large Language Models

- **Authors / year:** Yile Wang et al., Findings of ACL: EMNLP 2023
- **Links:** [ACL Anthology](https://aclanthology.org/2023.findings-emnlp.691/) · [arXiv](https://arxiv.org/abs/2310.05002) · [code](https://github.com/THUNLP-MT/SKR)
- **Role:** Core / historical baseline
- **Contribution type:** Capability Learning through an auxiliary gate
- **Primary relation to A1:** Self-knowledge-guided retrieval gating
- **Training mechanism:** Inference-time prompting/ICL, an auxiliary supervised BERT classifier, and non-parametric kNN; the target LLM itself is not updated.
- **Learning target:** A binary known/unknown gate that decides whether to retrieve.
- **Reward / supervision:** Ex-post paired no-retrieval versus retrieval-augmented answer outcomes; cases where retrieval helps become unknown/retrieve labels.
- **Counterfactual utility:** Medium. It explicitly compares the same question with and without retrieval, but reduces the result to a binary, setting-specific label.
- **Cost awareness:** Not Tested.
- **Ex-ante versus ex-post:** Labels are ex-post; the learned or prompted gate makes an ex-ante decision but does not estimate expected information value.
- **Training–capability alignment:** Partial. The paired outcome is close to retrieval necessity, but it is dependent on the LLM, retriever, corpus, and answer metric and does not update the LLM policy.
- **Evidence vector:** `Method Effectiveness: Strong`; `Capability Isolation: Medium`; `Generalization Evidence: Limited`.
- **Generalization:** Multiple QA datasets and retrieval corpora, but no systematic new-tool, new-environment, budget, or behavior-transfer evaluation.
- **Key finding:** Retrieval can harm answers, and a selective gate based on self-knowledge can outperform unconditional retrieval in the reported QA settings.
- **Limitation:** The strongest method is an auxiliary kNN gate rather than a learned LLM capability; there is no cost-sensitive objective, calibrated selective risk, semantic information gain, or cross-environment transfer.

---

## A1 final synthesis

The final A1 literature map now has six layers:

| Layer | Representative works | Current status |
|---|---|---|
| **Answerability / uncertainty signals** | KBM, SeaKR, DRAGIN | Multiple proxy-based triggers and evaluations exist |
| **Retrieval utility measurement** | LLM-Specific Utility, UDCG / Redefining Retrieval Evaluation | Direct model-consumer utility evaluation exists |
| **Utility-aware policy learning** | s3, CoBRA | Direct training methods exist; signals are mostly ex-post |
| **Information-gain reward learning** | InfoReasoner | Fine-grained posterior-shift reward exists; not yet calibrated ex ante |
| **Cost-aware decision learning** | Calibrate-Then-Act, CoBRA, IKEA | Explicit cost or call-count trade-offs exist; transfer is limited |
| **Cross-environment calibrated utility** | Partial evidence across the above | Not established as a reliable general capability |

The correct final A1 claim is:

> **Retrieval necessity is evolving from proxy-based triggering toward utility-aware policy learning. However, optimizing observed retrieval gains does not necessarily imply that an LLM can predict, calibrate, and transfer the value of information acquisition before taking action.**

## Three open problems that remain defensible

### 1. Ex-post reward versus ex-ante value

A policy can be rewarded after retrieval improves an answer without being able to predict, before acting, whether retrieval will help. Future work should evaluate pre-action estimates of benefit, uncertainty, and cost using calibration curves, selective risk, regret, and no-search/search decision accuracy.

### 2. Counterfactual utility estimation

A reliable utility estimate must separate retrieval benefit from generator capability, retriever quality, expert differences, judge noise, sampling randomness, and environment changes. The evaluation should use paired or multi-sample tool/no-tool potential outcomes, report variance and confidence intervals, and avoid treating a single realized branch difference as causal value.

### 3. Cost-calibrated generalization

The policy should be tested with new LLMs, retrievers, tools, corpora, domains, temporal states, and token/latency/API-price budgets. The target is not merely a better accuracy–call-count point in one environment, but stable calibration and low transfer regret under changed costs and information sources.

## Transition to A4

A1 should now be considered sufficiently mapped for the survey’s current stage. The next axis is A4 Evidence Evaluation:

- **A1:** Should the agent acquire information, and is the expected benefit worth the cost?
- **A4:** Once information is acquired, which evidence is relevant, reliable, sufficient, non-contradictory, and safe to use?

The two axes should share paired trajectories and cost logs where possible, but they must remain separate. Evidence quality or final answer improvement should not be retroactively treated as proof of A1 retrieval necessity.
