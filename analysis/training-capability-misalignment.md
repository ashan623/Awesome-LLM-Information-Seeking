# Training–Capability Misalignment Taxonomy

## Central claim

The most reusable cross-paper finding is not simply that “generalization is limited.” It is that the **training signal, the behavior changed by post-training, and the capability claimed by the paper are often different objects**.

这四类错位是互相区分、但可以同时发生的诊断类别：

1. **Objective–Capability Misalignment**：训练目标本身不是声称的能力的直接测量；
2. **Outcome–Behavior Confounding**：最终表现改善，却无法确定具体是哪一种行为改善；
3. **Hindsight–Deployment Gap**：训练信号依赖部署时不可得的事后信息；
4. **Environment-conditioned Learning vs. Transferable Capability**：策略适配了特定环境，却被解释为学会了可迁移规律。

它们不应合并成一个笼统的“reward misalignment”。每一类需要不同的实验来验证。

## 1. Objective–Capability Misalignment

### Definition

训练信号优化的是一个可计算的代理目标，而论文声称模型学会的是更高层能力。代理目标可以有效，但必须明确它到底对应能力的哪一部分。

### Cross-capability evidence

| Capability | Commonly optimized signal | Claimed capability | What the signal may actually teach |
|---|---|---|---|
| A1 | answerability、uncertainty、ex-post utility、call cost | ex-ante epistemic awareness / value calibration | known/unknown routing、轨迹后效用优化、成本条件下的调用习惯 |
| A2 | NDCG、Recall、entity hit、search-key F1、answer reward | semantic information-need formulation | backend dialect、词法匹配、固定语料上的证据命中 |
| A3 | query/document diversity、gold-document F1、tool-call efficiency | useful exploration in unfamiliar information spaces | 多样化查询、固定工具族协调、gold target coverage |
| A4 | relevance label、verifier reward、citation fidelity、sufficiency proxy | independent evidence quality judgment | claim/span matching、格式化引用、固定来源下的充分性分类 |
| A5 | answer EM/F1、citation span、evidence preference、supporting-fact SFT | causal evidence reliance and belief update | evidence-conditioned output、来源偏好、多文档答案模式 |
| A6 | stopping/depth、budget efficiency、retrospective critic、contribution weight | online feedback-conditioned search control | 搜索深度模板、预算分配、后验 credit reweighting |

### Representative examples

- **A2：** 优化 NDCG 可以证明 query 对某个 retriever 有用，但不能独立证明 query 保留了任务的完整 semantic intent。Environment-aware IR 的负迁移正好说明 backend adaptation 与 transferable query learning 需要拆分。
- **A4：** 引用数量、span overlap 或 verifier reward 可以增加 citation fidelity，却不一定证明模型判断了证据真实性、来源权威或冲突。
- **A5：** Answer Accuracy 变高可以来自参数记忆、reader 能力、检索质量或上下文跟随，而不是模型真正建立了必要的 evidence dependence。
- **A6：** 更好的 turn-level credit 可以让训练更稳定，却不能自动证明模型部署时知道当前反馈意味着 Retry、Switch、Verify 或 Stop。

### Required diagnostic

每篇论文应回答：

1. 训练信号直接测量目标能力的哪个可观察部分？
2. 哪些替代行为也能获得相同奖励？
3. 是否有独立的行为标签或受控干预来排除这些替代行为？

### Stronger experimental remedy

把能力拆成可干预的中间目标：

- A1：在行动前预测 retrieval/no-retrieval 的 expected net utility，并报告 calibration、regret 和 selective risk；
- A2：分别测 semantic intent、backend utility、evidence coverage 和 transfer；
- A3：将 query diversity、evidence diversity、information gain、useful coverage 分开；
- A4：分离 relevance、support、authority、sufficiency、conflict；
- A5：分开 output、compositional、process-level reliance；
- A6：分开 control action、feedback dependence、credit assignment 和 transfer。

## 2. Outcome–Behavior Confounding

### Definition

最终答案或系统效用改善，但无法确定目标信息寻求行为是否改善。多个模块同时变化时，结果差异无法局部归因。

### Typical confounders

1. **Retriever confound：** query 不变但 retriever 更好，或 query 改变带来的收益只是后端差异；
2. **Generator confound：** evidence 没有被真实使用，答案来自参数知识或 reader shortcut；
3. **Search-count confound：** A3 的更多覆盖可能只是调用次数增加；A6 的效率改善可能只是提前停止；
4. **Evidence-quality confound：** A4 的判断结果与证据质量、答案真值、来源流行度混在一起；
5. **Reasoning confound：** A5 的 multi-hop accuracy 可能来自更强的 reader，而不是必要证据组合；
6. **Judge confound：** LLM judge 或 verifier 的偏好被策略学会，而不是目标能力本身。

### Cross-capability examples

- **A1：** final accuracy 提高可能来自更强 generator 或更高召回，而不是 retrieval trigger 更准。
- **A3：** 获得更多相关文档可能是因为 query 数量增加，而不是发现了新的必要 information region。
- **A5：** 答案随 context 改变只能说明 output sensitivity，不足以证明内部 reasoning process 因果依赖该证据。
- **A6：** 平均 search depth 降低可能是 fixed depth shortcut，也可能是 evidence-sensitive stopping；需要固定历史和反馈干预才能区分。

### Required diagnostic

评价必须同时包含：

- target behavior metric；
- downstream task metric；
- module-fixed ablations；
- counterfactual intervention；
- failure cases and shortcut analysis。

### Stronger experimental remedy

- **A1：** paired no-tool/tool potential outcomes，控制 generator、retriever、evidence quality 和 sampling variance；
- **A2：** query-only intervention，固定 task、answer generator、Retriever、Corpus、Top-k、budget；
- **A3：** 删除必要 source 或覆盖区域，再观察替代来源和 useful coverage；
- **A4：** 固定 claim，独立改变 source authority、truthfulness、conflict ratio、order 和 freshness；
- **A5：** 删除/替换必要 evidence，观察答案、链和过程分别如何变化；
- **A6：** 固定历史，仅替换当前 feedback，直接测 next-action distribution。

## 3. Hindsight–Deployment Gap

### Definition

训练阶段可以使用 gold answer、完整未来轨迹、最终效用或 oracle state 来生成监督；部署阶段却要求模型在行动前、只凭当前可见信息作出决策。

### Most visible instances

#### A1 — Ex-post reward versus ex-ante value

s3、InfoReasoner、CoBRA、IKEA 等可以使用搜索后的答案增益、分支差异或终局奖励训练策略。它们能教模型哪些轨迹事后带来更好的结果，但不等于模型在 retrieval 之前已经校准地预测本次搜索的净收益。

真正的目标是：

\[
V(x)=E[U(y)\mid x, retrieve]-E[U(y)\mid x, no\ retrieve]-\lambda C
\]

并要求这个预测在模型、retriever、tool、corpus、domain 和 cost 改变后仍然校准。

#### A6 — Hindsight credit versus online control

CriticSearch 的 retrospective critic 读取完整轨迹和 gold answer，再给每一轮标记 Good/Bad。它可以改善训练中的 turn-level credit，但部署时策略不可能提前看到完整未来轨迹。去掉 gold answer 会降低 critic 质量，却没有消除 hindsight perspective。

AutoSearch 的最优深度由最早达到 gold exact match 的轮次定义，也属于事后目标。AnySearch 的 gap 较小，因为它在第二阶段移除显式 budget scaffold；不过它主要证明 budget internalization，而不是对检索内容反馈的全面恢复能力。

#### A5/A5.4 — Oracle evidence and state

LongMemEval 的 actual retrieval / oracle evidence 条件、A5 中的 gold supporting documents 与 evidence spans，可以帮助测量能力，但不能把 oracle condition 的收益直接解释成模型部署时能够自己识别必要证据或正确写入 memory state。

### Required diagnostic

每篇论文应记录：

- 训练时可见的信息；
- 部署时可见的信息；
- reward/label 是否依赖未来 trajectory、gold answer 或 oracle evidence；
- 是否有 no-privilege ablation；
- 去掉 privilege 后，目标行为是否仍然保持，而不是只保持最终 accuracy。

### Stronger experimental remedy

加入 train-time / deployment-time information parity：

1. 用当前 state-only signal 训练或评估；
2. 与 full-trajectory/gold oracle signal 分开报告；
3. 在相同历史下改变当前反馈；
4. 直接记录下一步 action，而不只看最终答案；
5. 报告 regret、calibration、false stop、unnecessary continue、failed recovery。

## 4. Environment-conditioned Learning vs. Transferable Capability

### Definition

策略在一个固定 search stack、retriever、corpus、tool inventory 或 reward model 中有效，并不代表模型获得了脱离环境的 information-seeking rule。

### Environment dependence by capability

| Capability | Environment-conditioned behavior | Missing transfer claim |
|---|---|---|
| A1 | utility and trigger depend on model, evidence, task, retriever and cost | cross-model/retriever/tool/corpus ex-ante calibration |
| A2 | query policy learns retriever-specific dialect | preserve semantic intent and adapt expression in a new backend |
| A3 | diversity/coverage reward works within fixed corpus/tool family | discover useful regions under unseen source inventory or knowledge structure |
| A4 | source/authority/sufficiency labels reflect fixed source ecology | robust judgment under source replacement, time shift, conflict and new tools |
| A5 | evidence following depends on fixed reader, corpus and context format | necessary evidence reliance under new source/order/domain/tool |
| A6 | depth/credit policy depends on fixed retriever and feedback format | same feedback-conditioned control rule under new tool/corpus/environment |

### Critical distinctions

- New benchmark ≠ behavior transfer;
- New backbone ≠ Retriever OOD;
- Multiple retrievers used in separate training runs ≠ train-on-A/test-on-B transfer;
- Unseen budget ≠ unseen tool or environment;
- Multi-domain evaluation ≠ domain-level calibration if target behavior is not separately measured.

### Stronger experimental remedy

For each capability, report transfer as a tuple rather than a binary label:

`(Task, Domain, Retriever, Tool, Corpus/Environment, Budget, Temporal Shift, Behavior Transfer)`.

An empty cell must be marked `Not Tested`; a degraded result must be marked `Tested but Failed`; a successful final score without behavior measurement must be marked `Task Transfer Only`.

## V. Cross-capability synthesis

The four failure modes interact as follows:

1. Objective–Capability Misalignment explains why a signal can optimize the wrong object.
2. Outcome–Behavior Confounding explains why endpoint improvement cannot identify the changed capability.
3. Hindsight–Deployment Gap explains why a training signal may be unavailable when the model must act.
4. Environment-conditioned Learning explains why behavior learned under one search ecology may fail elsewhere.

A paper can pass one diagnostic and fail another. For example, AnySearch has relatively strong Budget Generalization but limited Retriever/Tool Transfer. QREF has a relatively direct coverage signal but no independent Semantic Intent test. RLSeek has a direct narrow support-verification behavior but limited source/tool generalization. ClashEval has strong output-level evidence replacement but no process-level causal proof.

The survey should therefore avoid the sentence “the model learns information seeking.” It should say:

> **The model learns a particular behavior under a particular signal and environment; the experiments establish a particular scope of evidence, while leaving other capability and transfer cells untested.**

## VI. Open problems that are specific enough to test

1. **Ex-ante value calibration:** Can a policy predict per-instance retrieval net benefit before calling the tool?
2. **Semantic intent under backend adaptation:** Can one policy preserve user/task intent while adapting its expression to a new retriever?
3. **Useful coverage under unfamiliar space:** Can the agent find necessary evidence after source inventory or knowledge structure changes?
4. **Verifier-independent evidence judgment:** Can A4 behavior survive a held-out verifier, new source authority pattern, and conflict ratio?
5. **Necessary evidence reliance:** Does deleting one necessary evidence unit alter the corresponding conclusion and reasoning step?
6. **Online feedback-conditioned control:** Does changing current feedback change the next action in the correct direction?
7. **Behavior transfer protocol:** Can all of the above survive task, domain, retriever, tool, corpus, budget and temporal shifts without re-training the whole policy?
