# Generalization & Intervention Audit

## Audit principle

这份审计把两个经常被混在一起的问题分开：

- **Generalization audit：** 改变了什么环境条件，行为是否仍然成立？
- **Intervention audit：** 改变了什么外部信息，模型是否产生了符合能力定义的行为变化？

最终答案变化可以是 intervention 的一部分，但不能自动证明内部机制因果依赖；新 benchmark 成绩也不能自动证明 behavior transfer。

## I. Unified intervention protocol

| Capability | Fixed conditions | Intervention | Expected behavior | Strongest current approximation | Status of strict protocol |
|---|---|---|---|---|---|
| **A1 Epistemic Awareness** | task、generator、retriever、evidence quality、budget | 改变 expected retrieval benefit、evidence helpfulness 或 cost | 检索概率、verify/answer 决策随 expected net value 合理变化 | CoBRA paired no-tool/tool；Calibrate-Then-Act cost-conditioned action | **Partial**：缺少稳定的 per-instance ex-ante VOI calibration 和 multi-environment test |
| **A2 Information Need Formulation** | task、answer generator、Retriever、Corpus、Top-k、budget | 只替换、删除或语义扰动 query | 保持 semantic intent，同时改变/改善 necessary evidence coverage | QREF original/refined coverage；Environment-aware IR backend comparison | **Not Tested**：没有完整 query-only causal protocol |
| **A3 Information Space Exploration** | task、query generator、budget、answer reader | 删除必要 source，改变 unresolved region 或 source inventory | 选择替代来源、访问新区域、恢复 useful coverage，不只是增加调用 | MARAG gold-document coverage；DDPO diversity + Gold Passage Recall | **Not Tested**：缺 source-removal、coverage counterfactual 和 unseen-space navigation |
| **A4 Evidence Evaluation** | claim、task、reader、evidence amount、answer target | 改变 source authority、truthfulness、conflict ratio、order、freshness、noise | 依据内容与来源质量判断 support、authority、sufficiency、conflict | RAGChecker noise sensitivity；AuthorityBench authority perturbation；Sufficient Context pair labels | **Partial**：局部轴向干预，缺 factorial source/time/conflict protocol |
| **A5 Evidence Integration** | task、generator、retrieved set、reasoning budget | 删除、替换或重排 necessary/irrelevant evidence | 关键结论合理改变；无关证据变化保持不变；过程使用证据 | ClashEval/source-balancing replacement；TRACE component ablation | **Partial**：output sensitivity 有证据，strict necessary-evidence leave-one-out 和 process causality 缺失 |
| **A6 Adaptive Search Control** | task、completed search history、budget、tool/retriever | 只替换 current search feedback | sufficient→Stop/Answer；empty→Retry/Reformulate；repeated→new source；conflict→Verify | AnySearch budget intervention；AutoSearch depth；CriticSearch/CW-GRPO hindsight credit | **Not Tested**：固定历史、只改变 feedback、直接测 next action 尚不存在 |

## II. Generalization dimension audit

### C1 — In-distribution

**Status：最充分。** 大多数论文在训练和测试相似的任务、提示、retriever、corpus、tool API 和 reward format 中验证。C1 可以支持方法有效性，但不能支持行为迁移。

### C2 — Task OOD

**Status：中等。** 多数 A1–A6 论文报告多个 QA 或多跳 benchmark。这个维度对证明“不是只记住一个数据集”有价值，但只要后端、反馈格式和 target behavior 没有变化，就不能上升为 transferable capability。

建议编码为：

- `Task Transfer — Final Outcome Only`：新任务只报告答案；
- `Task Transfer — Behavior Measured`：新任务同时测目标行为；
- `Task Tested but Failed`：目标行为在新任务中明显退化。

### C3 — Domain OOD

**Status：有限到中等。** A1 有 medical/long-tail/decision-like 设置，A2 有医学、开放域和多后端，A3 的 InfoMosaic 覆盖多个工具领域，A5 的 RAFT 提供 domain-specific RAG。A4/A5/A6 的 domain transfer 多数仍以最终任务结果为主。

必须区分“数据来自多个领域”和“训练后的目标行为在未见领域中保持”。后者需要在新领域中重新测 trigger calibration、query intent、source judgment、evidence reliance 或 control response。

### C4 — Retriever / Tool OOD

**Status：总体薄弱。**

- A1：多数 utility/trigger 结果仍与特定 generator、retriever、evidence 和 cost 绑定；
- A2：Environment-aware IR 说明 backend dialect 真实存在，但主要证明 source→target 不稳定；QREF 的多 backbone 不是 retriever transfer；
- A3：工具仓库迁移论文是 boundary evidence，目标是 API exact match 而不是 task-relevant information acquisition；
- A4/A5：验证器、source ecology、reader 和 retriever 通常固定；
- A6：CriticSearch 固定 E5 + 2018 Wikipedia，AnySearch 未测试 retriever/tool shift。

### C5 — Corpus / Environment OOD

**Status：总体薄弱。** 仍缺少系统性的 temporal corpus、source replacement、noise/conflict shift、dynamic web、simulator-to-real、changing tool inventory 和 memory-backend shift。

新 corpus 如果只保持相同 QA 结构、检索器和 evidence format，应谨慎标记为 `Corpus Variation`，不要自动标记为 `Environment Transfer`。

### C6 — Behavior Transfer

**Status：六类能力中最关键、也最缺失。** 需要在新环境中重新测目标行为本身：

- A1：calibration、selective risk、decision regret；
- A2：semantic intent、backend adaptation、query-only coverage；
- A3：source choice、non-redundant exploration、useful coverage；
- A4：support/authority/sufficiency/conflict judgment；
- A5：output/compositional/process-level evidence reliance；
- A6：feedback-conditioned next action。

仅有 final QA、EM/F1、citation count、tool productivity 或平均搜索深度，不能填充 C6。

### C7 — Budget / Cost Generalization

**Status：A6 的 AnySearch 证据最强，其他能力局部存在。**

- A1：CoBRA/IKEA/Calibrate-Then-Act 改变显式 cost 或调用惩罚；
- A2：query policy 的 token/API/latency cost transfer 基本未测；
- A3：调用次数和搜索步数是邻近指标，不等于 useful coverage under budget；
- A4：证据评价的 budget transfer 基本未测；
- A5：BRIEF 的 compression/context budget 是局部 evidence integration 变化，不等于广泛成本泛化；
- A6：AnySearch 训练预算≤5，测试 6–8，是本池最明确的 unseen-budget evidence。

调用次数不等于真实成本。理想审计应同时记录 token、latency、API price、memory cost、privacy/risk cost 和 answer utility。

### C8 — Temporal / State Shift

该维度可作为 C5 的专项标记，尤其重要于 A1、A4、A5.4：

- 信息是否过期；
- 来源 authority 是否随时间变化；
- 用户 preference 是否更新；
- memory 是否 stale/conflicting；
- 当前反馈是否与训练时格式不同。

现有论文的 temporal/state shift 证据局部存在，但没有跨能力的统一行为审计。

## III. Tested but Failed versus Not Tested

这是最终 Survey 必须严格区分的记录。

| Label | Definition | Example |
|---|---|---|
| **Not Tested** | 论文没有设计该维度的实验，不能从结果推断 | A6 论文没有 fixed-history current-feedback intervention |
| **Tested but Failed** | 论文明确改变了该条件，目标行为或性能未保持 | Environment-aware IR 中 source retriever query policy 在 target retriever 上不稳定 |
| **Tested — Final Outcome Only** | 新条件下只报告最终任务结果 | A2 在新 benchmark 上 QA 变好，但没有测 query intent |
| **Tested — Behavior Measured** | 新条件下重新测量目标行为 | AnySearch 在 unseen budgets 上测 tool productivity/early stopping；仍只覆盖 C7 |
| **Partial / Mixed** | 只改变一个相关因素，或干预与目标能力有混淆 | A5 的 evidence replacement 改变输出，但没有 process-level attribution |

推荐不要使用单一的 `Generalization: Yes/No` 字段，而使用：

`Task = ...; Domain = ...; Retriever = ...; Tool = ...; Corpus = ...; Budget = ...; Temporal = ...; Behavior = ...`

每一项用上述五种状态之一。

## IV. Cross-capability audit table

| Capability | Strongest positive transfer | Strongest negative/limitation evidence | Empty cells that matter most |
|---|---|---|---|
| A1 | 部分 domain/task/cost-aware routing | same evidence helps/hurts different model；ex-post utility ≠ ex-ante calibration | Retriever/Tool/Corpus/Behavior calibration |
| A2 | 多任务、多后端 utility；QREF backbone transfer | Environment-aware IR 的 retriever-specific dialect 和负迁移 | Semantic intent under new backend；strict query-only intervention |
| A3 | MARAG task transfer；DDPO multi-benchmark coverage | diversity ≠ useful coverage；固定 tool/corpus | source inventory/tool API/corpus structure/behavior transfer |
| A4 | 多 benchmark measurement；局部 noise/authority perturbation | measurement > learning；authority popularity shortcut；conflict resolution weak | held-out verifier、source replacement、time/conflict shift |
| A5 | 多任务、部分 domain/context budget variation | output reliance > compositional > process; necessary evidence deletion absent | causal evidence reliance、source/order/time/tool transfer |
| A6 | AnySearch unseen budget 6–8 | credit assignment ≠ feedback-conditioned control | current-feedback intervention、Retriever/Tool/Environment transfer |

## V. Minimal experiment suite for the survey’s proposed protocol

为了让 Survey 不停留在批评层面，可以给出一个跨能力的最小实验套件：

1. **Paired potential outcomes：** no-information / actual-information / oracle-information；用于 A1、A5、A5.4；
2. **Query-only intervention：** 固定所有后端条件，只替换 query；用于 A2；
3. **Coverage/source intervention：** 删除必要信息源或隐藏未解决区域；用于 A3；
4. **Evidence-quality factorial test：** authority × truthfulness × conflict × order × freshness；用于 A4；
5. **Necessary-evidence leave-one-out：** 删除/替换关键证据，分别观察 output、composition、process；用于 A5；
6. **Fixed-history feedback swap：** 只替换当前 search feedback，测 next action；用于 A6；
7. **Transfer grid：** train on environment A, test on task/domain/retriever/tool/corpus/budget/temporal environment B，且在 B 中重新测行为而非只测答案。

这套 protocol 的价值不在于要求每篇论文都完成所有实验，而在于给出明确的证据边界：缺哪一项，就只能声称到对应范围。
