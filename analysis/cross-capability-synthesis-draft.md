# Round 9 — Cross-Capability Synthesis Draft

## What Was Actually Learned?

### Status of this draft

这不是完整 Survey，也不是 Introduction 或 Related Work。它只把现有矩阵转化为一章可以继续扩展的实证论证。所有判断都回指：

- [Final Cross-Paper Evidence Matrix](./final-cross-paper-evidence-matrix.md) 中的论文行 ID；
- [Claim–Evidence Matrix](./claim-evidence-matrix.md) 中的 C1–C4；
- `Supporting Evidence`、`Counterevidence`、`Partial Mitigation` 和 `Not Established` 四种角色。

本章的基本单位不是“某方法的最终分数”，而是：

```text
Training Signal
→ Observed Behavior
→ Capability Isolation
→ Deployment Information Parity
→ Generalization / Behavior Transfer
```

因此，“模型学会了某种信息寻求能力”只有在至少两个层次上得到支持时才应使用：一方面，训练后确实出现了目标行为；另一方面，实验能够排除主要的系统捷径，并在改变相关环境条件后重新观察到该行为。只有最终 QA、EM/F1、引用数量或平均搜索轮数时，本文将其称为 `observed task behavior`，而不直接称为完整 capability acquisition。

---

## 4.1 Training–Capability Alignment

### 4.1.1 Alignment is a spectrum, not an optimizer ranking

现有工作不构成一条简单的 `outcome reward → process reward → teacher signal → causal capability` 单调进化链。不同信号实际上监督了不同对象：最终答案 reward 监督任务结果，retrieval-gain reward 监督某个后端上的检索效用，evidence reward 监督支持关系或证据形式，state-value reward 监督某个估计器判断的进展，而 teacher distribution 则监督 teacher 与 student 的行为差异。信号更细，并不自动意味着它更接近 Survey 所定义的高层 capability。

这一点首先在 A1 中体现得最清楚。早期方法用 answerability 或 uncertainty 近似“是否需要检索”：When to Retrieve 的 `<RET>` 标签直接编码触发行为，KBM 将 knowledge boundary 作为可预测对象，而 DRAGIN、SeaKR 使用模型内部不确定性控制检索（A1-01–A1-04）。这些方法证明 retrieval triggering 可以被显式建模，但目标仍是“模型是否答对”或“模型是否不确定”，不是检索带来的预期净收益。后续 LLM-Specific Utility、Cost-Aware Evidence Selection 和 UDCG 将评价对象移动到“同一证据对特定 LLM 是否有用”以及“收益是否值得成本”（A1-06–A1-09）。CoBRA 和 Calibrate-Then-Act 则进一步将 paired no-tool/tool margin、cost 和 calibrated prior 放进策略决策（A1-12–A1-13）。

因此，A1 的正面结论是：**训练对象已经从 trigger proxy 逐步靠近 utility-aware decision**。但 C1 的 Supporting Evidence 同时表明，很多 utility signal 仍然是 ex-post realized outcome，不能证明模型在行动前已经学会了 calibrated value-of-information prediction。CoBRA 的 paired branch 是强的局部 boundary isolation，却仍然混入专家轨迹质量、retriever 质量和单次分支差异；Calibrate-Then-Act 的 calibrated prior 还是一个外部组件，不能把它的 RL 增益直接解释成 LLM 内生校准能力。A1 的 evidence alignment 因此是局部增强，而不是完整闭环。

本节应明确区分三个命题：**retrieval triggering**（模型是否调用工具）、**ex-post utility optimization**（调用后是否带来收益）和 **ex-ante value prediction**（调用前是否预测净收益足以超过成本）。现有论文对前两个命题有较强的局部证据，但对第三个命题仍缺少在固定问题、可控检索收益和可变成本下的预测校准与 regret 测试。因此，A1 的章节结论应采用“utility-aware policy improvement”而不是“learned value-of-information calibration”。

### 4.1.2 Query utility is not semantic information-need formulation

A2 展示了同样的结构。Query Rewriting、DeepRetrieval、ExpandSearch 和 ParallelSearch 证明模型可以学习改写、扩展、分解和并行表达（A2-01–A2-04）。但是这些工作的主要信号分别是 pseudo-query likelihood、Recall/NDCG、answer-hit、SQL execution、search count 或最终 EM。它们能证明 query 对某一检索后端有用，不能单独证明 query 保留了任务的完整 semantic intent。

StepSearch 和 QREF 提供了更强的 alignment evidence。StepSearch 把 search-key、support-document information gain 和 redundancy 放入 step-level reward；QREF 比较 original query batch 与 refined query batch 的 recovered、preserved、lost 和 missed entities（A2-05、A2-07）。这些信号比 final answer reward 更接近 query/evidence behavior，因此是 C1 的部分 Counterevidence：它们说明目标能力并非完全不可监督。

但两者优化的仍是 proxy。StepSearch 的 search-key 和 TF-IDF support similarity 可能奖励 lexical overlap；QREF 的 entity-hit reward 可能奖励 snippet/entity matching、Serper-specific ranking 或 query-batch size。两者都没有独立测量 Semantic Intent Preservation，也没有完整的 query-only intervention。Environment-aware IR 的关键意义就在于，它同时证明 learnability 和 non-generality：模型可以学会不同 retriever 的 query dialect，但 source-retriever policy 在 target retriever 上不稳定（A2-06）。所以 A2 最可靠的结论不是“模型学会了通用 query formulation”，而是：

> **模型可以学习 backend utility 和 reasoning-conditioned query behavior；semantic intent 与 backend adaptation 仍然没有被统一证明。**

### 4.1.3 Exploration signals supervise coverage, not necessarily unfamiliar-space navigation

A3 进一步要求把 query diversity、evidence diversity、information gain 和 useful coverage 分开。MARAG-R1 的 gold document-ID reward 使模型更可能恢复必要支持文档；DDPO 用 query/document diversity、log-det information gain 和 Gold Passage Recall 抑制 exploration collapse（A3-04、A3-05）。这是比纯 final QA 更有针对性的正面证据，说明 source coordination 和 coverage behavior 可以被训练。

然而，两者都在预定义的 tool/retriever/corpus family 内优化。MARAG-R1 的 source inventory 是固定的，gold document IDs 将 coverage 转化为监督式 retrieval target；DDPO 的 diversity reward 可能在一个固定 backend 中形成有效的 diversity preference，却不一定能在陌生 source inventory 或 knowledge structure 中发现新的必要区域。InfoMosaic-Bench、Table-as-Search 和 Agentic Search in the Wild 反而主要贡献了测量对象：source/tool selection、unresolved coverage state 和真实轨迹中的 exploration/repetition（A3-01–A3-03）。这构成一个重要的 evidence asymmetry：**A3 的 measurement 比 policy learning 更成熟。**

Transferable Tool Selection 和 ToolSearcher 对 C4 提供了必要的 Counterevidence：tool repository selection 可以在 repository 和 retriever 改变后迁移（A3-06、A3-07）。但它们的 target 是 gold tool-set exact match，不是执行工具后获得的 task-relevant evidence。它们说明“探索能力可以迁移”这一普遍命题不能被否定，却不能直接证明 Information Space Exploration 已经解决。

### 4.1.4 Evidence evaluation has narrow direct targets but weak general judgment

A4 的训练信号最容易被误读。CRAG、RAGTruth、RAGChecker、AuthorityBench、Sufficient Context 和 ConfRAG 建立了 relevance、support、authority、sufficiency 和 conflict 的测量资源（A4-01、A4-03、A4-04、A4-07、A4-09、A4-10）。但评价资源不等于模型通过 post-training 获得了 evidence-evaluation capability。

真正的 post-training anchors 是 S2G-RAG、RLSeek 和 EAPO。S2G-RAG 训练 context-only sufficiency/gap judge，RLSeek 训练 source-span support/refute，EAPO 用 evidence utility reward 训练 long-context extraction/citation（A4-02、A4-05、A4-06）。这些方法构成 C1 的 Counterevidence：只要 target 足够窄，SFT/GRPO 确实可以塑造可观察的 verification behavior。

但是它们也说明了 alignment 的边界。S2G-RAG 的 sufficiency truth 仍与 gold-title/Wikipedia multi-hop structure 相关；RLSeek 的 quote/LCS reward 可能被 copying 或 format shortcut 满足；EAPO 的 evidence reward 与 answer reward 和 reward-model co-evolution 耦合。即使结果改善，也不能自动推出模型学会了独立的 source reliability、conflict robustness 或 held-out verifier judgment。

新增的 ConflictingQA 反事实分析（A4-11）进一步提供了关键的 counterevidence：模型可能高度依赖 query–website relevance，却忽略科学引用和中性语气等人类认为重要的可信度线索。CONFLICTS 则说明冲突不是单一标签，而是具有不同成因和期望行为的类型化问题；冲突类别提示可以改善回答，但这仍然是 prompting/behavior organization，不是跨来源、跨时间的 policy learning（A4-12）。由此，CONFACT、AuthorityBench、What Evidence Do Language Models Find Convincing? 和 (D)RAGged Into a Conflict 应被写成 measurement 与 counterevidence，而不是直接证明 LLM 通过 post-training 学会了 source reliability。

A4 因此最适合写成：

> **局部 evidence judgment 可以被训练，但从 narrow verification behavior 到 general evidence evaluation 的跃迁没有被证明。**

### 4.1.5 Evidence integration moves from output sensitivity toward process dependence—but stops before causal reliance

A5 的核心不是 evidence 是否出现在 context，而是模型是否用它更新 reasoning、belief、generation、decision 或 state。ClashEval 和 Source-balancing 的 assertion replacement 提供了最清晰的 Output Reliance：当外部证据正确而参数知识错误时，模型应改变答案；当外部证据错误而参数知识正确时，模型应抵抗改变（A5-01、A5-06）。这是很强的局部 evidence-reliance 证据，也构成对“所有 A5 工作都只是 final QA”的 Counterevidence。

然而，Output Reliance 不等于 Compositional Reliance，更不等于 Process-level Reliance。RAFT、BRIEF、CARE 和 TRACE-the-Evidence 让模型选择、压缩、引用或组合多条证据（A5-07–A5-11）；它们因此展示了 evidence-conditioned composition behavior。但这些工作的主要限制是没有完成 necessary-evidence leave-one-out：删除某一个必要 hop 后，模型是否失去对应结论；或者模型是否只是生成了一个事后看起来合理的 chain。MIRAGE 的 attribution 更接近内部过程，但它是 post-hoc diagnostic，不是 causal process proof（A5-10）。

GuarantRAG、Efficient Prior-Guided Reasoning 和 Source-balancing 是真正的 parameter-updating interventions，但它们的 target 仍有差异：GuarantRAG 的 DPO 偏好 retrieved answer，存在 external obedience 风险；Efficient Prior-Guided Reasoning 用 answer-F1 reward，可能通过 parametric prior 或 answer shortcut 获得收益；Source-balancing 主要改善 answer-level source arbitration。

FRANQ 补上了一个不能省略的 measurement distinction：factuality 与 faithfulness 不是同一属性。一个答案可以是事实正确但没有被当前 retrieval 支持，也可以忠实复述一个错误的 retrieved claim（A5-18）。与此同时，Attribution Bias 的反事实作者元数据实验显示，引用/归因质量会在内容不变时因 authorship cue 而改变，说明 citation presence 不能直接当作 content-based evidence reliance（A5-19）。因而 A5 的证据层级应从 `factuality → faithfulness → output reliance → attribution sensitivity → compositional reliance → process-level reliance` 逐级审查，而不是用单一 faithfulness 或 citation 分数概括。

A5 的最稳健结论因此是：**evidence access、factuality/faithfulness diagnostics 和 output sensitivity 已经有较强证据，compositional reliance 只有间接证据，process-level causal reliance 尚未建立；即使存在 citation，也不能排除答案或归因由上下文匹配、参数知识或元数据 shortcut 产生。**

### 4.1.6 Adaptive control is where credit and capability are most easily conflated

A6 将“模型会搜索”拆成 continue、retry、reformulate、switch、verify、budget allocation 和 stop。AutoSearch 和 AnySearch 直接优化 depth/stopping 与 budget allocation；CriticSearch、CW-GRPO 和 TRACE-turn 主要改善 turn/round/state credit；SSPO 则把 teacher disagreement 转换为 step-level advantage weighting（A6-07–A6-13）。

这组工作说明 structured supervision 和 finer credit 确实可以改善训练稳定性、搜索深度、预算效率或 final performance。但 C1 的核心警告是：**credit assignment 的粒度不是 capability 的定义。** CriticSearch 证明 retrospective critic 能为 early action 提供更密的训练信号；TRACE-turn 证明 Gold-answer state-value 可以改善 turn-level RL；SSPO 证明 step-level teacher weighting 比直接 token distillation 更适合某种 deep-search optimization。它们都没有完成固定历史、只改变当前 feedback、直接测 next action 的 intervention。

Round 8 的 controlled comparison 进一步限制了“更复杂 reward 必然更好”的解释：corpus completeness、tool-call format 和 protocol choice 可能比 credit algorithm 的差异更影响结果，outcome-only reward 在多个设置仍是强 baseline（A6-13）。因此，A6 的可靠论证应该是：

> **后训练可以塑造停止、预算和搜索轮次行为，也可以改善 long-horizon credit；但这还不足以证明模型学会了部署时的 feedback-conditioned control rule。**

---

## 4.2 Capability Isolation

### 4.2.1 Three levels of evidence

Cross-Paper Matrix 显示，论文中的“behavior evidence”至少有三个层次：

1. **Endpoint evidence：** final answer、EM/F1、citation count、tool productivity 或 average turns 发生变化；
2. **Behavioral proxy evidence：** query coverage、source selection、support span、sufficiency label、search depth 或 memory operation 发生变化；
3. **Intervention evidence：** 只改变与 capability 定义相关的变量，并观察预期行为变化，例如 no-tool/tool branch、original/refined query、assertion replacement、source perturbation 或 fixed-history feedback swap。

很多论文从第一层直接跳到 capability claim。最终矩阵的价值，是将三层证据分开，而不是给论文一个单一 overall score。

### 4.2.2 Where isolation is genuinely stronger

现有文献并非没有能力隔离。A1 的 CoBRA 固定同一 query 比较 no-tool/tool branch，能够直接观察 over-calling 与 under-calling 的变化（A1-12）。A5 的 ClashEval 和 Source-balancing 替换 answer-bearing assertions，能够检测模型是否在正确的方向上更新或保持（A5-01、A5-06）。A2 的 QREF 比较 original/refined query batch，至少将 query refiner 的覆盖收益与 final answer reward 部分区分开（A2-07）。A4 的 RAGChecker、AuthorityBench 和 Sufficient Context 则把 claim support、authority、sufficiency 和 noise/format perturbation 转化为独立的测量轴（A4-04、A4-07、A4-09）。

A6 的 AnySearch 也比单纯 final QA 更接近行为证据：它报告 search count、token usage、early stopping、Tool Productivity 和 scaffold removal，并测试 unseen budgets（A6-08）。Round 8 的 controlled comparison 进一步对 corpus、backbone、format、budget 和 credit algorithm 进行统一控制（A6-13）。这些论文构成对 C2 的 Counterevidence 或 Partial Mitigation：当前领域已经出现了 capability isolation 的局部协议，不能把整个领域描述成“只有最终答案”，但局部隔离也不能被写成完整因果归因。

### 4.2.3 Why local isolation is not complete isolation

这些正面实验通常只隔离一个因素，仍然留下其他混淆：

- CoBRA 的 branch margin 仍混合 expert quality、retriever quality、judge noise 和 sampling variance；
- QREF 的 original/refined difference 仍依赖 Serper ranking、top-10 entity matching 和 Evidence Filter；
- ClashEval 的 answer switching 只说明 output sensitivity，不说明 reasoning process 的 evidence dependence；
- AnySearch 的 unseen-budget behavior 只覆盖 cost dimension，不覆盖 retriever/tool/environment；
- RAGChecker 的 metric isolation 说明 evaluation decomposition，不说明 policy 学会了 evidence judgment；
- Memory-R1 的 ADD/UPDATE/DELETE/NOOP action space 与 capability 对齐，但 downstream QA reward 仍不能判断某一次操作是否正确。

因此，Capability Isolation 不能被理解为“有 ablation 就是 Strong”。真正的强隔离需要同时固定 generator、reader、retriever、corpus 和 budget，并对目标外部信息做最小干预。对于 A6，最严格的协议是：固定 task 和 completed history，只替换 current feedback，然后测 next action distribution；目前矩阵中仍然没有论文完成这一测试（见 C2、C3 和 A6-07–A6-13）。

### 4.2.4 Capability isolation is capability-specific

不同能力需要不同干预，不能用一个通用 ablation 替代：

- A1 需要 paired no-tool/tool potential outcomes 和 pre-action calibration；
- A2 需要 query-only intervention，并同时测 semantic intent、retrieval utility 和 evidence coverage；
- A3 需要 source-removal 或 unresolved-region intervention；
- A4 需要 authority、truthfulness、conflict、order 和 freshness 的 factorial perturbation；
- A5 需要 necessary-evidence replacement/deletion，分开看 output、composition 和 process；
- A6 需要 fixed-history feedback swap，直接看下一动作。

这也是为什么“跨 benchmark 结果都提升”不能替代 capability isolation：benchmark 变化很少只改变一个能力相关变量。

---

## 4.3 Hindsight and Deployment Information Parity

### 4.3.1 Privileged supervision is useful but changes the claim

Gold Answer、完整未来轨迹、oracle evidence 和 realized branch outcome 并不是方法缺陷本身。它们可以显著改善 credit assignment，尤其是在 long-horizon search 中难以获得 dense signal 时。问题是：这些信息在训练时如何使用，以及部署时 student 是否能仅凭当前可见 state 复现相应策略。

CriticSearch 的 retrospective critic 读取完整轨迹和 Gold Answer，再对每轮动作赋 Good/Bad 标签；AutoSearch 用最早达到 gold exact match 的步骤定义 optimal depth；TRACE-turn 用 Gold Answer 的 prefix probability 构造 state value；SSPO 用 Evidence Anchors 和 student errors 生成 teacher signal（A6-09、A6-07、A6-11、A6-12）。这些论文都支持“privileged hindsight 可以帮助训练”的判断，却同时支持 C3：**训练期 credit improvement 不等于部署时 online information understanding。**

A1 中的 InfoReasoner、s3 和 CoBRA 具有同样结构。它们的 policy 在部署时可以 ex-ante 做决定，但 reward 是在搜索完成后由 gold answer、naive-RAG baseline 或 paired branch outcome 计算得到的（A1-10–A1-12）。这使它们能够学习一种 action policy，却尚未证明 policy 在行动前拥有相同的 value estimate。

### 4.3.2 Partial counterevidence: state-visible and scaffold-removal cases

C3 不能被写成“所有后训练都依赖 deployment-inaccessible information”。When to Retrieve 的 `<RET>` 触发在部署时只依赖当前问题，S2G-RAG 的 sufficiency judge 只接收 context，Calibrate-Then-Act 使用 pre-action calibrated prior 和 cost，AnySearch 在第二阶段移除显式 budget scaffold，Beneficial Reasoning Behaviors 先以行为标签训练再进行 outcome RL（A1-01、A4-02、A1-13、A6-08、A6-05）。这些是重要的 Counterevidence：部分能力可以被编码为 deployment-visible state 或通过 scaffold removal 检验。

但 state-visible input 不等于 state-sufficient capability。S2G-RAG 的标签仍由 gold-title/Wikipedia structure 影响；Calibrate-Then-Act 的 prior 是外部组件；AnySearch 的 scaffold removal 只验证预算内化，不验证空结果、重复证据或冲突反馈下的 retry/switch/verify；行为 priming 也未完成跨环境 behavior transfer。因此，parity 审计需要同时记录：

```text
training-visible information
→ student input at deployment
→ action target
→ no-privilege behavior retention
→ behavior under altered feedback
```

### 4.3.3 The required distinction

本 Survey 不应把 Gold-conditioned training 简单判为“无效”，也不应把使用 Gold 的方法直接称为 online policy learning。更加准确的区分是：

- **Hindsight credit:** 训练时利用未来或答案来判断过去动作的相对价值；
- **Deployment policy:** 部署时只根据当前 state 和 feedback 选择动作；
- **Parity evidence:** 证明 hindsight signal 没有把答案路径直接泄漏进 student，并且 student 在 current observation 下仍能做出合理决策。

当前矩阵对第一点有较强证据，对第二点有大量方法结果，对第三点只有局部证据。最缺失的不是更复杂的 critic，而是 same-history feedback intervention、no-privilege ablation 和 next-action measurement。

---

## 4.4 Generalization Beyond the Training Environment

### 4.4.1 Transfer must be decomposed

“Generalization”在现有论文中经常被用来指多个不同对象：新 benchmark、新 domain、新 backbone、新 retriever、新 tool、新 corpus 或 unseen budget。它们不能合并成一个 Yes/No 标签。矩阵采用以下 transfer tuple：

```text
Task · Domain · Retriever · Tool · Corpus/Environment · Budget/Cost · Temporal/State · Behavior
```

其中最重要的是最后一项 Behavior Transfer：改变环境后，目标行为本身是否重新被测量，而不是只报告最终答案。

### 4.4.2 Task and domain transfer are the most common positive evidence

当前池中最普遍的是 Task OOD 和有限 Domain OOD。A1 的 s3、CoBRA、InfoReasoner，A2 的 DeepRetrieval、StepSearch、QREF，A3 的 MARAG-R1/DDPO，A4/A5 的多数据集验证，以及 A6 的 CriticSearch/CW-GRPO，都报告了多个 benchmark 或 task setting。它们支持一个有限的正面结论：方法并非只在单一训练集上有效。

但这些 transfer 往往保持 retriever、tool API、corpus structure、feedback format 和 reader 近似不变，因此更多说明 task-level robustness，而不是 generalizable information-seeking capability。QREF 是典型例子：两个 DeepSearch Agent Backbone 上都有效，属于 Agent Backbone Transfer；两个 backbone 仍使用同一 Serper stack，因此不能计为 Retriever Transfer（A2-07）。

### 4.4.3 Retriever, tool and corpus transfer remain sparse

Environment-aware IR 是最重要的负向证据。它展示不同 retriever 的 query dialect，并报告 source retriever 到 target retriever 的不稳定迁移（A2-06）。这说明 backend adaptation 是真实可学习的行为，但也说明该行为可能被固定 retriever 的词法、字段结构和 ranking preferences 所条件化。

A3 的 MARAG-R1 和 DDPO 在固定 tool/retriever/corpus family 中取得 coverage/diversity gains；A6 的 AutoSearch、CriticSearch、CW-GRPO 和 TRACE-turn 大多在固定 Wikipedia/E5 或闭合搜索环境中改变 task；A4/A5 的 source ecology、reader、context format 和 verifier 通常不变。Round 8 的 controlled comparison 因此是必需的：如果 corpus 缺少 supporting documents，policy 可能依赖 parametric guessing，算法之间的差异就不再能够直接解释为 capability learning（A6-13）。

### 4.4.4 Genuine counterevidence exists, but it is dimension-specific

C4 不能被写成“没有任何迁移”。当前有几类真实的 Counterevidence：

- **Budget:** AnySearch 从训练预算不超过 5 迁移到 6–8，是本池最强的 C7 evidence（A6-08）；
- **Tool-repository transfer:** Transferable Tool Selection 在不同 tool repository 和 BM25/COLT 间迁移，但 target 是工具集合，不是信息获取（A3-06）；
- **Agent-backbone transfer:** QREF 在不同 DeepSearch backbone 上保持 query-refinement gains，但共享 web stack（A2-07）；
- **Model/domain variation:** KBM、s3、CoBRA、A4/A5 多篇工作跨模型、任务或 domain 测试，但行为是否保持通常没有重新测量。

这些结果说明 transfer 是分维度的：一个方法可以有 strong Budget Generalization，同时没有 Retriever OOD；可以有 Tool Discovery Transfer，同时没有 Useful Coverage Transfer；可以有 final QA Task OOD，同时没有 Behavior Transfer。

### 4.4.5 The most important empty cell is Behavior Transfer

C6 Behavior Transfer 在六类能力中都是最关键、同时最稀缺的维度：

- A1 需要在新模型、retriever、cost 下重测 calibration/regret；
- A2 需要在新 backend 下重测 semantic intent 和 query-only coverage；
- A3 需要在新 source inventory 下重测 source choice 和 useful coverage；
- A4 需要在 source replacement、time shift 和 held-out verifier 下重测 judgment；
- A5 需要在 evidence order、source quality 和 necessary-evidence deletion 下重测 reliance；
- A6 需要在新 feedback format 下重测 next-action response。

当前文献的大部分 transfer evidence 仍然停留在 `Task Transfer — Final Outcome Only`。这足以说明方法有一定实用性，却不足以证明目标 capability 脱离原始信息环境后仍然存在。

---

## Provisional answer to the central question

基于当前矩阵，Survey 的阶段性回答可以写成四层：

1. **What can be learned?** LLMs can learn specific information-seeking behaviors: selective retrieval, query reformulation, backend-aware querying, complementary coverage, support-span verification, source balancing, stopping, budget allocation, memory operations, and contribution-aware search updates.
2. **What do signals shape?** Outcome signals shape endpoint-oriented search policies; backend utility signals shape retriever-conditioned query behavior; evidence/process/teacher signals improve local observability or credit assignment; cost signals shape budget and stopping behavior.
3. **What is actually isolated?** The strongest current evidence is local: output-level replacement, paired tool-use boundaries, query-coverage comparison, narrow evidence judgment, depth/budget behavior, and controlled protocol comparisons. Process-level causal reliance and fixed-history feedback-conditioned control remain largely unisolated.
4. **What transfers?** Task, domain, backbone and budget transfer exist in selected cases. Retriever, tool, corpus/environment and behavior transfer are substantially less established and must be reported dimension by dimension.

因此，本章的核心结论不是“post-training 没有教会模型 information seeking”，而是：

> **Post-training creates useful, capability-shaped behaviors, but the current evidence usually identifies a behavior under a signal and an information ecology—not a fully isolated, deployment-parity, transferable information-seeking capability.**

这句话同时保留了正面和负面证据：它承认局部能力学习已经发生，也限制了从方法有效性推导一般能力、因果依赖和跨环境迁移的范围。

## Writing guardrails for the final Survey

- 不要把四类 Claim 写成四个先验结论；每节必须展示 Supporting、Counterevidence、Partial Mitigation 和 Not Established。
- 不要把 `Task OOD`、`Budget Generalization` 或 `Agent Backbone Transfer` 统一写成 `Generalization`。
- 不要把 step/turn credit 直接翻译成 process capability。
- 不要把 output replacement 直接翻译成 process-level evidence reliance。
- 不要把后端适配写成 semantic intent preservation。
- 不要把 Memory system utility 写成 LLM policy learning，除非参数或 policy target 确实被更新。
- 不要把本章写成“所有论文都泛化不足”；应该写成“不同论文填充了不同 evidence cells，而最关键的 behavior-transfer cells 仍然稀疏”。
