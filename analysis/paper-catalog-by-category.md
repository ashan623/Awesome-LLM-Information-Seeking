# Current Paper Catalog by Category

> 更新时间：2026-10-26
>
> 本清单严格依据当前 `README.md`、A1–A6论文池、Round 8机制核验和 Related-Survey Comparison 整理。它不是按“论文主题”简单罗列，而是按本 Survey 的 **What to Learn（A1–A6）× How to Learn（B1–B4）× Evidence Role** 组织。

## 0. 先看总览

### 当前仓库 README 中的 79 条条目

| 层级 | 数量 | 说明 |
|---|---:|---|
| 相邻综述 | 5 | 用于 Related Work 和 novelty positioning |
| A1 Epistemic Awareness | 15 | 检索必要性、检索效用、价值预测和 utility-aware policy learning |
| A2 Information Need Formulation | 7 | 查询改写、查询扩展、子问题分解、retriever-aware query policy |
| A3 Information Space Exploration | 7 | 信息源/工具选择、探索覆盖和未见信息空间导航；当前作为跨能力维度，不单独扩展成完整章节 |
| A4 Evidence Evaluation | 12 | 相关性/支持、来源权威、证据充分性、冲突与鲁棒性 |
| A5 Evidence Integration | 13 | belief revision、多证据组合、faithful evidence utilization |
| A5.4 Cross-turn Information Persistence | 6 | A5 内部的有界子方向，不是独立 Memory Survey |
| A6 Adaptive Search Control | 4 | 搜索反馈适应、停止和预算控制、长程 credit |
| How to Learn / B1–B4 | 3 | 跨能力机制核验：数据/信号、优化、更新位置、credit assignment |
| Cross-cutting anchors | 7 | 跨多个能力的搜索策略、检索器—agent耦合和行为分析论文 |
| **合计** | **79** | README中的全部条目，其中包含上面的5篇相邻综述 |

### Related-Survey Comparison 中另外核验的 4 篇综述

这4篇已用于 novelty comparison，但没有重复放入 README 的 79 条主目录，因此如果把所有相关综述也算入“当前使用的文献”，总数是 **83 篇工作**：

- [Reinforcement Learning Foundations for Deep Research Systems: A Survey](https://arxiv.org/abs/2509.06733) —— Deep Research 的 RL 数据、奖励、credit assignment 和训练系统。
- [SoK: Agentic Retrieval-Augmented Generation (RAG): Taxonomy, Architectures, Evaluation, and Research Directions](https://arxiv.org/abs/2603.07379) —— Agentic RAG 的 POMDP、架构、控制环和评价。
- [The Landscape of Agentic Reinforcement Learning for LLMs: A Survey](https://arxiv.org/abs/2509.02547) —— 更宽的 Agentic RL 能力—应用地图。
- [From Reasoning to Agentic: Credit Assignment in Reinforcement Learning for Large Language Models](https://arxiv.org/abs/2604.09459) —— credit assignment 的证据等级、干预和可识别性规范。

---

## 1. Neighboring Surveys：相邻综述

这些论文不是 A1–A6 的能力证据，而是用来界定本 Survey 与已有 Search Agent、Deep Research、Agentic Search 综述的关系。

1. [A Survey of LLM-based Deep Search Agents: Paradigm, Optimization, Evaluation, and Challenges](https://arxiv.org/html/2508.05668v3) —— Search Agent 的架构、优化、评价地图；与 A2/A3/A6 相邻。
2. [A Comprehensive Survey on Reinforcement Learning-based Agentic Search: Foundations, Roles, Optimizations, Evaluations, and Applications](https://arxiv.org/html/2510.16724) —— RL 在检索控制、查询优化、检索—推理整合等方面的角色；与 A6 相邻。
3. [Deep Research: A Survey of Autonomous Research Agents](https://arxiv.org/html/2508.12752v1) —— Planning、Question Developing、Web Exploration、Report Generation 四阶段；与 A2/A3/A5 相邻。
4. [Towards Agentic RAG with Deep Reasoning: A Survey of RAG-Reasoning Systems in LLMs](https://arxiv.org/abs/2507.09477) —— Reasoning-Enhanced RAG、RAG-Enhanced Reasoning 和 agentic RAG-reasoning 协同；与 A4/A5/A6 相邻。
5. [A Survey of Reasoning-Intensive Retrieval: Progress and Challenges](https://arxiv.org/abs/2605.00063) —— reasoning-integrated retrieval pipeline、RIR benchmark、retriever/reranker 与方法分类；与 A2/A4/A5 相邻。

---

## 2. A1 — Epistemic Awareness / Retrieval Necessity and Utility

**核心问题：** 模型能否判断信息是否值得获取，并预测检索相对于不检索的实际收益？

### A1.1 Search Necessity and Epistemic Awareness

| Paper | Evidence role / use |
|---|---|
| [When to Retrieve: Teaching LLMs to Utilize Information Retrieval Effectively](https://aclanthology.org/2025.ranlp-1.73/) | Core method；检索触发和是否需要外部信息 |
| [KBM: Delineating Knowledge Boundary for Adaptive Retrieval in Large Language Models](https://arxiv.org/abs/2411.06207) | Core method；知识边界与自适应检索 |
| [DRAGIN: Dynamic Retrieval Augmented Generation based on the Real-time Information Needs of Large Language Models](https://aclanthology.org/2024.acl-long.702/) | Core method；实时信息需求和动态触发 |
| [SeaKR: Self-aware Knowledge Retrieval for Adaptive Retrieval Augmented Generation](https://aclanthology.org/2025.acl-long.1312/) | Core method；自我觉察、检索触发和生成中控制 |
| [Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home](https://aclanthology.org/2025.acl-long.319/) | Evaluation anchor；检验 uncertainty/self-knowledge 是否足以决定检索价值 |

### A1.2 Retrieval Utility and Value of Information

| Paper | Evidence role / use |
|---|---|
| [LLM-Specific Utility for Retrieval-Augmented Generation](https://arxiv.org/abs/2510.11358) | Core evaluation；同一证据对不同LLM的实际效用不同 |
| [When Knowledge Is Not Free: Cost-Aware Evidence Selection in Retrieval-Augmented Generation](https://arxiv.org/abs/2606.02245v1) | Core evaluation；证据成本、停止和收益—成本权衡 |
| [Value of Information: A Framework for Human–Agent Communication](https://aclanthology.org/2026.acl-long.1987/) | Supporting theory；用预期信息价值指导是否请求/获取信息 |
| [Redefining Retrieval Evaluation in the Era of LLMs](https://aclanthology.org/2026.eacl-long.391/) | Evaluation anchor；从传统相关性转向对下游LLM的实际效用 |

### A1.3 Utility-aware Policy Learning

| Paper | Evidence role / use |
|---|---|
| [s3: You Don’t Need That Much Data to Train a Search Agent via RL](https://aclanthology.org/2025.emnlp-main.1095/) | Core method；低数据量的搜索策略RL与检索收益 |
| [Optimizing Agentic Reasoning with Retrieval via Synthetic Semantic Information Gain Reward](https://arxiv.org/abs/2602.00845v3) | Core method；synthetic semantic information gain reward |
| [CoBRA: Learning Tool-Use Boundaries via Counterfactual Margins](https://arxiv.org/abs/2609.00967) | Core method；反事实边际学习工具/检索边界 |
| [Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents](https://arxiv.org/abs/2602.16699v3) | Core method；校准、成本和探索决策 |
| [IKEA: Reinforced Internal-External Knowledge Synergistic Reasoning](https://arxiv.org/abs/2505.07596) | Core method；内部知识与外部检索的协同决策 |
| [Self-Knowledge Guided Retrieval Augmentation for Large Language Models](https://aclanthology.org/2023.findings-emnlp.691/) | Historical/core baseline；自知识引导的检索增强 |

**A1最终定位：** `uncertainty/answerability signal → utility estimation → ex-ante cost-sensitive policy`。不能把“最终检索后答对了”直接等同于模型已经学会预测检索价值。

---

## 3. A2 — Information Need Formulation

**核心问题：** 模型能否把真实的信息缺口表达成有效查询、子查询或查询计划，并在不同搜索环境中保持语义正确和可迁移？

| Paper | A2 sub-direction / use |
|---|---|
| [Query Rewriting in Retrieval-Augmented Large Language Models](https://aclanthology.org/2023.emnlp-main.322/) | Query rewriting；基础查询意图表达 |
| [DeepRetrieval: Hacking Real Search Engines and Retrievers with Large Language Models via Reinforcement Learning](https://arxiv.org/abs/2503.00223) | Retrieval-gain RL；直接用搜索结果训练 query policy |
| [Beyond the limitation of a single query: Train your LLM for query expansion with Reinforcement Learning](https://arxiv.org/abs/2510.10009) | Multi-query expansion；提升信息覆盖 |
| [ParallelSearch: Train your LLMs to Decompose Query and Search Sub-queries in Parallel with Reinforcement Learning](https://arxiv.org/abs/2508.09303) | Parallel decomposition；将复杂需求拆成互补子查询 |
| [StepSearch: Igniting LLMs Search Ability via Step-Wise Proximal Policy Optimization](https://aclanthology.org/2025.emnlp-main.1106/) | Step-level query/search policy；过程级训练信号 |
| [Understanding the Behaviors of Environment-aware Information Retrieval](https://aclanthology.org/2026.acl-long.2013/) | Backend adaptation；查询表达依赖Retriever和搜索环境 |
| [QREF: Improving DeepSearch Agents by Aligning Search with Reasoning via Query Refinement and Evidence Filtering](https://arxiv.org/abs/2610.21512) | Query refinement + evidence coverage；A2收束时的定向补充 |

**A2内部分析轴：** `Semantic Intent Preservation`、`Backend Adaptation`、`Transferable Query Learning`。Query质量不能只用最终QA分数衡量，应尽量固定corpus/retriever/top-k/预算进行 query-only intervention。

---

## 4. A3 — Information Space Exploration

**核心问题：** 模型能否发现尚未覆盖但对任务必要的信息区域、信息源或工具，而不是仅生成更多重复查询？

> A3 当前保留为独立能力定义和跨能力评价维度，但不扩展为完整独立章节。重点区分 `Query Diversity ≠ Evidence Diversity ≠ Information Gain ≠ Useful Coverage`。

| Paper | A3 sub-direction / use |
|---|---|
| [InfoMosaic-Bench: Evaluating Multi-Source Information Seeking in Tool-Augmented Agents](https://arxiv.org/abs/2510.02271) | A3.1/A3.2；多来源工具选择与信息覆盖评价 |
| [Table-as-Search: Agentic Information Seeking is Table Completion](https://aclanthology.org/2026.findings-acl.743/) | A3.2；结构化搜索状态、覆盖和信息缺口 |
| [Agentic Search in the Wild: Intents and Trajectory Dynamics from 14M+ Real Search Requests](https://arxiv.org/abs/2601.17617) | A3.2；真实搜索轨迹中的重复、覆盖和探索行为 |
| [MARAG-R1: Beyond Single Retriever via Reinforcement-Learned Multi-Tool Agentic Retrieval](https://arxiv.org/abs/2510.27569) | A3.1/A3.2；多Retriever、多工具协调 |
| [Restoring Exploration via Diversity-Driven Policy Optimization in Search-Based LLM Agents](https://www.sciencedirect.com/science/article/abs/pii/S0306457326004942) | A3.2；query/evidence diversity reward；需警惕多样性不等于有用信息增益 |
| [Training Small Language Models for Transferable Tool Selection](https://arxiv.org/abs/2610.31483) | A3.1 boundary；跨Retriever/工具库迁移，但与一般tool selection有边界 |
| [ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning](https://arxiv.org/abs/2609.30906) | A3.1 boundary；大规模工具库探索与event-level credit，需区分信息获取和一般功能调用 |

---

## 5. A4 — Evidence Evaluation

**核心问题：** 模型能否判断已获取信息是否相关、支持结论、可信、充分，或存在冲突和误导？

| Paper | A4 sub-direction / use |
|---|---|
| [Corrective Retrieval Augmented Generation](https://arxiv.org/abs/2401.15884) | A4.1；query–document relevance与纠错路由 |
| [S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA](https://aclanthology.org/2026.acl-long.1185/) | A4.3；证据充分性和信息缺口判断 |
| [RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models](https://aclanthology.org/2024.acl-long.585/) | A4.1/A4.4；unsupported/contradictory span检测 |
| [RAGChecker: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation](https://proceedings.neurips.cc/paper_files/paper/2024/hash/27245589131d17368cccdfa990cbf16e-Abstract-Datasets_and_Benchmarks_Track.html) | A4.1/A4.3；细粒度support、precision、faithfulness和noise诊断 |
| [RLSeek: Evidence-Grounded Reasoning for RAG Hallucination Detection](https://aclanthology.org/2026.acl-long.1492/) | A4.1；用GRPO学习证据支持/反驳行为 |
| [Evidence-Augmented Policy Optimization with Reward Co-Evolution for Long-Context Reasoning](https://aclanthology.org/2026.acl-long.1404/) | A4.1/A4.3；证据质量奖励和reasoning policy联合优化 |
| [AuthorityBench: Benchmarking LLM Authority Perception for Reliable Retrieval-Augmented Generation](https://arxiv.org/abs/2603.25092) | A4.2；来源权威感知和authority perturbation |
| [What Evidence Do Language Models Find Convincing?](https://aclanthology.org/2024.acl-long.403/) | A4.2/A4.4；反事实分析显示相关性不等于convincingness/可靠性 |
| [Resolving Conflicting Evidence in Automated Fact-Checking: A Study on Retrieval-Augmented LLMs](https://www.ijcai.org/proceedings/2025/1073) | A4.2/A4.4；来源可信度和冲突事实核查 |
| [(D)RAGged Into a Conflict: Detecting and Addressing Conflicting Sources in Retrieval-Augmented LLMs](https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/) | A4.2/A4.4；冲突类型 taxonomy、期望行为与CONFLICTS benchmark |
| [Sufficient Context: A New Lens on Retrieval Augmented Generation Systems](https://arxiv.org/abs/2411.06037v3) | A4.3；context sufficiency与生成失败解耦 |
| [Benchmarking LLM’s Capability in Reasoning over Conflicting Web References](https://aclanthology.org/2026.acl-long.11/) | A4.4；多来源冲突组织和reason coverage |

**A4最终定位：** `Relevance/Support`、`Source Reliability`、`Evidence Sufficiency`、`Conflict/Robustness`必须分开；评价指标丰富不代表模型已经学会可迁移的证据判断能力。

---

## 6. A5 — Evidence Integration

**核心问题：** 模型能否忠实地将获得的信息整合进推理、信念、答案和决策，而非只是在上下文中展示或复制证据？

### A5.1 Knowledge Integration and Belief Revision

| Paper | A5 sub-direction / use |
|---|---|
| [ClashEval: Quantifying the Tug-of-War between an LLM’s Internal Prior and External Evidence](https://proceedings.neurips.cc/paper_files/paper/2024/hash/3aa291abc426d7a29fb08418c1244177-Abstract-Datasets_and_Benchmarks_Track.html) | A5.1 evaluation；内部先验与外部证据冲突 |
| [FaithfulRAG: Fact-Level Conflict Modeling for Context-Faithful Retrieval-Augmented Generation](https://aclanthology.org/2025.acl-long.1062/) | A5.1/A5.3；fact-level conflict和context faithfulness |
| [Belief Revision: The Adaptability of Large Language Models Reasoning](https://aclanthology.org/2024.emnlp-main.586/) | A5.1 evaluation；belief update/maintenance行为 |
| [GuarantRAG: Guaranteeing Knowledge Integration with Joint Decoding for Retrieval-Augmented Generation](https://aclanthology.org/2026.acl-long.192/) | A5.1/A5.2/A5.3；DPO、joint decoding和知识整合 |
| [Efficient Prior-Guided Reasoning for Robust Retrieval-Augmented Generation under Conflicts](https://aclanthology.org/2026.acl-long.1013/) | A5.1/A5.2；冲突证据下的prior-guided reasoning |
| [How Large Language Models Balance Internal Knowledge with User and Document Assertions](https://aclanthology.org/2026.findings-acl.1267/) | A5.1 evaluation/training；内部知识、用户断言和文档断言的来源平衡 |

### A5.2 Multi-evidence Composition and A5.3 Faithful Evidence Utilization

| Paper | A5 sub-direction / use |
|---|---|
| [RAFT: Adapting Language Model to Domain Specific RAG](https://openreview.net/forum?id=rzQGHXNReU) | A5.2/A5.3；gold/distractor documents与quote-bearing SFT |
| [BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression](https://aclanthology.org/2025.findings-naacl.301/) | A5.2/A5.3；多文档proposition组合与压缩 |
| [CARE: Improving Context Fidelity via Native Retrieval-Augmented Reasoning](https://aclanthology.org/2025.emnlp-main.1075/) | A5.3/A5.2；retrieval-span SFT+GRPO和context fidelity |
| [MIRAGE: Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation](https://aclanthology.org/2024.emnlp-main.347/) | A5.3 evaluation；模型内部attribution诊断，不是post-training方法 |
| [Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation](https://aclanthology.org/2026.findings-acl.338/) | A5.3/A4.1；区分factuality与faithfulness的诊断方法 |
| [Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models](https://aclanthology.org/2025.findings-acl.1087/) | A5.3/A4.2；反事实作者元数据扰动揭示attribution shortcut |
| [TRACE the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation](https://aclanthology.org/2024.findings-emnlp.496/) | A5.2/A5.3 inference-only；多跳证据链构造 |

**A5关键证据层级：** `Factuality → Faithfulness → Output Reliance → Attribution Sensitivity → Compositional Reliance → Process-level Reliance`。当前文献对前几层有越来越细的诊断，但严格的必要证据删除/替换和过程级因果依赖仍不足。

---

## 7. A5.4 — Cross-turn Information Persistence and State Updating

**核心问题：** 模型能否跨轮持久化、修订和复用已获取信息，同时避免过期、冲突或无关记忆累积？

> A5.4 是 A5 的 bounded subdirection。Memory 是实现载体和评测环境，不是本 Survey 的独立 Memory taxonomy。

| Paper | A5.4 dimension / use |
|---|---|
| [LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d813d324dbf0598bbdc9c8e79740ed01-Abstract-Conference.html) | A5.4.2/A5.4.3；长期交互、时间更新和未来任务效用评价 |
| [MemoryAgentBench: Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fd1eff9dd295df50a41f2521942fa31d-Abstract-Conference.html) | A5.4.1/A5.4.2；增量交互、冲突、时间和选择性遗忘评价 |
| [Memory-R1: Enhancing Large Language Model Agents to Manage and Utilize Memories via Reinforcement Learning](https://aclanthology.org/2026.acl-long.583/) | A5.4.1/A5.4.2/A5.4.4；ADD/UPDATE/DELETE/NOOP的PPO/GRPO policy learning |
| [MemRL: Self-Evolving Agents via Runtime Reinforcement Learning on Episodic Memory](https://arxiv.org/abs/2601.03192) | A5.4.3；学习外部episodic memory的future utility，不更新基础LLM参数 |
| [A-MEM: Agentic Memory for LLM Agents](https://proceedings.neurips.cc/paper_files/paper/2025/hash/19909c36f51abc4856b4560aff3d36d6-Abstract-Conference.html) | A5.4.1/A5.4.2；memory organization和metadata evolution，非policy post-training |
| [Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory](https://doi.org/10.3233/FAIA251160) | A5.4.1/A5.4.2；实际memory lifecycle system，对照和supporting baseline |

---

## 8. A6 — Adaptive Search Control

**核心问题：** 模型进入搜索过程后，能否根据反馈继续、重试、改写、切换、验证、分配预算或停止？

| Paper | A6 sub-direction / use |
|---|---|
| [AutoSearch: Adaptive Search Depth for Efficient Agentic RAG via Reinforcement Learning](https://aclanthology.org/2026.findings-acl.1399/) | A6.2；搜索深度和停止 |
| [One Policy, Any Budget: Internalizing Budget-Aware Search via Reinforcement Learning](https://arxiv.org/abs/2609.00813) | A6.2；预算感知的Reason/Search/Answer控制和未见预算迁移 |
| [CriticSearch: Fine-Grained Credit Assignment for Search Agents via a Retrospective Critic](https://aclanthology.org/2026.findings-acl.596/) | A6.1/A6.3；反馈恢复和回顾式turn-level credit |
| [Enhancing LLM-based Search Agents via Contribution Weighted Group Relative Policy Optimization](https://aclanthology.org/2026.acl-long.1462/) | A6.1/A6.3；novel retrieval utility × reasoning correctness的贡献加权credit |

**A6关键边界：** turn-level reward或contribution-aware reward改善训练，不等于已经证明部署时模型理解了反馈。最强的行为隔离实验仍是：固定任务和历史，只改变当前检索反馈，观察下一步动作是否合理改变。

---

## 9. How to Learn — B1–B4机制锚点

这三篇论文用于补强与 A1–A6 正交的机制轴，不构成新的能力分类。

| Mechanism | Paper | What it contributes |
|---|---|---|
| **B1 Data / Signals** | [Beyond Outcome Rewards: Step-Level Self-Distilled Policy Optimization for Deep Search Agents](https://arxiv.org/abs/2608.12764) | teacher–student disagreement、evidence anchors和step-level signal；重点审查privileged information与policy-input leakage |
| **B2 Optimization / Post-training objective** | [TRACE: Turn-level Reward Assignment via Credit Estimation for Long-Horizon Agents](https://arxiv.org/abs/2607.13988) | gold-answer state value、turn-level TD credit和GRPO；改进credit但不自动证明online adaptive control |
| **B3 Update Locus / Environment control** | [Retrieval, Reward, and Training Protocols: What Matters in Training Search Agents?](https://arxiv.org/abs/2605.27881) | controlled comparison；统一GRPO下比较corpus、reward、credit、data diversity、off-policy和budget，揭示环境因素混淆 |
| **B4 Credit Assignment** | 以上三篇共同覆盖 | B4不是单独新增论文集合，而是对credit粒度、归因和部署可用性的正交分析轴 |

> README中的标签因历史迭代仍可能出现 B1/B2/B4 的多标签；正式稿应按“Data/Signals、Optimization、Update Locus、Credit Assignment”四个机制维度统一重编码，避免把 B 类误读成新的能力章节。

---

## 10. Cross-cutting anchors：跨能力使用的论文

这些论文在 README 中单列，但它们跨越多个 A 类，不应强行塞进单一能力章节。

| Paper | Main categories | Use in the survey |
|---|---|---|
| [Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection](https://arxiv.org/html/2310.11511) | A1/A4/A6 | 早期的retrieve–generate–critique闭环；检索必要性、证据批评和局部控制锚点 |
| [Beneficial Reasoning Behaviors in Agentic Search and Effective Training Methods to Obtain Them](https://aclanthology.org/2026.findings-acl.1400/) | A4/A6，兼有A3 | Information Verification、Authority Evaluation、Adaptive Search、Error Recovery的行为标签与训练 |
| [Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning](https://arxiv.org/html/2503.09516v5) | A2/A5/A6 | outcome-RL搜索策略的代表性端到端基线；query、搜索调用和证据整合混在同一结果中 |
| [R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning](https://arxiv.org/html/2503.05592v2) | A6，兼有A2 | 分阶段学习search invocation与search-augmented reasoning |
| [ZeroSearch: Incentivize the Search Capability of LLMs without Searching](https://arxiv.org/html/2505.04588v3) | A6，兼有A2/A3 | simulated search environment、curriculum RL与sim-to-real边界 |
| [Agentic-R: Learning to Retrieve for Agentic Search](https://aclanthology.org/2026.findings-acl.785/) | A5，兼有A2/A4/A6 | retriever–agent joint optimization；用于说明系统增益不能直接归因于LLM policy能力 |
| [Rethinking Reasoning-Intensive Retrieval: Evaluating and Advancing Retrievers in Agentic Search Systems](https://aclanthology.org/2026.acl-long.1705/) | A4/A5 | reasoning-intensive retrieval评价；区分retriever质量与证据整合/下游效用 |

---

## 11. 最终使用建议：正文如何引用

1. **正文主章节：** A1、A2、A4、A5、A6；A3作为跨能力维度和独立小节保留；A5.4作为A5内部bounded subdirection。
2. **横向机制章节：** 不要再按B1–B4新增一套论文综述，而是把三篇Round 8机制论文映射到已有A1–A6，回答 `Learning Signal → Update Locus → Credit → Observed Behavior`。
3. **Related Work：** 使用7篇相邻综述，但准确写成“本 Survey在已有系统/流程/RL/Agentic RAG/credit assignment地图之上，增加能力—机制—证据对齐”，不要声称首次采用capability视角。
4. **证据强弱：** Evaluation/benchmark论文用于定义和测量能力，Learning论文用于分析post-training，Inference-only和Boundary论文用于提供对照；三类证据不能混写。
5. **不要把标签当结论：** A1–A6是分析单位，不表示每篇论文已经证明相应能力被学会；最终判断必须回到行为测量、受控干预、特权监督、policy-input leakage、部署行为证据和泛化。

## 12. 与仓库其他核心文件的关系

- [Final Cross-Paper Evidence Matrix](./final-cross-paper-evidence-matrix.md)：逐篇记录训练信号、优化器、更新位置、credit、行为证据、干预、特权监督和泛化。
- [Claim–Evidence Matrix](./claim-evidence-matrix.md)：把跨能力结论拆成 Supporting、Counterevidence、Partial Mitigation、Not Established。
- [Cross-Capability Synthesis Draft](./cross-capability-synthesis-draft.md)：把以上论文转化成“实际学到了什么”的综合论证。
- [Related-Survey Comparison](./related-survey-comparison.md)：7篇相邻综述的正式比较。
