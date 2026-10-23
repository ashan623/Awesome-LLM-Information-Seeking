# First-Round Seed Papers

**Scope:** field-map and seed verification round, 2026-10-23. This file contains three neighboring surveys and seven representative method/evaluation papers. It is a curated starting set, not an exhaustive bibliography.

## How to read the annotations

- **Primary capability** is the main information-seeking behavior directly studied or trained.
- **Learning mechanism** records the mechanism used to acquire that behavior, not merely the paper's implementation detail.
- **Evidence strength** distinguishes claims from direct measurements. `Strong` means the paper includes behavior-level, transfer, or environment-level evidence; `Medium` means the method is evaluated beyond a trivial baseline but capability isolation is incomplete; `Limited` means the main evidence is downstream task accuracy.
- **Generalization** is marked only when the paper reports an explicit test beyond the main training setup. A higher score on an in-domain QA benchmark is not counted as capability transfer.

---

## I. Neighboring surveys

### S1. A Survey of LLM-based Deep Search Agents: Paradigm, Optimization, Evaluation, and Challenges

- **Authors / year:** Yunjia Xi et al., 2025
- **Link:** [arXiv:2508.05668v3](https://arxiv.org/html/2508.05668v3)
- **Role:** Core / neighboring survey
- **Primary capability:** A2 Information Need Formulation; A3 Source Navigation; A6 Adaptive Control
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
- **Primary capability:** A6 Adaptive Control
- **Secondary capabilities:** A2 Information Need Formulation; A3 Source Navigation; A4 Evidence Evaluation; A5 Evidence Integration
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
- **Primary capability:** A2 Information Need Formulation; A3 Source Navigation; A5 Evidence Integration
- **Secondary capabilities:** A4 Evidence Evaluation; A6 Adaptive Control
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
- **Link:** [arXiv:2310.11511](https://arxiv.org/html/2310.11511) · [ICLR paper](https://proceedings.iclr.cc/paper_files/paper/2024/file/25f7be9694d7b32d5cc670927b8091e1-Paper-Conference.pdf)
- **Role:** Core
- **Primary capability:** A1 Epistemic Awareness
- **Secondary capabilities:** A4 Evidence Evaluation; A5 Evidence Integration; A6 Adaptive Control
- **Learning mechanism:** B2 Behavior Learning; B3 Reward/Feedback Design; supervised training with reflection-token annotations distilled from a critic.
- **Learning target:** LLM generator and critic behavior.
- **Supervision:** Retrieval necessity, relevance, support/attribution, and usefulness judgments; the method uses reflection tokens.
- **Epistemic objective:** Grounding/acquisition and verification/disambiguation.
- **Update target:** Current generation/context and reflective control state.
- **What is learned:** The model learns to decide whether retrieval is needed, evaluate retrieved evidence, and critique its own output.
- **Evidence strength:** Medium. It directly defines and trains retrieval-on-demand and critique signals, but broad transfer across tools and environments is not the central experiment.
- **Generalization:** Limited relative to the survey’s C-axis; evaluate as a foundational awareness/evidence-control method, not as proof of general search policy learning.
- **Why it belongs in the seed set:** It is the cleanest early example of separating “retrieval necessity” from unconditional retrieval and of making evidence evaluation part of the model’s generation behavior.
- **Research gap exposed:** Reflection labels largely describe local decisions and output support. They do not fully solve long-horizon query planning, source exploration, delayed credit assignment, or cross-environment transfer.

### M2. Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning

- **Authors / year:** Bowen Jin et al., 2025
- **Link:** [arXiv:2503.09516v5](https://arxiv.org/html/2503.09516v5) · [code](https://github.com/PeterGriffinJin/Search-R1)
- **Role:** Core
- **Primary capability:** A2 Information Need Formulation
- **Secondary capabilities:** A3 Source Navigation; A5 Evidence Integration; A6 Adaptive Control
- **Learning mechanism:** B4 Policy Optimization; outcome-based RL with multi-turn search/reasoning rollouts and retrieved-token masking.
- **Learning target:** LLM search-and-reasoning policy.
- **Supervision:** Primarily final answer/outcome reward plus format constraints.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** LLM policy and within-episode reasoning/search state.
- **What is learned:** The model autonomously issues multiple search queries during reasoning and integrates returned information into the trajectory.
- **Evidence strength:** Medium. It compares against RAG baselines and evaluates multiple QA datasets, but outcome reward makes it difficult to isolate query quality, evidence quality, and search necessity.
- **Generalization:** Reports several datasets and online-search settings; code the exact split as in-domain, out-of-domain, or tool transfer rather than treating all datasets as equal transfer evidence.
- **Why it belongs in the seed set:** It is a canonical example of turning search into an RL environment action rather than a fixed RAG preprocessing step.
- **Research gap exposed:** It leaves open whether outcome-only reward teaches robust evidence evaluation or merely discovers answer-producing search patterns; it also raises cost, credit-assignment, and environment-shift questions.

### M3. R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning

- **Authors / year:** Hao Song et al., 2025
- **Link:** [arXiv:2503.05592v2](https://arxiv.org/html/2503.05592v2)
- **Role:** Core
- **Primary capability:** A3 Source Navigation
- **Secondary capabilities:** A2 Information Need Formulation; A5 Evidence Integration; A6 Adaptive Control
- **Learning mechanism:** B4 Policy Optimization; two-stage outcome-based RL.
- **Learning target:** LLM policy for retrieval invocation and search-augmented reasoning.
- **Supervision:** Stage 1 retrieval/format reward; Stage 2 answer reward and format penalty.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** LLM policy and current reasoning state.
- **What is learned:** First, how to invoke the external retrieval environment; second, how to use retrieval to solve the task.
- **Evidence strength:** Medium. The staged reward design exposes a meaningful distinction between tool accessibility and evidence utilization, but direct capability isolation remains incomplete.
- **Generalization:** Includes an unseen Bamboogle/online-search setting according to the paper; record this as a useful but narrow tool/domain transfer test.
- **Why it belongs in the seed set:** It operationalizes a useful decomposition: learning that search is available is different from learning how search contributes to task solving.
- **Research gap exposed:** The first-stage retrieval reward may incentivize unnecessary searching; the second-stage answer reward may not identify which search action was useful. This makes it central for studying search-worthiness and credit assignment.

### M4. ZeroSearch: Incentivize the Search Capability of LLMs without Searching

- **Authors / year:** Hao Sun et al., 2025
- **Link:** [arXiv:2505.04588v2](https://arxiv.org/html/2505.04588v2) · [code](https://github.com/Alibaba-NLP/ZeroSearch)
- **Role:** Core
- **Primary capability:** A3 Source Navigation
- **Secondary capabilities:** A2 Information Need Formulation; A6 Adaptive Control
- **Learning mechanism:** B1 Data/Trajectory Construction; B2 Behavior Learning; B4 Policy Optimization. A simulator LLM is SFT-trained to generate useful or noisy documents, then used in curriculum-based RL rollouts.
- **Learning target:** LLM search policy; simulator/retrieval module is also trained.
- **Supervision:** Useful/noisy document construction, curriculum quality degradation, and RL outcome feedback.
- **Epistemic objective:** Grounding/acquisition and exploration under controlled noise.
- **Update target:** LLM policy plus simulated search environment.
- **What is learned:** Search behavior without live search-engine interaction during RL training.
- **Evidence strength:** Medium. It directly studies environment construction and scalability, but a simulated search engine may encode the same knowledge and regularities as the policy model.
- **Generalization:** Reports transfer across model sizes and inference with Google Web Search; code the simulator-to-real-search transition explicitly as tool/environment transfer.
- **Why it belongs in the seed set:** It raises a foundational question for the survey: if the training environment is simulated, what exactly has been learned—the search policy, a distributional prior about documents, or a simulator-specific interaction protocol?
- **Research gap exposed:** Simulator validity, information leakage, source diversity, adversarial noise, and calibration under real search-engine shift require stronger evaluation.

### M5. Beneficial Reasoning Behaviors in Agentic Search and Effective Training Methods to Obtain Them

- **Authors / year:** Jiahe Jin, Abhijay Sai Paladugu, Chenyan Xiong, Findings of ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.1400/) · [arXiv:2510.06534](https://arxiv.org/abs/2510.06534)
- **Role:** Core
- **Primary capability:** A4 Evidence Evaluation
- **Secondary capabilities:** A3 Source Navigation; A6 Adaptive Control; A5 Evidence Integration
- **Learning mechanism:** B2 Behavior Learning followed by B4 Policy Optimization; behavior priming with SFT before RL.
- **Learning target:** Agentic search reasoning policy.
- **Supervision:** Trajectory-level behavior labels, followed by outcome-based RL.
- **Epistemic objective:** Verification/disambiguation and grounding/acquisition.
- **Update target:** LLM policy and search trajectory behavior.
- **What is learned:** Information Verification, Authority Evaluation, Adaptive Search, and Error Recovery.
- **Evidence strength:** Strong relative to the seed set. It compares successful and failed trajectories, directly identifies behavior patterns, and evaluates behavior priming before RL.
- **Generalization:** Reports web and multi-hop QA benchmark groups; record exact benchmark transfer and distinguish behavior-level transfer from score transfer.
- **Why it belongs in the seed set:** It is unusually close to the survey’s central object: not simply whether the final answer is correct, but which search behaviors are learned and whether they help RL.
- **Research gap exposed:** The behavior ontology is promising but still mainly web-search/QA-centered. It does not yet establish whether these behaviors transfer to memory, scientific databases, enterprise search, or high-risk domains.

### M6. Agentic-R: Learning to Retrieve for Agentic Search

- **Authors / year:** Wenhan Liu et al., Findings of ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.785/) · [arXiv:2601.11888](https://arxiv.org/abs/2601.11888) · [code](https://github.com/8421BCD/Agentic-R)
- **Role:** Core
- **Primary capability:** A5 Evidence Integration
- **Secondary capabilities:** A2 Information Need Formulation; A4 Evidence Evaluation; A6 Adaptive Control
- **Learning mechanism:** B6 Joint Optimization; iterative bidirectional optimization of the retriever and search agent.
- **Learning target:** Retriever and search-agent policy.
- **Supervision:** Local query-passage relevance plus global answer correctness; evolving agent-generated queries.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** Retriever, search policy, and their coupled interaction.
- **What is learned:** Passage utility is defined by downstream answer usefulness in multi-turn search, not only by local similarity; the retriever and agent improve each other iteratively.
- **Evidence strength:** Medium to strong for joint optimization; the paper evaluates multiple single-hop and multi-hop QA benchmarks but does not by itself establish broad cross-tool transfer.
- **Generalization:** Multiple benchmarks and different search agents; code whether the transfer is across agents, tasks, or only datasets.
- **Why it belongs in the seed set:** It directly challenges the assumption that a passage useful for a query is necessarily useful for downstream reasoning, making it central to the “evidence utility versus topical relevance” distinction.
- **Research gap exposed:** Global answer correctness is still a coarse signal. More direct evidence-level credit assignment, sufficiency, contradiction handling, and cost-aware utility remain open.

### M7. Rethinking Reasoning-Intensive Retrieval: Evaluating and Advancing Retrievers in Agentic Search Systems

- **Authors / year:** Yilun Zhao et al., ACL 2026
- **Link:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1705/)
- **Role:** Core / evaluation
- **Primary capability:** A5 Evidence Integration
- **Secondary capabilities:** A4 Evidence Evaluation
- **Learning mechanism:** Evaluation framework and retriever comparison; not primarily a post-training method.
- **Learning target:** Retriever quality as measured inside agentic workflows.
- **Supervision:** Expert-annotated reasoning aspects, positive documents, reference response, and evaluation rubrics.
- **Epistemic objective:** Grounding/acquisition and decision/action support.
- **Update target:** Retriever and agent–retriever interface.
- **What is learned:** The paper does not mainly train an LLM capability; it changes how reasoning-intensive retrievers should be evaluated, using Bright-Pro and downstream agentic utility.
- **Evidence strength:** Strong for the evaluation claim; it explicitly criticizes isolated retriever evaluation and measures practical utility inside agentic systems.
- **Generalization:** Broad professional-domain queries and agentic evaluation; record the domains and system configurations separately.
- **Why it belongs in the seed set:** It provides an essential measurement anchor for the survey’s claim that task success, evidence quality, and retrieval utility are not the same thing.
- **Research gap exposed:** The next step is to connect downstream utility metrics with learned search behavior and transfer, rather than only evaluating retrievers within fixed agent workflows.

---

## Initial coding matrix

| Paper | Primary capability | Main learning mechanism | Main update target | Strongest current evidence | Main unresolved question |
|---|---|---|---|---|---|
| Deep Search survey | A2/A3/A6 | Taxonomy of methods | Agent/system | Field map | How to separate capability from pipeline? |
| RL-Agentic Search survey | A6 | RL taxonomy | Policy/modules/system | Functional RL roles | Does RL learn transferable information seeking? |
| Deep Research survey | A2/A3/A5 | Pipeline/optimization | Plan/state/report | Benchmark map | How much is search versus synthesis? |
| Self-RAG | A1 | Reflection-token SFT/distillation | Generator/critic | Retrieval necessity and evidence critique | Long-horizon and cross-tool transfer |
| Search-R1 | A2 | Outcome-based RL | Search-reasoning policy | Multi-turn search behavior | Outcome reward and credit assignment |
| R1-Searcher | A3 | Two-stage outcome RL | Search policy | Tool invocation vs usage split | Unnecessary search and evidence attribution |
| ZeroSearch | A3 | Simulator SFT + curriculum RL | Policy + environment | Simulator-based scalable training | Simulator-to-real transfer |
| Behavior Priming | A4 | Behavior SFT + RL | Search reasoning policy | Direct behavior analysis | Cross-domain capability transfer |
| Agentic-R | A5 | Retriever-agent joint optimization | Retriever + policy | Downstream passage utility | Fine-grained evidence credit |
| Bright-Pro | A5/A4 | Agentic evaluation | Retriever/system | Workflow-level retrieval utility | Behavior-level and transfer evaluation |

## Immediate observations from round 1

1. **A1 and A4 are more directly observable than A3 in current training papers.** Self-RAG explicitly teaches retrieval necessity and critique; Behavior Priming explicitly identifies verification and authority behaviors. Search-R1 and R1-Searcher often infer capability from final outcomes.
2. **The central gap is not the absence of search training, but weak capability isolation.** Many methods train or evaluate a full search-and-answer loop, making it difficult to attribute gains to query quality, source navigation, evidence evaluation, or reasoning.
3. **Outcome reward is a recurring bottleneck.** It is scalable and often effective, but it provides weak attribution over multi-turn search actions and can reward benchmark-specific shortcuts.
4. **The retriever is becoming downstream-task-aware.** Agentic-R and Bright-Pro shift from topical similarity to reasoning utility and workflow-level evaluation. This is highly relevant to A5 Evidence Integration.
5. **Environment validity is under-studied.** ZeroSearch makes the search environment controllable and cheap, but the simulator-to-real gap should be treated as a first-class C3/C4 generalization question.
6. **Current papers are heavily QA/web-centric.** Memory, scientific search, enterprise search, medical search, recommendation retrieval, and long-term state update should be added in later rounds rather than mixed into the initial map.

## Inclusion status for next rounds

- **Include in core reading:** Self-RAG, Search-R1, R1-Searcher, Behavior Priming, Agentic-R, Bright-Pro.
- **Include as field-map anchors:** Deep Search survey, RL-based Agentic Search survey, Deep Research survey.
- **Do not yet treat as proof of general capability:** any paper reporting only final QA accuracy without behavior-level or transfer-level measurement.
- **Next search boundary:** A1 epistemic awareness and retrieval necessity; A4 evidence evaluation/verification; memory/state-update papers; then C1–C6 transfer and evaluation papers.
