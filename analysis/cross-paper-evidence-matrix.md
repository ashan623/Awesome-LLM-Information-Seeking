# Cross-Paper Evidence Matrix

## Purpose and coding rule

这份矩阵不是给论文做一个综合排名，而是把不同能力放到同一条证据链中：

> **Learning Target → Training Signal → Observed Behavior → Capability Isolation → Generalization Evidence**

每个单元都必须回答五个不同问题：训练目标是什么；奖励或监督信号实际测量什么；模型表现出了什么行为；有没有受控实验把目标能力从其他因素中隔离出来；泛化证据到底跨越了什么维度。

`Task OOD`、`Domain OOD`、`Retriever OOD`、`Tool OOD`、`Corpus/Environment OOD`、`Budget/Cost` 和 `Behavior Transfer` 分开记录。新 benchmark 或新 backbone 不自动记为行为泛化。

本版采用两层矩阵：第一层是六类能力的横向总览；第二层是当前论文池中信息密度最高的代表性论文。低信息量的重复实现、邻近 survey、纯系统工程和已标记为 boundary 的论文仍保留在各自主题文件中，但不在第二层重复展开。

## I. Capability-level matrix

| Capability | Learning Target | Main Training Signals | Observed Behavior | Strongest Capability Isolation Available | Generalization Evidence | Not Yet Established |
|---|---|---|---|---|---|---|
| **A1 Epistemic Awareness** | 识别信息缺口，决定是否获取信息，并预测检索的净收益 | Answerability / uncertainty → counterfactual utility → cost-aware and ex-post utility rewards | 选择性触发检索、减少不必要调用、成本—准确率权衡、utility-aware routing | CoBRA 的 paired no-tool/tool boundary；Calibrate-Then-Act 的 cost-conditioned action matching；均仍不是完整的 per-instance ex-ante VOI calibration | Task/domain 有中等证据；budget/cost 有局部证据；Retriever/Tool/Corpus/Behavior Transfer 有限 | 行动前是否能预测 `E[U|retrieve] - E[U|no-retrieve] - cost`，并在新模型、后端、语料和成本下保持校准 |
| **A2 Information Need Formulation** | 把目标和知识缺口表达为完整、相关、可执行的 query、sub-question 或 search plan | Query SFT、pseudo-query、Recall/NDCG、answer-hit、search-key F1、evidence coverage、entity delta、answer reward | 单查询改写、多查询扩展、并行分解、retriever-aware dialect、reasoning-conditioned refinement | QREF 的 original-versus-refined coverage 对照是最强近似；Environment-aware IR 最清楚展示了 backend dependence，但严格 Query-only intervention 尚不存在 | Task OOD 相对常见；Domain 有限—中等；Retriever/Tool/Corpus/Behavior Transfer 有限；预算迁移基本未测 | Semantic Intent Preservation 与 Backend Adaptation 能否同时成立；query policy 能否在新 retriever/tool/corpus 中快速迁移 |
| **A3 Information Space Exploration** | 发现新的信息源或尚未覆盖的必要信息区域，避免只做重复检索 | Document coverage、Gold Passage Recall、query/document diversity、log-det information gain、source/tool feedback、coverage state | 多工具协调、非冗余查询、互补证据覆盖、表格/候选—约束空间补全 | MARAG-R1 的 gold-document coverage 和 DDPO 的 diversity/coverage metrics；两者都只有 Medium 级隔离，没有 source-removal 或 coverage counterfactual | Task/domain breadth 有一定证据；未见 source inventory、tool API、corpus structure 和 behavior transfer 基本缺失 | Diversity 是否带来 Useful Coverage；模型能否在陌生 information space 中发现此前未覆盖的必要区域 |
| **A4 Evidence Evaluation** | 判断证据相关性、支持关系、来源可靠性、集合充分性和冲突鲁棒性 | Relevance labels、entailment、support/refute、authority、sufficiency/gap、verifier reward、evidence utility | 相关性路由、claim/span 支持判断、source authority ranking、充分性/缺口判断、冲突组织与噪声敏感性 | RAGChecker 的 noise sensitivity；AuthorityBench 的 authority perturbation；Sufficient Context 的 query–context sufficiency；均为局部受控证据 | C1 最充分；C2/C3 有限；Retriever/Tool/Environment/Behavior Transfer 弱；C7 基本未测 | 是否通过 post-training 学会独立、可迁移的 evidence judgment，而不是迎合 verifier、source popularity 或答案标签 |
| **A5 Evidence Integration** | 把已获取且已评价的信息用于更新推理、belief、generation、decision 和跨轮状态 | Answer reward、citation/span、evidence preference、supporting-fact SFT、GRPO/RLVR、memory utility | 外部断言纠正/拒绝、multi-hop composition、evidence-conditioned answer、部分 memory persistence | ClashEval/source-balancing 的 evidence replacement 与双向 conflict；TRACE/BRIEF 提供间接 composition；严格 necessary-evidence leave-one-out 尚不存在 | Task/domain 有部分证据；source/order/time/tool/environment/behavior transfer 不充分；A5.4 的操作级 transfer 更弱 | Causal Evidence Reliance：删除或替换必要证据后，推理和结论是否必然合理变化；是否能避免参数捷径和盲从错误外部证据 |
| **A6 Adaptive Search Control** | 根据搜索反馈和资源状态继续、重试、改写、切换、验证、分配预算或停止 | Stopping/depth reward、budget reward、turn-level hindsight credit、contribution-weighted advantage | 搜索深度控制、预算分配、早停、轮级 credit、基于新证据的训练偏好 | AnySearch 的 unseen-budget transfer；AutoSearch 的 depth/stop；CriticSearch/CW-GRPO 的 credit assignment；固定历史只改 feedback 的强干预尚不存在 | C7 budget transfer 以 AnySearch 最强；Task OOD 有限；Retriever/Tool/Environment/Behavior Transfer 基本缺失 | Better credit assignment 是否真的产生 feedback-conditioned control；部署时遇到空、重复、冲突或新证据是否改变下一动作 |

## II. Training signal progression across capabilities

不同能力的信号不能被排成一条单调的“弱到强”序列。更准确的说法是，不同信号把训练对象从一个层次移动到另一个层次，但并不自动扩大能力证明范围。

| Signal family | Directly supervises | Typical capability evidence | Main misalignment risk |
|---|---|---|---|
| **Final outcome / answer reward** | 最终任务成功 | Search-RL 的答案提升、多跳 QA、端到端工具调用 | 无法定位具体 query、证据、验证或控制动作；容易奖励 generator 或 retriever shortcut |
| **Invocation / cost / stopping reward** | 调用次数、深度或预算效率 | A1/A6 的 selective retrieval、search depth、tool productivity | 节省调用不等于知道何时有价值；固定深度或 budget template 可能被误认为能力 |
| **Backend utility reward** | 指定 retriever 上的 Recall/NDCG/entity coverage/SQL execution | A2 query utility、A3 document coverage、A6 search efficiency | 优化的是 backend dialect 或 corpus statistics，不等于 semantic intent 或 transferable exploration |
| **Evidence-quality signal** | support、sufficiency、authority、conflict | A4 claim verification、gap judging、source ranking | 标签质量、来源流行度、答案泄漏或 verifier gaming；评价资源不等于 post-training capability |
| **Evidence-conditioned preference / process signal** | citation span、supporting fact、reasoning consistency、evidence contribution | A5 evidence following、A6 contribution-aware credit | 输出随证据变化不等于 process-level causal reliance；LLM judge 可能只提供后验解释 |
| **Hindsight / counterfactual signal** | 事后分支收益、gold answer、full trajectory、actual/oracle state | A1 utility learning、A6 retrospective critic、A5 replacement tests | 训练时拥有部署时不可得的信息；改善 credit 不等于部署时知道下一步动作 |

## III. Orthogonal learning-mechanism axis

能力矩阵还需要一条正交的 **How to Learn** 轴。不能把 `Learning Signal`、`Optimization Method` 和 `Learning Target` 合并成一个“训练方法”字段：

| Dimension | Core question | Typical values | What must remain separate |
|---|---|---|---|
| **Learning Signal** | 从什么反馈中学习？ | outcome、process、preference、teacher distribution、counterfactual utility、state/value | signal 的语义不等于能力本身 |
| **Optimization Method** | 如何把反馈转成参数更新？ | SFT、DPO、OPD、PPO、GRPO、REINFORCE、hybrid | GRPO 不代表 outcome reward；OPD 不自动等于 reward-based RL |
| **Learning Target / Update Locus** | 哪个组件被更新？ | search policy、query rewriter、verifier、retriever、reader、memory、critic、joint system | retriever 或系统增益不能自动归因于 LLM capability |
| **Credit Granularity** | 成功/失败归因到何种粒度？ | episode、turn、step、token/span、state transition | 更细 credit 不自动意味着更真实的因果 credit |
| **Deployment Information Parity** | 训练时的信息部署时是否可得？ | state-only、feedback、future trajectory、gold answer、oracle evidence | teacher/critic 的 hindsight privilege 必须单独记录 |

因此，代表性论文表中的 `Training Signal` 只能回答一部分问题；完整编码应同时记录 `Optimization Method`、`Update Target`、`Credit Granularity` 和 `Deployment Availability`。详细字段和 B1–B7 定义见 [`learning-mechanism-framework.md`](./learning-mechanism-framework.md)。

## IV. Mechanism × capability cross-check

| Capability | Dominant update targets | Direct signals in the current pool | Main mechanism–capability risk |
|---|---|---|---|
| **A1** | search policy、router、utility/value predictor | answerability、uncertainty、counterfactual utility、cost | ex-post utility 被解释成 ex-ante value calibration |
| **A2** | query rewriter、planner、retriever–query coupling | pseudo-query、Recall/NDCG、entity coverage、search-key、answer utility | backend dialect/词面命中被解释成 semantic intent |
| **A3** | multi-tool policy、source selector、coverage planner | document coverage、Gold Passage Recall、diversity、information gain | diversity/gold-document matching 被解释成 unfamiliar-space exploration |
| **A4** | verifier、sufficiency judge、source ranker | support/refute、authority、sufficiency/gap、noise robustness | verifier 适配被解释成 general evidence judgment |
| **A5** | reader/reasoning policy、belief update、memory policy | answer reward、citation/span、evidence preference、supporting facts | evidence-conditioned output 被解释成 process-level causal reliance |
| **A6** | search policy、critic、value model、budget controller | stopping/depth、budget、turn credit、contribution utility | credit assignment 改善被解释成 feedback-conditioned control |

这张交叉表不是新增分类，而是把已有六类能力与学习机制重新对齐。它应与后面的误差分类一起使用：一个方法可以在 `Optimization Method` 上很先进，但在 `Learning Target`、`Deployment Information Parity` 或 `Behavior Transfer` 上仍然证据不足。

## V. Representative paper-level matrix

下表只列当前文献池中能直接支撑横向论证的代表性论文。`Isolation` 和 `Transfer` 是分开的证据状态，不是综合评分。

### A1 — Epistemic Awareness

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| When to Retrieve | known/unknown trigger | base-model correctness → `<RET>` | 正确问题少检索、未知问题触发检索 | 行为目标直接，但标签混入 base-model error；非 counterfactual utility | QA-focused；跨后端行为 transfer 未测 |
| KBM | knowledge-boundary routing | consistency / answerability | 直接回答、检索或多步检索路由 | knowledge boundary proxy；不是净收益 | benchmark/task transfer 有限 |
| DRAGIN | real-time retrieval need | entropy、attention、hidden-state signals | token/step-level dynamic retrieval | inference heuristic；未直接训练 VOI | fixed backend；calibration transfer 未建 |
| SeaKR | self-aware adaptive retrieval | uncertainty / multi-sample variation | uncertainty-triggered retrieval and reranking | uncertainty 与 usefulness 分离不足 | cross-environment calibration 未测 |
| LLM-Specific Utility | model-consumer evidence utility | paired model/evidence utility measurement | 同一 evidence 对不同 model 的收益可不同 | 强 utility measurement；不是 policy learning | model/task/evidence dependence 明确，transfer policy 未建 |
| CoBRA | tool-use boundary | paired no-tool/tool counterfactual margin + cost | 减少 over-calling/under-calling | 当前 A1 最强局部 boundary isolation；单次分支、成本和 judge 混杂 | tool/domain transfer 初步；ex-ante calibration 未证明 |
| Calibrate-Then-Act | cost-conditioned action | calibrated prior + cost | retrieve/answer、verify/commit、test/submit 随 cost 改变 | cost-conditioned oracle match；prior 是外部组件 | cost variation 有限；LLM intrinsic VOI 未证明 |
| InfoReasoner | semantic information-gain search | posterior/answer probability shift | 搜索、查询、继续/停止 | posterior shift 是 ex-post signal | task/domain breadth；新 tool/corpus calibration 未建 |

### A2 — Information Need Formulation

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| Query Rewriting | one-shot query rewrite | pseudo-query SFT + reader EM/F1 | q→q′ rewrite | independent rewriter but answer-hit mixes reader/backend | fixed Bing/BM25-like stack |
| DeepRetrieval | backend-specific enhanced query | Recall/NDCG/SQL execution/evidence hit | field/Boolean/SQL-aware query | strong backend utility; no semantic intent test | multiple backends mostly environment-bound |
| ExpandSearch | query expansion | answer reward + format/search signals | semantic/keyword/paraphrase/decomposition query sets | expansion behavior visible; query quality confounded with count/squeezer | task transfer, backend transfer limited |
| ParallelSearch | parallel sub-question decomposition | EM + decomposition/search-count signals | independent subqueries in parallel | isolates parallelization better than query quality | fixed retrieval setting; each subquery quality untested |
| StepSearch | reasoning-conditioned search key | answer F1 + key F1 + support-doc information gain + redundancy penalty | query reformulation after unresolved hop | closest to process-level query/evidence credit; no strict query-only test | benchmark/model transfer; retriever transfer limited |
| Environment-aware IR | retriever-aware dialect | target retriever passage-ID NDCG | query style changes with retriever/corpus | strongest Backend Utility and negative cross-retriever evidence | shows dependence, not transferable intent |
| QREF | refined query batch with complementary coverage | original-vs-refined recovered/preserved/lost/missed entities | reasoning-conditioned query refinement | strongest Query-only approximation; evidence filtering must be separated | Agent Backbone Transfer; same web stack, not Retriever Transfer |

### A3 — Information Space Exploration

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| InfoMosaic-Bench | source/tool selection | benchmark task success, tool-use correctness, budget curves | heterogeneous web + domain-tool choice | strong measurement, not post-training learning | multi-source inventory; no learned policy transfer |
| Table-as-Search | coverage-state navigation | unresolved candidate–constraint cells, table completion | fill missing regions, global verification | explicit coverage state; policy learning not isolated | system/state support, not behavior transfer |
| Agentic Search in the Wild | trajectory exploration/repetition | large-scale log labels and CTAR | horizontal exploration vs repetition patterns | behavioral analysis, not training | real logs but shared backend |
| MARAG-R1 | fixed multi-tool retrieval coordination | answer F1 + gold document-ID F1 + tool-call efficiency | complementary retrieval and useful document coverage | document reward/tool ablations; no unfamiliar-source intervention | task transfer within fixed tool family |
| DDPO | exploration diversity and coverage | query/document diversity + log-det information gain + Gold Passage Recall | less exploration collapse, broader evidence | Medium behavior isolation; no source-removal counterfactual | multiple benchmarks; fixed retriever/corpus |
| Transferable Tool Selection | tool-repository selection | gold tool-set exact match | API search and tool-set transfer | boundary contrast; no task-evidence coverage | strong tool/retriever transfer, not A3 information acquisition |

### A4 — Evidence Evaluation

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| CRAG | query–document relevance routing | evaluator score + corrective route | correct/incorrect/ambiguous retrieval handling | narrow relevance gate; title/lexical shortcut risk | fixed evaluation environment |
| S2G-RAG | sufficiency and missing-gap judgment | teacher-distilled context-only sufficiency/gap SFT | stop/continue and identify bridge entity/attribute/relation | strongest direct A4.3 learning signal; gold-title/Wikipedia limitation | set-level independent truth and cross-tool transfer limited |
| RAGTruth | unsupported/contradictory span detection | human span annotations | claim/span support and contradiction diagnosis | strong measurement; detector not general evidence policy | corpus/domain transfer limited |
| RAGChecker | fine-grained RAG diagnosis | claim/chunk metrics + noise sensitivity | support, precision/recall, faithfulness, noise response | strongest local noise perturbation | evaluation framework, not policy learning |
| RLSeek | source-span support/refute verification | citation/source locating reward + GRPO | locate support, cite, explain support/refute | direct narrow A4.1 post-training | source given; authority/sufficiency/conflict/tool transfer absent |
| EAPO | evidence utility in long-context reasoning | evidence-quality reward co-evolved with answer reward | evidence extraction and citation behavior | direct but answer-coupled; verifier/format shortcut risk | long-context/task variation; general transfer limited |
| AuthorityBench | source authority perception | authority ranking benchmark | pointwise/pairwise/listwise source ranking | source/format/authority perturbations; evaluation only | popularity/TLD/centerity shortcut; temporal transfer weak |
| CONFACT | credibility + support/refute | credibility predictor + source filtering/weighting | conflict-aware fact-checking | local source/conflict intervention; frozen verifier | conflict ratio/source replacement/time shift absent |
| Sufficient Context | query–context sufficiency | human/expert pair labels | separate insufficient context from generation failure | relatively independent pair-level truth; small scale | set-level, domain/tool transfer limited |
| ConfRAG | multi-source conflict organization | human-adjudicated viewpoint/reason coverage | cluster sources and cover reasons | organization/coverage, not truth selection | static web conflict; robust resolution untested |

### A5 — Evidence Integration and Persistent Information

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| ClashEval | parametric–external conflict response | controlled assertion replacement | switch to correct external evidence, resist false evidence | strong Output Reliance replacement test; mainly single-fact QA | process reliance and persistence untested |
| FaithfulRAG | fact-level context-faithful generation | fact mining/alignment/conflict handling | context-conditioned factual output | replacement/noise settings; no necessary-evidence deletion | cross-domain/process transfer limited |
| Belief Revision | update vs maintain after new premise | inference-time belief update protocol | revise or maintain answer | belief behavior measured; not parameter post-training | synthetic/controlled premises |
| GuarantRAG | knowledge integration and joint decoding | contrastive DPO + retrieval/fusion training | prefer retrieved evidence and integrate multi-document context | output-level integration; external-obedience risk | process reliance and false-evidence resistance limited |
| Efficient Prior-Guided Reasoning | parametric–external balance | GRPO/RLVR + prior dropout + answer F1 | robust reasoning under noisy/conflicting context | conflict/noise/full-wrong contexts; answer reward confounds | task/model variation, environment transfer limited |
| Source-balancing study | accept correct / reject incorrect assertion | mixed LoRA SFT with source/order controls | PAR+/SDR+ changes under source correctness | strongest answer/probability-level bidirectional control | synthetic setting; no process causality |
| RAFT | domain RAG adaptation | oracle/distractor documents + cited answer SFT | evidence selection and cited answer | distractor/document-count variation | domain coverage; necessary-evidence causal test absent |
| BRIEF | multi-hop evidence compression | proposition compression and cross-document coverage SFT | compactly preserve complementary hops | compression/context budget and reader transfer | necessary hop deletion absent |
| CARE | native retrieval-augmented reasoning | retrieval-span SFT + GRPO reward | evidence-conditioned reasoning and retrieval spans | partial process signal; key evidence necessity absent | multiple tasks/models; environment transfer limited |
| MIRAGE | answer attribution to context/model internals | internal attribution diagnostics | context-sensitive prediction and attribution | post-hoc internal diagnostic, not causal proof | task/model variation; causal process test absent |
| TRACE | structured evidence chain | chain construction and component ablations | 2–4 hop evidence chains and multi-hop answer | strongest indirect composition evidence; no strict leave-one-out | several readers/Wikipedia; source/time transfer absent |
| LongMemEval | long-term interactive memory evaluation | benchmark conditions | temporal reasoning, knowledge update, selective recall | measurement resource; no operation-level state audit | dialogue/task coverage; memory causal ladder incomplete |
| MemoryAgentBench | incremental memory behavior | benchmark conditions | test-time learning, forgetting, memory use | measurement resource; no learned policy target | multi-turn coverage; backend/tool transfer weak |
| Memory-R1 | memory lifecycle policy | PPO/GRPO, ADD/UPDATE/DELETE/NOOP + downstream QA | explicit memory operations and utilization | action space aligned; sparse QA reward cannot prove operation correctness | policy post-training direct; cross-environment operation transfer absent |
| MemRL | future memory utility | frozen LLM + runtime Q-value/ranking updates | useful episodic memory selected for future tasks | future utility more direct; not full lifecycle policy | external memory/backend-specific |

### A6 — Adaptive Search Control

| Paper | Learning Target | Training Signal | Observed Behavior | Isolation / Intervention | Transfer Boundary |
|---|---|---|---|---|---|
| Search-R1 | search invocation and reasoning loop | outcome reward with search interaction | invoke search, reason, answer | endpoint behavior; action credit coarse | fixed search environment; behavior transfer limited |
| StepSearch | step-wise search/query/evidence alignment | step reward + outcome reward | continue, query, use evidence, answer | more localized than outcome-only; A2/A6 entangled | task/model transfer, retriever transfer limited |
| Beneficial Reasoning Behaviors | beneficial search behaviors | behavior labels and training comparisons | evidence-aware reasoning and action patterns | behavior taxonomy useful; independent feedback intervention absent | benchmark/task evidence; environment transfer limited |
| AutoSearch | search depth and stopping | gold-derived optimal depth + over/under-search reward | adaptive depth, fewer redundant searches | depth/stop measured; fixed-history feedback intervention absent | multiple QA/model sizes; environment transfer limited |
| AnySearch | budget allocation and early stopping | curriculum GRPO + adaptive efficiency reward | Reason/Search/Answer allocation, early stop, unseen-budget improvement | budget behavior and scaffold removal; content-feedback intervention absent | strongest C7 budget transfer; no Retriever/Tool OOD |
| CriticSearch | turn-level long-horizon credit | retrospective critic sees full trajectory + gold answer | reformulate after unhelpful evidence; denser action learning | hindsight credit assignment; deployment privilege gap | task OOD under fixed E5/Wikipedia/four-turn setup |
| CW-GRPO | contribution-aware search-round credit | novelty-aware retrieval utility × reasoning correctness | concentrate learning on useful non-redundant rounds | per-round credit and human calibration; no explicit next-action intervention | multi-benchmark; Retriever/Tool/Corpus transfer absent |

## VI. Matrix-level conclusions

### 1. The survey is not comparing “which method is best”

The unit of comparison is the relationship between a signal and a claimed capability. A method may be effective while its signal is only partially aligned with the capability. A benchmark may isolate a behavior while providing no evidence that the behavior was learned. A new dataset may provide Task OOD while saying nothing about Retriever, Tool, Environment, Budget, or Behavior Transfer.

### 2. The strongest positive evidence differs by capability

- **A1:** CoBRA and Calibrate-Then-Act most directly isolate selective tool use and cost-conditioned decisions, but ex-ante value calibration remains unproven.
- **A2:** QREF is the strongest query-only approximation; Environment-aware IR is the clearest evidence that query policies learn backend dialects and can fail under retriever shift.
- **A3:** MARAG-R1 is strongest for fixed-tool useful document coverage; DDPO is strongest for diversity and coverage-oriented exploration, but neither proves unfamiliar-space navigation.
- **A4:** S2G-RAG and RLSeek are the clearest post-training anchors, each for a narrow behavior; measurement coverage is broader than learning and transfer coverage.
- **A5:** ClashEval and source-balancing provide the clearest output-level evidence replacement; TRACE is the strongest indirect composition anchor; process-level causal reliance remains unproven.
- **A6:** AnySearch is strongest for budget generalization; AutoSearch is strongest for depth/stopping; CriticSearch and CW-GRPO improve credit assignment without proving online feedback-conditioned control.

### 3. The shared negative boundary

Across all six capabilities, existing post-training methods often improve an observable endpoint while leaving the intended capability under-isolated:

> **A better outcome is evidence that the system works under the tested conditions. It is not, by itself, evidence that the model acquired the claimed information-seeking capability or that the capability transfers.**

### 4. The proposed survey thesis

> **LLM post-training can produce useful information-seeking behaviors, but the literature rarely proves that the optimized signal corresponds to the claimed capability, that the behavior is causally isolated from retriever/generator/system shortcuts, and that it transfers beyond the original information environment.**

The next analysis should therefore use the same matrix for every capability rather than adding more method families. The four cross-capability failure modes are documented separately in [`training-capability-misalignment.md`](./training-capability-misalignment.md), while the transfer and intervention audit is in [`generalization-intervention-audit.md`](./generalization-intervention-audit.md).
