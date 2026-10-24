# Survey Structure — Learning Information-Seeking Capabilities in LLMs

## Recommended title

### Primary title

**What Do LLMs Learn to Seek? A Survey of Post-training for Generalizable Information-Seeking Capabilities**

### Alternative title

**Beyond Retrieval: Training, Isolating, and Generalizing Information-Seeking Capabilities in Large Language Models**

第一版更强调研究问题，第二版更强调与已有 Retrieval/RAG/Search-Agent survey 的区分。建议主标题使用第一版，副标题可以使用第二版的后半部分。

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

## Recommended chapter outline

### 1. Introduction

回答三个问题：

- 为什么 Retrieval/Search 是 foundation model 完成复杂任务的基础能力；
- 为什么“能调用检索工具”不等于“学会了信息寻求”；
- 为什么需要从 post-training signal、behavior isolation 和 transfer evidence 重新审视现有工作。

本章提出 central question 和三条贡献：

1. 一个 capability-centered taxonomy；
2. 一个 Training Signal → Behavior → Isolation → Generalization 的统一分析 protocol；
3. 四类跨论文 training–capability misalignment taxonomy。

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

### 4. A1 — Epistemic Awareness: Is Information Acquisition Worthwhile?

核心问题：

> Can the model predict whether acquiring external information will improve utility enough to justify its cost?

建议结构：

1. answerability / uncertainty proxy；
2. retrieval utility measurement；
3. ex-post utility-aware policy learning；
4. cost-sensitive decision；
5. capability isolation and ex-ante value gap；
6. model/evidence/task/cost dependence。

本章结论：

> Retrieval necessity is evolving from proxy-based triggering toward utility-aware policy learning, but observed retrieval gains do not establish transferable ex-ante value calibration.

### 5. A2 — Information Need Formulation: Can the Model Express What It Needs?

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

### 6. A3 — Information Space Exploration: What Has Not Been Covered?

作为 cross-capability subsection，而非完全独立 learning chapter。

建议结构：

1. source/tool selection；
2. coverage state and unresolved regions；
3. query diversity, evidence diversity, information gain, useful coverage；
4. MARAG-R1 and DDPO as learning anchors；
5. tool discovery as boundary contrast；
6. unseen-environment navigation gap。

本章结论：

> Exploration can be trained when rewards measure coverage or diversity, but evidence is insufficient for a transferable policy that discovers useful information regions in unfamiliar spaces.

### 7. A4 — Evidence Evaluation: Which Acquired Information Is Worth Trusting?

核心问题：

> Can the model judge relevance, support, source reliability, sufficiency, and conflict before using evidence?

建议结构：

1. relevance and claim support；
2. source authority/reliability；
3. evidence sufficiency and missing gaps；
4. conflict and robustness；
5. measurement resources versus actual post-training；
6. held-out verifier and controlled perturbation gap。

本章结论：

> Measurement is ahead of post-training and cross-environment behavior transfer, especially for source reliability and conflict robustness.

### 8. A5 — Evidence Integration: Does the Model Actually Use the Evidence?

核心问题：

> Can the model faithfully integrate acquired and evaluated information into reasoning, belief, generation, decision, and persistent state?

按三层 evidence reliance 组织：

1. Output Reliance；
2. Compositional Reliance；
3. Process-level Reliance。

然后讨论：

- parametric–external conflict and belief revision；
- multi-evidence composition；
- evidence-conditioned generation and citation；
- A5.4 cross-turn persistence/state updating。

本章结论：

> Current literature most reliably shows evidence access and output sensitivity, less reliably shows compositional reliance, and does not yet establish necessary process-level causal evidence reliance that transfers across environments.

### 9. A6 — Adaptive Search Control: How Should the Policy React?

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

### 10. Cross-capability analysis: What does post-training actually teach?

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

### 11. Evaluation protocol for future work

提出跨能力最小实验套件：

1. Paired no-information / actual-information / oracle-information；
2. Query-only intervention；
3. Source/coverage intervention；
4. Evidence-quality factorial perturbation；
5. Necessary-evidence leave-one-out；
6. Fixed-history feedback swap；
7. Full transfer grid。

这一章的目标不是要求所有工作一次性完成全部实验，而是给出可复用的证据标准。

### 12. Open problems and research agenda

按可检验问题写，而不是泛泛地说“需要更好的泛化”：

- ex-ante value calibration；
- semantic intent under backend adaptation；
- useful coverage under unfamiliar spaces；
- verifier-independent evidence evaluation；
- necessary evidence reliance；
- online feedback-conditioned control；
- behavior transfer across task/domain/tool/environment/cost。

### 13. Conclusion

收束为一句话：

> The next frontier is not teaching LLMs to retrieve more, but establishing which information-seeking behaviors post-training actually creates, how those behaviors can be causally isolated, and whether they survive changes in the information environment.

## Expected contribution claims

最终论文可以较有把握地声称：

1. 现有 Retrieval/RAG/Search-Agent survey 多按 pipeline、architecture、optimization 或 application 分类；本 Survey 改为 capability-centered comparison；
2. 统一了 training signal、observed behavior、capability isolation、generalization evidence 四个层次；
3. 提出了四类可由现有论文支持的 misalignment patterns，而不是简单声称“泛化不足”；
4. 将 A3 明确为横向探索能力，将 A5.4 限制为持久信息整合子方向，避免分类发散；
5. 给出了跨能力的 intervention and transfer audit，可作为未来 post-training information-seeking 工作的最低证据协议。

## What the survey should not claim

- 不要声称所有现有方法都没有学到能力；
- 不要把 final QA improvement 说成完整 information-seeking capability；
- 不要把新 benchmark 说成 behavior transfer；
- 不要把 hindsight credit assignment 说成 online feedback control；
- 不要把 memory architecture survey、retriever optimization survey 和本 Survey 的 capability-learning 问题混为一谈；
- 不要为了强调新颖性而声称“完全没有类似工作”。更准确的说法是：已有 survey 讨论 Search/RAG/Agent pipeline，而本 Survey 关注 post-training 是否真正产生可迁移 information-seeking capability，以及如何证实这一点。

## Existing analysis artifacts

- [Cross-Paper Evidence Matrix](./cross-paper-evidence-matrix.md)
- [Training–Capability Misalignment Taxonomy](./training-capability-misalignment.md)
- [Generalization & Intervention Audit](./generalization-intervention-audit.md)
- [Unified Coding Protocol](../notes/coding-protocol.md)
