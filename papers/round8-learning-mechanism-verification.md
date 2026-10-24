# Round 8 — Learning Mechanism Verification

## Scope

本轮只核验三篇 P0 机制候选，不重新扩展 A1–A6，也不把 How to Learn 扩展成通用 Post-training Survey。统一使用六个字段：

`Learning Signal · Optimizer · Update Locus · Credit Assignment · Deployment Parity · Capability Evidence`

最终分类只使用：`Core Mechanism`、`Supporting`、`Excluded`。

## Executive decision

| Paper | Final decision | Why it matters for the survey | Main limitation |
|---|---|---|---|
| **SSPO** | **Core Mechanism** | 将 teacher–student disagreement 转成 step-level advantage weight，并明确讨论 privileged information leakage；直接改变对 OPD/step-level supervision 的机制分类 | 主要证据仍是 final QA、turn efficiency 和 ablation；尚未独立证明 Query Refinement、Evidence Seeking 或 Adaptive Control 行为发生了可迁移改变 |
| **TRACE** | **Core Mechanism** | 用冻结 reference model 的 gold-answer state value 变化构造 turn-level TD credit，在统一 GRPO/搜索设置下改善长程 agent RL，并提供 credit ablation 与 closed/open-web transfer | credit signal 依赖训练时 Gold Answer；性能和平均搜索行为的改善不能直接等同于 online feedback-conditioned control |
| **Retrieval, Reward, and Training Protocols** | **Core Mechanism — controlled comparison** | 在统一训练框架下控制 corpus、reward、credit、data diversity、off-policy degree 和 budget，显示 corpus coverage 可能比算法差异更决定结果，并对 process reward 的收益提出反例 | 主要是本地 corpus、QA 和 RL protocol comparison；不能直接证明某个单独 capability 被学会 |

这三篇论文共同强化的不是“某一种优化算法最好”，而是：**训练机制的效果必须在 corpus、retriever、rollout budget、reward 和 policy update 对齐后才可解释。**

## 1. SSPO — Beyond Outcome Rewards

**Paper:** [Beyond Outcome Rewards: Step-Level Self-Distilled Policy Optimization for Deep Search Agents](https://arxiv.org/abs/2608.12764) [1]

### Learning Signal

SSPO 使用两种信号：

1. GRPO 的 outcome reward 决定一条 trajectory 的更新方向；
2. teacher–student disagreement 被转换成 step-level advantage weights，调节每一步的更新幅度。

SSPO 只对 incorrect trajectories 使用 self-distillation signal，correct trajectories 保持不变，以避免把正确轨迹压向 teacher 的单一模式。[1]

### Optimizer

核心优化是 **GRPO + Step-Level Self-Distillation**。论文同时比较了 direct token-level distillation、token-level advantage weighting 和 step-level advantage weighting。相同 scaffold 下，SSPO 在 BrowseComp、GAIA 和 FRAMES 上优于对应 GRPO baseline，并报告了更好的 sample efficiency。[1]

### Update Locus

更新的是 Qwen3-8B search policy。teacher 只用于生成监督/权重，不更新 retriever，也不构成 verifier 或 retriever optimization。

### Credit Assignment

SSPO 的关键单位是 **search step**，而不是 token 或整条 trajectory。它将“更新方向”和“更新强度”分离：outcome reward 决定是否强化，teacher disagreement 决定在哪些 search steps 上加强或减弱更新。[1]

### Deployment Parity

论文明确承认 teacher 和 student 存在信息不对称。teacher 可以访问 privileged information，例如 Evidence Anchors 和错误答案；student 部署时只看到问题、当前轨迹和工具反馈。Evidence Anchors 被设计成包含关键证据但不直接暴露完整答案路径，且 teacher–student signal 被作为 advantage weight，而不是直接把 privileged teacher distribution 当作 student target。[1]

这使 SSPO 成为一个很有价值的 **Deployment Information Parity case**，但不能简单写成“已经解决了 privilege leakage”：

- Evidence Anchors 仍由强模型根据 ground-truth QA 构造；
- teacher 明确知道学生的错误答案；
- 主要消融比较的是 token/step、是否使用 anchor、direct distillation 与 advantage weighting；
- 还缺少在完全去除 privileged information 后的等价行为迁移测试。

因此应编码为：`Deployment Parity: Partial / Explicitly Addressed, Not Fully Established`。

### Capability Evidence

SSPO 证明了：更细的 step-level self-distillation signal 能在相同搜索 scaffold 下改善训练效率和 final search-agent performance。它尚未直接证明模型学会了某一项独立的信息寻求能力，例如：

- query refinement 是否变得更好；
- evidence-seeking action 是否在受控反馈下改变；
- 空结果、重复证据和冲突证据是否触发不同控制动作；
- step-level behavior 是否迁移到新的 retriever、tool 或 corpus。

**最终编码：**

- `Learning Signal:` Outcome Reward + Teacher Distribution / Self-Distillation
- `Optimizer:` GRPO + Step-Level Advantage Weighting
- `Update Locus:` LLM Search Policy
- `Credit:` Step-level；teacher disagreement；privileged evidence / incorrect answer
- `Capability Evidence:` Core mechanism evidence；capability isolation partial；task transfer present, behavior transfer not established
- `Role:` Core Mechanism

## 2. TRACE — Turn-level Reward Assignment

**Paper:** [TRACE: Turn-level Reward Assignment via Credit Estimation for Long-Horizon Agents](https://arxiv.org/abs/2607.13988) [2]

### Learning Signal

TRACE 使用冻结 reference model 计算每个 search prefix 对 Gold Answer 的平均 log-probability，再将其转换为 log-ratio state value。相邻工具调用边界之间的 value change 被作为 turn-level TD credit，并与 trajectory-level outcome advantage 共同用于 policy optimization。[2]

这里的 signal 不是纯粹的 outcome reward，也不是一个独立的 learned critic，而是：

`Gold-answer prefix readiness → log-ratio state value → turn-level TD credit`

最终答案的 outcome reward 仍然是任务成功的最终裁决。[2]

### Optimizer

TRACE 将 turn-level advantage 与 GRPO 的 outcome advantage 混合。论文强调不需要额外训练 process-reward model 或 learned critic；reference model 只作为冻结的 value probe。

### Update Locus

更新的是 Search-R1-style 的 LLM search policy。reference model 不参与梯度更新，retriever、browser action space 和训练 corpus 在 controlled baselines 中保持固定。

### Credit Assignment

TRACE 的核心是 **turn-level / state-transition-level credit**：

- episode-level：最终 outcome reward；
- turn-level：工具调用前后的 state-value change；
- state-value：reference model 对 Gold Answer 的 prefix readiness；
- policy update：将 turn credit 与 outcome advantage 合并。

论文的 credit ablation 比较 raw log-probability delta、remaining-gap normalization 和 log-ratio TD credit。log-ratio 版本在该设置下获得最高结果，并且中等 TD look-ahead 优于关闭 dense credit 或过度传播 credit。[2]

### Deployment Parity

TRACE 的 reference model 在训练时计算 Gold Answer 的 prefix probability，而 Gold Answer 在部署时不可获得。因此它是明确的 **privileged training signal**。这不是方法缺陷本身，但必须与 online control 分开：

- reference model 只用于训练期 credit estimation；
- 部署 policy 不会调用 reference model 来获取 Gold Answer probability；
- 论文证明的是这种训练期 signal 有助于 policy optimization；
- 还不能直接证明 policy 在部署时能够从当前 retrieval feedback 中重建同样的 value judgment。

### Capability Evidence

TRACE 的证据强于单纯 final QA comparison 的地方在于：

- 同一 Qwen backbone、browser action space、rollout protocol、training data、terminal reward 和 evaluation interface 下比较 controlled baselines；
- 有 raw delta、remaining gap、log-ratio、turn coefficient、TD horizon 和 reference checkpoint ablations；
- 从 closed-web BrowseComp-Plus 测试到 BrowseComp、GAIA、xbench-DeepSearch 的 open-web evaluation，包含跨语言设置。[2]

但它仍不能直接写成“学会了 online adaptive control”：

- 大部分 transfer evidence 是 final performance，而不是 feedback-conditioned action behavior；
- 平均 tool turns 和训练曲线只能说明搜索过程发生变化，不能证明固定搜索历史下 current feedback 改变会导致正确的 next action；
- Gold-answer state value 可能成为强大的训练期 proxy，但部署时并不存在同等观测。

**最终编码：**

- `Learning Signal:` Outcome Reward + Gold-answer State Value / TD Credit
- `Optimizer:` GRPO + Turn-level Advantage
- `Update Locus:` LLM Search Policy
- `Credit:` Turn / State Transition；frozen reference model；Gold Answer
- `Capability Evidence:` Core credit-assignment evidence；behavior isolation medium；task/environment outcome transfer present, feedback behavior transfer not established
- `Role:` Core Mechanism

## 3. Retrieval, Reward, and Training Protocols

**Paper:** [Retrieval, Reward, and Training Protocols: What Matters in Training Search Agents?](https://arxiv.org/abs/2605.27881) [3]

### Learning Signal

论文在统一 GRPO objective 下比较四种 credit strategies：

- Search-R1：outcome reward；
- GiGPO：cross-trajectory state grouping；
- IGPO：per-turn ground-truth log-probability heuristic；
- Tree-GRPO：branch-level structural comparison。[3]

它还系统改变 retrieval corpus、reward design、data diversity、off-policy degree 和 search budget。

### Optimizer

所有方法被重新实现到相同 GRPO 框架内，使用相同或可控的 Qwen backbone、tool-call interface、training set、turn limit 和评估流程。这样论文重点不是提出新的 optimizer，而是估计在统一 protocol 下不同 reward/credit 与环境因素的相对影响。[3]

### Update Locus

核心更新对象是 LLM Search Policy。Retriever 本身不作为 policy optimization 的共同更新对象；但 retrieval corpus 被显式改变，因此论文能够识别“policy learning gain”与“retrieval environment gain”的混淆。

### Credit Assignment

论文最重要的机制分析包括：

1. Outcome-only reward 是强 baseline，在五个设置中的三个设置取得最高平均 EM；
2. Process-level credit 不保证更好，IGPO 的 log-probability heuristic 在较弱模型上可能成为噪声，并导致 over-search；
3. GiGPO 的 discounted step credit 倾向于减少搜索深度；
4. Tree-GRPO 的 branch comparison 更稳定，但性能上限较低；
5. corpus completeness、tool-call format 和 training protocol 可能比 credit algorithm 的差异更决定结果。[3]

### Deployment Parity

这篇论文的价值不主要在 teacher privilege，而在于控制环境混淆。它显示 Wiki-18 缺失大量 gold supporting documents，会让模型在不可检索的问题上依赖 parametric guessing，并给 search policy 产生误导性 reward。补齐 corpus 后，性能增益可超过不同训练算法之间的差异。[3]

对于 IGPO 等使用 ground-truth log-probability 的方法，仍需记录 Gold Answer dependence；但这篇论文的主要结论是：即便 credit mechanism 被统一比较，corpus coverage 和 training protocol 也可能主导结果。

### Capability Evidence

这篇论文不是某个单一 capability 的 post-training 方法，而是一个 **controlled mechanism comparison**。它对 Survey 的价值在于提供了重要反例：

> 更复杂的 process reward 不一定优于简单的 outcome reward；如果 retrieval corpus 不完整，算法间的比较可能主要反映环境可答性和 parametric guessing，而不是 credit assignment 的真实质量。

论文还测量了平均搜索 turns、训练曲线、data diversity 和 search budget，但这些仍然是行为的间接指标，不能单独证明 query quality、evidence evaluation 或 adaptive control 已被学会。

**最终编码：**

- `Learning Signal:` Outcome / Process / Structural Credit / Ground-truth Value Heuristic
- `Optimizer:` Shared GRPO Objective
- `Update Locus:` LLM Search Policy; retrieval corpus treated as controlled environment
- `Credit:` Episode / Turn / Branch / State comparison
- `Capability Evidence:` Core controlled-comparison evidence; capability learning is indirect; transfer is mainly environment/task outcome comparison
- `Role:` Core Mechanism — controlled comparison

## 4. Preference / DPO gap check

本轮没有新增 search-specific Preference/DPO 论文。现有文献池已经包含 GuarantRAG 的 contrastive DPO，但其 DPO 主要训练 evidence model / retrieved-vs-inner answer preference，并且存在偏向外部证据、缺乏 false-external-evidence test 的问题。它更适合放在 A5 Evidence Integration 的 supporting example，而不是作为 How to Learn 中 search-policy preference learning 的核心锚点。

因此当前判断是：

- Preference/DPO 机制在 Survey 中应保留；
- 现有 evidence 足以说明其 update target 和 alignment risk；
- 暂不新增一篇仅使用 DPO、但没有直接 information-seeking behavior evidence 的论文；
- 如果后续发现一篇直接用 preference 优化 query/tool/control behavior，并提供 behavior isolation，才值得加入机制核心集。

## 5. Round 8 conclusion

三篇 P0 已经足以补强四个学习机制小节：

| Mechanism question | Best anchor | What it establishes | What it does not establish |
|---|---|---|---|
| Teacher distribution / OPD 是否带来不同的 Search Policy supervision？ | SSPO | step-level self-distillation 可以在统一 GRPO scaffold 下改善 search-agent optimization，并显式处理 privilege leakage | 没有独立证明 transferable query/evidence/control capability |
| State-value credit 是否比 outcome-only 更有用？ | TRACE | turn-level state-progress credit 在 controlled setting 下改善 RL training，并有 credit ablations | 没有证明 online feedback-conditioned control 或因果 action value |
| Reward/optimizer 的收益是否被环境因素混淆？ | Retrieval, Reward, and Training Protocols | corpus coverage、format、data protocol 和 budget 可能比 reward algorithm 更决定结果；outcome reward 仍是强 baseline | 不是某个 capability 的直接学习证据 |

最终应保留的中心结论是：

> **Structured supervision and finer credit can improve training efficiency or task performance, but they do not by themselves prove that the intended information-seeking behavior was learned, causally isolated, or transferred. Controlled environment comparisons are therefore part of capability-learning evidence, not merely engineering analysis.**

## References

[1]: https://arxiv.org/abs/2608.12764 "Beyond Outcome Rewards: Step-Level Self-Distilled Policy Optimization for Deep Search Agents"
[2]: https://arxiv.org/abs/2607.13988 "TRACE: Turn-level Reward Assignment via Credit Estimation for Long-Horizon Agents"
[3]: https://arxiv.org/abs/2605.27881 "Retrieval, Reward, and Training Protocols: What Matters in Training Search Agents?"
