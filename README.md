<div align="center">

# Beyond Retrieval

### Learning Information-Seeking Capabilities in Large Language Models

A curated paper list for understanding **what information-seeking abilities LLMs should learn, how post-training teaches them, and whether those abilities generalize beyond specific search tasks and environments**.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Papers](https://img.shields.io/badge/Papers-10-blue.svg)](./papers/information-seeking-seed.md)
[![Status](https://img.shields.io/badge/Status-Round%201%20Field%20Map-orange.svg)](./notes/search-log-2026-10-23-round-1.md)
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
| **A4** | **Evidence Evaluation** | Can it judge relevance, authority, support, sufficiency, contradiction, and reliability? |
| **A5** | **Evidence Integration** | Can it use evidence to update reasoning, memory, beliefs, generation, or decisions? |
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

The detailed coding records for the current round are maintained in [`papers/information-seeking-seed.md`](./papers/information-seeking-seed.md). Each entry records the research problem, contribution type, primary and secondary capabilities, learning mechanism, learning target, supervision, three-dimensional evidence, training–capability alignment, generalization regime, limitations, and research opportunities. The binding annotation rules are in [`notes/coding-protocol.md`](./notes/coding-protocol.md).

The current round has surfaced five working hypotheses:

1. **Tool use is not capability.** Calling a search tool more often does not by itself demonstrate learned information-seeking.
2. **Outcome reward is scalable but coarse.** It often improves final QA scores while providing weak credit assignment over multi-turn search behavior.
3. **Evidence quality is downstream-dependent.** Local query–passage relevance does not guarantee utility for reasoning or final answer generation.
4. **Environment shift is central.** Simulated search, fixed retrievers, and benchmark-specific APIs require explicit simulator-to-real and cross-tool tests.
5. **Capability isolation is missing.** Many papers optimize a full search–reasoning loop without separately measuring search necessity, query quality, source choice, evidence sufficiency, and stopping.

## Search Protocol

This bibliography is intentionally built in rounds rather than through one exhaustive crawl:

1. **Round 1 — Field map:** neighboring surveys and a small seed set of methods/evaluation papers.
2. **Round 2 — A1 + A4:** epistemic awareness, retrieval necessity, uncertainty, evidence sufficiency, verification, reliability, and contradiction handling.
3. **Round 3 — A2 + A3 + A6:** query planning, source navigation, search policy, stopping, retry, and budget-aware control.
4. **Round 4 — A5 and state update:** reasoning integration, memory retrieval, belief update, and long-term state.
5. **Round 5 — C1–C6:** transfer, robustness, environment shift, budget generalization, and cross-domain evaluation.
6. **Snowballing:** references and citing papers from high-value seeds, with special attention to empty or weak cells in the capability × learning matrix.

See the current search record in [`notes/search-log-2026-10-23-round-1.md`](./notes/search-log-2026-10-23-round-1.md).

The search is deliberately convergent. We will not expand the bibliography with minor variants of existing Search-RL methods unless they provide new evidence about capability learning, training–capability alignment, or transfer. A paper that does not sharpen the central question should remain outside the core set or be recorded only as background.

## Contributing

Contributions are welcome. Please keep entries concise and use the following format:

```markdown
- **Paper Title**<br>
  *Authors* [[paper](URL)] [[code](URL)] YEAR. ![](TYPE_BADGE) ![](CAPABILITY_BADGE) ![](VENUE_BADGE)
```

For a new entry, also add a detailed annotation to [`papers/information-seeking-seed.md`](./papers/information-seeking-seed.md) or the relevant thematic file. At minimum, record the primary capability, learning mechanism, update target, supervision signal, evaluation evidence, generalization evidence, and one unresolved research question.
