# Survey Structure — Learning Information-Seeking Capabilities in LLMs

## Recommended title

### Primary title

**Beyond Retrieval: Learning Information-Seeking Capabilities in Large Language Models**

### Alternative title

**What Do LLMs Learn to Seek? A Survey of Post-training for Generalizable Information-Seeking Capabilities**

主标题使用 `Beyond Retrieval`，避免把文章理解成单纯的 Evidence Audit；副标题和摘要中用 `What Do LLMs Learn to Seek?` 明确研究问题。`Capability-centric, evidence-driven` 是本文的分析视角，而不是对文章范围的缩减。不要在标题中声称 `The First Systematic Survey`。

## Central research question

> **What information-seeking capabilities do LLMs actually acquire through post-training, how do training signals shape these behaviors, and what evidence establishes their transfer beyond the training environment?**

这比“LLM 如何使用 retrieval”更精确，因为它不把搜索工具调用、最终答案提升和信息寻求能力默认视为同一件事。

## Main thesis

> **Post-training can induce useful information-seeking behaviors, but current literature rarely proves all three links simultaneously: that the training signal directly corresponds to the claimed capability, that the target behavior is causally isolated from retriever/generator/system shortcuts, and that it transfers beyond the original task and information environment.**

中文表述：

> 后训练可以让 LLM 学会有用的信息获取行为，但现有研究很少同时证明三件事：训练信号确实对应声称的能力；目标行为已经从检索器、生成器和系统捷径中被因果隔离；以及该行为能够迁移到原始任务和信息环境之外。

## The survey’s unit of analysis

本 Survey 不以“模型架构”或“应用场景”作为一级分类，而以 information-seeking capability 作为一级分类：

1. **A1 Epistemic Awareness**：是否知道何时缺信息、何时值得获取；
2. **A2 Information Need Formulation**：能否把目标和知识缺口表达成有效 query/sub-question/plan；
3. **A3 Information Space Exploration**：能否发现新的信息源或未覆盖的必要区域；
4. **A4 Evidence Evaluation**：能否判断信息是否相关、支持、可靠、充分、无冲突；
5. **A5 Evidence Integration**：能否用这些信息更新推理、belief、generation、decision 和跨轮状态；
6. **A6 Adaptive Search Control**：能否根据反馈和成本继续、重试、改写、切换、验证或停止。

A3 保留为独立定义的横向能力维度和专门小节，但不强行扩展为完整 vertical learning chapter。A5.4 作为 A5 中的 bounded subdirection，讨论跨轮信息持久化和状态更新，不另起 Memory Survey。

## The common analysis protocol

每类能力都用同样的五步结构：

### 1. Capability definition

明确模型需要学会的行为，而不是论文使用的系统组件。例如，A2 不是“会生成 query”，而是“能表达真正的信息需求，并在后端变化时保持意图或合理适配”。

### 2. Training signal

编码 SFT、preference、process supervision、RL、GRPO/PPO、counterfactual reward、cost reward、evidence reward、hindsight critic 等训练信号。

### 3. Learned or observed behavior

只记录实验真正观察到的行为：触发、改写、覆盖、判断、证据依赖、停止、恢复、预算分配等。不把最终 QA 分数直接翻译成能力名称。

### 4. Capability isolation

询问是否存在 query-only、source-removal、evidence replacement、noise/authority perturbation、necessary-evidence deletion、fixed-history feedback intervention 等受控实验。

### 5. Generalization evidence

按 Task、Domain、Retriever、Tool、Corpus/Environment、Budget/Cost、Temporal Shift、Behavior Transfer 分开报告。`Not Tested` 和 `Tested but Failed` 必须严格区分。

## Frozen top-level manuscript structure

为避免 `Capability Taxonomy → How to Learn → A1–A6` 造成重复，最终论文只保留以下七个顶层章节。A1–A6 是 **Section 3 — What to Learn** 的能力子节，不再作为与 Taxonomy 平行的第二套分类。

1. **Introduction：** 问题定义、范围、中心问题和三个贡献；
2. **Scope and Related Surveys：** 文献选择方法、相邻 Survey 对比和边界；
3. **What to Learn：** A1–A6 能力定义、边界、局部学习证据；A3 作为横向探索能力小节，A5.4 保持为 A5 的 bounded subdirection；
4. **How to Learn：** 四个学习机制维度；
5. **What Was Actually Learned?：** Training–Capability Alignment、Capability Isolation、Deployment Information Parity、四类 Claim 和跨环境迁移；
6. **Evaluation Principles and Open Problems：** 可复用的 intervention/transfer checklist 与未解决问题；
7. **Conclusion：** 核心发现和研究议程。

后文的详细 A1–A6 结构是 Section 3–5 的写作映射，不应再被排版成额外的顶层章节。

## Recommended chapter outline

### 1. Introduction

回答三个问题：

- 为什么 Retrieval/Search 是 foundation model 完成复杂任务的基础能力；
- 为什么“能调用检索工具”不等于“学会了信息寻求”；
- 为什么需要从 post-training signal、behavior isolation 和 transfer evidence 重新审视现有工作。

本章提出 central question 和三项贡献：

1. **Capability Taxonomy：** 定义 A1–A6 Information-Seeking Capabilities 及其边界；
2. **Learning Mechanism Synthesis：** 以四维 post-training mechanism 解释训练信号、优化、更新对象和 credit assignment 如何塑造行为；
3. **Capability Evidence and Generalization：** 通过行为测量、受控干预、特权监督、policy-input leakage、部署行为证据和迁移审查 What Was Actually Learned。四类跨论文 misalignment patterns 是第三项贡献的分析结果，而不是额外分类。

### 2. Scope and conceptual framework

简要区分：

- retrieval model / retriever optimization；
- RAG generation；
- search agent / browser agent；
- tool use；
- memory system；
- information-seeking capability learning。

本章不按系统架构穷举，而是说明这些系统只有在能支持能力定义、训练机制、能力测量或泛化边界时才进入核心文献。

### 3. A capability-centered taxonomy

给出六类能力图，并说明边界：

`Need to acquire → Formulate need → Explore space → Evaluate evidence → Integrate evidence → Control loop`

需要强调这不是一个严格线性 pipeline。A1、A2、A3、A6 会循环发生；A4/A5 作用于获取到的证据；A5.4 是 A5 的跨轮状态子方向。

### 4. How to Learn Information-Seeking Capabilities

这一章不新增 capability，也不扩展成通用 RL Survey，只保留四个机制小节：

1. **Data and Learning Signals：** demonstrations、trajectories、outcome、process、preference、teacher distribution、counterfactual utility；
2. **Policy Learning and Optimization：** offline SFT/distillation、DPO/IPO、PPO/GRPO/REINFORCE、OPD 和 hybrid methods；
3. **Learning Targets and Optimization Loci：** LLM search/reasoning policy、query rewriter、retriever、verifier、reader、memory policy 和 joint optimization；
4. **Long-horizon Credit Assignment：** episode、turn、step、token/span、state-transition credit，以及 outcome/process credit 的差异。

必须明确：GRPO 是优化方法，不代表使用 outcome reward；OPD 是 teacher–student 学习机制，不自动等于传统 reward-based RL；process reward 的存在也不等于模型获得了 process-level capability。

Deployment Information Parity 不作为第五个学习机制，而作为后续 Cross-Capability Analysis 中贯穿 A1、A5、A6 的 Training–Capability Alignment 诊断。核心问题是：teacher、critic 或 verifier 是否使用了部署时不可得的 privileged information，以及去除这些信息后 student 是否仍然学会在线策略。

Round 8 的定向机制核验已经完成，只纳入三篇 P0：SSPO、TRACE 和 controlled training comparison。它们分别补强 on-policy/self-distillation、turn/state-value credit 和环境/协议混淆控制；没有为了凑足 4–6 篇而继续扩展。详细核验见 [`round8-learning-mechanism-verification.md`](../papers/round8-learning-mechanism-verification.md)。

机制章节的统一表格应采用：

`Learning Signal × Optimization Method × Update Target × Credit Granularity`

完整定义见 [`How to Learn Information-Seeking Capabilities`](./learning-mechanism-framework.md)。

### 5. A1 — Epistemic Awareness: Is Information Acquisition Worthwhile?

核心问题：

> Can the model predict whether acquiring external information will improve utility enough to justify its cost?

建议结构：

1. answerability / uncertainty proxy；
2. retrieval utility measurement；
3. ex-post utility-aware policy learning；
4. cost-sensitive decision；
5. ex-post utility versus ex-ante value prediction；
6. external calibration prior versus intrinsic LLM calibration；
7. capability isolation and model/evidence/task/cost dependence。

本章结论：

> Retrieval necessity is evolving from proxy-based triggering toward utility-aware policy learning, but observed retrieval gains do not establish transferable ex-ante value calibration.

### 6. A2 — Information Need Formulation: Can the Model Express What It Needs?

核心问题：

> Can the model formulate queries or sub-questions that preserve the true information need while adapting to search environments?

建议结构：

1. single-query rewriting；
2. query expansion and decomposition；
3. reasoning-conditioned refinement；
4. retriever-aware query dialect；
5. Query Quality vs Backend Utility vs Transferable Query Learning；
6. strict Query-only intervention。

本章结论：

> LLMs learn useful query behaviors, but current evidence is mostly retriever- or benchmark-conditioned backend utility rather than generalizable information-need formulation.

### 7. A3 — Information Space Exploration: What Has Not Been Covered?

作为 cross-capability subsection，而非完全独立 learning chapter。

建议结构：

1. source/tool selection；
2. coverage state and unresolved regions；
3. query diversity, evidence diversity, information gain, useful coverage；
4. MARAG-R1 and DDPO as learning anchors；
5. GISA as trajectory-level useful-coverage evaluation；
6. SearchOS-V1 as coverage-state and failure-recovery boundary evidence；
7. tool discovery as boundary contrast；
8. unseen-environment navigation gap。

本章结论：

> Exploration can be trained when rewards measure coverage or diversity, but evidence is insufficient for a transferable policy that discovers useful information regions in unfamiliar spaces.

### 8. A4 — Evidence Evaluation: Which Acquired Information Is Worth Trusting?

核心问题：

> Can the model judge relevance, support, source reliability, sufficiency, and conflict before using evidence?

建议结构：

1. relevance and claim support；
2. source authority, convincingness, and reliability cues；
3. evidence sufficiency and missing gaps；
4. typed conflict and desired response behavior；
5. measurement resources versus actual post-training；
6. source/order/time perturbation and held-out verifier gap。

本章结论：

> Measurement is ahead of post-training and cross-environment behavior transfer, especially for source reliability and conflict robustness.

### 9. A5 — Evidence Integration: Does the Model Actually Use the Evidence?

核心问题：

> Can the model faithfully integrate acquired and evaluated information into reasoning, belief, generation, decision, and persistent state?

先区分 factuality 与 faithfulness，再按 evidence reliance 组织：

1. Factuality and Faithfulness；
2. Counterfactual evidence necessity and external intervention（CF-RAG）；
3. Output Reliance；
4. Attribution Sensitivity；
5. Compositional Reliance；
6. Process-level Reliance。

然后讨论：

- parametric–external conflict and belief revision；
- multi-evidence composition；
- evidence-conditioned generation, citation, and attribution shortcuts；
- A5.4 cross-turn persistence/state updating。

本章结论：

> Current literature most reliably shows evidence access and output sensitivity, less reliably shows compositional reliance, and does not yet establish necessary process-level causal evidence reliance that transfers across environments.

### 10. A6 — Adaptive Search Control: How Should the Policy React?

核心问题：

> Can the model condition its next search action on current feedback and cost, rather than follow a fixed search template?

按控制对象组织，而不是按“所有 Search-RL 方法”组织：

1. search depth and stopping；
2. budget allocation；
3. feedback recovery；
4. hindsight and contribution-aware credit assignment；
5. fixed-history feedback intervention；
6. budget versus Retriever/Tool/Environment transfer。

本章结论：

> Better credit assignment does not necessarily imply better feedback-conditioned control.

### 11. Cross-capability analysis: What does post-training actually teach?

这是 Survey 的核心章节，不是附录。

#### 10.1 Unified evidence matrix

展示六类能力的 Learning Target、Training Signal、Observed Behavior、Capability Isolation、Generalization Evidence。

#### 10.2 Four misalignment patterns

1. Objective–Capability Misalignment；
2. Outcome–Behavior Confounding；
3. Hindsight–Deployment Gap；
4. Environment-conditioned Learning vs Transferable Capability。

#### 10.3 Evidence maturity is multidimensional

不要给能力做单一成熟度排名，而是分别比较：

- training signal directness；
- behavior observability；
- capability isolation；
- transfer coverage；
- failure attribution quality。

#### 10.4 Positive evidence and negative evidence

正面证据说明某个行为在某些条件下可被训练；负面证据说明该行为在改变 retriever/tool/corpus/feedback 后未被证明或发生退化。两者都应进入矩阵。

#### 10.5 Deployment Information Parity and Generalization

Deployment Information Parity 与 Generalization 是统一的横向诊断维度，而不是新的 Learning Method。需要记录：训练时可见的信息、部署时可见的信息、是否存在 gold/future/oracle privilege、去除 privilege 后的行为保持情况，以及目标行为在新 task/domain/retriever/tool/corpus/budget 中是否迁移。

这一区分避免把“使用了 Gold Answer 的训练”简单判为错误，也避免把 teacher-guided learning 的结果直接解释成在线策略学习。真正需要验证的是：特权信息是否泄漏进 student 的行动目标，以及 student 是否能够在当前观察下做出正确决策。

### 12. Evaluation protocol for future work

提出跨能力最小实验套件：

1. Paired no-information / actual-information / oracle-information；
2. Query-only intervention；
3. Source/coverage intervention；
4. Evidence-quality factorial perturbation；
5. Necessary-evidence leave-one-out；
6. Fixed-history feedback swap；
7. Full transfer grid。

这一章的目标不是要求所有工作一次性完成全部实验，而是给出可复用的证据标准。

### 13. Open problems and research agenda

按可检验问题写，而不是泛泛地说“需要更好的泛化”：

- ex-ante value calibration；
- semantic intent under backend adaptation；
- useful coverage under unfamiliar spaces；
- verifier-independent evidence evaluation；
- necessary evidence reliance；
- online feedback-conditioned control；
- behavior transfer across task/domain/tool/environment/cost。

### 14. Conclusion

收束为一句话：

> The next frontier is not teaching LLMs to retrieve more, but establishing which information-seeking behaviors post-training actually creates, how those behaviors can be causally isolated, and whether they survive changes in the information environment.

## Expected contribution claims

最终论文可以较有把握地声称三项平衡的贡献：

1. **Capability Taxonomy：** 现有 Retrieval/RAG/Search-Agent survey 多按 pipeline、architecture、optimization 或 application 分类；本 Survey 以 A1–A6 Information-Seeking Capabilities 为主要比较单位，并明确它们之间的边界；
2. **Learning Mechanism Synthesis：** 将 data/trajectory regime、learning signal、optimization、update locus 和 credit assignment 与实际观察到的 information-seeking behavior 联系起来，而不是罗列 SFT/RL/DPO/OPD 变体；
3. **Evidence and Generalization Framework：** 将 method effectiveness、behavior measurement、controlled intervention、privileged supervision、policy-input leakage、deployment behavior evidence 和跨环境 transfer 分开审查，并组织 Supporting、Counterevidence、Partial Mitigation 和 Not Established 四类证据。

四类 Training–Capability Misalignment patterns、How to Learn 的正交机制轴，以及跨能力 intervention/transfer audit，是第三项贡献的具体实现，而不是额外的 taxonomy。

## What the survey should not claim

- 不要声称所有现有方法都没有学到能力；
- 不要把 final QA improvement 说成完整 information-seeking capability；
- 不要把新 benchmark 说成 behavior transfer；
- 不要把 hindsight credit assignment 说成 online feedback control；
- 不要把 memory architecture survey、retriever optimization survey 和本 Survey 的 capability-learning 问题混为一谈；
- 不要为了强调新颖性而声称“完全没有类似工作”。更准确的说法是：已有 survey 讨论 Search/RAG/Agent pipeline，而本 Survey 关注 post-training 是否真正产生可迁移 information-seeking capability，以及如何证实这一点。

## Positioning and next execution phase

这篇 Survey 不应被定位为“第一个 Search-Agent Survey”，也不应缩减成“只有严格因果证据才纳入”的 Evaluation Survey。合适的定位是：

> **A capability-centric, evidence-driven survey of what post-training actually teaches LLMs about information seeking.**

其中 `evidence-driven` 是贯穿三项贡献的分析方法。Capability Learning 是核心证据；Capability Measurement 和 System/Boundary Evidence 仍然必须纳入，但不能被直接解释为证明 LLM 学会了对应能力。

下一阶段按以下顺序执行：

1. **完成 Related-Survey Comparison Table：** 核验相邻 Search Agent、RL-based Agentic Search、Deep Research、Agentic RAG 和通用 Agentic RL 综述，比较其 primary unit、learning axis、evidence audit 和 transfer analysis；
2. **补写 Systematic Search Method：** 公开检索来源、检索日期、关键词簇、纳入/排除标准、去重规则、证据角色编码和更新截止日期；当前工作可以被准确称为 structured critical survey，只有完成可复查检索流程后再决定是否使用 systematic literature review 表述；
3. **进入正式 Survey 写作：** 先写 central question、three contributions、A1–A6 capability chapters 和 What Was Actually Learned?，不再扩展 A1–A6 边界或新增第七类能力。

## Existing analysis artifacts

- [Related-Survey Comparison](./related-survey-comparison.md)
- [Final Cross-Paper Evidence Matrix](./final-cross-paper-evidence-matrix.md)
- [Earlier Capability-Level Matrix](./cross-paper-evidence-matrix.md)
- [Claim–Evidence Matrix](./claim-evidence-matrix.md)
- [Cross-Capability Synthesis Draft](./cross-capability-synthesis-draft.md)
- [Training–Capability Misalignment Taxonomy](./training-capability-misalignment.md)
- [Generalization & Intervention Audit](./generalization-intervention-audit.md)
- [How to Learn Information-Seeking Capabilities](./learning-mechanism-framework.md)
- [A1/A4/A5 Claim–Evidence Audit](./a1-a4-a5-claim-evidence-audit.md)
- [Unified Coding Protocol](../notes/coding-protocol.md)
