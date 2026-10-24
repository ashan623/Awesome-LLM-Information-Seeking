# How to Learn Information-Seeking Capabilities

## Role of this chapter

本章不是新增第七类 Information-Seeking Capability，也不是 PPO/GRPO 变体综述。它是与 A1–A6 正交的第二条主轴，用来回答：

> **What feedback does the model learn from, how is that feedback converted into parameter updates, which component is updated, and what capability can the resulting evidence actually support?**

现有能力章节回答 **What to Learn**；本章回答 **How to Learn**。二者的交叉才是 Survey 的核心分析单位。

## I. Three orthogonal questions that must not be collapsed

### 1. Learning Signal — 从什么反馈中学习？

记录监督或反馈的语义来源：

- **Outcome Reward：** 最终答案、任务成功、EM/F1、工具调用后效用；
- **Process Reward：** token、step、turn 或 evidence span 的中间质量；
- **Preference Signal：** pairwise preference、DPO/IPO 类偏好、source-balancing preference；
- **Teacher Distribution / Distillation Signal：** teacher logits、teacher action distribution、on-policy teacher–student difference；
- **Counterfactual / Utility Signal：** no-tool/tool、actual/oracle、evidence replacement 或 cost-adjusted utility；
- **State/Value Signal：** state value change、advantage、critic estimate 或 future memory utility。

信号的名字不能直接推出能力。例如，process reward 只说明监督粒度更细，不说明模型已经学会 process-level capability。

### 2. Optimization Method — 如何把反馈转化为参数更新？

记录更新算法，而不是把算法名称当作训练目标：

- Offline SFT / behavior cloning；
- trajectory distillation；
- on-policy distillation / teacher–student policy optimization；
- preference optimization，例如 DPO/IPO；
- policy-gradient RL，例如 REINFORCE、PPO、GRPO、RLOO；
- hybrid optimization，例如 SFT → RL、DPO → joint decoding、process reward + outcome reward；
- test-time or runtime learning，若基础 LLM 参数不更新，则必须与 parameter post-training 分开。

`GRPO` 是优化方法，不代表一定使用 outcome reward；`OPD` 是 teacher–student on-policy learning 机制，不应被自动归为传统 reward-based RL。

### 3. Learning Target — 究竟更新哪个组件？

记录参数更新或学习状态的对象：

- **Search Policy：** 何时调用搜索、继续、改写、切换、验证、停止；
- **Query Rewriter / Planner：** query、sub-question、search plan；
- **Evidence Evaluator / Verifier：** relevance、support、sufficiency、authority、conflict；
- **Retriever / Retrieval Interface：** retriever、reranker、query–retriever coupling；
- **Reader / Reasoning Policy：** evidence integration、belief revision、answer generation；
- **Memory Policy / External Memory State：** ADD、UPDATE、DELETE、NOOP、memory utility；
- **Critic / Value Model：** action value、turn credit、state progress；
- **Joint System：** retriever、policy、verifier、reader 或 memory manager 联合更新。

同一个训练算法更新不同对象时，能力含义完全不同。一个 retriever 优化结果不能直接写成 LLM 学会了 information-seeking capability。

## II. Extended learning-mechanism taxonomy

保留原有 B1–B6 主干，但重新定义每一类的对象，避免把 data、signal、optimizer 和 target 混写。

| Dimension | Core question | Typical values | Main analysis risk |
|---|---|---|---|
| **B1 Data & Trajectory Construction** | 模型看到什么交互和失败案例？ | expert trajectories、self-rollouts、simulator、negative/failed traces、counterfactual branches | 数据分布限制能力；专家轨迹可能包含部署时不可得的信息 |
| **B2 Learning Signal** | 从什么反馈学习？ | outcome、process、preference、teacher distribution、utility/value | 信号语义与声称能力不一致；奖励可被 shortcut |
| **B3 Optimization Method** | 反馈如何更新参数？ | SFT、DPO、OPD、PPO、GRPO、REINFORCE、hybrid | 算法名称掩盖真正监督对象；不同优化器结果不可直接比较 |
| **B4 Learning Target / Update Locus** | 哪个组件被更新？ | policy、query rewriter、verifier、retriever、reader、memory、critic | 系统增益被错误归因给 LLM capability |
| **B5 Credit Assignment** | 成功/失败归因到什么粒度？ | episode、turn、step、token、evidence、state transition | 更细粒度信用不等于更真实的因果信用；hindsight privilege 可能泄漏 |
| **B6 Joint Optimization & Coupling** | 哪些模块共同变化？ | retriever–agent、verifier–policy、reader–retriever、memory–answer agent | 联合优化提高结果但难以隔离单个能力 |
| **B7 Deployment Information Parity** | 训练时看到的信息部署时是否可得？ | state-only、partial feedback、full trajectory、gold answer、oracle evidence | teacher/critic 可能拥有 privileged information，产生 hindsight–deployment gap |

B7 是跨所有训练机制的诊断字段，而不是新的优化方法。

## III. Mechanism × capability matrix

| Capability | Dominant learning targets | Direct signals in current pool | Common optimization routes | Main learning–capability risk |
|---|---|---|---|---|
| **A1 Epistemic Awareness** | search policy、router、value/utility predictor | answerability、uncertainty、counterfactual utility、cost | SFT trigger、RL、cost-aware policy、calibration interface | ex-post utility 改善被解释成 ex-ante value calibration |
| **A2 Information Need Formulation** | query rewriter、planner、retriever–query coupling | pseudo-query、Recall/NDCG、entity coverage、search-key、answer utility | SFT、PPO/GRPO、query preference、process reward | backend dialect/词面命中被解释成 semantic intent |
| **A3 Information Space Exploration** | multi-tool policy、source selector、coverage planner | document coverage、Gold Passage Recall、diversity、information gain | SFT+RL、diversity reward、coverage reward | diversity 或 gold-document matching 被解释成 unfamiliar-space exploration |
| **A4 Evidence Evaluation** | verifier、sufficiency judge、source ranker | support/refute、authority、sufficiency/gap、noise robustness | SFT、LoRA、GRPO、evaluation-only diagnostics | benchmark/verifier 适配被解释成 general evidence judgment |
| **A5 Evidence Integration** | reader/reasoning policy、belief update、memory policy | answer reward、citation/span、evidence preference、supporting facts | SFT、DPO、GRPO/RLVR、joint decoding | evidence-conditioned output 被解释成 process-level causal reliance |
| **A6 Adaptive Search Control** | search policy、critic、value model、budget controller | stopping/depth、budget、turn credit、contribution utility | PPO/GRPO、critic-guided RL、advantage reweighting、OPD | credit assignment 改善被解释成 feedback-conditioned next-action control |

## IV. Signal progression is not a capability ranking

当前文献常呈现如下信号演进：

`Outcome Reward → Process/Step Reward → Preference / Evidence Reward → Teacher Distribution / On-policy Distillation → Counterfactual Utility or State Progress`

这不应写成简单的“越后面越先进”。每种信号只解决不同的监督问题：

- Outcome reward 提供可扩展的端到端目标，但信用粗；
- Process reward 提供更局部的学习信号，但标签可能是 proxy 或 noisy；
- Preference/evidence reward 可表达相对偏好，但可能学习 verifier 偏见；
- Teacher distribution 能提供动作级行为信息，但 teacher 可能拥有 privileged information；
- Counterfactual utility 更接近决策价值，但估计方差、分支质量和部署可获得性必须单独审计。

因此，论文必须同时记录：

`Signal Semantics × Optimization Method × Update Target × Credit Granularity × Deployment Availability`。

## V. Distillation deserves an explicit separation

### Offline trajectory distillation

模型模仿已经收集的 expert/search trajectory。它可以教会查询、工具调用或证据组织的表面行为，但容易受到 expert distribution、轨迹覆盖和 teacher privileged information 的限制。

### On-policy distillation

student 先在当前 policy 分布下产生状态，再从 teacher 得到 action distribution 或 advantage-like signal。它更接近 student 将会遇到的部署状态，但仍需检查 teacher 是否利用 gold answer、未来轨迹或不可部署的证据。

### Teacher information leakage

如果 teacher 在训练时知道最终答案、完整搜索轨迹、oracle evidence 或未来状态，student 可能模仿的是“知道答案后的动作”，而不是在不确定条件下真正探索。

因此，OPD/OPSD 类方法的关键问题不是“是否比 outcome reward 高”，而是：

1. teacher 在每个状态可见什么；
2. student 在部署时可见什么；
3. advantage 或 action target 是否需要 future/gold information；
4. 去掉 privileged information 后，目标 information-seeking behavior 是否仍保持；
5. transfer 到新 retriever/tool/corpus 后，student 是否仍能使用该 policy。

## VI. Credit assignment should be coded by both granularity and information source

| Credit level | What is assigned | Typical evidence | Main limitation |
|---|---|---|---|
| **Episode-level** | entire trajectory or final answer | Search-R1-like outcome RL | cannot identify useful query/evidence/control step |
| **Turn-level** | one search/tool call | CriticSearch, CW-GRPO | may still rely on hindsight trajectory or judge labels |
| **Step-level** | query/action/evidence step | StepSearch, SSPO-like methods | step reward can be proxy-driven or teacher-dependent |
| **Token/span-level** | search key, citation span, reasoning token | retrieval span, key F1, citation reward | lexical matching does not prove semantic or causal reliance |
| **State-transition-level** | value/progress change after an action | value-difference credit, state progress | value estimator may be miscalibrated and action contribution confounded |

每个 credit 方法还要增加一个 `Credit Information Source` 字段：`current state`、`observed feedback`、`future trajectory`、`gold answer`、`oracle evidence` 或 `LLM judge`。

更细的 credit 粒度只有在 credit information source 不引入部署时不可得的 privileged information 时，才更有说服力。

## VII. Minimal learning–capability alignment test

对每一篇核心论文，至少检查以下五个问题：

1. **Signal semantics：** reward/label 真正在测什么？
2. **Target locus：** 哪个组件被更新？是否确实是声称的 LLM capability？
3. **Behavioral observability：** 是否测了目标行为，而不只是最终答案？
4. **Information parity：** 训练 signal 是否使用了部署时不可得的未来/gold/oracle information？
5. **Transfer:** 训练后的行为是否在改变 task、domain、retriever、tool、corpus、budget 或 feedback 后保持？

可用如下非综合性标记：

- `Signal–Capability Alignment: Direct / Partial / Indirect / Not Applicable`
- `Update Target: Policy / Query / Verifier / Retriever / Reader / Memory / Critic / Joint`
- `Credit Granularity: Episode / Turn / Step / Token / State Transition`
- `Deployment Availability: Available / Partially Privileged / Fully Privileged`
- `Learning Evidence: Behavior Measured / Outcome Only / Evaluation Only`

这些标记是诊断字段，不应合并为一个总分。

## VIII. Bounded reinforcement plan

本章只需要一次定向补强，不需要重新打开 A1–A6 的广泛搜集。建议最多补入 4–6 篇机制论文，按机制缺口而不是按任务场景选取：

1. **On-policy distillation / teacher-guided policy learning：** 重点审计 teacher privilege、student state distribution 和 search-step advantage；
2. **Process credit assignment：** 比较 turn-level、step-level、state-value difference 和 outcome reward；
3. **Preference-based learning：** 检查 preference 是否针对信息寻求行为，还是只偏好最终答案/引用格式；
4. **Controlled training comparison：** 控制 corpus、retriever、reward 和 optimization protocol，判断收益来自哪里；
5. **Optional generic OPD background：** 仅作为优化机制背景，不能算作 Information-Seeking capability evidence，除非实验直接测 search/query/evidence/control behavior。

用户提供的 SSPO、TRACE 以及 controlled training comparison 工作可以作为这一轮的 P0 核验候选；在正式核验元数据、训练细节和能力隔离实验之前，不应直接写入核心矩阵或用来支撑结论。

## IX. Recommended Learning chapter

建议将本章放在 capability taxonomy 之后、A1–A6 之前，作为 Survey 的“第二主轴”：

1. Data & Trajectory Construction；
2. Learning Signal；
3. Optimization Method；
4. Learning Target / Update Locus；
5. Credit Assignment；
6. Joint Optimization；
7. Deployment Information Parity；
8. Cross-capability mechanism comparison。

章节主问题可以写成：

> **When should information-seeking policies learn from outcomes, intermediate rewards, preferences, or teacher distributions—and what capabilities does each signal actually induce?**

结论不要写成“process reward 一定优于 outcome reward”，而应写成：

> **More structured supervision can improve credit assignment or behavior observability, but it does not by itself establish that the induced behavior is the claimed capability, causally isolated, or transferable.**

## X. What this reinforcement adds to the survey

补强后，Survey 的二维结构变为：

- **What to Learn：** A1–A6 information-seeking capabilities；
- **How to Learn：** B1–B7 learning mechanisms and evidence conditions。

最终比较单位不是“哪种算法最好”，而是：

> **Which signal, optimized through which method, updates which component, produces which observable behavior, under what information privilege, and with what transfer evidence?**

这会显著增强 Survey 的机制解释力，同时不改变原有能力边界，也不会把论文扩展成通用 RL、Memory 或 RAG Survey。
