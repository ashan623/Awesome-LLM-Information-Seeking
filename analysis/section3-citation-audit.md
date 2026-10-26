# Section 3 Citation Completeness Audit

> 审计对象：用户提供的 `What to Learn: Information-Seeking Capabilities` Section 3 草稿。
>
> 对照来源：当前 `paper-catalog-by-category.md`、`final-cross-paper-evidence-matrix.md`、A1–A6 论文池，以及 Round 10 新增的 A4/A5 定向补充论文。
>
> 审计原则：Section 3 不需要穷举整个论文库，但每个能力至少应同时覆盖代表性 Learning、Evaluation/Measurement 和 Counterevidence/Boundary 论文。未进入正文的论文可以保留在 Section 4、Section 5、表格或附录中。

## 1. Overall verdict

当前 Section 3 的六类能力定义已经完整，A2 的核心引用也基本完整；主要不足不在 taxonomy，而在 evidence-role coverage：

- A1 缺少对 retrieval-trigger、ex-post utility 和 ex-ante value prediction 的反例与训练对照；
- A3 缺少真实搜索轨迹的 exploration measurement；
- A4 缺少 EAPO 和 typed-conflict 诊断；
- A5 缺少 belief revision、prior-guided training、TRACE 的多证据链，以及 factuality/faithfulness/attribution 三个诊断锚点；
- A6 缺少 turn-level hindsight credit 和 contribution-aware credit 的代表性工作。

Section 3 当前引用覆盖可以概括为：

| Section | 当前正文已覆盖 | 当前论文池中的相关条目 | 结论 |
|---|---:|---:|---|
| A1 | 9 | 15 | 能力主线成立，但 evidence coverage 偏向方法，缺少反例和 ex-ante 对照 |
| A2 | 7 | 7 个核心条目 | 核心覆盖完整；LESER/WebDancer 是边界条目，不必放正文主体 |
| A3 | 4 | 7 | 至少应补 Agentic Search in the Wild；另外两篇属于 tool-selection boundary |
| A4 | 10 | 12 | 应补 EAPO 和 (D)RAGged Into a Conflict |
| A5.1–A5.3 | 8 | 13 | 应补 Belief-R、Efficient Prior-Guided Reasoning、TRACE、FRANQ、Attribution Bias |
| A5.4 | 3 | 6 | 应补 MemoryAgentBench；A-MEM/Mem0 可作为系统对照 |
| A6 | 7 | 9 个正文/跨能力锚点中的主要条目 | 应补 CriticSearch 和 CW-GRPO |

这里的“完整”不是指每篇都必须在正文出现，而是指每个 subsection 的正面方法、测量证据和反证是否齐全。

## 2. Papers that should be added to Section 3正文

### A1 — Epistemic Awareness and Retrieval Utility

当前正文已经引用 `When to Retrieve`、`KBM`、`DRAGIN`、`SeaKR`、`LLM-Specific Utility`、`When Knowledge Is Not Free`、`CoBRA`、`InfoReasoner` 和 `IKEA`。建议补充以下三篇核心论文：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [Adaptive Retrieval Without Self-Knowledge?](https://aclanthology.org/2025.acl-long.319/) | 第10行第一段末尾，接在 `DRAGIN`/`SeaKR` 后 | 直接提醒：uncertainty/self-knowledge proxy 不等于 retrieval utility；是 A1 最重要的 counterevidence |
| [s3: You Don't Need That Much Data to Train a Search Agent via RL](https://aclanthology.org/2025.emnlp-main.1095/) | 第12行，接在 `CoBRA` 前后 | 代表用 Gain Beyond RAG 进行 utility-aware search policy learning；同时说明 reward 主要是 ex-post |
| [Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents](https://arxiv.org/abs/2602.16699v3) | 第12行 `CoBRA` 后 | 代表显式 cost-aware decision；必须说明 calibrated prior 是外部组件，不能直接解释为 LLM 内生校准能力 |

以下三篇不一定需要占用正文主段落，但建议至少在 A1 脚注或段末列出：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [Value of Information: A Framework for Human–Agent Communication](https://aclanthology.org/2026.acl-long.1987/) | 第14行结尾脚注或 A1 open-problem 句子 | 理论上的 value-of-information 对照；注意它研究的是澄清/通信，不是文档检索 policy |
| [Redefining Retrieval Evaluation in the Era of LLMs](https://aclanthology.org/2026.eacl-long.391/) | 第12行与 `LLM-Specific Utility` 并列 | 把传统 relevance 与 LLM-consumer utility 区分开；属于 measurement anchor，不是 post-training method |
| [Self-Knowledge Guided Retrieval Augmentation for Large Language Models](https://aclanthology.org/2023.findings-emnlp.691/) | 第10行或历史工作脚注 | 早期 paired no-retrieval/retrieval gate；但它主要更新 auxiliary gate，不是目标 LLM 的 policy post-training |

A1 不建议继续增加更多 uncertainty-trigger 论文；当前真正需要的是把 ex-post reward 和 ex-ante value prediction 的区别写清楚。

### A2 — Information-Need Formulation

A2 的七篇核心论文已经全部进入正文：`Query Rewriting`、`DeepRetrieval`、`ExpandSearch`、`ParallelSearch`、`StepSearch`、`QREF` 和 `Environment-aware IR`。这一节不需要补核心论文。

当前论文池中另有两篇，但建议只放在边界说明或 Section 5，而不是当作 A2 核心证据：

- `LESER`：电商生产搜索中的 query-set optimization，适合说明 backend-specific application，但不能证明可泛化的信息需求表达；
- `WebDancer`：完整 web-agent policy，query 只是 click/visit/answer trajectory 中的一个动作，不能作为 standalone query formulation 证据。

### A3 — Information-Space Exploration

当前正文已引用 `MARAG-R1`、`DDPO`、`InfoMosaic-Bench` 和 `Table-as-Search`。建议补：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [Agentic Search in the Wild](https://arxiv.org/abs/2601.17617) | 第28行，在 `InfoMosaic-Bench`/`Table-as-Search` 后 | 提供大规模真实搜索轨迹中的 exploration、repetition、coverage 行为测量；注意 CTAR 不等于 useful information gain |

以下两篇建议不放 A3 核心段落，只在第30行“tool selection differs from necessary evidence source”之后作为 boundary contrast：

- `Training Small Language Models for Transferable Tool Selection`：证明 tool-repository selection 可以跨 retriever/repository 迁移，但不等于 task-relevant evidence acquisition；
- `ToolSearcher`：提供 event-level tool discovery credit，但没有证明发现了任务所需的新证据区域。

### A4 — Evidence Evaluation

当前正文已覆盖 `CRAG`、`RLSeek`、`RAGTruth`、`RAGChecker`、`S2G-RAG`、`Sufficient Context`、`AuthorityBench`、`CONFACT`、`ConfRAG` 和 `What Evidence Do Language Models Find Convincing?`。需要补两篇：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [EAPO: Evidence-Augmented Policy Optimization with Reward Co-Evolution for Long-Context Reasoning](https://aclanthology.org/2026.acl-long.1404/) | 第37行，紧接 `RLSeek` 后 | 补齐 evidence-quality reward + GRPO 的 post-training 证据；同时要注明 reward model 与 answer reward 耦合，不能直接视作通用 source reliability learning |
| [(D)RAGged Into a Conflict](https://research.google/pubs/dragged-into-a-conflict-detecting-and-addressing-conflicting-sources-in-retrieval-augmented-llms/) | 第39行，接在 `ConfRAG` 或 `CONFACT` 后 | 提供 typed conflict taxonomy 和 CONFLICTS benchmark；可支持“冲突不是单一标签”，但 prompting 改善不等于 transferable conflict-resolution policy |

A4 的关键叙事应保持：`relevance/support → authority/convincingness → sufficiency → typed conflict`，不要将新增两篇写成已解决 source reliability 的证据。

### A5 — Evidence Integration and Belief Revision

当前正文已经引用 `ClashEval`、`Source-balancing`、`GuarantRAG`、`FaithfulRAG`、`RAFT`、`BRIEF`、`CARE` 和 `MIRAGE`。以下五篇应补入正文：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [Belief-R](https://aclanthology.org/2024.emnlp-main.586/) | 第49行，在 `ClashEval` 后 | 直接提供 belief update/maintenance 行为测试；可作为“模型可能不更新或过度更新”的 evaluation counterevidence |
| [Efficient Prior-Guided Reasoning](https://aclanthology.org/2026.acl-long.1013/) | 第49行，在 `Source-balancing` 或 `GuarantRAG` 后 | 代表 GRPO/RLVR 在 noisy/conflicting context 下的 prior-guided reasoning；同时要指出 answer-F1 reward 不能单独证明 evidence use |
| [TRACE the Evidence](https://aclanthology.org/2024.findings-emnlp.496/) | 第52行，在 `BRIEF` 或 `CARE` 后 | 最适合补 A5.2 multi-evidence composition；它显式构造 multi-hop evidence chain，但没有 necessary-hop leave-one-out |
| [FRANQ](https://aclanthology.org/2026.findings-acl.338/) | 第55行，在 `MIRAGE` 后 | 明确区分 factuality 和 faithfulness；建议作为 A5.3 的 measurement distinction |
| [Evaluation of Attribution Bias in Generator-Aware RAG](https://aclanthology.org/2025.findings-acl.1087/) | 第55行，紧接 FRANQ 后 | 通过作者元数据反事实说明 citation/attribution 可能受 metadata shortcut 影响，不能把 citation presence 等同于 content-based evidence reliance |

A5 的证据层级建议在第55行明确写成：

```text
factuality → faithfulness → output reliance → attribution sensitivity
→ compositional reliance → process-level reliance
```

### A5.4 — Cross-turn Information Persistence

当前正文只引用 `LongMemEval`、`Memory-R1` 和 `MemRL`。建议至少补：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [MemoryAgentBench](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fd1eff9dd295df50a41f2521942fa31d-Abstract-Conference.html) | 第58行，在 `LongMemEval` 后 | 补增量交互、冲突、时间变化和选择性遗忘的 evaluation 维度；它比单纯长期 QA 更贴近 state revision |
| [A-MEM](https://proceedings.neurips.cc/paper_files/paper/2025/hash/19909c36f51abc4856b4560aff3d36d6-Abstract-Conference.html) | 第58行末尾，作为 system baseline | 说明 memory organization/metadata evolution 是 system mechanism，不等于 learned LLM persistence policy |
| [Mem0](https://doi.org/10.3233/FAIA251160) | 第58行末尾，和 A-MEM 并列 | 作为 practical lifecycle baseline；同样不应被写成 LLM parameter post-training 证据 |

### A6 — Adaptive Search Control

当前正文已经引用 `Self-RAG`、`Search-R1`、`R1-Searcher`、`ZeroSearch`、`AutoSearch`、`AnySearch` 和 `Beneficial Reasoning Behaviors`。建议在第67行之后补：

| Paper | 建议位置 | 作用 |
|---|---|---|
| [CriticSearch](https://aclanthology.org/2026.findings-acl.596/) | 第67行，与 `Beneficial Reasoning Behaviors` 后半段并列 | 代表 retrospective critic 和 turn-level hindsight credit；必须注明 full-trajectory/Gold Answer critic 不等于 deployment-time online feedback understanding |
| [CW-GRPO](https://aclanthology.org/2026.acl-long.1462/) | 第67行，和 CriticSearch 并列 | 代表 contribution-aware retrieval utility × reasoning correctness weighting；可补 action-level contribution，但仍需要区分 reward weighting 与真正 next-action adaptation |

## 3. Collected papers that should not be forced into Section 3

以下论文已经搜集，但更适合放到 Section 4 `How to Learn`、Section 5 `What Was Actually Learned?`、Related Work 或附录，不建议为了“引用完整”而塞进 Section 3：

- `SSPO`：重点是 step-level self-distilled policy optimization、teacher distribution 和 privileged information，属于 How to Learn 机制轴；
- `TRACE: Turn-level Reward Assignment via Credit Estimation for Long-Horizon Agents`：注意它不是 `TRACE the Evidence`，属于 credit-assignment 机制论文；建议放 Section 4 或 Section 5；
- `Retrieval, Reward, and Training Protocols: What Matters in Training Search Agents?`：是 controlled training comparison，最适合 Section 4 的 environment/protocol confound；
- `Agentic-R`：retriever–agent joint optimization，适合 Section 5 讨论 system-component confounding；
- `Rethinking Reasoning-Intensive Retrieval`：适合 Section 5 讨论 retriever quality 与 downstream evidence integration 的区分；
- `LESER`、`WebDancer`、`Training Small Language Models for Transferable Tool Selection`、`ToolSearcher`：适合作为 boundary/application contrast，不应直接当作独立 information-seeking capability learning 证据；
- 7 篇相邻 surveys：放 Related Work，不放 Section 3 的能力证据段落。

## 4. Recommended minimum revision

如果不想显著增加 Section 3 长度，最小修改只需要加入以下 13 个 citation anchors：

1. A1：`Adaptive Retrieval Without Self-Knowledge?`、`s3`、`Calibrate-Then-Act`；
2. A3：`Agentic Search in the Wild`；
3. A4：`EAPO`、`(D)RAGged Into a Conflict`；
4. A5：`Belief-R`、`Efficient Prior-Guided Reasoning`、`TRACE the Evidence`、`FRANQ`、`Attribution Bias`；
5. A6：`CriticSearch`、`CW-GRPO`。

如果希望 A5.4 的 coverage 也完整，再增加 `MemoryAgentBench`，并把 `A-MEM`/`Mem0` 放在一句 system-baseline 说明中。

## 5. Final judgment

当前 Section 3 **不是引用不完整到需要重写**，而是已经有正确的 taxonomy 和主线，但存在明显的“正面方法覆盖优于反例/诊断覆盖”的不平衡。最应优先补的是：

1. A5 的 factuality–faithfulness–attribution distinction；
2. A4 的 source convincingness 与 typed conflict；
3. A1 的 ex-ante value prediction 对照；
4. A6 的 hindsight credit 与 contribution-aware control。

完成这些补充后，Section 3 就能更自然地承接 Section 5 的核心问题：论文报告的最终答案提升，究竟证明了哪一种 information-seeking capability，而没有证明哪一种更强的 capability？
