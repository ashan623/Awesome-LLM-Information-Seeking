# How to Learn Information-Seeking Capabilities

## Role of this chapter

本章不是新增第七类 Information-Seeking Capability，也不是通用 LLM Post-training 或 RL Survey。它是与 A1–A6 正交的第二条主轴，用来回答：

> **What feedback teaches information-seeking policies, how is that feedback converted into updates, which component is updated, and how is long-horizon behavior supervised?**

A1–A6 回答 **What to Learn**；本章回答 **How to Learn**。至于训练时的特权信息是否能迁移到部署，以及能力能否跨环境保持，属于后面的统一诊断维度，不作为新的学习机制章节。

## I. Four learning-mechanism dimensions

### 3.1 Data and Learning Signals

这一节同时讨论“模型看到什么数据”与“从什么反馈中学习”，但不把每一种 reward 写成独立方法。

#### Data and trajectory regime

- demonstrations / expert trajectories；
- self-rollouts and on-policy trajectories；
- simulator-generated interactions；
- negative or failed traces；
- counterfactual branches，例如 no-tool/tool、evidence replacement、actual/oracle state；
- mixed trajectories with query, tool, evidence and answer states。

数据和轨迹分布决定模型实际会遇到哪些信息缺口、失败模式和搜索路径。专家轨迹也可能包含学生部署时不可得的未来信息，因此应与 Deployment Information Parity 一起审计。

#### Learning signal

- **Outcome Reward：** 最终答案、任务成功、EM/F1、工具调用后的终局效用；
- **Process Reward：** token、step、turn、evidence span 或中间状态的质量；
- **Preference Signal：** pairwise preference、DPO/IPO、source balancing 或证据偏好；
- **Teacher Distribution：** teacher logits、teacher action distribution、on-policy teacher–student difference；
- **Counterfactual / Utility Signal：** no-tool/tool、actual/oracle、evidence replacement 或 cost-adjusted utility；
- **State / Value Signal：** state value change、advantage、critic estimate 或 future memory utility。

核心问题不是哪种 reward 名字更复杂，而是：

1. 信号真正测量了哪一种行为；
2. 哪些 shortcut 也能得到相同反馈；
3. 信号是否与声称的 capability 对齐；
4. 信号是否依赖部署时不可得的 gold、future 或 oracle information。

### 3.2 Policy Learning and Optimization

这一节只解释优化方法如何利用信号，不做算法推导或 PPO/GRPO 变体的百科式罗列。

- **Offline SFT / Behavior Cloning：** 从 demonstrations 或 expert trajectories 学习 query、tool、evidence 和 control behavior；
- **Offline Trajectory Distillation：** 模仿已有搜索轨迹，需审计 teacher coverage 和 privileged information；
- **Preference Optimization：** DPO/IPO 等从相对偏好中学习答案、证据或行动选择；
- **RL Policy Optimization：** REINFORCE、PPO、GRPO、RLOO 等将 outcome/process/utility 信号转为 policy update；
- **On-policy Distillation：** student 在自身状态分布上获得 teacher distribution 或 advantage-like supervision；它是独立机制，但不自动等于 reward-based RL；
- **Hybrid Methods：** SFT → RL、DPO → joint decoding、process reward + outcome reward 等组合。

必须保持以下区分：

- GRPO 是 optimization method，不代表一定使用 outcome reward；
- OPD 是 teacher–student policy learning mechanism，不代表一定使用 reward；
- process supervision 是 signal granularity，不代表模型获得了 process-level capability；
- 更复杂的 optimization protocol 不等于更强的 capability transfer。

### 3.3 Learning Targets and Optimization Loci

这一节判断一篇论文究竟是不是直接的 Information-Seeking Capability Learning evidence。

- **LLM Search / Reasoning Policy：** 学习何时搜索、如何继续、如何使用证据和何时停止；
- **Query Rewriter / Planner：** 学习 query、sub-question 或 search plan；
- **Verifier / Evidence Judge：** 学习 relevance、support、sufficiency、authority 或 conflict；
- **Retriever / Retrieval Interface：** 更新 retriever、reranker 或 query–retriever coupling；
- **Reader / Integration Policy：** 学习 evidence integration、belief revision、answer generation；
- **Memory Policy / External State：** 学习 ADD、UPDATE、DELETE、NOOP 或 future memory utility；
- **Critic / Value Model：** 学习 action value、turn credit 或 state progress；
- **Joint System：** retriever、policy、verifier、reader 或 memory manager 联合更新。

同一个 PPO、GRPO 或 DPO，当它更新不同组件时，能力含义完全不同。Retriever 或 verifier 的改进不能自动归因于 LLM policy；联合优化带来的下游收益也不能自动证明某一个 capability 被隔离地学会。

### 3.4 Long-horizon Credit Assignment

这一节讨论最终反馈如何分配给搜索过程，而不是简单地把所有 process reward 归为一类。

| Credit level | What is assigned | Typical interpretation | Main limitation |
|---|---|---|---|
| **Episode-level** | entire trajectory or final answer | outcome-based search policy learning | 无法定位 useful query、evidence 或 control step |
| **Turn-level** | one search/tool call | turn contribution or retrospective critic | 可能依赖完整未来轨迹、Gold Answer 或 LLM judge |
| **Step-level** | query/action/evidence step | process reward、search-key 或 evidence progress | step reward 可能只是 proxy，或依赖 teacher privilege |
| **Token / Span-level** | search key、citation span、reasoning token | lexical/evidence alignment | matching 不证明 semantic intent 或 causal evidence reliance |
| **State-transition-level** | value/progress change after an action | action contribution from state improvement | value model 可能误校准，且 action effect 与 retriever/generator 混杂 |

Credit assignment 还要记录来源：`current state`、`observed feedback`、`future trajectory`、`gold answer`、`oracle evidence` 或 `LLM judge`。

本节的核心判断是：

> **Better credit assignment does not necessarily imply better online control.**

更细的 credit 只有在它能够减少错误归因、且没有把部署时不可得的 hindsight information 直接泄漏给 policy 时，才真正增强 capability-learning 证据。

## II. Compact mechanism × capability matrix

| Capability | Signal examples | Optimization examples | Update locus | Credit focus | Key gap |
|---|---|---|---|---|---|
| **A1 Epistemic Awareness** | answerability、uncertainty、counterfactual utility、cost | SFT trigger、SFT/RL、cost-aware policy | router / search policy / utility predictor | outcome / action | ex-ante value calibration |
| **A2 Information Need Formulation** | pseudo-query、retrieval gain、coverage、search-key | SFT、PPO/GRPO、preference、process reward | query policy / planner / retriever coupling | query / step | semantic intent transfer |
| **A3 Information Space Exploration** | diversity、document coverage、information gain | SFT+RL、coverage/diversity optimization | exploration policy / source selector | search action | useful coverage in unfamiliar spaces |
| **A4 Evidence Evaluation** | support/refute、authority、sufficiency、noise robustness | SFT、LoRA、GRPO、evaluation-only diagnostics | verifier / judge / source ranker | span / claim / set | generalizable evidence judgment |
| **A5 Evidence Integration** | answer/evidence reward、citation、supporting facts | SFT、DPO、GRPO/RLVR、joint decoding | reader / reasoning policy / memory policy | evidence / outcome | causal evidence reliance |
| **A6 Adaptive Search Control** | budget、stopping、turn contribution、state progress | PPO/GRPO、critic-guided RL、OPD、advantage reweighting | search policy / critic / budget controller | turn / step / episode | online feedback adaptation |

这张矩阵只保留每类能力最有区分度的机制变量，不重复列出全部论文。具体论文仍放在完整 Cross-Paper Evidence Matrix 中。

## III. Deployment Information Parity is a cross-cutting diagnostic

Deployment Information Parity 不是一种 Learning Method，而是 Training–Capability Alignment 的诊断维度。训练时使用 Gold Answer、完整轨迹或 oracle evidence 本身不构成方法缺陷；真正需要检查的是：

1. teacher、critic 或 reward 是否依赖部署时不可获得的信息；
2. 这些信息是否直接泄漏进 student 的 action input 或 output target；
3. 去除特权信息后，student 是否仍能依据当前 observation 做出正确决策；
4. 是否有行为干预证明 student 学到的是 online policy，而不是事后 teacher pattern；
5. 在新 retriever、tool、corpus 或 feedback format 中，目标行为是否保持。

因此，这一维度应贯穿 A1、A5、A6，尤其适用于：

- A1：ex-post utility reward 与 ex-ante value prediction；
- A5：oracle evidence 与真正的 evidence integration；
- A6：retrospective critic 与 online feedback-conditioned control；
- OPD：teacher distribution 是否编码了 student 部署时不可得的答案或未来状态。

## IV. Bounded mechanism reinforcement

How to Learn 只需要最后一次定向核验，不需要重新打开 A1–A6 的广泛搜集。最多补入 4–6 篇能够改变机制分类或修正现有判断的论文：

1. **On-policy distillation / teacher-guided policy learning：** 是否提供不同于 SFT/RL 的 Search Policy 学习信号；
2. **Process credit assignment：** turn、step、state-value credit 是否真正优于 outcome-only reward；
3. **Preference-based learning：** preference 是否针对信息寻求行为，而不是只偏好最终答案或引用格式；
4. **Controlled training comparison：** 是否能隔离 signal、optimizer、retriever、corpus 和 rollout budget 的收益来源。

用户提供的 SSPO、TRACE 和 training-protocol comparison 工作可以作为候选，但在正式核验元数据、训练细节和能力隔离实验之前，不应写入核心结论。Generic OPD background 也不能直接算作 Information-Seeking Capability evidence，除非其评价真正测量 search/query/evidence/control behavior。

筛选标准只有一个：

> **这篇论文是否改变我们对“什么反馈、通过什么更新机制、能教会什么能力”的判断？**

不为了凑数量而继续检索。

## V. Recommended chapter structure

建议 How to Learn 只保留四个小节：

1. **Data and Learning Signals**；
2. **Policy Learning and Optimization**；
3. **Learning Targets and Optimization Loci**；
4. **Long-horizon Credit Assignment**。

然后把 `Deployment Information Parity` 和 `Generalization` 放到 Cross-Capability Analysis，与 Training–Capability Alignment、Capability Isolation 一起讨论。

章节主问题可以写成：

> **When should information-seeking policies learn from outcomes, intermediate rewards, preferences, or teacher distributions—and what capabilities does each signal actually induce?**

推荐结论：

> **More structured supervision may improve credit assignment or behavior observability, but it does not by itself establish that the induced behavior is the claimed capability, causally isolated, or transferable.**

## VI. What this adds to the survey

补强后，Survey 的结构保持收敛：

- **What to Learn：** A1–A6 information-seeking capabilities；
- **How to Learn：** 四个 learning-mechanism 小节；
- **What Was Actually Learned？** Training–Capability Alignment、Capability Isolation、Deployment Information Parity 和 Generalization。

最终比较单位是：

> **Which signal, optimized through which method, updates which component, produces which observable behavior, and what evidence shows that the behavior is the intended and transferable capability?**

这会增强 Survey 的中心论点，而不会把它扩展成通用 post-training、Memory、RAG 或 RL Survey。
