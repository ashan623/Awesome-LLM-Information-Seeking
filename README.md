<div align="center">

# Beyond Retrieval

### Learning Information-Seeking Capabilities in Large Language Models

A curated paper list for understanding **what information-seeking abilities LLMs should learn, how post-training teaches them, and whether those abilities generalize beyond specific search tasks and environments**.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Papers](https://img.shields.io/badge/Papers-46-blue.svg)](./papers/a5-evidence-integration-second.md)
[![Status](https://img.shields.io/badge/Status-A5.1--A5.3%20Map%20Closed-orange.svg)](./notes/search-log-2026-10-24-round-4-a5-final.md)
[![Contributions](https://img.shields.io/badge/Contributions-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Last Commit](https://img.shields.io/github/last-commit/ashan623/awesome/main?logo=github&color=blue)](https://github.com/ashan623/awesome/commits/main)

</div>

## Content

- [Research Questions](#research-questions)
- [Taxonomy](#taxonomy)
  - [Capability: What to Learn](#capability-what-to-learn)
  - [Learning: How to Learn](#learning-how-to-learn)
  - [Generalization: Does It Transfer](#generalization-does-it-transfer)
  - [Auxiliary and Evidence Labels](#auxiliary-and-evidence-labels)
- [Keywords Convention](#keywords-convention)
- [Papers](#papers)
  - [Neighboring Surveys](#neighboring-surveys)
  - [A1 Search Necessity and Epistemic Awareness](#a1-search-necessity-and-epistemic-awareness)
  - [A1 Retrieval Utility and Value of Information](#a1-retrieval-utility-and-value-of-information)
  - [A1 Utility-aware Policy Learning](#a1-utility-aware-policy-learning)
  - [A4 Evidence Evaluation](#a4-evidence-evaluation)
  - [A5 Evidence Integration — First Batch](#a5-evidence-integration--first-batch)
  - [A5 Evidence Integration — Second Batch](#a5-evidence-integration--second-batch)
  - [Retrieval Necessity and Evidence Critique](#retrieval-necessity-and-evidence-critique)
  - [Search Policy and Reinforcement Learning](#search-policy-and-reinforcement-learning)
  - [Retriever--Agent Coupling and Evaluation](#retriever--agent-coupling-and-evaluation)
- [Reading Notes](#reading-notes)
- [Search Protocol](#search-protocol)
- [Contributing](#contributing)

## Research Questions

This collection is built around three questions:

1. **What to Learn?** What information-seeking capabilities should a foundation model acquire?
2. **How to Learn?** How do data construction, SFT, behavior learning, process supervision, RL, distillation, and joint optimization teach these capabilities?
3. **Does It Generalize?** Does the model learn transferable information-seeking behavior, or only benchmark-specific tool-use patterns?

The central distinction is between **using a search tool** and **learning how to seek information**. A successful model should know when information is missing, formulate a useful information need, navigate sources, evaluate evidence, integrate it into reasoning or memory, and stop or recover adaptively.

## Taxonomy

### Capability: What to Learn

| Code | Capability | Guiding question |
|:---:|---|---|
| **A1** | **Epistemic Awareness** | Does the model know what it does not know, when retrieval is necessary, and when search has value? |
| **A2** | **Information Need Formulation** | Can it turn a task goal and knowledge gap into a query, sub-question, or search plan? |
| **A3** | **Information Space Exploration** | Can it explore unknown information spaces, discover new paths, and move across sources? |
| **A4** | **Evidence Evaluation** | Can it judge evidence quality, claim support, source credibility, sufficiency, contradiction, and reliability? |
| **A5** | **Evidence Integration** | Can it correctly use evaluated evidence to update reasoning, memory, beliefs, generation, or decisions? |
| **A6** | **Adaptive Search Control** | Can it decide when to search, adjust the search behavior, recover, stop, and trade off utility against cost? |

### Learning: How to Learn

| Code | Mechanism | Typical signals or objects |
|:---:|---|---|
| **B1** | **Data / Trajectory Construction** | Search trajectories, success/failure pairs, synthetic environments, curriculum rollouts |
| **B2** | **Behavior Learning** | SFT, imitation learning, behavior priming, trajectory distillation |
| **B3** | **Reward / Feedback Design** | Outcome, process, evidence, preference, information gain, cost, and risk feedback |
| **B4** | **Policy Optimization** | RL, GRPO, PPO, REINFORCE, and test-time policy optimization |
| **B5** | **Credit Assignment** | Attributing downstream success or failure to multi-turn search actions |
| **B6** | **Joint Optimization** | Retriever–agent, reasoner–retriever, verifier–policy, memory–policy, and system co-training |

### Generalization: Does It Transfer?

| Code | Generalization regime | What to test |
|:---:|---|---|
| **C1** | **In-distribution** | Training and test tasks, tools, and corpora are substantially aligned |
| **C2** | **Task OOD** | A new task type or reasoning form |
| **C3** | **Domain OOD** | A new knowledge domain such as science, medicine, enterprise, or e-commerce |
| **C4** | **Tool OOD** | A different search interface, retriever, API, browser, database, or memory backend |
| **C5** | **Environment OOD** | New or changing corpora, temporal knowledge, source conflicts, noise, or simulator-to-real shift |
| **C6** | **Behavior Transfer** | Whether the target search behavior itself survives in a new setting |
| **C7** | **Budget / Cost Generalization** | Different token, latency, API-call, memory, or monetary budgets |

### Auxiliary and Evidence Labels

- **D — Epistemic Objective:** grounding/acquisition, verification/disambiguation, exploration/discovery, decision/action support, or learning/update.
- **E — Update Target:** context/belief, plan/policy state, memory/index, retriever/verifier, LLM policy, or joint system.
- **Contribution Type:** `Capability Learning`, `Training Infrastructure`, `Retriever Optimization`, or `Evaluation`.
- **Evidence Vector:** separately record `Method Effectiveness`, `Capability Isolation`, and `Generalization Evidence` as `Strong`, `Medium`, `Limited`, or `Not Tested`.
- **Training–Capability Alignment:** record whether the training signal directly reflects the claimed capability, partially reflects it, or is only an indirect downstream proxy.
- **Role:** `Core`, `Supporting`, `Background`, or `Evaluation`.

## Keywords Convention

![Survey](https://img.shields.io/badge/Survey-4c78a8.svg)
![Method](https://img.shields.io/badge/Method-59a14f.svg)
![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg)
![A1 Awareness](https://img.shields.io/badge/A1%20Awareness-2a9d8f.svg)
![A2 Need Formulation](https://img.shields.io/badge/A2%20Need%20Formulation-457b9d.svg)
![A3 Navigation](https://img.shields.io/badge/A3%20Navigation-f4a261.svg)
![A4 Evidence](https://img.shields.io/badge/A4%20Evidence-e76f51.svg)
![A5 Integration](https://img.shields.io/badge/A5%20Integration-8d5a97.svg)
![A6 Control](https://img.shields.io/badge/A6%20Control-6c757d.svg)

Conference and publication badges use the same compact convention as the reference Awesome list, for example `Arxiv2025`, `ICLR2024`, and `ACL2026`.

## Papers

### Neighboring Surveys

- **A Survey of LLM-based Deep Search Agents: Paradigm, Optimization, Evaluation, and Challenges**<br>
  *Yunjia Xi et al.* [[paper](https://arxiv.org/html/2508.05668v3)] 2025. ![Survey](https://img.shields.io/badge/Survey-4c78a8.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A3](https://img.shields.io/badge/A3-f4a261.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **A Comprehensive Survey on Reinforcement Learning-based Agentic Search: Foundations, Roles, Optimizations, Evaluations, and Applications**<br>
  *Minhua Lin et al.* [[paper](https://arxiv.org/html/2510.16724)] 2025. ![Survey](https://img.shields.io/badge/Survey-4c78a8.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **Deep Research: A Survey of Autonomous Research Agents**<br>
  *Wenlin Zhang et al.* [[paper](https://arxiv.org/html/2508.12752v1)] 2025. ![Survey](https://img.shields.io/badge/Survey-4c78a8.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A3](https://img.shields.io/badge/A3-f4a261.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

### A1 Search Necessity and Epistemic Awareness

- **When to Retrieve: Teaching LLMs to Utilize Information Retrieval Effectively**<br>
  *Tiziano Labruna et al.* [[paper](https://aclanthology.org/2025.ranlp-1.73/)] [[code](https://github.com/tLabruna/Adapt-LLM)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/RANLP2025-orange.svg)

- **KBM: Delineating Knowledge Boundary for Adaptive Retrieval in Large Language Models**<br>
  *Zhen Zhang et al.* [[paper](https://arxiv.org/abs/2411.06207)] 2024/2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **DRAGIN: Dynamic Retrieval Augmented Generation based on the Real-time Information Needs of Large Language Models**<br>
  *Weihang Su et al.* [[paper](https://aclanthology.org/2024.acl-long.702/)] [[code](https://github.com/oneal2000/DRAGIN)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/ACL2024-orange.svg)

- **SeaKR: Self-aware Knowledge Retrieval for Adaptive Retrieval Augmented Generation**<br>
  *Zijun Yao et al.* [[paper](https://aclanthology.org/2025.acl-long.1312/)] [[code](https://github.com/THU-KEG/SeaKR)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/ACL2025-orange.svg)

- **Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home**<br>
  *Viktor Moskvoretskii et al.* [[paper](https://aclanthology.org/2025.acl-long.319/)] [[code](https://github.com/s-nlp/AdaRAGUE)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/ACL2025-orange.svg)

### A1 Retrieval Utility and Value of Information

- **LLM-Specific Utility for Retrieval-Augmented Generation**<br>
  *Hengran Zhang et al.* [[paper](https://arxiv.org/abs/2510.11358)] [[code](https://github.com/Trustworthy-Information-Access/LLM_specific_utility)] 2025/2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/CIKM2026-orange.svg)

- **When Knowledge Is Not Free: Cost-Aware Evidence Selection in Retrieval-Augmented Generation**<br>
  *Mingyan Wu et al.* [[paper](https://arxiv.org/abs/2606.02245v1)] [[code](https://github.com/Mignonmy/Cost-Aware)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/Cost--Aware-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **Value of Information: A Framework for Human–Agent Communication**<br>
  *Yijiang River Dong et al.* [[paper](https://aclanthology.org/2026.acl-long.1987/)] [[code](https://github.com/dong-river/VOI_communication)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Redefining Retrieval Evaluation in the Era of LLMs**<br>
  *Giovanni Trappolini et al.* [[paper](https://aclanthology.org/2026.eacl-long.391/)] [[code](https://github.com/GiovanniTRA/UDCG)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/EACL2026-orange.svg)

### A1 Utility-aware Policy Learning

- **s3: You Don’t Need That Much Data to Train a Search Agent via RL**<br>
  *Pengcheng Jiang et al.* [[paper](https://aclanthology.org/2025.emnlp-main.1095/)] [[code](https://github.com/pat-jj/s3)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **Optimizing Agentic Reasoning with Retrieval via Synthetic Semantic Information Gain Reward**<br>
  *Senkang Hu et al.* [[paper](https://arxiv.org/abs/2602.00845v3)] [[code](https://github.com/dl-m9/InfoReasoner)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ICML2026-orange.svg)

- **CoBRA: Learning Tool-Use Boundaries via Counterfactual Margins**<br>
  *Wenhao Zou et al.* [[paper](https://arxiv.org/abs/2609.00967)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/Counterfactual-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents**<br>
  *Wenxuan Ding et al.* [[paper](https://arxiv.org/abs/2602.16699v3)] [[code](https://github.com/NSF-Simons-CosmicAI-Institute/CalibrateThenAct)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/Cost--Aware-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **IKEA: Reinforced Internal-External Knowledge Synergistic Reasoning**<br>
  *Ziyang Huang et al.* [[paper](https://arxiv.org/abs/2505.07596)] [[code](https://github.com/hzy312/knowledge-r1)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **Self-Knowledge Guided Retrieval Augmentation for Large Language Models**<br>
  *Yile Wang et al.* [[paper](https://aclanthology.org/2023.findings-emnlp.691/)] [[code](https://github.com/THUNLP-MT/SKR)] 2023. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/EMNLP2023-orange.svg)

### A4 Evidence Evaluation

- **Corrective Retrieval Augmented Generation**<br>
  *Shi-Qi Yan et al.* [[paper](https://arxiv.org/abs/2401.15884)] [[code](https://github.com/HuskyInSalt/CRAG)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![](https://img.shields.io/badge/Arxiv2024-orange.svg)

- **S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA**<br>
  *Minghan Li et al.* [[paper](https://aclanthology.org/2026.acl-long.1185/)] [[code](https://github.com/nianaaa/S2G-RAG)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models**<br>
  *Cheng Niu et al.* [[paper](https://aclanthology.org/2024.acl-long.585/)] [[code](https://github.com/particlemedia/ragtruth)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/ACL2024-orange.svg)

- **RAGChecker: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation**<br>
  *Dongyu Ru et al.* [[paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/27245589131d17368cccdfa990cbf16e-Abstract-Datasets_and_Benchmarks_Track.html)] [[code](https://github.com/amazon-science/RAGChecker)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![](https://img.shields.io/badge/NeurIPS2024-orange.svg)

- **RLSeek: Evidence-Grounded Reasoning for RAG Hallucination Detection**<br>
  *Zhaoheng Huang et al.* [[paper](https://aclanthology.org/2026.acl-long.1492/)] [[code](https://github.com/WaldenRUC/RLSeek)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Evidence-Augmented Policy Optimization with Reward Co-Evolution for Long-Context Reasoning**<br>
  *Xin Guan et al.* [[paper](https://aclanthology.org/2026.acl-long.1404/)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **AuthorityBench: Benchmarking LLM Authority Perception for Reliable Retrieval-Augmented Generation**<br>
  *Zhihui Yao et al.* [[paper](https://arxiv.org/abs/2603.25092)] [[code](https://github.com/Trustworthy-Information-Access/AuthorityBench)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **Resolving Conflicting Evidence in Automated Fact-Checking: A Study on Retrieval-Augmented LLMs**<br>
  *Ziyu Ge et al.* [[paper](https://www.ijcai.org/proceedings/2025/1073)] [[code](https://github.com/zoeyyes/CONFACT)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/IJCAI2025-orange.svg)

- **Sufficient Context: A New Lens on Retrieval Augmented Generation Systems**<br>
  *Hailey Joren et al.* [[paper](https://arxiv.org/abs/2411.06037v3)] [[code](https://github.com/hljoren/sufficientcontext)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![](https://img.shields.io/badge/ICLR2025-orange.svg)

- **Benchmarking LLM’s Capability in Reasoning over Conflicting Web References**<br>
  *Yizhen Yuan et al.* [[paper](https://aclanthology.org/2026.acl-long.11/)] [[code](https://github.com/XaiverYuan/ConfRAG)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### A5 Evidence Integration — First Batch

- **ClashEval: Quantifying the Tug-of-War between an LLM’s Internal Prior and External Evidence**<br>
  *Kevin Wu et al.* [[paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/3aa291abc426d7a29fb08418c1244177-Abstract-Datasets_and_Benchmarks_Track.html)] [[code](https://github.com/kevinwu23/StanfordClashEval)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/NeurIPS2024-orange.svg)

- **FaithfulRAG: Fact-Level Conflict Modeling for Context-Faithful Retrieval-Augmented Generation**<br>
  *Qinggang Zhang et al.* [[paper](https://aclanthology.org/2025.acl-long.1062/)] [[code](https://github.com/XMUDeepLIT/Faithful-RAG)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/ACL2025-orange.svg)

- **Belief Revision: The Adaptability of Large Language Models Reasoning**<br>
  *Bryan Wilie et al.* [[paper](https://aclanthology.org/2024.emnlp-main.586/)] [[code](https://github.com/HLTCHKUST/belief-revision)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

- **GuarantRAG: Guaranteeing Knowledge Integration with Joint Decoding for Retrieval-Augmented Generation**<br>
  *Zhengyi Zhao et al.* [[paper](https://aclanthology.org/2026.acl-long.192/)] [[paper](https://arxiv.org/abs/2604.08046)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Efficient Prior-Guided Reasoning for Robust Retrieval-Augmented Generation under Conflicts**<br>
  *Xiaowei Yuan et al.* [[paper](https://aclanthology.org/2026.acl-long.1013/)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **How Large Language Models Balance Internal Knowledge with User and Document Assertions**<br>
  *Shuowei Li et al.* [[paper](https://aclanthology.org/2026.findings-acl.1267/)] [[code](https://github.com/shuowl/llm-source-balancing)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### A5 Evidence Integration — Second Batch

- **RAFT: Adapting Language Model to Domain Specific RAG**<br>
  *Tianjun Zhang et al.* [[paper](https://openreview.net/forum?id=rzQGHXNReU)] [[code](https://github.com/ShishirPatil/gorilla/tree/main/raft)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/COLM2024-orange.svg)

- **BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression**<br>
  *Yuankai Li et al.* [[paper](https://aclanthology.org/2025.findings-naacl.301/)] [[code](https://github.com/JasonForJoy/BRIEF/tree/main/BRIEF)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/NAACL2025-orange.svg)

- **CARE: Improving Context Fidelity via Native Retrieval-Augmented Reasoning**<br>
  *Suyuchen Wang et al.* [[paper](https://aclanthology.org/2025.emnlp-main.1075/)] [[code](https://github.com/FoundationAgents/CARE)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **MIRAGE: Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation**<br>
  *Jirui Qi et al.* [[paper](https://aclanthology.org/2024.emnlp-main.347/)] [[code](https://github.com/Betswish/MIRAGE)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

- **TRACE the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation**<br>
  *Jinyuan Fang et al.* [[paper](https://aclanthology.org/2024.findings-emnlp.496/)] [[code](https://github.com/jyfang6/trace)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

### Retrieval Necessity and Evidence Critique

- **Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection**<br>
  *Akari Asai et al.* [[paper](https://arxiv.org/html/2310.11511)] [[code](https://github.com/akariasai/self-rag)] 2023. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/ICLR2024-orange.svg)

- **Beneficial Reasoning Behaviors in Agentic Search and Effective Training Methods to Obtain Them**<br>
  *Jiahe Jin, Abhijay Sai Paladugu, Chenyan Xiong* [[paper](https://aclanthology.org/2026.findings-acl.1400/)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### Search Policy and Reinforcement Learning

- **Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning**<br>
  *Bowen Jin et al.* [[paper](https://arxiv.org/html/2503.09516v5)] [[code](https://github.com/PeterGriffinJin/Search-R1)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning**<br>
  *Huatong Song et al.* [[paper](https://arxiv.org/html/2503.05592v2)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Search%20Invocation-8c6bb1.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **ZeroSearch: Incentivize the Search Capability of LLMs without Searching**<br>
  *Hao Sun et al.* [[paper](https://arxiv.org/html/2505.04588v3)] [[code](https://github.com/Alibaba-NLP/ZeroSearch)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Training%20Infrastructure-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

### Retriever–Agent Coupling and Evaluation

- **Agentic-R: Learning to Retrieve for Agentic Search**<br>
  *Wenhan Liu et al.* [[paper](https://aclanthology.org/2026.findings-acl.785/)] [[code](https://github.com/8421BCD/Agentic-R)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![](https://img.shields.io/badge/Retriever%20Co--adaptation-9c755f.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Rethinking Reasoning-Intensive Retrieval: Evaluating and Advancing Retrievers in Agentic Search Systems**<br>
  *Yilun Zhao et al.* [[paper](https://aclanthology.org/2026.acl-long.1705/)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

## Reading Notes

The detailed coding records for the current round are maintained in [`papers/information-seeking-seed.md`](./papers/information-seeking-seed.md), the [`A1 Search Necessity paper pool`](./papers/a1-search-necessity.md), the [`A1 Retrieval Utility sub-pool`](./papers/a1-retrieval-utility.md), the [`A1 Utility-aware Policy Learning sub-pool`](./papers/a1-utility-policy-learning.md), the [`A4 Evidence Evaluation anchor pool`](./papers/a4-evidence-evaluation.md), the [`A4 second-batch and closure analysis`](./papers/a4-evidence-evaluation-second.md), the [`A5 Evidence Integration first batch`](./papers/a5-evidence-integration.md), and the [`A5 second batch and closure analysis`](./papers/a5-evidence-integration-second.md). Each entry records the research problem, contribution type, primary and secondary capabilities, learning mechanism, learning target, supervision, three-dimensional evidence, training–capability alignment, generalization regime, limitations, and research opportunities. The binding annotation rules are in [`notes/coding-protocol.md`](./notes/coding-protocol.md).

The current round has surfaced five working hypotheses:

1. **Tool use is not capability.** Calling a search tool more often does not by itself demonstrate learned information-seeking.
2. **Outcome reward is scalable but coarse.** It often improves final QA scores while providing weak credit assignment over multi-turn search behavior.
3. **Evidence quality is downstream-dependent.** Local query–passage relevance does not guarantee utility for reasoning or final answer generation.
4. **Environment shift is central.** Simulated search, fixed retrievers, and benchmark-specific APIs require explicit simulator-to-real and cross-tool tests.
5. **Capability isolation is missing.** Many papers optimize a full search–reasoning loop without separately measuring search necessity, query quality, source choice, evidence sufficiency, and stopping.

## Search Protocol

This bibliography is intentionally built in rounds rather than through one exhaustive crawl:

1. **Round 1 — Field map:** neighboring surveys and a small seed set of methods/evaluation papers.
2. **Round 2A — A1 Search Necessity:** knowledge boundaries, retrieval triggering, uncertainty, and self-knowledge evaluation.
3. **Round 2B — A1 Retrieval Utility:** value of information, counterfactual retrieval benefit, cost-sensitive evidence selection, and LLM-specific utility.
4. **Round 2C — A1 final utility-aware training check:** ex-post utility rewards, information-gain rewards, counterfactual margins, cost-aware policy learning, and historical baselines.
5. **Round 3A — A4 anchor map:** relevance/support, source reliability, evidence sufficiency, conflict/robustness, and the A4/A5 boundary.
6. **Round 3B — A4 gap-focused expansion and closure:** source credibility, human-verified sufficiency, multi-source conflict, Judgment Target, Ground Truth Quality, and Controlled Perturbation.
7. **Round 4A — A5 Evidence Integration:** knowledge integration, belief revision, multi-evidence composition, and faithful utilization.
8. **Round 4B — A5 gap-focused expansion and closure:** process-level evidence reliance, counterfactual interventions, reasoning-chain attribution, and parametric–external balance.
9. **Round 5 — Memory and state update:** persistent memory retrieval, memory write correctness, state consistency, and stale-memory correction.
10. **Round 6 — Generalization:** task, domain, tool, environment, behavior, and budget transfer.
11. **Snowballing:** references and citing papers from high-value seeds, with special attention to empty or weak cells in the capability × learning matrix.

See the current search record in [`notes/search-log-2026-10-24-round-4-a5-final.md`](./notes/search-log-2026-10-24-round-4-a5-final.md).

The search is deliberately convergent. We will not expand the bibliography with minor variants of existing Search-RL methods unless they provide new evidence about capability learning, training–capability alignment, or transfer. A paper that does not sharpen the central question should remain outside the core set or be recorded only as background.

## Contributing

Contributions are welcome. Please keep entries concise and use the following format:

```markdown
- **Paper Title**<br>
  *Authors* [[paper](URL)] [[code](URL)] YEAR. ![](TYPE_BADGE) ![](CAPABILITY_BADGE) ![](VENUE_BADGE)
```

For a new entry, also add a detailed annotation to [`papers/information-seeking-seed.md`](./papers/information-seeking-seed.md) or the relevant thematic file. At minimum, record the primary capability, learning mechanism, update target, supervision signal, evaluation evidence, generalization evidence, and one unresolved research question.
