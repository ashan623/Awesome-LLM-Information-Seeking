# 相关综述比较与本 Survey 的定位

> **范围与证据边界。** 本报告仅依据题设提供的已核验结果撰写，不新增未经结果支持的事实。文中的 `[S1]`–`[S7]` 均对应文末 References 中的 `source_urls`。凡涉及本 Survey 的表述，均是依据题设中反复出现的差异化描述所作的定位归纳；A1–A6 的具体定义、四维 post-training mechanism 的正式命名和“三项贡献”的最终措辞，应以本 Survey 正式稿为准。

## 1. 总览比较表

| 综述 | 出版/版本状态（据核验结果） | Primary Unit | Learning Axis | Evidence Axis | Generalization | Main Contribution | 与本 Survey 的主要关系 |
|---|---|---|---|---|---|---|---|
| **A Survey of Large Language Model-Based Search Agents**（Xi et al.） | ACL 2026 正式发表；arXiv:2508.05668 的 v3 为预印本修订版。[S1] | 搜索代理系统、信息寻求轨迹及其工作流；按 parallel、sequential、hybrid 搜索结构和 single-/multi-agent 组织归类，而非被训练模型的可迁移能力。[S1] | 主要回答如何优化搜索代理：tuning-free、SFT、RL、SFT+RL、test-time scaling；讨论 query rewriting、planning、searching、reflection、generation 及多目标奖励。[S1] | 外部检索证据与系统评价信号：来源、推理/检索中间过程、答案正确性、citation accuracy、evidence quality，以及 rule-based、LLM-as-a-Judge、Agent-as-a-Judge 和 human judgment。它不是“学习证据”审查。[S1] | 覆盖多类任务、领域、来源和环境扩展，也承认 tuning-based 方法的有限泛化及学术结果与商业部署之间的缺口；未形成跨能力、机制、领域/环境的系统泛化审查。[S1] | 围绕 how to search、how to optimize、how to apply、how to evaluate 组织 LLM-based Search Agents 文献，并总结信息源融合、imperfect retrieval、多模态、定制 RL、基础设施和 self-evolution 等挑战。[S1] | 与本 Survey 在动态信息寻求、后训练、检索/证据质量、行为评测、开放式任务、部署和泛化风险上有实质重合；本 Survey 可补充能力—机制—证据/因果审查，但不宜声称首创 capability-centric taxonomy。[S1] |
| **A Comprehensive Survey on Reinforcement Learning-based Agentic Search**（Lin et al.） | 2025 arXiv 预印本，arXiv:2510.16724；正式页面未说明期刊/会议接收或发表状态。[S2] | 与搜索环境交互的 RL-based LLM agentic-search system，包括 policy、环境、动作、奖励和轨迹；另按 single-/multi-agent、module/step-level、system-level 作用域拆分。[S2] | 三条轴：RL 的功能角色（retrieval control、query optimization、reasoning–retrieval integration、multi-agent collaboration、tool/knowledge integration）；RL 的使用方式（SFT、环境、算法、curriculum/self-evolving、奖励）；RL 的作用域。[S2] | 主要是 benchmark evidence：答案质量、retrieval effectiveness、效率和 information gain、query quality、evidence utilization rate 等过程指标；没有统一的行为测量、受控干预、特权监督、policy-input leakage 或部署行为证据审查。[S2] | 将 cross-domain generalization 作为开放挑战，提出 meta-learning、通用搜索原则、领域自适应和异构检索环境等方向；未提供统一的 OOD、检索器迁移、部署分布变化或长期泛化评估。[S2] | 连接 agentic search 与 RL 基础，以“RL 做什么、如何用 RL、在哪里应用 RL”三维框架归纳方法、训练/奖励机制、数据集、指标、应用和挑战。[S2] | 与本 Survey 在 RL/post-training、检索时机、查询改写、检索—推理交错、效率、记忆、多 agent、工具/多模态和奖励设计上高度相邻；本 Survey进一步追问 A1–A6 能力是否真正被学到，以及证据是否可作因果归因。[S2] |
| **Deep Research: A Survey of Autonomous Research Agents**（Zhang et al.） | 2025 arXiv:2508.12752；正式页面未显示已出版的期刊/会议版本；正式标题不含 “Systematic”。[S3] | Deep Research system 的四个 pipeline stages/modules：Planning、Question Developing、Web Exploration、Report Generation；不是 post-training mechanism 或行为轨迹。[S3] | 高层 workflow/parameter optimization：single-/multi-agent workflow、contrastive learning、RL、curriculum training，以及 reward-optimized 与 supervision-driven 方法；未按本 Survey 的四维机制建立分类。[S3] | 主要是 benchmark 和 end-task/output 指标，包括 success rate、accuracy、calibration error、EM、KPR/KPC、precision/recall、clarity、insight、agreement，以及 citation accuracy、report fidelity、factuality、source attribution；未形成学习归因证据轴。[S3] | 主要作为开放问题讨论：规划策略跨任务迁移不足，缺少可积累的 generalizable planning knowledge，并提到 personalization、dynamic adaptation、continual learning；没有系统 OOD/跨任务/部署泛化审查。[S3] | 给出 autonomous deep research 的四阶段 pipeline 总览，分类规划、查询生成、网页探索、报告生成方法，汇总 workflow/参数优化和 benchmark，并讨论多工具、事实性、多模态、个性化等问题。[S3] | 与本 Survey在规划、信息获取、查询/问题开发、证据整合、训练优化和评测上重合；本 Survey可把 pipeline/module 重新组织为“能力（What to Learn）× 机制（How to Learn）× 实际学习证据”，而不把四阶段自动等同于 A1–A6。[S3] |
| **Reinforcement Learning Foundations for Deep Research Systems: A Survey**（Li et al.） | 2025 arXiv 预印本，arXiv:2509.06733；正式页面标为 arXiv 预印本，未核验期刊/会议出版。[S4] | 单个 planner policy/model 及其长程工具交互 trajectory；同时把 agentic RL training stack/framework 作为系统单元，明确不是 Planner–Coordinator–Executors 全栈联合训练。[S4] | training-first、RL-centric：data synthesis & curation、agentic research 的 RL methods、agentic RL training systems/frameworks；覆盖奖励、credit assignment、multi-objective/multimodal RL 和异步/分布式训练。[S4] | 主要是任务奖励和 benchmark 结果：终局答案、格式、证据效用、检索必要性，部分工作加入 turn/step/token-level process reward；未形成六类“实际学到了什么”的统一证据审查。[S4] | 部分涉及跨域/开放网页/多模态 benchmark、OOD/真实网页迁移和长期交互，但主要是缺口或零散结果；没有统一的 Generalization taxonomy、训练—部署分布对照或 leakage 审计。[S4] | 将 deep research 的 RL 文献组织为数据、RL 方法、训练系统三大部分，综述任务构造、奖励与多步 credit assignment、稳定性/样本效率/长上下文/多目标/多模态训练及基础设施。[S4] | 与本 Survey在数据、奖励、credit assignment、过程质量、长程搜索和真实网页表现上高度重合；本 Survey的补充点是将训练配方对齐到 A1–A6，并审查干预、特权信息、输入泄漏、部署行为和泛化。[S4] |
| **SoK: Agentic Retrieval-Augmented Generation (RAG): Taxonomy, Architectures, Evaluation, and Research Directions**（Mishra et al.） | 2026 arXiv 预印本，arXiv:2603.07379 v1；正式页面未列期刊/会议出版状态。[S5] | Agentic RAG 系统及其多步 control loop/trajectory；以 policy、state/memory、retrieval/tool actions、observations、termination 和系统 topology 为核心。[S5] | 相对于本 Survey 的四维机制轴为 Unknown/部分覆盖：讨论 prompting/fine-tuning、heuristic/hybrid/learned policy、RL 作为未来方向，但未系统分类 post-training mechanism 或证明具体学习结果。[S5] | SoK 式形式化、文献综合和框架性论证；提出 component-/trajectory-/system-level 评估及 faithfulness、retrieval efficiency、tool reliability、robustness、Progress Rate、Effective Information Rate 等指标，但未形成学习因果归因审查。[S5] | 间接涉及跨领域工具、zero-shot domain use、环境变化下的 adaptive resilience、persistent memory 和真实部署；不是系统的跨任务、跨域、OOD 或 post-trained behavior 泛化实验。[S5] | 用有限时域 POMDP 形式化 Agentic RAG，提出属性式 taxonomy，分解规划、检索、推理/控制、记忆、工具编排、验证/自纠错，并将评估从静态答案扩展到组件—轨迹—系统层，同时总结风险与研究方向。[S5] | 与本 Survey在动态检索、规划、工具、记忆、自我修正、中间行为、部署成本和鲁棒性上重合；其 architecture/control/evaluation 形式化可作邻接背景，但不等于 A1–A6 或四维机制—学习证据框架。[S5] |
| **The Landscape of Agentic Reinforcement Learning for LLMs: A Survey**（Zhang et al.） | arXiv:2509.02547；OpenReview 记录显示 2026 年 1 月 14 日被 TMLR 接收，并列有 TMLR 2026 记录；arXiv 当前核验为 v5。[S6] | Agentic RL 中的智能体能力/模块及任务域；能力侧覆盖 planning、tool use、memory、reasoning、self-improvement/reflection、perception 等，理论上以 MDP/POMDP 统一抽象。[S6] | 将 RL 描述为把静态/启发式组件转化为适应性 agentic behavior，讨论 planning、memory、tool use、reflection/self-improvement、reasoning/perception；但未采用本 Survey 的 A1–A6 或四维机制轴。[S6] | 主要是二次综合及被综述工作报告的任务成绩、reward、环境、benchmark 和 framework；可涉及 outcome reward、verifiable reward、tool-use emergence、robustness/OOD，但未形成统一六项学习证据审查。[S6] | 明确把 cross-domain generalization、评测协议不一致和可扩展通用 agent 列为缺口；汇总部分 robustness/OOD/跨环境结果并讨论 sim-to-real、在线部署和成本，但未按 A1–A6 做统一泛化审查。[S6] | 用 MDP/POMDP 区分传统 LLM-RL 与时序、部分可观测的 Agentic RL；提出能力—应用双重 taxonomy，汇总五百余项工作、环境、基准和框架，并讨论开放挑战。[S6] | 与本 Survey在 planning、tool use、memory、reasoning、长时程交互、奖励/策略优化和泛化上相邻；其能力集合更宽且主要服务于 Agentic RL 领域地图，本 Survey聚焦信息寻求能力、机制和实际学习证据。[S6] |
| **From Reasoning to Agentic: Credit Assignment in Reinforcement Learning for Large Language Models**（Zhang） | arXiv:2604.09459 v3；正式页面列为 arXiv preprint，未列期刊/会议正式出版状态。[S7] | 被分配 credit 或被替换/比较的 action/intervention unit：token、segment、step/turn、tool call、memory operation、message edge 及 agent-level 单位。[S7] | credit-granularity × methodology：Monte Carlo、TD/value、model-based/LLM-as-Critic、game-theoretic/Shapley/counterfactual、information-theoretic/information-gain/entropy；核心是如何估计/分配 credit，而非 A1–A6 能力本体。[S7] | 明确提出四级证据：restored-interventional、满足 exchangeability/positivity 的 observational-causal、model-relative counterfactual、predictive/proxy；另有六项诊断、CA-ID Card、falsification controls 和报告清单。它审查 credit estimand 的可识别性，不直接证明信息寻求能力已被学会。[S7] | 讨论 reasoning→agentic transfer/adaptation 及 replay fidelity、checkpointability、matched budgets；提出跨设置 protocol，但未完成跨 A1–A6、跨任务/环境 OOD 或真实部署泛化审查。[S7] | 覆盖 2024-01 至 2026-07-31 的 69 篇文献，提出 credit assignment taxonomy、六诊断、CA-ID Card、reporting audit 和最小 controlled evaluation protocol；贡献重点是 credit 的因果可识别性与报告规范。[S7] | 与本 Survey在 controlled intervention、特权信息、replay/状态恢复、uncertainty、budget parity、报告规范和 leakage 风险上有方法论交集；本 Survey可将这些证据规范嵌入信息寻求能力的 post-training 审查，但不应把两者的分析对象混为一谈。[S7] |

## 2. 按五个维度的综合比较

### 2.1 Primary Unit：系统、流程、训练栈、能力，还是可干预单位？

七篇综述的 primary unit 大致形成一条从“系统/流程”到“证据/干预”的谱系。Xi et al. 以搜索代理系统、信息寻求轨迹和工作流为单位；Lin et al. 以 RL agentic-search system、环境、动作、奖励和轨迹为单位；Zhang et al. 的 Deep Research 综述以四阶段 pipeline/module 为单位；Li et al. 则更偏向 planner policy、长程 trajectory 和训练系统；Mishra et al. 以 Agentic RAG 的 POMDP control loop、状态/记忆和工具动作作为单位。[S1][S2][S3][S4][S5]

Agentic RL 综述把单位提升为智能体能力/模块及任务域，credit assignment 综述则进一步把单位收窄为 token、segment、turn、tool call、memory operation、message edge 或 agent-level intervention。[S6][S7] 因此，已有工作分别擅长回答“系统由什么构成”“流程如何运行”“训练栈如何扩展”“哪个动作/片段应获得 credit”，但这些单位并不自动等于“某项信息寻求能力是否被 post-training 学会”。

本 Survey 的可支持定位是把 **A1–A6 information-seeking capabilities** 作为 What to Learn 的固定分析单位；这与按搜索架构、pipeline、RL training stack 或 credit unit 编目正交。该定位不要求宣称首创“能力视角”，因为 Agentic RL 综述已经使用较宽的 capability-centered taxonomy；更准确的差异在于：本 Survey把能力范围固定在信息寻求，并把能力与机制及学习证据显式连接。[S6]

### 2.2 Learning Axis：已有综述主要回答“如何组织/优化”，本 Survey还要回答“哪种机制改变了什么能力”

Xi et al. 以 tuning-free、SFT、RL、混合训练和 test-time scaling 组织搜索代理优化；Lin et al. 进一步沿 RL 的功能角色、使用方式和作用域展开；Deep Research 综述按 workflow/parameter optimization 汇总 contrastive learning、RL、curriculum 和监督驱动方法；Li et al. 以数据构造、RL 方法和训练系统为主轴；Agentic RL 综述按能力/模块和应用域讨论 RL 如何塑造 agentic behavior。[S1][S2][S3][S4][S6]

这些 Learning Axis 对“采用了什么训练配方、优化了什么环节、奖励或 credit 如何定义”很有覆盖，但不一定提供统一的“机制—能力”映射。Mishra et al. 对 prompting/fine-tuning、heuristic/hybrid/learned policy 和 RL 的讨论相对框架化；credit assignment 综述则专注 credit 的粒度与估计方法，回答的是 credit 如何被估计，而非 A1–A6 能力如何被获得。[S5][S7]

因此，本 Survey 可安全表述为：在已有 optimization/training-first 综述之上，采用四维 post-training mechanism 作为 How to Learn，并将每一机制与 A1–A6 的 What to Learn 对齐，再检查观测到的增益是否能归因于目标能力。依据核验结果，已有综述没有以这套四维机制—A1–A6 对齐方式形成统一框架；但这应表述为“未见统一采用/可支持的差异化空间”，而非绝对断言“此前无人讨论这些机制”。[S1][S2][S3][S4][S5][S6][S7]

### 2.3 Evidence Axis：从任务分数和系统信号，走向“实际学到了什么”的证据审查

现有综述的证据大多来自文献报告的 benchmark、最终答案或报告质量、检索效果、过程奖励、来源/citation 指标、效率/成本、环境和系统信号。Xi et al. 明确讨论来源引用可靠性、不可信信息和 imperfect retrieval；Lin et al. 汇总 answer quality、retrieval effectiveness、效率与 information gain/query quality/evidence utilization；Deep Research 综述汇总 end-task 与 report/citation/factuality 指标；Li et al. 和 Agentic RL 综述强调终局及逐步奖励、credit assignment、工具交互和过程质量。[S1][S2][S3][S4][S6]

Mishra et al. 将评估扩展到 component、trajectory、system 三层，并提出 faithfulness、retrieval efficiency、tool reliability、robustness 等指标；这有助于观察系统行为，但仍不是“某项能力已由 post-training 学会”的统一因果证据。[S5] Zhang 的 credit assignment 综述在证据可识别性上最为细化：区分 restored-interventional、observational-causal、model-relative counterfactual 和 predictive/proxy 证据，并要求明确 unit、estimand、state、intervention provenance、support/uncertainty 和 falsification controls。[S7]

本 Survey 的证据轴可据此明确为六项审查：**Behavior Measurement、Controlled Intervention、Privileged Supervision、Policy-input Leakage、Deployment Behavior Evidence 和 Generalization**。其目标不是再列一组 benchmark，而是区分：性能/奖励是否伴随目标行为改变；改变能否由受控干预支持；监督是否提供了部署时不可得的特权信息；策略输入是否含有答案或等价线索；部署条件下是否仍出现该行为；以及行为是否跨任务、跨域或跨环境保持。对这些审查项，核验结果支持的是“已有综述未形成统一协议/系统核验”，而不是声称所有相关工作都没有任何局部证据。[S1]–[S7]

### 2.4 Generalization：已有工作常提出问题或汇总零散结果，较少建立统一审查

Generalization 在不同综述中呈现为三种形态。第一类是挑战/未来方向：Lin et al. 将 cross-domain generalization 作为开放挑战；Deep Research 综述强调跨任务规划迁移不足和 generalizable planning knowledge 的缺失；Agentic RL 综述将 cross-domain generalization 和可扩展通用 agent 列为研究缺口。[S2][S3][S6]

第二类是部分任务、领域、来源或环境覆盖：Xi et al. 覆盖 closed/open-ended QA、multi-hop/long-horizon QA、fact-checking、finance/medicine/code 等场景，并讨论来源、私有数据、多模态和部署问题；Li et al. 汇总跨域、开放网页、多模态和真实网页/长期交互相关方向；Mishra et al. 讨论跨领域工具、环境变化、zero-shot domain use 和真实部署。[S1][S4][S5] 这些内容说明泛化是共同关切，但不能直接推出“能力已被证明在部署分布中迁移”。

第三类是方法论上的迁移条件与协议：credit assignment 综述讨论 reasoning→agentic transfer，并强调 replay fidelity、checkpointability、matched budgets 和 protocol-specific contrast；但其提出的跨设置 controlled evaluation protocol 并不等于已经完成了跨 A1–A6 能力或部署泛化审查。[S7]

综合而言，本 Survey 的 Generalization 可定位为独立审查轴：要求把训练分布、测试任务/领域/环境、部署条件和可用信息明确区分，并避免从 benchmark 列表、开放网页或多模态覆盖直接推断可迁移能力。对每篇相关综述，最稳妥的归纳是“提出并汇总泛化议题/零散结果，但未建立本 Survey 式统一的能力级泛化审查”，而不是“完全没有泛化讨论”。[S1]–[S7]

### 2.5 Main Contribution：相关综述分别填补地图、训练、形式化或证据规范中的一块

- **搜索代理地图与评价地图：** Xi et al. 以 how to search/how to optimize/how to apply/how to evaluate 组织领域，并覆盖结构、优化、应用、评估与挑战。[S1]
- **RL-based agentic search 的三维组织：** Lin et al. 连接 agentic search 与 RL 基础，按 RL 的功能角色、优化策略和作用域归纳工作。[S2]
- **Deep Research pipeline：** Zhang et al. 把 autonomous deep research 组织为 Planning、Question Developing、Web Exploration、Report Generation 四阶段，并汇总方法与 benchmark。[S3]
- **Deep Research 的 RL foundations：** Li et al. 以数据、RL 方法和训练系统为主线，集中讨论复杂任务构造、奖励、credit assignment 和可扩展训练基础设施。[S4]
- **Agentic RAG 的系统形式化与 SoK：** Mishra et al. 用有限时域 POMDP 和属性式 taxonomy 统一 architecture/control/evaluation/risk 讨论。[S5]
- **Agentic RL 的能力—应用地图：** Zhang et al. 用 MDP/POMDP 区分 Agentic RL，并整理 planning、tool use、memory、reasoning、reflection 等能力及五百余项工作、环境、基准和框架。[S6]
- **Credit assignment 的可识别性与报告规范：** Zhang 的综述提出 evidence tiers、六诊断、CA-ID Card、source-located audit 和最小 controlled evaluation protocol，核心是 credit estimand 与证据边界。[S7]

这些贡献互补但不相同。本 Survey 不必与它们争夺系统地图、RL 训练综述、POMDP 形式化、能力领域地图或 credit assignment taxonomy 的首创性；更稳妥的定位是，在这些已有地图和规范之间增加一个固定的信息寻求能力—post-training mechanism—实际学习证据的对齐层。

## 3. 本 Survey 的三项贡献如何形成互补

> 以下“三项贡献”是根据题设核验结果中反复出现的差异化描述归纳的工作性表述，不替代本 Survey 正式稿中的最终贡献句。

### 贡献一：以 A1–A6 固定 What to Learn

本 Survey 将 A1–A6 information-seeking capabilities 作为核心分析对象，要求讨论“训练后究竟要获得哪种能力”，而不是把搜索架构、pipeline stage、RL training stack、Agentic RAG topology 或 credit unit 直接当作能力本身。它可吸收 Xi et al. 的搜索代理地图、Deep Research 的四阶段 pipeline、Agentic RL 的宽能力地图和 Agentic RAG 的 control taxonomy 作为背景，但不把这些不同粒度的系统/模块类别自动等同于 A1–A6。[S1][S3][S5][S6]

**互补关系：**

1. 对 Xi et al. 和 Lin et al.，能力轴把“采用何种搜索结构/RL 角色”与“哪个信息寻求能力发生变化”分开。[S1][S2]
2. 对 Deep Research 综述，能力轴把 Planning、Question Developing、Web Exploration、Report Generation 的流程描述转化为可检验的能力问题，而不将四阶段本身视为学习结果。[S3]
3. 对 Agentic RL 综述，能力轴在其宽泛 planning/tool/memory/reasoning 等地图之外收束到本 Survey 的信息寻求范围，因此是范围和可检验对象的收束，而不是否定已有 capability-centered work。[S6]

### 贡献二：以四维 post-training mechanism 固定 How to Learn

本 Survey 将四维 post-training mechanism 作为与 A1–A6 正交的 How to Learn 轴，目标是把训练信号、优化/数据机制与推理时架构控制区分开，并明确某一机制试图改变哪一项能力。核验结果显示，相关综述分别从 tuning-free/SFT/RL、RL 的功能角色与作用域、workflow/parameter optimization、数据—奖励—训练系统、prompting/fine-tuning/heuristic/hybrid/learned policy、以及 credit-granularity/methodology 等角度组织文献，但未以本 Survey 的四维机制与 A1–A6 形成统一对齐。[S1]–[S7]

**互补关系：**

- 它补足搜索架构综述的“用了什么系统结构”和 RL 综述的“用了什么算法/奖励”之间的中间层：同一能力可由不同机制获得，同一机制也可能作用于不同能力。[S1][S2]
- 它把 Deep Research 和 Deep Research RL 综述中的 workflow、数据构造、reward、credit assignment 和训练系统放入可比较的机制位置，但不把训练配方本身当作能力证据。[S3][S4]
- 它与 credit assignment 综述的关系是互补而非替代：后者说明如何估计/分配 credit 及其可识别性，本 Survey进一步问这些优化是否改变了目标信息寻求能力，以及这种改变是否在部署中保持。[S7]

### 贡献三：以证据与泛化审查 What Was Actually Learned

本 Survey 以 Behavior Measurement、Controlled Intervention、Privileged Supervision、Policy-input Leakage、Deployment Behavior Evidence 和 Generalization 六项审查来约束“学到了什么”的结论。其重点不是增加一个更长的指标列表，而是要求把 benchmark/奖励/过程信号与可归因的行为改变区分开，并检查特权信息、策略输入泄漏、部署条件和跨分布保持性。

**互补关系：**

- 对 Xi、Lin、Deep Research 和 Li 等主要提供 benchmark、终局/过程奖励、输出/检索质量或训练结果的综述，本贡献增加行为测量、干预、泄漏和部署证据层。[S1][S2][S3][S4]
- 对 Mishra et al. 的 component—trajectory—system 评估，本贡献把多层行为评估进一步连接到 post-training 学习归因，而不是仅把组件指标视为能力获得的证明。[S5]
- 对 Agentic RL 综述，本贡献把宽泛的能力/应用地图转化为能力级证据问题，并把零散 OOD、跨环境和部署讨论提升为必须明确条件的 Generalization 审查。[S6]
- 对 credit assignment 综述，本贡献可采用其 evidence tiers、controlled bifurcation、replica/noise floor、budget parity、uncertainty 和 reporting discipline 作为方法论邻接资源，但研究对象仍是信息寻求能力是否被学会，而非 credit estimand 本身。[S7]

三项贡献合在一起形成的定位是：**What to Learn（A1–A6）× How to Learn（四维 post-training mechanism）× What Was Actually Learned（六项证据/泛化审查）**。这是一种“机制—能力—证据”的审查层，建立在已有搜索系统、Deep Research、RL、Agentic RAG、Agentic RL 和 credit assignment 综述之上，而不是再次按架构、应用、数据集或训练算法编目。[S1]–[S7]

## 4. 可安全使用的 novelty / positioning wording

以下表述尽量避免超出核验结果的“首个/首次/唯一”断言，可用于摘要、引言或 related work；正式稿应根据本 Survey 的实际内容再做删改。

### 4.1 推荐的核心定位

> **本 Survey 提供一个面向信息寻求后训练的机制—能力—证据分析框架：以 A1–A6 information-seeking capabilities 表示 What to Learn，以四维 post-training mechanism 表示 How to Learn，并通过行为测量、受控干预、特权监督、policy-input leakage、部署行为证据和泛化审查，评估 What Was Actually Learned。**

> **与主要按搜索架构、Deep Research pipeline、RL 优化角色、训练系统、Agentic RAG 控制环或 credit assignment 单位组织文献的综述不同，本 Survey 将能力目标、后训练机制和学习证据放在同一分析坐标中。** [S1]–[S7]

> **本 Survey 的差异化不在于重新声称“能力视角”本身的新颖性，而在于将信息寻求能力范围、post-training mechanism 和可归因学习证据进行显式对齐。** [S6][S7]

### 4.2 对不同相关工作的精确定位

> **相较于搜索代理综述，本 Survey 从“系统采用了什么结构、获得了什么任务分数”进一步转向“哪些信息寻求能力被何种后训练机制改变，以及这一结论由什么证据支持”。** [S1]

> **相较于 RL-based agentic search 与 deep-research RL 综述，本 Survey 不替代其对 RL 角色、奖励、数据、credit assignment 和训练系统的总结，而是增加能力级目标与学习归因审查。** [S2][S4]

> **相较于 autonomous deep research 的 pipeline 综述，本 Survey 将阶段/模块描述与能力获得、机制对齐和部署泛化证据区分开来。** [S3]

> **相较于 Agentic RAG SoK，本 Survey 不重复其 POMDP、架构和风险 taxonomy，而将其控制环与评估信号纳入信息寻求后训练的能力—证据审查。** [S5]

> **相较于 Agentic RL 的宽能力—应用地图，本 Survey 聚焦信息寻求能力，并进一步审查某项能力是否由特定 post-training mechanism 实际获得、是否依赖泄漏或特权信息、以及是否在部署分布中保持。** [S6]

> **相较于 credit assignment 综述，本 Survey 借鉴其对 intervention provenance、estimand、uncertainty、replica floor 和 matched budgets 的证据纪律，但把审查对象扩展到信息寻求能力的行为改变与部署泛化，而不是把 credit assignment 本身作为终点。** [S7]

### 4.3 可使用但应加限定的词

可以使用：

- “提出/采用一个面向信息寻求后训练的机制—能力—证据分析框架”；
- “将 A1–A6 作为 What to Learn、四维机制作为 How to Learn”；
- “补充现有系统/流程/训练综述中的学习归因与部署行为审查”；
- “提供一个用于区分性能提升与目标能力学习的审查视角”；
- “把特权监督、policy-input leakage、受控干预和部署行为纳入统一检查清单”；
- “与已有能力地图、RL 综述和 credit assignment 证据规范形成互补”。

不建议无限定使用：

- “首次提出 capability-centric taxonomy”；
- “首次系统综述 agentic search / deep research / agentic RL”；
- “证明现有综述都没有能力视角”；
- “证明现有方法没有泛化”；
- “证明某种 post-training mechanism 一定学会了某项能力”；
- “建立了真实部署中的因果证据”（除非正式论文确有相应实证）。

## 5. 正式论文中仍需谨慎表述的结论

1. **关于“首个/首次”。** Xi et al. 已自称 LLM-based Search Agents 的第一份系统性综述，Lin et al. 已自称专门面向 RL-based agentic search 的首个综合综述，Li et al. 也将自身定位为据其所知首篇讨论 deep research systems 的 RL foundations 的综述；Agentic RL 综述和其他工作也已有能力、训练与证据地图。[S1][S2][S4][S6] 因此，本 Survey不应笼统声称“首次综述该领域”或“首次采用能力视角”，应把新颖性限定为 A1–A6、四维机制和六项学习证据审查的具体组合与对齐。

2. **关于“未覆盖/没有”。** 核验结果中的 Unknown 或“未形成统一框架”是基于正文范围的审慎归纳，不应改写为“相关论文完全没有任何行为、干预、泛化或部署评估”。更安全的措辞是“未见其建立本 Survey 所定义的统一审查协议”“不是其主要分析轴”“在核验范围内未报告系统执行”。[S1]–[S7]

3. **关于能力等同。** 搜索代理系统、Deep Research 四阶段、Agentic RAG 模块、Agentic RL 能力集合、RL 功能角色或 credit unit 都不能自动等同于本 Survey 的 A1–A6 information-seeking capabilities。尤其是 Agentic RL 综述已经采用较宽的 capability-centered taxonomy，因此应说本 Survey“收束并操作化信息寻求能力”，而非“首次发现能力应被分析”。[S3][S5][S6][S7]

4. **关于 benchmark、奖励与过程指标。** 最终答案正确、检索质量、citation/factuality、reward、information gain、evidence utilization、过程质量或 tool-use emergence 都是结果/过程信号；除非有行为测量、受控干预、排除特权监督与输入泄漏、并在适当分布上复现，否则不应直接写成“证明模型学会了某能力”。[S1]–[S7]

5. **关于因果归因。** Zhang 的 credit assignment 综述明确区分 restored-interventional、observational-causal、model-relative counterfactual 和 predictive/proxy 证据，并强调 estimand、state、intervention provenance、support、uncertainty 与 falsification controls。[S7] 本 Survey若声称“机制导致能力提升”，应明确干预对象、比较基线、可用信息、预算、环境状态和不确定性；不能把相关性、模型评审分数或 proxy 指标升级为因果证明。

6. **关于 privileged supervision 与 policy-input leakage。** 训练时可见的 gold evidence、完整轨迹、隐藏环境状态、答案相关提示或特权 critic 信息，可能使结果不能代表部署时可获得的能力。正式论文应明确列出训练/评估输入、监督信号和 critic 所见信息；若未进行审计，应写“未核验/未知”，而不是写“无泄漏”。[S2][S4][S7]

7. **关于 deployment behavior evidence。** 综述中出现真实网页、在线部署、成本/延迟、商业系统或 sim-to-real 讨论，不等于已获得部署时行为证据。只有在部署条件、可用工具、分布变化、预算和行为测量均被明确报告时，才可作较强的 deployment claim；否则应区分“部署相关议题”与“部署行为已验证”。[S1][S4][S5][S6]

8. **关于泛化。** 跨任务、跨域、OOD、检索器迁移、环境变化和长期交互往往以开放挑战、零散 benchmark 或未来 protocol 出现。不能从覆盖多个领域/数据集推断 A1–A6 在部署分布中泛化，也不能把提出 controlled evaluation protocol 写成已经完成系统泛化实验。[S1]–[S7]

9. **关于出版状态与版本。** 相关条目的 venue/status 必须按核验结果逐项写：Xi et al. 为 ACL 2026 正式发表；Zhang et al. 的 Agentic RL 综述有 OpenReview 的 TMLR 接收/记录；其余若未在正式页面显示期刊/会议出版状态，应保守写为 arXiv preprint 或出版状态未核验。版本号、日期和标题变化也不应在未核对时混写。[S1][S3][S4][S5][S6][S7]

10. **关于本 Survey 三项贡献的正式表述。** 本报告将三项贡献归纳为能力目标、机制轴和证据/泛化审查；正式论文仍需确认这是否与作者最终贡献列表完全一致，尤其要核对“四维机制”的正式名称、A1–A6 定义、六项审查是否全部属于已完成工作，还是部分属于拟议协议。若某项只是 future protocol，应改写为“提出审查议程/协议”，不能写成“已实证验证”。

## References

### [S1] Xi et al.

Yunjia Xi, Jianghao Lin, Yongzhao Xiao, Zheli Zhou, Rong Shan, Te Gao, Jiachen Zhu, Weiwen Liu, Yong Yu, and Weinan Zhang. **A Survey of Large Language Model-Based Search Agents.** ACL 2026, *Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)*, pp. 8244–8279, DOI: 10.18653/v1/2026.acl-long.374. Related verified sources:

- https://aclanthology.org/2026.acl-long.374/
- https://aclanthology.org/2026.acl-long.374.pdf
- https://arxiv.org/abs/2508.05668
- https://arxiv.org/html/2508.05668v3

### [S2] Lin et al.

Minhua Lin, Zongyu Wu, Zhichao Xu, Hui Liu, Xianfeng Tang, Qi He, Charu Aggarwal, Hui Liu, Xiang Zhang, and Suhang Wang. **A Comprehensive Survey on Reinforcement Learning-based Agentic Search.** arXiv:2510.16724, 2025. Related verified sources:

- https://arxiv.org/abs/2510.16724v2
- https://arxiv.org/html/2510.16724v2
- https://export.arxiv.org/api/query?id_list=2510.16724

### [S3] Zhang et al.

Wenlin Zhang, Xiaopeng Li, Yingyi Zhang, Pengyue Jia, Yichao Wang, Huifeng Guo, Yong Liu, and Xiangyu Zhao. **Deep Research: A Survey of Autonomous Research Agents.** arXiv:2508.12752, 2025. Related verified sources:

- https://arxiv.org/abs/2508.12752
- https://arxiv.org/html/2508.12752v1
- https://arxiv.org/pdf/2508.12752

### [S4] Li et al.

Wenjun Li, Zhi Chen, Jingru Lin, Hannan Cao, Wei Han, Sheng Liang, Zhi Zhang, Kuicai Dong, Dexun Li, Chen Zhang, and Yong Liu. **Reinforcement Learning Foundations for Deep Research Systems: A Survey.** arXiv:2509.06733, 2025. Related verified sources:

- https://arxiv.org/abs/2509.06733
- https://arxiv.org/html/2509.06733v2
- https://doi.org/10.48550/arXiv.2509.06733

### [S5] Mishra et al.

Saroj Mishra, Suman Niroula, Umesh Yadav, Dilip Thakur, Srijan Gyawali, and Shiva Gaire. **SoK: Agentic Retrieval-Augmented Generation (RAG): Taxonomy, Architectures, Evaluation, and Research Directions.** arXiv:2603.07379, 2026. Related verified sources:

- https://arxiv.org/abs/2603.07379
- https://arxiv.org/html/2603.07379v1
- https://doi.org/10.48550/arXiv.2603.07379

### [S6] Zhang et al.

Guibin Zhang, Hejia Geng, Xiaohang Yu, Zhenfei Yin, Zaibin Zhang, Zelin Tan, Heng Zhou, Zhongzhi Li, Xiangyuan Xue, Yijiang Li, Yifan Zhou, Yang Chen, Chen Zhang, Yutao Fan, Zihu Wang, Songtao Huang, Francisco Piedrahita-Velez, Yue Liao, Hongru Wang, Mengyue Yang, Heng Ji, Jun Wang, Shuicheng Yan, Philip Torr, and Lei Bai. **The Landscape of Agentic Reinforcement Learning for LLMs: A Survey.** arXiv:2509.02547; OpenReview/TMLR record, 2026. Related verified sources:

- https://arxiv.org/abs/2509.02547
- https://arxiv.org/html/2509.02547v5
- https://openreview.net/forum?id=RY19y2RI1O

### [S7] Zhang

Chenchen Zhang. **From Reasoning to Agentic: Credit Assignment in Reinforcement Learning for Large Language Models.** arXiv:2604.09459 v3, 2026. Related verified sources:

- https://arxiv.org/abs/2604.09459v3
- https://arxiv.org/html/2604.09459v3
- https://arxiv.org/pdf/2604.09459v3
