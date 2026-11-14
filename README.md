<div align="center">

# Beyond Retrieval

### From Search to Decision: How Post-Training Shapes Information-Seeking Capabilities in Large Language Models

A curated paper list for understanding **what information-seeking abilities LLMs should learn, how post-training teaches them, and whether those abilities generalize beyond specific search tasks and environments**.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Papers](https://img.shields.io/badge/Papers-82-blue.svg)](#papers)
[![Paper](https://img.shields.io/badge/Paper-arXiv%3A2611.14149v1-b31b1b.svg)](https://arxiv.org/abs/2611.14149v1)
[![Contributions](https://img.shields.io/badge/Contributions-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Last Commit](https://img.shields.io/github/last-commit/ashan623/awesome/main?logo=github&color=blue)](https://github.com/ashan623/awesome/commits/main)

</div>

## Content

- [Our Survey Paper](#our-survey-paper)
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
  - [A2 Information Need Formulation](#a2-information-need-formulation)
  - [A3 Information Space Exploration](#a3-information-space-exploration)
  - [A4 Evidence Evaluation](#a4-evidence-evaluation)
  - [A5 Evidence Integration — Knowledge Revision](#a5-evidence-integration--knowledge-revision)
  - [A5 Evidence Integration — Multi-Evidence and Faithful Use](#a5-evidence-integration--multi-evidence-and-faithful-use)
  - [A5.4 Cross-turn Information Persistence and State Updating](#a54-cross-turn-information-persistence-and-state-updating)
  - [A6 Adaptive Search Control](#a6-adaptive-search-control)
  - [Retrieval Necessity and Evidence Critique](#retrieval-necessity-and-evidence-critique)
  - [Search Policy and Reinforcement Learning](#search-policy-and-reinforcement-learning)
  - [Retriever--Agent Coupling and Evaluation](#retriever--agent-coupling-and-evaluation)
- [Detailed Paper Notes](#detailed-paper-notes)
- [Research Resources](#research-resources)
- [Contributing](#contributing)

## Our Survey Paper

**From Search to Decision: How Post-Training Shapes Information-Seeking Capabilities in Large Language Models**<br>
Haohang Huang, Xuan Lu, Mingjin Kuai, Yuxuan Zhang, Wei Ji, and Haiyang Zhang.<br>
[arXiv:2611.14149v1](https://arxiv.org/abs/2611.14149v1) · [PDF](https://arxiv.org/pdf/2611.14149v1)

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
| **B1** | **Data and Learning Signals** | Demonstrations, trajectories, outcome/process/preference/teacher/utility signals |
| **B2** | **Policy Learning and Optimization** | SFT, trajectory distillation, DPO, OPD, PPO, GRPO, REINFORCE, and hybrid methods |
| **B3** | **Learning Targets and Optimization Loci** | LLM policy, query rewriter, verifier, retriever, reader, memory policy, critic, or joint system |
| **B4** | **Long-horizon Credit Assignment** | Episode, turn, step, token/span, and state-transition credit |

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
- **Cross-cutting diagnostics:** `Deployment Information Parity` and `Generalization` are not additional B categories; they are evaluated across A1–A6.

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

- **Towards Agentic RAG with Deep Reasoning: A Survey of RAG-Reasoning Systems in LLMs**<br>
  *Yangning Li et al.* [[paper](https://arxiv.org/abs/2507.09477)] 2025. ![Survey](https://img.shields.io/badge/Survey-4c78a8.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **A Survey of Reasoning-Intensive Retrieval: Progress and Challenges**<br>
  *Yiyang Wei et al.* [[paper](https://arxiv.org/abs/2605.00063)] 2026. ![Survey](https://img.shields.io/badge/Survey-4c78a8.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A5](https://img.shields.io/badge/A5-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

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
  *Pengcheng Jiang et al.* [[paper](https://aclanthology.org/2025.emnlp-main.1095/)] [[code](https://github.com/pat-jj/s3)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **Optimizing Agentic Reasoning with Retrieval via Synthetic Semantic Information Gain Reward**<br>
  *Senkang Hu et al.* [[paper](https://arxiv.org/abs/2602.00845v3)] [[code](https://github.com/dl-m9/InfoReasoner)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ICML2026-orange.svg)

- **CoBRA: Learning Tool-Use Boundaries via Counterfactual Margins**<br>
  *Wenhao Zou et al.* [[paper](https://arxiv.org/abs/2609.00967)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/Counterfactual-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents**<br>
  *Wenxuan Ding et al.* [[paper](https://arxiv.org/abs/2602.16699v3)] [[code](https://github.com/NSF-Simons-CosmicAI-Institute/CalibrateThenAct)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/Cost--Aware-9c755f.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **IKEA: Reinforced Internal-External Knowledge Synergistic Reasoning**<br>
  *Ziyang Huang et al.* [[paper](https://arxiv.org/abs/2505.07596)] [[code](https://github.com/hzy312/knowledge-r1)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **Self-Knowledge Guided Retrieval Augmentation for Large Language Models**<br>
  *Yile Wang et al.* [[paper](https://aclanthology.org/2023.findings-emnlp.691/)] [[code](https://github.com/THUNLP-MT/SKR)] 2023. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A1](https://img.shields.io/badge/A1-2a9d8f.svg) ![](https://img.shields.io/badge/EMNLP2023-orange.svg)

### A2 Information Need Formulation

- **Query Rewriting in Retrieval-Augmented Large Language Models**<br>
  *Xinbei Ma et al.* [[paper](https://aclanthology.org/2023.emnlp-main.322/)] [[code](https://github.com/xbmxb/RAG-query-rewriting)] 2023. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2023-orange.svg)

- **DeepRetrieval: Hacking Real Search Engines and Retrievers with Large Language Models via Reinforcement Learning**<br>
  *Pengcheng Jiang et al.* [[paper](https://arxiv.org/abs/2503.00223)] [[code](https://github.com/pat-jj/DeepRetrieval)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/COLM2025-orange.svg)

- **Beyond the limitation of a single query: Train your LLM for query expansion with Reinforcement Learning**<br>
  *Shu Zhao et al.* [[paper](https://arxiv.org/abs/2510.10009)] [[project](https://shuzhao.me/ExpandSearchProject/)] [[code](https://github.com/Tree-Shu-Zhao/ferret/tree/main/recipe/expand_search)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **ParallelSearch: Train your LLMs to Decompose Query and Search Sub-queries in Parallel with Reinforcement Learning**<br>
  *Shu Zhao et al.* [[paper](https://arxiv.org/abs/2508.09303)] [[project](https://shuzhao.me/ParallelSearchProject/)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **StepSearch: Igniting LLMs Search Ability via Step-Wise Proximal Policy Optimization**<br>
  *Xuhui Zheng et al.* [[paper](https://aclanthology.org/2025.emnlp-main.1106/)] [[code](https://github.com/Zillwang/StepSearch)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **Understanding the Behaviors of Environment-aware Information Retrieval**<br>
  *Ruifeng Yuan et al.* [[paper](https://aclanthology.org/2026.acl-long.2013/)] [[code](https://github.com/LCO-Embedding/Envs-aware-Information-Retrieval)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![C4](https://img.shields.io/badge/C4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **QREF: Improving DeepSearch Agents by Aligning Search with Reasoning via Query Refinement and Evidence Filtering**<br>
  *Yujia Chen et al.* [[paper](https://arxiv.org/abs/2610.21512)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A2](https://img.shields.io/badge/A2-457b9d.svg) ![A4](https://img.shields.io/badge/A4-e76f51.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

### A3 Information Space Exploration

- **InfoMosaic-Bench: Evaluating Multi-Source Information Seeking in Tool-Augmented Agents**<br>
  *Yaxin Du et al.* [[paper](https://arxiv.org/abs/2510.02271)] [[paper](https://proceedings.iclr.cc/paper_files/paper/2026/file/44f09d1973a88529b40029e6c3693ded-Paper-Conference.pdf)] [[code](https://github.com/DorothyDUUU/Info-Mosaic)] 2025/2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A3.1](https://img.shields.io/badge/A3.1-4f8cc9.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![](https://img.shields.io/badge/ICLR2026-orange.svg)

- **Table-as-Search: Agentic Information Seeking is Table Completion**<br>
  *Tian Lan et al.* [[paper](https://aclanthology.org/2026.findings-acl.743/)] [[code](https://github.com/AIDC-AI/Marco-Search-Agent)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Agentic Search in the Wild: Intents and Trajectory Dynamics from 14M+ Real Search Requests**<br>
  *Jingjie Ning et al.* [[paper](https://arxiv.org/abs/2601.17617)] [[logs](https://huggingface.co/datasets/cx-cmu/deepresearchgym-agentic-search-logs)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![A6](https://img.shields.io/badge/A6-6c7576.svg) ![](https://img.shields.io/badge/SIGIR2026-orange.svg)

- **MARAG-R1: Beyond Single Retriever via Reinforcement-Learned Multi-Tool Agentic Retrieval**<br>
  *Q. Luo et al.* [[paper](https://arxiv.org/abs/2510.27569)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A3.1](https://img.shields.io/badge/A3.1-4f8cc9.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2025-orange.svg)

- **Restoring Exploration via Diversity-Driven Policy Optimization in Search-Based LLM Agents**<br>
  *Xinyue Chen et al.* [[paper](https://www.sciencedirect.com/science/article/abs/pii/S0306457326004942)] 2026/2027. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/IPM%202027-orange.svg)

- **Training Small Language Models for Transferable Tool Selection**<br>
  *Zhaoyi Zhou et al.* [[paper](https://arxiv.org/abs/2610.31483)] 2026. ![Boundary](https://img.shields.io/badge/Boundary-9c755f.svg) ![A3.1](https://img.shields.io/badge/A3.1-4f8cc9.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![C4](https://img.shields.io/badge/C4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning**<br>
  *Zhenlong Dai et al.* [[paper](https://arxiv.org/abs/2609.30906)] [[code](https://github.com/zhenlongDai/ToolSearcher)] 2026. ![Boundary](https://img.shields.io/badge/Boundary-9c755f.svg) ![A3.1](https://img.shields.io/badge/A3.1-4f8cc9.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **GISA: A Benchmark for General Information-Seeking Assistant**<br>
  *Yutao Zhu et al.* [[paper](https://arxiv.org/abs/2602.08543v2)] [[code](https://github.com/RUC-NLPIR/GISA)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![A3.3](https://img.shields.io/badge/A3.3-4f8cc9.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration**<br>
  *Yuyao Zhang et al.* [[paper](https://arxiv.org/abs/2607.15257v1)] [[code](https://github.com/antins-labs/SearchOS)] 2026. ![Boundary](https://img.shields.io/badge/Boundary-9c755f.svg) ![A3.2](https://img.shields.io/badge/A3.2-4f8cc9.svg) ![A6.3](https://img.shields.io/badge/A6.3-6c7576.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

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
  *Zhaoheng Huang et al.* [[paper](https://aclanthology.org/2026.acl-long.1492/)] [[code](https://github.com/WaldenRUC/RLSeek)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Evidence-Augmented Policy Optimization with Reward Co-Evolution for Long-Context Reasoning**<br>
  *Xin Guan et al.* [[paper](https://aclanthology.org/2026.acl-long.1404/)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **AuthorityBench: Benchmarking LLM Authority Perception for Reliable Retrieval-Augmented Generation**<br>
  *Zhihui Yao et al.* [[paper](https://arxiv.org/abs/2603.25092)] [[code](https://github.com/Trustworthy-Information-Access/AuthorityBench)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **What Evidence Do Language Models Find Convincing?**<br>
  *Alexander Wan et al.* [[paper](https://aclanthology.org/2024.acl-long.403/)] [[arXiv](https://arxiv.org/abs/2402.11782)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/ACL2024-orange.svg)

- **Resolving Conflicting Evidence in Automated Fact-Checking: A Study on Retrieval-Augmented LLMs**<br>
  *Ziyu Ge et al.* [[paper](https://www.ijcai.org/proceedings/2025/1073)] [[code](https://github.com/zoeyyes/CONFACT)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/IJCAI2025-orange.svg)

- **(D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs**<br>
  *Arie Cattan et al.* [[Google Research](https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/)] [[arXiv](https://arxiv.org/abs/2506.08500)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/Google%20Research-4c78a8.svg)

- **Sufficient Context: A New Lens on Retrieval Augmented Generation Systems**<br>
  *Hailey Joren et al.* [[paper](https://arxiv.org/abs/2411.06037v3)] [[code](https://github.com/hljoren/sufficientcontext)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.3](https://img.shields.io/badge/A4.3-e76f51.svg) ![](https://img.shields.io/badge/ICLR2025-orange.svg)

- **Benchmarking LLM’s Capability in Reasoning over Conflicting Web References**<br>
  *Yizhen Yuan et al.* [[paper](https://aclanthology.org/2026.acl-long.11/)] [[code](https://github.com/XaiverYuan/ConfRAG)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### A5 Evidence Integration — Knowledge Revision

- **ClashEval: Quantifying the Tug-of-War between an LLM’s Internal Prior and External Evidence**<br>
  *Kevin Wu et al.* [[paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/3aa291abc426d7a29fb08418c1244177-Abstract-Datasets_and_Benchmarks_Track.html)] [[code](https://github.com/kevinwu23/StanfordClashEval)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/NeurIPS2024-orange.svg)

- **FaithfulRAG: Fact-Level Conflict Modeling for Context-Faithful Retrieval-Augmented Generation**<br>
  *Qinggang Zhang et al.* [[paper](https://aclanthology.org/2025.acl-long.1062/)] [[code](https://github.com/XMUDeepLIT/Faithful-RAG)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/ACL2025-orange.svg)

- **Belief Revision: The Adaptability of Large Language Models Reasoning**<br>
  *Bryan Wilie et al.* [[paper](https://aclanthology.org/2024.emnlp-main.586/)] [[code](https://github.com/HLTCHKUST/belief-revision)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

- **GuarantRAG: Guaranteeing Knowledge Integration with Joint Decoding for Retrieval-Augmented Generation**<br>
  *Zhengyi Zhao et al.* [[paper](https://aclanthology.org/2026.acl-long.192/)] [[paper](https://arxiv.org/abs/2604.08046)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Efficient Prior-Guided Reasoning for Robust Retrieval-Augmented Generation under Conflicts**<br>
  *Xiaowei Yuan et al.* [[paper](https://aclanthology.org/2026.acl-long.1013/)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **How Large Language Models Balance Internal Knowledge with User and Document Assertions**<br>
  *Shuowei Li et al.* [[paper](https://aclanthology.org/2026.findings-acl.1267/)] [[code](https://github.com/shuowl/llm-source-balancing)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.1](https://img.shields.io/badge/A5.1-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### A5 Evidence Integration — Multi-Evidence and Faithful Use

- **RAFT: Adapting Language Model to Domain Specific RAG**<br>
  *Tianjun Zhang et al.* [[paper](https://openreview.net/forum?id=rzQGHXNReU)] [[code](https://github.com/ShishirPatil/gorilla/tree/main/raft)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/COLM2024-orange.svg)

- **BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression**<br>
  *Yuankai Li et al.* [[paper](https://aclanthology.org/2025.findings-naacl.301/)] [[code](https://github.com/JasonForJoy/BRIEF/tree/main/BRIEF)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/NAACL2025-orange.svg)

- **CARE: Improving Context Fidelity via Native Retrieval-Augmented Reasoning**<br>
  *Suyuchen Wang et al.* [[paper](https://aclanthology.org/2025.emnlp-main.1075/)] [[code](https://github.com/FoundationAgents/CARE)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2025-orange.svg)

- **MIRAGE: Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation**<br>
  *Jirui Qi et al.* [[paper](https://aclanthology.org/2024.emnlp-main.347/)] [[code](https://github.com/Betswish/MIRAGE)] 2024. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

- **Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation**<br>
  *Ekaterina Fadeeva et al.* [[paper](https://aclanthology.org/2026.findings-acl.338/)] [[arXiv](https://arxiv.org/abs/2505.21072)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![A4.1](https://img.shields.io/badge/A4.1-e76f51.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models**<br>
  *Amin Abolghasemi et al.* [[paper](https://aclanthology.org/2025.findings-acl.1087/)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![A4.2](https://img.shields.io/badge/A4.2-e76f51.svg) ![](https://img.shields.io/badge/ACL2025-orange.svg)

- **TRACE the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation**<br>
  *Jinyuan Fang et al.* [[paper](https://aclanthology.org/2024.findings-emnlp.496/)] [[code](https://github.com/jyfang6/trace)] 2024. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.2](https://img.shields.io/badge/A5.2-8d5a97.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![](https://img.shields.io/badge/EMNLP2024-orange.svg)

- **Counterfactual Reasoning for Retrieval-Augmented Generation**<br>
  *Huaiyu Qin et al.* [[paper](https://iclr.cc/virtual/2026/poster/10011109)] [[OpenReview](https://openreview.net/forum?id=9U51rOnGko)] 2026. ![Inference-only](https://img.shields.io/badge/Inference--only-9c755f.svg) ![A5.3](https://img.shields.io/badge/A5.3-8d5a97.svg) ![A4.4](https://img.shields.io/badge/A4.4-e76f51.svg) ![](https://img.shields.io/badge/ICLR2026-orange.svg)

### A5.4 Cross-turn Information Persistence and State Updating

- **LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory**<br>
  *Di Wu et al.* [[paper](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d813d324dbf0598bbdc9c8e79740ed01-Abstract-Conference.html)] [[code](https://github.com/xiaowu0162/LongMemEval)] 2025. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.4.2](https://img.shields.io/badge/A5.4.2-8d5a97.svg) ![](https://img.shields.io/badge/ICLR2025-orange.svg)

- **MemoryAgentBench: Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions**<br>
  *Yuanzhe Hu et al.* [[paper](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fd1eff9dd295df50a41f2521942fa31d-Abstract-Conference.html)] [[code](https://github.com/HUST-AI-HYZ/MemoryAgentBench)] 2026. ![Evaluation](https://img.shields.io/badge/Evaluation-e15759.svg) ![A5.4.2](https://img.shields.io/badge/A5.4.2-8d5a97.svg) ![](https://img.shields.io/badge/ICLR2026-orange.svg)

- **Memory-R1: Enhancing Large Language Model Agents to Manage and Utilize Memories via Reinforcement Learning**<br>
  *Sikuan Yan et al.* [[paper](https://aclanthology.org/2026.acl-long.583/)] [[code](https://github.com/yansikuan/memory-r1)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.4.1](https://img.shields.io/badge/A5.4.1-8d5a97.svg) ![A5.4.2](https://img.shields.io/badge/A5.4.2-8d5a97.svg) ![A5.4.4](https://img.shields.io/badge/A5.4.4-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **MemRL: Self-Evolving Agents via Runtime Reinforcement Learning on Episodic Memory**<br>
  *Shengtao Zhang et al.* [[paper](https://arxiv.org/abs/2601.03192)] [[code](https://github.com/MemTensor/MemRL)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.4.3](https://img.shields.io/badge/A5.4.3-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **A-MEM: Agentic Memory for LLM Agents**<br>
  *Wujiang Xu et al.* [[paper](https://proceedings.neurips.cc/paper_files/paper/2025/hash/19909c36f51abc4856b4560aff3d36d6-Abstract-Conference.html)] [[code](https://github.com/WujiangXu/A-mem-sys)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.4.1](https://img.shields.io/badge/A5.4.1-8d5a97.svg) ![A5.4.2](https://img.shields.io/badge/A5.4.2-8d5a97.svg) ![](https://img.shields.io/badge/NeurIPS2025-orange.svg)

- **Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory**<br>
  *Prateek Chhikara et al.* [[paper](https://doi.org/10.3233/FAIA251160)] [[code](https://github.com/mem0ai/mem0)] 2025. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A5.4.1](https://img.shields.io/badge/A5.4.1-8d5a97.svg) ![A5.4.2](https://img.shields.io/badge/A5.4.2-8d5a97.svg) ![](https://img.shields.io/badge/ECAI2025-orange.svg)

### A6 Adaptive Search Control

- **AutoSearch: Adaptive Search Depth for Efficient Agentic RAG via Reinforcement Learning**<br>
  *Jingbo Sun et al.* [[paper](https://aclanthology.org/2026.findings-acl.1399/)] [[paper](https://arxiv.org/abs/2604.17337)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6.2](https://img.shields.io/badge/A6.2-6c7576.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **One Policy, Any Budget: Internalizing Budget-Aware Search via Reinforcement Learning**<br>
  *Xiaowei Sun et al.* [[paper](https://arxiv.org/abs/2609.00813)] [[code](https://github.com/xwsun01/AnySearch)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6.2](https://img.shields.io/badge/A6.2-6c7576.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![C7](https://img.shields.io/badge/C7-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **CriticSearch: Fine-Grained Credit Assignment for Search Agents via a Retrospective Critic**<br>
  *Yaocheng Zhang et al.* [[paper](https://aclanthology.org/2026.findings-acl.596/)] [[paper](https://arxiv.org/abs/2511.12159)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6.1](https://img.shields.io/badge/A6.1-6c7576.svg) ![A6.3](https://img.shields.io/badge/A6.3-6c7576.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

- **Enhancing LLM-based Search Agents via Contribution Weighted Group Relative Policy Optimization**<br>
  *Junzhe Wang et al.* [[paper](https://aclanthology.org/2026.acl-long.1462/)] [[code](https://github.com/zsxmwjz/CW-GRPO)] 2026. ![Method](https://img.shields.io/badge/Method-59a14f.svg) ![A6.1](https://img.shields.io/badge/A6.1-6c7576.svg) ![A6.3](https://img.shields.io/badge/A6.3-6c7576.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/ACL2026-orange.svg)

### How to Learn — Cross-Capability Mechanisms

- **Beyond Outcome Rewards: Step-Level Self-Distilled Policy Optimization for Deep Search Agents**<br>
  *Haoze Wu, Chuqiao Kuang, Tianyi Zhuang, Xiaoguang Li* [[paper](https://arxiv.org/abs/2608.12764)] 2026. ![Core Mechanism](https://img.shields.io/badge/Core%20Mechanism-59a14f.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **TRACE: Turn-level Reward Assignment via Credit Estimation for Long-Horizon Agents**<br>
  *Leitian Tao et al.* [[paper](https://arxiv.org/abs/2607.13988)] 2026. ![Core Mechanism](https://img.shields.io/badge/Core%20Mechanism-59a14f.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

- **Retrieval, Reward, and Training Protocols: What Matters in Training Search Agents?**<br>
  *Yibo Zhao et al.* [[paper](https://arxiv.org/abs/2605.27881)] 2026. ![Core Comparison](https://img.shields.io/badge/Core%20Comparison-9c755f.svg) ![B1](https://img.shields.io/badge/B1-8d5a97.svg) ![B2](https://img.shields.io/badge/B2-8d5a97.svg) ![B4](https://img.shields.io/badge/B4-8d5a97.svg) ![](https://img.shields.io/badge/Arxiv2026-orange.svg)

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

## Detailed Paper Notes

The root README is the primary public catalog. These companion notes provide deeper paper-level evidence summaries and comparisons by capability:

- [Seed papers and cross-cutting anchors](./papers/information-seeking-seed.md)
- [A1: Search necessity](./papers/a1-search-necessity.md), [retrieval utility](./papers/a1-retrieval-utility.md), and [utility-aware policy learning](./papers/a1-utility-policy-learning.md)
- [A2: Information need formulation](./papers/a2-information-need-formulation.md)
- [A3: Information-space exploration](./papers/a3-information-space-exploration.md) and [targeted evaluation supplement](./papers/a3-targeted-evaluation-supplement.md)
- [A4: Evidence evaluation](./papers/a4-evidence-evaluation.md), [source/conflict supplement](./papers/a4-source-reliability-and-conflict.md), and [additional evaluation work](./papers/a4-evidence-sufficiency-and-conflict.md)
- [A5: Evidence integration](./papers/a5-evidence-integration.md), [multi-evidence and faithfulness](./papers/a5-multi-evidence-composition.md), [factuality and attribution](./papers/a5-faithfulness-and-attribution.md), and [counterfactual evidence](./papers/a5-counterfactual-evidence-supplement.md)
- [A5.4: Cross-turn persistence and state updating](./papers/a5.4-memory-updating.md)
- [A6: Adaptive search control](./papers/a6-adaptive-search-control.md)
- [Cross-capability learning mechanisms](./papers/learning-mechanism-comparison.md)

## Research Resources

These companion documents capture the consolidated evidence analysis, learning taxonomy, generalization framework, and positioning against adjacent surveys:

- [Paper catalog by category](./analysis/paper-catalog-by-category.md)
- [Final cross-paper evidence matrix](./analysis/final-cross-paper-evidence-matrix.md)
- [Claim–Evidence Matrix](./analysis/claim-evidence-matrix.md)
- [Training–Capability Misalignment taxonomy](./analysis/training-capability-misalignment.md)
- [Generalization and intervention audit](./analysis/generalization-intervention-audit.md)
- [Related-survey comparison](./analysis/related-survey-comparison.md)

## Contributing

Contributions are welcome. Add papers that directly inform at least one A1–A6 information-seeking capability or a cross-capability learning mechanism. Please include a stable paper link and keep the entry concise:

```markdown
- **Paper Title**<br>
  *Authors* [[paper](URL)] [[code](URL)] YEAR. ![](TYPE_BADGE) ![](CAPABILITY_BADGE) ![](VENUE_BADGE)
```

Use the existing capability and contribution labels. Distinguish directly measured behavior from the capability a paper claims to teach; an in-domain score increase alone is not evidence of generalization. Add substantive paper-level context to the relevant thematic file under [`papers/`](./papers/) when useful. Do not add search logs, temporary drafts, or round-by-round status notes to the public catalog.
