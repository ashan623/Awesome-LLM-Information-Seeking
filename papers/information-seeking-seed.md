# First-Round Seed Papers

**Scope:** field-map and seed verification round, 2026-10-23. This file contains three neighboring surveys and seven representative method/evaluation papers. It is a curated starting set, not an exhaustive bibliography.

## How to read the annotations

- **Primary capability** is the main information-seeking behavior directly studied or trained.
- **Learning mechanism** records the mechanism used to acquire that behavior, not merely the paper's implementation detail.
- **Contribution type** is orthogonal to A1–A6: `Capability Learning`, `Training Infrastructure`, `Retriever Optimization`, or `Evaluation`.
- **Evidence** is a three-dimensional vector: `Method Effectiveness`, `Capability Isolation`, and `Generalization Evidence`. Each dimension is `Strong`, `Medium`, `Limited`, or `Not Tested`.
- **Generalization** is coded by regime: `In-distribution`, `Task OOD`, `Domain OOD`, `Tool OOD`, `Environment OOD`, `Behavior Transfer`, and, when relevant, `Budget / Cost Generalization`. A higher score on an in-domain QA benchmark is not counted as capability transfer.
- **Training–Capability Alignment** asks whether the training signal directly reflects the claimed capability, only partially reflects it, or is an indirect downstream proxy.

## Converged coding protocol

The survey line is fixed at **capability learning → training–capability alignment → capability transfer**. A paper should enter the core set only when it helps answer at least one of these questions. Small implementation differences among otherwise similar Search-RL papers are not sufficient reason to expand the core set.

The boundary between **A3 Information Space Exploration** and **A6 Adaptive Search Control** is explicit. A3 concerns discovering paths and moving across information sources; A6 concerns deciding when to search, how to adjust search, how to invoke a tool, how to recover, and when to stop. A paper may have no directly trained A1–A6 capability when its main contribution is infrastructure, retriever optimization, or evaluation; use `Not Directly Trained` rather than forcing a primary capability.

---

## I. Neighboring surveys

### S1. A Survey of LLM-based Deep Search Agents: Paradigm, Optimization, Evaluation, and Challenges

- **Authors / year:** Yunjia Xi et al., 2025
- **Link:** [arXiv:2508.05668v3](https://arxiv.org/html/2508.05668v3)
- **Role:** Core / neighboring survey
- **Primary capability:** A2 Information Need Formulation; A3 Information Space Exploration; A6 Adaptive Search Control
- **Secondary capabilities:** A4 Evidence Evaluation; A5 Evidence Integration
- **Learning mechanism:** architecture, optimization, application, and evaluation taxonomy; includes tuning-free and tuning-based methods, including RL.
- **Learning target:** Search agent architecture and policy; in some methods, LLM parameters.
- **Epistemic objective:** Grounding/acquisition, exploration/discovery, decision/action support.
- **Update target:** Context/plan/policy state; sometimes model parameters.
- **Generalization coverage:** Surveys benchmarks and applications, but does not use cross-task/tool/environment transfer as the main organizing principle.
- **Why it matters:** Establishes the closest neighboring field. It frames search agents through architecture, optimization, application, and evaluation, and describes the shift from static or rule-based retrieval toward multi-turn autonomous information seeking.
- **Limitation for our survey:** Its central unit is the search-agent system/pipeline. Our survey should instead code what information-seeking capability is learned, what learning signal forms it, and whether it transfers.
- **Research opportunity:** Re-encode its method inventory by capability formation and transfer evidence rather than by system architecture.

### S2. A Comprehensive Survey on Reinforcement Learning-based Agentic Search: Foundations, Roles, Optimizations, Evaluations, and Applications

- **Authors / year:** Minhua Lin et al., 2025
- **Link:** [arXiv:2510.16724v2](https://arxiv.org/html/2510.16724)
- **Role:** Core / neighboring survey
- **Primary capability:** A6 Adaptive Search Control
- **Secondary capabilities:** A2 Information Need Formulation; A3 Information Space Exploration; A4 Evidence Evaluation; A5 Evidence Integration
- **Learning mechanism:** RL foundations, reward design, policy learning, advanced optimization, and scope of intervention.
- **Learning target:** Agent-level, module-level, step-level, multi-agent, and system-level policy components.
- **Epistemic objective:** Adaptive retrieval, reasoning support, decision making.
- **Update target:** LLM policy, search planner, retriever/selector, multi-agent system.
- **Generalization coverage:** Evaluation and application coverage is broad, but systematic capability transfer is not the primary axis.
- **Why it matters:** Its explicit three-part organization—what RL is for, how RL is used, and where RL is applied—is the nearest existing framework to the proposed Learning axis.
- **Limitation for our survey:** It is RL-centric and mainly asks how RL optimizes agentic search. It does not make “what information-seeking capabilities are acquired” the primary unit.
- **Research opportunity:** Separate learning object, supervision source, and generalization regime; avoid treating all RL-based methods as evidence of a general search capability.

### S3. Deep Research: A Survey of Autonomous Research Agents

- **Authors / year:** Wenlin Zhang et al., 2025
- **Link:** [arXiv:2508.12752v1](https://arxiv.org/html/2508.12752v1)
- **Role:** Supporting / neighboring survey
- **Primary capability:** A2 Information Need Formulation; A3 Information Space Exploration; A5 Evidence Integration
- **Secondary capabilities:** A4 Evidence Evaluation; A6 Adaptive Search Control
- **Learning mechanism:** Pipeline and optimization survey for autonomous research agents.
- **Learning target:** Planning, sub-question generation, web exploration, and report generation.
- **Epistemic objective:** Exploration/discovery and decision/action support.
- **Update target:** Plan, evidence state, report, and agent policy.
- **Generalization coverage:** Covers search-oriented and research-oriented benchmarks; cross-tool and cross-domain generalization is discussed as an open issue rather than used as the main taxonomy.
- **Why it matters:** Gives a clear boundary between deep search and deep research, and identifies planning, question developing, web exploration, and report generation as a pipeline.
- **Limitation for our survey:** Its four stages are workflow components. Our framework should treat them as manifestations of capabilities and test whether the capabilities survive new environments.
- **Research opportunity:** Ask whether report quality improvements come from better information seeking, better synthesis, more compute, or benchmark-specific workflow optimization.

---

## II. Core methods and evaluation papers

### M1. Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection

- **Authors / year:** Akari Asai et al., 2023; ICLR 2024
- **Link:** [arXiv:2310.11511](https://arxiv.org/html/2310.11511) · [ICLR paper](https://proceedings.iclr.cc/paper_files/paper/2024/file/25f7be9694d7b32d5cc670927b8091e-Paper-Conference.pdf)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A4 Evidence Evaluation; A5 Evidence Integration; A6 Adaptive Search Control
- **Learning mechanism:** B2 Behavior Learning; B3 Reward/Feedback Design; supervised training with reflection-token annotations distilled from a critic.
- **Learning target:** LLM generator and critic behavior.
- **Supervision:** Retrieval necessity, relevance, support/attribution, and usefulness judgments; the method uses reflection tokens.
- **Epistemic objective:** Grounding/acquisition and verification/disambiguation.
- **Update target:** Current generation/context and reflective control state.
- **What is learned:** The model learns to decide whether retrieval is needed, evaluate retrieved evidence, and critique its own output.
- **Method Effectiveness:** Medium.
- **Capability Isolation:** Medium.
- **Generalization Evidence:** Limited.
- **Training–Capability Alignment:** Partial; reflection labels directly approximate retrieval necessity and evidence support, but not long-horizon search.
- **Generalization:** Primarily In-distribution; limited evidence for Task OOD, Tool OOD, Environment OOD, or Behavior Transfer.
- **Why it belongs in the seed set:** It is the cleanest early example of separating “retrieval necessity” from unconditional retrieval and of making evidence evaluation part of the model’s generation behavior.
- **Research gap exposed:** Reflection labels largely describe local decisions and output support. They do not fully solve long-horizon query planning, source exploration, delayed credit assignment, or cross-environment transfer.

### M2. Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning

- **Authors / year:** Bowen Jin et al., 2025
- **Link:** [arXiv:2503.09516v5](https://arxiv.org/html/2503.09516v5) · [code](https://github.com/PeterGriffinJin/Search-R1)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A2 Information Need Formulation
- **Secondary capabilities:** A5 Evidence Integration; A6 Adaptive Search Control
- **Learning mechanism:** B4 Policy Optimization; outcome-based RL with multi-turn search/reasoning rollouts and retrieved-token masking.
- **Learning target:** LLM search-and-reasoning policy.
- **Supervision:** Primarily final answer/outcome reward plus format constraints.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** LLM policy and within-episode reasoning/search state.
- **What is learned:** The model autonomously issues multiple search queries during reasoning and integrates returned information into the trajectory.
- **Method Effectiveness:** Medium.
- **Capability Isolation:** Limited.
- **Generalization Evidence:** Limited.
- **Training–Capability Alignment:** Indirect; final answer reward is a proxy for search policy and cannot localize useful search steps.
- **Generalization:** Multiple QA datasets with a limited online-search transition; code the exact split as In-distribution, Task OOD, or Tool OOD rather than treating all datasets as equal transfer evidence.
- **Why it belongs in the seed set:** It is a canonical example of turning search into an RL environment action rather than a fixed RAG preprocessing step.
- **Research gap exposed:** It leaves open whether outcome-only reward teaches robust evidence evaluation or merely discovers answer-producing search patterns; it also raises cost, credit-assignment, and environment-shift questions.

### M3. R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning

- **Authors / year:** Huatong Song et al., 2025
- **Link:** [arXiv:2503.05592v2](https://arxiv.org/html/2503.05592v2)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A6 Adaptive Search Control (search invocation)
- **Secondary capabilities:** A2 Information Need Formulation; A5 Evidence Integration
- **Learning mechanism:** B4 Policy Optimization; two-stage outcome-based RL.
- **Learning target:** LLM policy for retrieval invocation and search-augmented reasoning.
- **Supervision:** Stage 1 retrieval/format reward; Stage 2 answer reward and format penalty.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** LLM policy and current reasoning state.
- **What is learned:** First, how to invoke the external retrieval environment; second, how to use retrieval to solve the task.
- **Method Effectiveness:** Medium.
- **Capability Isolation:** Medium.
- **Generalization Evidence:** Limited.
- **Training–Capability Alignment:** Partial; invocation reward teaches access while answer reward teaches use, but neither directly identifies necessary search actions.
- **Generalization:** Includes an unseen Bamboogle/online-search setting; record this as narrow Task OOD plus Tool OOD evidence, not as general Behavior Transfer.
- **Why it belongs in the seed set:** It operationalizes a useful decomposition: learning that search is available is different from learning how search contributes to task solving.
- **Research gap exposed:** The first-stage retrieval reward may incentivize unnecessary searching; the second-stage answer reward may not identify which search action was useful. This makes it central for studying search-worthiness and credit assignment.

### M4. ZeroSearch: Incentivize the Search Capability of LLMs without Searching

- **Authors / year:** Hao Sun et al., 2025; arXiv v3 revised 2026
- **Link:** [arXiv:2505.04588v3](https://arxiv.org/html/2505.04588v3) · [code](https://github.com/Alibaba-NLP/ZeroSearch)
- **Role:** Core
- **Contribution type:** Training Infrastructure
- **Primary capability:** Not Directly Trained; supports A6 Adaptive Search Control
- **Secondary capabilities:** A2 Information Need Formulation; A3 Information Space Exploration
- **Learning mechanism:** B1 Data/Trajectory Construction; B2 Behavior Learning; B4 Policy Optimization. A simulator LLM is SFT-trained to generate useful or noisy documents, then used in curriculum-based RL rollouts.
- **Learning target:** LLM search policy; simulator/retrieval module is also trained.
- **Supervision:** Useful/noisy document construction, curriculum quality degradation, and RL outcome feedback.
- **Epistemic objective:** Grounding/acquisition and exploration under controlled noise.
- **Update target:** LLM policy plus simulated search environment.
- **What is learned:** Search behavior without live search-engine interaction during RL training.
- **Method Effectiveness:** Medium.
- **Capability Isolation:** Limited.
- **Generalization Evidence:** Medium.
- **Training–Capability Alignment:** Indirect; the simulator and curriculum shape the policy signal, so simulator validity is part of the learned behavior.
- **Generalization:** Reports transfer across model sizes and inference with Google Web Search; code the simulator-to-real-search transition explicitly as Environment OOD and Tool OOD evidence.
- **Why it belongs in the seed set:** It raises a foundational question for the survey: if the training environment is simulated, what exactly has been learned—the search policy, a distributional prior about documents, or a simulator-specific interaction protocol?
- **Research gap exposed:** Simulator validity, information leakage, source diversity, adversarial noise, and calibration under real search-engine shift require stronger evaluation.

### M5. Beneficial Reasoning Behaviors in Agentic Search and Effective Training Methods to Obtain Them

- **Authors / year:** Jiahe Jin, Abhijay Sai Paladugu, Chenyan Xiong, Findings of ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.1400/) · [arXiv:2510.06534](https://arxiv.org/abs/2510.06534)
- **Role:** Core
- **Contribution type:** Capability Learning
- **Primary capability:** A4 Evidence Evaluation
- **Secondary capabilities:** A3 Information Space Exploration; A6 Adaptive Search Control (error recovery); A5 Evidence Integration
- **Learning mechanism:** B2 Behavior Learning followed by B4 Policy Optimization; behavior priming with SFT before RL.
- **Learning target:** Agentic search reasoning policy.
- **Supervision:** Trajectory-level behavior labels, followed by outcome-based RL.
- **Epistemic objective:** Verification/disambiguation and grounding/acquisition.
- **Update target:** LLM policy and search trajectory behavior.
- **What is learned:** Information Verification, Authority Evaluation, Adaptive Search, and Error Recovery.
- **Method Effectiveness:** Strong.
- **Capability Isolation:** Strong.
- **Generalization Evidence:** Medium.
- **Training–Capability Alignment:** Direct; behavior labels are explicitly constructed before RL and evaluated as behaviors rather than only final answers.
- **Generalization:** Reports web and multi-hop QA benchmark groups; record exact Task OOD or In-distribution status and distinguish behavior-level transfer from score transfer.
- **Why it belongs in the seed set:** It is unusually close to the survey’s central object: not simply whether the final answer is correct, but which search behaviors are learned and whether they help RL.
- **Research gap exposed:** The behavior ontology is promising but still mainly web-search/QA-centered. It does not yet establish whether these behaviors transfer to memory, scientific databases, enterprise search, or high-risk domains.

### M6. Agentic-R: Learning to Retrieve for Agentic Search

- **Authors / year:** Wenhan Liu et al., Findings of ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.785/) · [arXiv:2601.11888](https://arxiv.org/abs/2601.11888) · [code](https://github.com/8421BCD/Agentic-R)
- **Role:** Core
- **Contribution type:** Retriever Optimization
- **Primary capability:** Not Directly Trained; Retriever–Agent Co-adaptation
- **Secondary capabilities:** A2 Information Need Formulation; A4 Evidence Evaluation; A5 Evidence Integration; A6 Adaptive Search Control
- **Learning mechanism:** B6 Joint Optimization; iterative bidirectional optimization of the retriever and search agent.
- **Learning target:** Retriever and search-agent policy.
- **Supervision:** Local query-passage relevance plus global answer correctness; evolving agent-generated queries.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** Retriever, search policy, and their coupled interaction.
- **What is learned:** Passage utility is defined by downstream answer usefulness in multi-turn search, not only by local similarity; the retriever and agent improve each other iteratively.
- **Method Effectiveness:** Medium to Strong.
- **Capability Isolation:** Medium.
- **Generalization Evidence:** Limited.
- **Training–Capability Alignment:** Partial; local relevance and downstream utility jointly shape retriever updates, but global answer correctness remains a coarse proxy.
- **Generalization:** Multiple benchmarks and different search agents; code whether the result is Tool OOD, Task OOD, or only dataset-level In-distribution variation.
- **Why it belongs in the seed set:** It directly challenges the assumption that a passage useful for a query is necessarily useful for downstream reasoning, making it central to the “evidence utility versus topical relevance” distinction.
- **Research gap exposed:** Global answer correctness is still a coarse signal. More direct evidence-level credit assignment, sufficiency, contradiction handling, and cost-aware utility remain open.

### M7. Rethinking Reasoning-Intensive Retrieval: Evaluating and Advancing Retrievers in Agentic Search Systems

- **Authors / year:** Yilun Zhao et al., ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1705/)
- **Role:** Core / evaluation
- **Contribution type:** Evaluation
- **Primary capability:** Evaluation of Evidence Utility
- **Secondary capabilities:** A4 Evidence Evaluation
- **Learning mechanism:** Evaluation framework and retriever comparison; not primarily a post-training method.
- **Learning target:** Retriever quality as measured inside agentic workflows.
- **Supervision:** Expert-annotated reasoning aspects, positive documents, reference response, and evaluation rubrics.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** Retriever and agent–retriever interface.
- **What is learned:** The paper does not mainly train an LLM capability; it changes how reasoning-intensive retrievers should be evaluated, using Bright-Pro and downstream agentic utility.
- **Method Effectiveness:** Strong for the evaluation claim.
- **Capability Isolation:** Strong for evidence-utility measurement.
- **Generalization Evidence:** Medium across professional-domain queries and agentic workflows.
- **Training–Capability Alignment:** Not Applicable as a training method; the contribution is an evaluation signal for future retriever and policy learning.
- **Generalization:** Broad professional-domain queries and agentic evaluation; record Domain OOD and system configuration separately, without treating the benchmark alone as Behavior Transfer.
- **Why it belongs in the seed set:** It provides an essential measurement anchor for the survey’s claim that task success, evidence quality, and retrieval utility are not the same thing.
- **Research gap exposed:** The next step is to connect downstream utility metrics with learned search behavior and transfer, rather than only evaluating retrievers within fixed agent workflows.

---

## Initial coding matrix

| Paper | Contribution type | Primary capability | Main learning mechanism | Method effectiveness | Capability isolation | Generalization evidence | Training–capability alignment |
|---|---|---|---|---|---|---|---|
| Deep Search survey | Survey / taxonomy | A2/A3/A6 | Taxonomy of methods | Not tested | Not tested | Not tested | Not applicable |
| RL-Agentic Search survey | Survey / taxonomy | A6 | RL taxonomy | Not tested | Not tested | Not tested | Not applicable |
| Deep Research survey | Survey / taxonomy | A2/A3/A5 | Pipeline/optimization | Not tested | Not tested | Not tested | Not applicable |
| Self-RAG | Capability Learning | A1 | Reflection-token SFT/distillation | Medium | Medium | Limited | Partial: reflection labels approximate necessity and support |
| Search-R1 | Capability Learning | A2 | Outcome-based RL | Medium | Limited | Limited | Indirect: final answer reward proxies search policy |
| R1-Searcher | Capability Learning | A6 / invocation | Two-stage outcome RL | Medium | Medium | Limited | Partial: invocation reward teaches access, answer reward teaches use |
| ZeroSearch | Training Infrastructure | Not directly trained; supports A6 | Simulator SFT + curriculum RL | Medium | Limited | Medium | Indirect: simulator quality shapes the policy signal |
| Behavior Priming | Capability Learning | A4 | Behavior SFT + RL | Strong | Strong | Medium | Relatively direct: behavior labels precede RL |
| Agentic-R | Retriever Optimization | Retriever–Agent Co-adaptation | Retriever-agent joint optimization | Medium–Strong | Medium | Limited | Partial: local relevance + downstream utility |
| Bright-Pro | Evaluation | Evidence Utility | Agentic evaluation | Strong | Strong | Medium | Evaluation-oriented, not a training signal |

## Immediate observations from round 1

1. **A1 and A4 are more directly observable than A3 in current training papers.** Self-RAG explicitly teaches retrieval necessity and critique; Behavior Priming explicitly identifies verification and authority behaviors. Search-R1 and R1-Searcher often infer capability from final outcomes.
2. **The central gap is not the absence of search training, but weak capability isolation.** Many methods train or evaluate a full search-and-answer loop, making it difficult to attribute gains to query quality, information-space exploration, evidence evaluation, or adaptive control.
3. **Outcome reward is a recurring bottleneck.** It is scalable and often effective, but it provides weak attribution over multi-turn search actions and can reward benchmark-specific shortcuts. This is a training–capability alignment problem, not merely a reward-engineering detail.
4. **The retriever is becoming downstream-task-aware.** Agentic-R and Bright-Pro shift from topical similarity to reasoning utility and workflow-level evaluation. This is relevant to evidence utility, but neither result alone proves a transferable LLM capability.
5. **Environment validity is under-studied.** ZeroSearch makes the search environment controllable and cheap, but the simulator-to-real gap should be treated as a first-class C5 Environment OOD question.
6. **Current papers are heavily QA/web-centric.** Memory, scientific search, enterprise search, medical search, recommendation retrieval, and long-term state update should be added only when they test the same capability-learning questions rather than as disconnected application categories.

## Inclusion status for next rounds

- **Include in core reading:** Self-RAG, Search-R1, R1-Searcher, Behavior Priming, Agentic-R, Bright-Pro.
- **Include as field-map anchors:** Deep Search survey, RL-based Agentic Search survey, Deep Research survey.
- **Do not yet treat as proof of general capability:** any paper reporting only final QA accuracy without behavior-level or transfer-level measurement.
- **Do not force a primary A1–A6 label:** infrastructure, retriever optimization, and evaluation papers may be `Not Directly Trained` while remaining central to the survey.
- **Next search boundary:** A1 epistemic awareness and retrieval necessity; A4 evidence evaluation/verification; memory/state-update papers; then C1–C7 transfer and evaluation evidence.
