# A6 — Adaptive Search Control

## Central question

> **Can LLMs learn feedback-conditioned search-control policies that adapt their actions, allocate resources, and stop appropriately across changing information environments?**

A6 is not a general Search-Agent RL chapter. It studies what the policy does **after observing a search state or retrieval outcome**. The target is a control decision, not merely the ability to call a search tool.

A1 asks whether information acquisition is worthwhile before or at the start of search. A2 asks how to express the information need. A3 asks which information regions or sources to explore. A6 asks how to adapt after feedback: continue, retry, reformulate, switch, verify, allocate more budget, or stop.

## A6 behavior axes

- **A6.1 Feedback-conditioned adaptation:** after failed, repeated, insufficient, contradictory, or newly informative evidence, does the next action change appropriately?
- **A6.2 Search budget and stopping:** does the policy stop when the expected value of another search is low, continue when information is insufficient, and allocate different search depths under different budgets?
- **A6.3 Long-horizon credit assignment:** can training attribute eventual success or failure to the specific search-control actions that caused it?

The most important capability-isolation test is a controlled feedback intervention: hold the task and prior search history fixed, change the current retrieval feedback, and test whether the policy changes its next action in the expected direction. None of the four first-round papers fully establishes this test.

For consistent coding, each A6 paper is annotated with **Control Action**, **Observed Feedback**, **Feedback Dependence**, **Control Credit**, **Deployment Availability**, **Control Transfer**, and **Shortcut Risk**. These fields distinguish a policy that reacts to evidence from one that only learns a search-depth template, and distinguish online state information from hindsight or privileged training supervision.

---

## First-round targeted papers

### A6-1. AutoSearch: Adaptive Search Depth for Efficient Agentic RAG via Reinforcement Learning

- **Authors / venue:** Jingbo Sun et al., Findings of ACL 2026.
- **Links:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.1399/) · [arXiv](https://arxiv.org/abs/2604.17337) · [code/search record](https://arxiv.org/html/2604.17337v1)
- **Primary direction:** A6.2 Search Budget and Stopping.
- **Control actions trained:** Continue searching versus terminate with an answer. The search process also produces intermediate answers, but the central learned control variable is search depth.
- **Training signal:** For each trajectory, the earliest step whose intermediate answer exactly matches the gold answer is treated as the capability-aware optimal depth. The reward gives positive credit to effective search before that depth, positive but smaller credit for under-searching when the answer remains incorrect, and negative reward for over-searching after the answer has become correct. Repeated retrieval is marked invalid.
- **Feedback dependence:** Partial. The policy conditions on accumulated retrieved observations and intermediate answers, so the reward is not merely a fixed step limit. However, the training target for sufficient depth is defined with the gold answer and exact match. The paper does not provide a controlled fixed-history intervention where only the current feedback is changed and the next action is measured.
- **Capability isolation:** Medium. Search depth, search efficiency, intermediate-answer F1, and final accuracy are all reported. This separates stopping efficiency from final QA more than an outcome-only reward would. It does not fully distinguish evidence-sensitive stopping from learning a benchmark-specific depth pattern.
- **Generalization:** Multiple general and multi-hop QA benchmarks and multiple model sizes provide Task OOD and Model OOD evidence. No systematic Retriever OOD, Tool OOD, changing-corpus, or budget-shift transfer is established in the verified material.
- **A1/A6 boundary:** A6-primary. It assumes the search loop has started and optimizes when to stop or continue. It should not be interpreted as learning whether retrieval is worthwhile at the initial decision, which is A1.
- **A2/A6 boundary:** The agent may produce new queries, but the paper’s signal targets depth and redundancy rather than query intent quality.
- **Main limitation:** Gold-answer-derived optimal depth is an ex-post training target. At deployment, the policy must infer sufficiency from its own intermediate state; the paper shows improved efficiency but not a direct intervention-based proof that the policy responds to changed evidence.
- **Evidence vector:** Method Effectiveness: Strong; Capability Isolation: Medium; Generalization Evidence: Limited.
- **Decision:** Include as A6 Core for stopping/depth control.

### A6-2. One Policy, Any Budget: Internalizing Budget-Aware Search via Reinforcement Learning

- **Authors / status:** Xiaowei Sun et al., arXiv preprint submitted 1 September 2026.
- **Links:** [arXiv abstract](https://arxiv.org/abs/2609.00813) · [arXiv HTML](https://arxiv.org/html/2609.00813v1) · [code](https://github.com/xwsun01/AnySearch)
- **Primary direction:** A6.2 Budget-aware control and stopping.
- **Control actions trained:** At each state, the policy chooses Reason, Search, or Answer. Search consumes one unit of budget. The policy must decide whether the current evidence is sufficient and whether another search is worthwhile under the remaining budget.
- **Training:** Two-phase curriculum RL with GRPO. Phase I injects remaining/used/total budget and structured information-sufficiency reasoning. Phase II removes the scaffold and adaptively samples weak budget levels, so the policy must internalize budget awareness rather than rely on explicit per-turn budget tags.
- **Training signal:** Composite answer accuracy, format validity, length penalty, and adaptive tool-efficiency rewards. The efficiency reward contains absolute budget saving and relative progress toward efficient correct trajectories. It is attenuated for low-accuracy groups and amplified for high-accuracy groups.
- **Feedback dependence:** Medium-to-Strong for budget feedback, Limited for evidence feedback. The state contains accumulated context and remaining budget, and the scaffold explicitly asks whether information is sufficient. The policy demonstrates early stopping and budget conservation, but the paper does not isolate success/failure/repetition/new-evidence feedback under a fixed history.
- **Capability isolation:** Stronger than final QA alone. It reports Tool Productivity, search counts, token usage, early-stopping cases, scaffold-removal results, and an efficiency-reward ablation. The early-stopping analysis compares the full policy with a no-efficiency-reward control under the same training setup.
- **Generalization:** Strong Budget / Cost Generalization. Training budgets are capped at five, while evaluation includes unseen budgets six through eight. The paper reports continued improvement and higher tool productivity at unseen budgets. It does not establish Retriever OOD, Tool OOD, or changing-corpus transfer.
- **A1/A6 boundary:** A6-primary. It assumes the policy has a search budget and learns how to allocate it. The information-sufficiency assessment is used to control continuation, not to define the initial value-of-information problem in A1.
- **A2/A6 boundary:** The policy can issue search queries, but query quality is not the main learned object.
- **Main limitation:** The control space is mainly Reason/Search/Answer. It does not separately train or evaluate Retry, Switch, Verify, or explicit query reformulation after a failed retrieval. Unseen-budget transfer is not equivalent to unseen-retriever or unseen-tool transfer.
- **Evidence vector:** Method Effectiveness: Strong; Capability Isolation: Medium-to-Strong; Generalization Evidence: Strong for budget, Limited for environment.
- **Decision:** Include as A6 Core for budget allocation and stopping.

### A6-3. CriticSearch: Fine-Grained Credit Assignment for Search Agents via a Retrospective Critic

- **Authors / venue:** Yaocheng Zhang et al., Findings of ACL 2026.
- **Links:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.596/) · [arXiv](https://arxiv.org/abs/2511.12159) · [arXiv HTML](https://arxiv.org/html/2511.12159v1)
- **Primary direction:** A6.3 Long-horizon Credit Assignment; secondary A6.1 Feedback-conditioned adaptation.
- **Control actions trained:** Search actions at each turn, including query reformulation and continued search after unhelpful or redundant evidence. The paper’s examples show the policy changing keywords after an unhelpful first retrieval.
- **Training signal:** A frozen retrospective critic receives the complete trajectory and gold answer during training. It labels each search action Good or Bad and converts the judgments into dense turn-level rewards. These are combined with global outcome advantages.
- **Feedback dependence:** Partial. The policy is trained to favor actions that, in hindsight, led toward useful evidence. However, the critic uses privileged future trajectory and gold-answer information that is unavailable during forward deployment. This demonstrates training-time credit assignment, not necessarily deployed online feedback understanding.
- **Capability isolation:** Medium. The paper compares sparse outcome reward, no-critic, critic-size variants, gold-answer removal, and human alignment of turn-level rewards. The critic’s turn labels correlate with human annotations, but the evaluation does not directly test whether the policy changes its next action when the same history receives a different current observation.
- **Generalization:** Task OOD across HotpotQA, 2WikiMultiHopQA, MuSiQue, and Bamboogle. Training and evaluation use the same 2018 Wikipedia dump, E5 retriever, and four-search-turn limit. Therefore this is not Retriever OOD, Tool OOD, Environment OOD, or Budget Generalization.
- **A1/A6 boundary:** A6-primary because it assigns credit to actions after search begins. It is not primarily about whether search should be initiated.
- **A2/A6 boundary:** Query reformulation appears as a control response, but the training signal is action usefulness in the full trajectory, not standalone semantic intent preservation.
- **Main limitation:** Hindsight labels may teach a policy to imitate patterns correlated with successful trajectories without proving robust feedback-conditioned recovery. Removing the gold answer reduces critic quality but does not eliminate the privileged full-trajectory perspective during training.
- **Evidence vector:** Method Effectiveness: Strong; Capability Isolation: Medium; Generalization Evidence: Limited.
- **Decision:** Include as A6 Core for credit assignment, with an explicit hindsight/online-feedback caveat.

### A6-4. Enhancing LLM-based Search Agents via Contribution Weighted Group Relative Policy Optimization

- **Authors / venue:** Junzhe Wang et al., ACL 2026.
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1462/) · [arXiv](https://arxiv.org/abs/2604.14267) · [code](https://github.com/zsxmwjz/CW-GRPO)
- **Primary direction:** A6.3 Contribution-aware Credit Assignment; secondary A6.1 feedback-sensitive search behavior.
- **Control actions trained:** Search rounds within multi-turn trajectories. CW-GRPO does not introduce a new explicit Continue/Retry/Switch action; it changes how outcome advantage is assigned to existing search-round actions.
- **Training signal:** An LLM judge assigns two per-round binary signals: Retrieval Utility is one only when the round introduces novel task-relevant evidence absent from previous context; Reasoning Correctness is one when the reasoning correctly interprets the current context. Their conjunction becomes a contribution gate that rescales outcome advantages for successful trajectories. Failed trajectories receive uniform round weights because their local failure causes are ambiguous.
- **Feedback dependence:** Partial-to-Strong as a training signal, Limited as direct control evidence. The utility rubric explicitly depends on the difference between current and previous evidence. Nevertheless, the paper does not run a controlled feedback intervention to show that the deployed policy changes its next control action under altered observations.
- **Capability isolation:** Medium. The judge is calibrated against 97 manually annotated search rounds with a reported 95% consensus rate. The method uses hard-case search evaluation and analyzes concentrated contributions in successful trajectories. This is stronger than trajectory-level outcome reward, but the LLM judge and outcome reward jointly supervise the policy, so the causal effect on Continue, Reformulate, Verify, or Stop remains under-isolated.
- **Generalization:** Seven knowledge-intensive QA benchmarks, including a hard-case AgentGym-SearchQA test set, provide Task OOD evidence. Training uses merged NQ and HotpotQA data. The verified paper does not establish Retriever OOD, Tool OOD, changing-corpus transfer, or Budget Generalization.
- **A1/A6 boundary:** A6-primary only for action credit after search has started. The retrieval-utility judge should not be confused with an ex-ante value-of-information predictor from A1.
- **A2/A6 boundary:** Novel evidence may result from better queries, but the paper attributes contribution at the round level rather than directly evaluating semantic intent preservation.
- **Main limitation:** Contribution attribution is reliable mainly for successful trajectories. The paper explicitly notes that failed trajectories may contain reasonable verification attempts, retriever-recall failures, or cascading interpretation errors, so uniform weighting avoids spurious supervision but leaves failure recovery underlearned.
- **Evidence vector:** Method Effectiveness: Strong; Capability Isolation: Medium; Generalization Evidence: Limited.
- **Decision:** Include as A6 Core for contribution-aware credit assignment, not as a complete adaptive-control solution.

---

## Cross-paper synthesis

The four papers cover the three intended A6 directions, but they do not solve the same problem.

**AutoSearch** learns a capability-aware stopping depth from intermediate answers. **AnySearch** learns to allocate search calls under explicit and unseen budgets. **CriticSearch** learns to distribute hindsight credit over search turns. **CW-GRPO** learns to reweight outcome advantages according to retrieval utility and reasoning correctness at each round.

This produces a useful training-signal progression:

> **Outcome Reward → Search-depth / Cost Reward → Retrospective Turn Credit → Contribution-weighted Evidence Credit**

The progression is not monotonic evidence of stronger capability learning. Each signal teaches a different object. AutoSearch and AnySearch directly target control decisions. CriticSearch and CW-GRPO primarily improve attribution of learning signals to earlier actions. Better credit assignment can improve a control policy, but it does not by itself prove that the final policy understands feedback at deployment.

### Feedback Dependence is the unresolved A6 criterion

All four papers condition training on some form of search state or retrieved evidence. None provides the strongest controlled test proposed for this survey:

> Hold the task and completed history fixed; change only the current feedback; then measure whether the next action changes appropriately.

Examples of the desired test include the following. With newly sufficient and consistent evidence, the policy should become more likely to stop or proceed to answer. With repeated or irrelevant evidence, it should reformulate or switch strategy. With an empty result, it should retry with a different query or source. With contradictory evidence, it should verify rather than simply continue the same search pattern.

Current evidence is weaker. AutoSearch shows different search depths and efficiency, AnySearch shows budget-conditioned allocation, CriticSearch provides trajectory examples of query adjustment, and CW-GRPO uses a novelty-aware retrieval utility rubric. These demonstrate plausible control learning, but not full feedback-conditioned policy identification.

### Generalization pattern

A6 currently has an uneven transfer profile. AnySearch provides strong evidence for **Budget / Cost Generalization**. CriticSearch and CW-GRPO provide mostly **Task OOD** results under fixed retriever/corpus settings. AutoSearch reports multiple benchmarks and model sizes but limited environment variation. Across the four papers, systematic **Retriever OOD**, **Tool OOD**, and **Environment OOD** remain largely untested.

## A6 first-round decision

The four candidates are sufficient to close the first A6 paper pass. A6 should remain an independent capability definition in the taxonomy, but its literature should be organized by **control object**, not by a generic “agentic search RL” label:

1. Search depth and stopping;
2. Budget allocation;
3. Feedback recovery and action adaptation;
4. Credit assignment for long-horizon control.

The most important survey-level conclusion is:

> **Training a policy with turn-level or contribution-aware rewards is not equivalent to proving that the policy has learned a feedback-conditioned control rule.**

After this bounded A6 pass, stop longitudinal paper collection and begin the cross-capability analysis. Existing Search-R1, StepSearch, and Beneficial Reasoning Behaviors in Agentic Search can be reused as anchors; no duplicate entries are needed here.
