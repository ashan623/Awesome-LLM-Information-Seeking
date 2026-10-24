# Search Log — Round 6C — A6 Adaptive Search Control First Batch

## Scope

This pass verified exactly four candidates: AutoSearch, AnySearch, CriticSearch, and CW-GRPO. The analysis was restricted to A6 control after search begins. Search-R1, StepSearch, and Beneficial Reasoning Behaviors in Agentic Search were treated as existing anchors and were not duplicated.

The three required checks were:

1. **Control Action:** which actions are trained — Continue, Retry, Reformulate, Switch, Verify, Allocate, or Stop;
2. **Feedback Dependence:** does the policy respond to observed retrieval feedback rather than imitate a fixed depth or action template;
3. **Control Credit and Transfer:** can reward be assigned to specific actions, and does the behavior transfer across budgets, Retrievers, Tools, Corpora, or environments?

## AutoSearch

AutoSearch is published in Findings of ACL 2026. It uses self-generated intermediate answers after each retrieval step to identify the earliest step whose answer matches the gold answer. Search-depth rewards encourage effective search before that depth and penalize over-searching after it; repeated retrieval is invalidated.

The paper directly targets Continue versus Stop and reports search depth, search efficiency, intermediate-answer quality, and final accuracy over several general and multi-hop QA benchmarks and two model sizes. This is stronger than a fixed maximum-depth baseline. However, the optimal depth is defined ex post using the gold answer during training. The verified evidence does not include a fixed-history intervention that changes only current feedback and measures a different next action.

**Coding decision:** Core A6.2. Method Effectiveness Strong; Capability Isolation Medium; Generalization Limited. The main caveat is that evidence-sensitive stopping is not fully distinguished from learned task/depth patterns.

Sources: [ACL Anthology](https://aclanthology.org/2026.findings-acl.1399/), [arXiv](https://arxiv.org/abs/2604.17337), [HTML](https://arxiv.org/html/2604.17337v1).

## AnySearch

One Policy, Any Budget: Internalizing Budget-Aware Search via Reinforcement Learning is an arXiv preprint submitted 1 September 2026. It formulates the loop as Reason, Search, or Answer under a remaining search budget. Phase I injects budget state and structured information-sufficiency prompts; Phase II removes the scaffold and adaptively samples weak budget levels using GRPO.

AnySearch provides the strongest A6 transfer evidence in this batch for Budget / Cost Generalization. It trains with budgets up to five and evaluates unseen budgets six through eight, reporting continued performance gains and better tool productivity. Ablations show the importance of adaptive efficiency weighting, and comparisons with a no-efficiency-reward policy analyze early stopping and budget conservation.

The main limitation is scope of the action set. AnySearch learns budget-aware allocation of Reason/Search/Answer, but does not independently train Retry, Switch, Verify, or explicit reformulation after a failed retrieval. It also does not test Retriever OOD, Tool OOD, or changing-corpus transfer.

**Coding decision:** Core A6.2. Method Effectiveness Strong; Capability Isolation Medium-to-Strong; Budget Generalization Strong; Environment Generalization Limited.

Sources: [arXiv abstract](https://arxiv.org/abs/2609.00813), [HTML](https://arxiv.org/html/2609.00813v1), [code](https://github.com/xwsun01/AnySearch).

## CriticSearch

CriticSearch is published in Findings of ACL 2026. A frozen critique LLM retrospectively receives the full trajectory and gold answer during training, labels each search action Good or Bad, and generates turn-level rewards. It is designed to replace sparse trajectory rewards with dense credit over multi-turn search.

The paper tests critic size, no-critic/sparse variants, removal of the gold answer, and agreement between critique labels and human annotations. It also reports out-of-domain QA datasets. The qualitative examples show query reformulation after unhelpful evidence.

The central A6 contribution is credit assignment, not an online feedback policy. The critic has privileged future trajectory and gold-answer access, so its labels show how training can assign hindsight credit, not what the deployed policy can know before taking its next action. Training and evaluation use the same 2018 Wikipedia corpus, E5 retriever, and four-turn limit; benchmark transfer is therefore not Retriever or Environment Transfer.

**Coding decision:** Core A6.3 with a strong hindsight caveat. Method Effectiveness Strong; Capability Isolation Medium; Generalization Limited.

Sources: [ACL Anthology](https://aclanthology.org/2026.findings-acl.596/), [arXiv](https://arxiv.org/abs/2511.12159), [HTML](https://arxiv.org/html/2511.12159v1).

## CW-GRPO

Enhancing LLM-based Search Agents via Contribution Weighted Group Relative Policy Optimization is published in ACL 2026. For each search round, an LLM judge assigns Retrieval Utility only when the round introduces novel task-relevant evidence absent from earlier context, and Reasoning Correctness when the reasoning correctly uses the current context. The conjunction produces a per-round contribution weight that rescales outcome advantages for successful trajectories. Failed trajectories receive uniform round weights because their local failure causes are ambiguous.

This is a direct contribution-aware credit signal, and the retrieval-utility rubric is explicitly feedback-relative. The judge is calibrated on 97 search rounds with a reported 95% consensus rate with human experts. However, the paper changes the learning weight of existing search rounds; it does not introduce or isolate a particular Continue, Retry, Reformulate, Switch, Verify, or Stop policy. It also does not include the strongest fixed-history feedback intervention.

The paper evaluates seven knowledge-intensive QA benchmarks, including a hard-case AgentGym-SearchQA test set, but the verified evidence does not establish Retriever, Tool, Corpus, or Budget Generalization. Its failure analysis is especially relevant: failed rounds may be reasonable verification attempts, suffer from retriever recall, or be downstream of earlier interpretation errors. This motivates uniform weighting but also shows why feedback-conditioned failure recovery remains unresolved.

**Coding decision:** Core A6.3 for contribution-aware credit assignment, not a complete adaptive-control solution. Method Effectiveness Strong; Capability Isolation Medium; Generalization Limited.

Sources: [ACL Anthology](https://aclanthology.org/2026.acl-long.1462/), [arXiv](https://arxiv.org/abs/2604.14267), [HTML](https://arxiv.org/html/2604.14267v1), [code](https://github.com/zsxmwjz/CW-GRPO).

## Cross-paper synthesis

The four candidates cover different control objects:

- AutoSearch: search depth and stopping;
- AnySearch: budget allocation and early stopping;
- CriticSearch: retrospective turn-level credit;
- CW-GRPO: contribution-aware advantage redistribution.

Their training signals form a useful progression:

> **Outcome Reward → Search-depth / Cost Reward → Retrospective Turn Credit → Contribution-weighted Evidence Credit**

This is not a simple ranking. AutoSearch and AnySearch directly optimize control decisions. CriticSearch and CW-GRPO primarily improve the attribution of learning signals to earlier search rounds. Better credit assignment can improve a policy, but it does not prove that the deployed policy has learned a feedback-conditioned control rule.

## Feedback Dependence finding

None of the four papers performs the strongest controlled intervention proposed for A6: hold the task and completed history fixed, change only the current feedback, and measure whether the next action changes appropriately.

AutoSearch demonstrates adaptive search depth, AnySearch demonstrates budget-conditioned allocation, CriticSearch provides hindsight action labels and qualitative reformulation examples, and CW-GRPO uses a novelty-aware utility rubric. These are meaningful but indirect evidence of Feedback Dependence.

The missing tests should distinguish the following cases:

- sufficient consistent evidence → Stop / Answer;
- empty or failed retrieval → Retry / Reformulate / Switch;
- repeated evidence → avoid identical search and seek a new source or verification;
- contradictory evidence → Verify or seek an independent source;
- unresolved information with remaining budget → Continue when marginal value is positive.

## Generalization finding

A6 currently has an uneven transfer profile. AnySearch gives strong Budget / Cost Generalization. CriticSearch and CW-GRPO mainly report Task OOD under a fixed corpus and retriever. AutoSearch reports several benchmarks and model sizes but limited environment variation. Across this first batch, systematic Retriever OOD, Tool OOD, and Environment OOD are still missing.

## First-round decision

The four papers are sufficient to close the first A6 pass. A6 remains an independent capability definition, but its literature should be organized by control object rather than under a generic Search-Agent RL heading. After this pass, stop longitudinal collection and move to the cross-capability comparison:

> **Training Signal × Learned Behavior × Capability Isolation × Generalization**
