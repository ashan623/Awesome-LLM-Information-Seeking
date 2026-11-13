# A2 — Information Need Formulation

## Central question

> **Can LLMs learn to formulate effective information needs and search queries that generalize across tasks and search environments?**

A2 studies the expression and structuring of an information need: query generation, query reformulation, query expansion, sub-question decomposition, parallel search-plan construction, and query-level evidence seeking. A1 asks whether information acquisition is necessary or worthwhile. A6 asks what the policy should do after observing search/tool feedback: continue, modify, verify, retry, switch tools, or stop.

A query generated inside an adaptive search loop is not automatically an A2 contribution. The paper must isolate, train, or measure query formulation as a meaningful behavior. Final-answer improvement alone is not enough to establish query quality.

## Coding dimensions

Every entry is evaluated along the common survey axis:

> **Training Signal → Learned Query Capability → Generalization Evidence**

The A2-specific fields are:

- **Information Need Target:** missing fact, entity, relation, sub-question, evidence set, or search plan;
- **Query Action:** rewrite, expand, decompose, diversify, clarify, rank sub-questions, or formulate a new query;
- **Training Signal:** query labels, relevance labels, retrieval gain, evidence utility, answer reward, preference, process trace, or self-generated signal;
- **Training–Capability Alignment:** whether the signal directly supervises the query or only provides downstream proxy feedback;
- **Retriever Dependence:** fixed retriever, retriever-aware training, retriever transfer, or retriever-free evaluation;
- **Query Reliance:** whether controlled changes to the query change retrieved evidence and downstream behavior in a useful way;
- **Generalization Evidence:** task, domain, retriever, tool, environment, language, and budget transfer;
- **Shortcut Risk:** memorized entities, benchmark-specific templates, retriever exploitation, query length, query count, or answer leakage;
- **A2/A6 Boundary:** whether query generation is the learned capability or merely one action inside an adaptive search policy.

---

## Core map

### A2-1. Query Rewriting in Retrieval-Augmented Large Language Models

- **Authors / year:** Xinbei Ma, Yeyun Gong, Pengcheng He, Hai Zhao, Nan Duan, 2023.
- **Venue / links:** EMNLP 2023; [ACL Anthology](https://aclanthology.org/2023.emnlp-main.322/) · [arXiv](https://arxiv.org/abs/2305.14283) · [code](https://github.com/xbmxb/RAG-query-rewriting)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Single-step query rewriting and reformulation.
- **Secondary capabilities:** Multi-query generation for multi-hop questions; query–retriever–reader alignment; limited “no retrieval” behavior in PopQA.
- **Information need target:** The knowledge required by an open-domain QA or MMLU task, expressed as a short keyword/phrase query suitable for Bing retrieval.
- **Query action:** A trainable T5-large rewriter generates one or more queries before one retrieval call. It does not observe retrieved documents while generating the query and does not learn continuation, retry, verification, tool switching, or stopping.
- **Training mechanism:** ChatGPT-generated pseudo-query SFT warm-up followed by PPO with KL regularization.
- **Learning target:** Maximize frozen-reader answer utility and answer-hit reward after fixed Bing retrieval. The target is not an independently annotated ideal query.
- **Training signal:** Pseudo-query sequence likelihood; then `EM + F1 + answer-hit` reward, with answer-hit based on whether retrieved documents contain the gold answer.
- **Training–capability alignment:** Medium. The policy directly generates the query and receives retrieval feedback, but final reader EM/F1 and gold-answer hit mix query quality with retriever and reader behavior.
- **Retriever dependence:** Strong. The main pipeline is tied to Bing snippets or Bing URL retrieval followed by BM25 passage selection.
- **Query reliance:** Limited. The paper reports rewriter and retrieval-hit comparisons, but lacks query swap, deletion, fixed-evidence, length-matched, or counterfactual query interventions.
- **Generalization evidence:** Limited. It covers HotpotQA, AmbigNQ, PopQA, MMLU categories, and some reader settings, but not unseen retrievers, search APIs, dynamic environments, budgets, or languages.
- **Shortcut risk:** High. Fixed examples, semicolon-separated query format, EOS constraints, Bing/BM25 dependence, answer-hit reward, and possible option/answer copying in MMLU are not stress-tested.
- **Method effectiveness:** Medium.
- **Capability isolation:** Limited.
- **Generalization evidence:** Limited.
- **A2/A6 boundary:** A2 Core; retrieval feedback is compressed into training reward, not an adaptive search-control loop.
- **Main finding:** This is a useful historical anchor showing that a separately trained query policy can improve a fixed RAG pipeline. It also makes the central limitation visible: the query capability is measured indirectly through a specific reader–retriever stack.
- **Decision:** Include Core.

### A2-2. DeepRetrieval: Hacking Real Search Engines and Retrievers with Large Language Models via Reinforcement Learning

- **Authors / year:** Pengcheng Jiang, Jiacheng Lin, Lang Cao, Runchu Tian, SeongKu Kang, Zifeng Wang, Jimeng Sun, Jiawei Han, 2025.
- **Venue / links:** COLM 2025; [arXiv](https://arxiv.org/abs/2503.00223) · [OpenReview](https://openreview.net/forum?id=u9JXu4L17I) · [code](https://github.com/pat-jj/DeepRetrieval)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Direct RL query generation and query reformulation for multiple retrieval interfaces.
- **Secondary capabilities:** Structured Boolean/field queries; text-to-SQL query formulation; backend-aware query adaptation.
- **Information need target:** The original natural-language need, PICO-style medical request, evidence-seeking target, classic IR target, or database question, transformed into a query optimized for the target backend.
- **Query action:** One enhanced query is generated and sent to one retriever or execution backend. It is not an adaptive continuation/stopping policy.
- **Training mechanism:** PPO with GAE, KL regularization, entropy reward, and structured `<think>/<answer>` outputs.
- **Learning target:** Maximize task-specific retrieval or execution return for the generated query without requiring a reference query.
- **Training signal:** PubMed/ClinicalTrials recall, evidence answer-span rank or hit, BM25/dense NDCG, SQL execution accuracy, syntax/format reward, and backend success signals.
- **Training–capability alignment:** Medium. The rewards directly measure backend utility, but they do not directly measure semantic information-need quality, intent preservation, or unknown-backend robustness.
- **Retriever dependence:** Medium to strong. The paper covers PubMed and ClinicalTrials APIs, BM25, E5, BGE, Contriever, and SQL, but mostly trains and evaluates within separately bound backend environments. Train-on-A/test-on-B transfer is not established.
- **Query reliance:** Partial. Reasoning ablations, answer-injection cleaning, and backend results show query effects, but no systematic fixed-evidence query-only causal test is reported.
- **Generalization evidence:** Medium. It spans medical, Wikipedia, fact-checking, biomedical, MS-MARCO, and SQL tasks and several retrievers, but not unseen backends, tools, budgets, dynamic environments, or languages.
- **Shortcut risk:** High. Backend syntax, IDF/field preferences, query length, answer injection, and format reward can substitute for general information-need understanding. The paper’s answer-injection cleaning analysis is especially important because it shows that raw evidence-seeking reward can be inflated by inserting an answer into the query.
- **Method effectiveness:** Strong.
- **Capability isolation:** Medium.
- **Generalization evidence:** Medium.
- **A2/A6 boundary:** A2 Core; the main action is pre-retrieval q→q′, not feedback-driven search control.
- **Main finding:** DeepRetrieval is the strongest example in this pool of direct retrieval-gain reward for query learning. It demonstrates that LLMs can learn backend-specific query behavior, but multi-backend coverage should not be confused with cross-backend policy transfer.
- **Decision:** Include Core.

### A2-3. Beyond the limitation of a single query: Train your LLM for query expansion with Reinforcement Learning

- **Authors / year:** Shu Zhao, Tan Yu, Anbang Xu, 2025.
- **Venue / links:** [arXiv](https://arxiv.org/abs/2510.10009) · [project](https://shuzhao.me/ExpandSearchProject/) · [code](https://github.com/Tree-Shu-Zhao/ferret/tree/main/recipe/expand_search)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** RL-trained multi-query expansion and reformulation.
- **Secondary capabilities:** Sub-question-like decomposition, iterative search, retrieval-context compression, and answer generation.
- **Information need target:** A compound or multi-hop QA need whose relevant evidence may be missed by one surface formulation.
- **Query action:** The policy emits multiple syntax, semantic, keyword, paraphrase, and decomposed sub-question queries in one search block. A frozen squeezer compresses the returned evidence.
- **Training mechanism:** PPO-style end-to-end multi-turn RL in a Search-R1-like environment.
- **Learning target:** Generate valid multi-query search calls and use compressed evidence to answer correctly.
- **Training signal:** Final answer EM plus format reward. The released training configuration sets retrieval correctness reward to zero; there is no query-level relevance or recall reward.
- **Training–capability alignment:** Limited. The paper provides a clear behavioral target, but query quality is inferred from final QA and is confounded by search count, squeezer behavior, reasoning, and answer generation.
- **Retriever dependence:** Strong. The experiments use fixed E5 retrieval over a 2018 Wikipedia dump and a fixed squeezer.
- **Query reliance:** Limited. Expansion count, type ablations, squeezer ablations, and retrieval-depth changes are useful, but no query-only causal intervention is performed.
- **Generalization evidence:** Medium for task OOD, limited for environment OOD. It evaluates NQ/HotpotQA training and TriviaQA, PopQA, 2WikiMultiHopQA, MuSiQue, and Bamboogle, but no unseen retriever, tool, corpus, language, or systematic budget transfer.
- **Shortcut risk:** Medium–High. Final EM may reward more search, query count, squeezer shortcuts, fixed E5/Wikipedia patterns, query templates, and answer leakage.
- **Method effectiveness:** Medium.
- **Capability isolation:** Limited.
- **Generalization evidence:** Medium for task transfer; Limited for tool/environment transfer.
- **A2/A6 boundary:** A2 Core with A6 secondary. Multi-query expansion is explicit, but the end-to-end loop also learns search continuation and answer behavior.
- **Main finding:** ExpandSearch is a useful low-alignment contrast: RL can induce multi-query behavior even without a query-level reward, but final-answer reward does not prove that the model learned better information-need formulations.
- **Decision:** Include Core as an indispensable contrast.

### A2-4. ParallelSearch: Train your LLMs to Decompose Query and Search Sub-queries in Parallel with Reinforcement Learning

- **Authors / year:** Shu Zhao, Tan Yu, Anbang Xu, Japinder Singh, Aaditya Shukla, Rama Akkiraju, 2025.
- **Venue / links:** [arXiv](https://arxiv.org/abs/2508.09303) · [project](https://shuzhao.me/ParallelSearchProject/) · [DBLP record](https://dblp.org/rec/journals/corr/abs-2508-09303.html)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Parallel sub-question decomposition and parallel query construction.
- **Secondary capabilities:** Search-plan construction, parallel tool invocation, search-call efficiency control.
- **Information need target:** Independent facts required by comparison or parallelizable multi-hop questions.
- **Query action:** The model emits multiple `##`-separated subqueries in one search action. The retriever executes them in parallel and aggregates the returned evidence.
- **Training mechanism:** PPO/RLVR with answer, decomposition, search-count, and format rewards.
- **Learning target:** Recognize when a problem is parallelizable, generate independent subqueries, and avoid forcing parallelization on sequential problems.
- **Training signal:** Final answer EM; benchmark-derived parallelization/decomposition labels; search-count reward; format reward. No direct relevance, coverage, independence, or information-gain labels for each query.
- **Training–capability alignment:** Medium for the decomposition decision, limited for the query text itself. The benchmark label directly supervises whether to split, but not whether each subquery expresses a useful information need.
- **Retriever dependence:** High. The main setting uses E5 over a 2018 Wikipedia dump, with fixed top-k; no cross-retriever or live-tool evaluation.
- **Query reliance:** Not Tested. There is no query deletion, shuffle, substitution, or fixed-evidence comparison.
- **Generalization evidence:** Medium for task and problem-structure transfer; limited for retriever/tool/environment transfer. It evaluates NQ, HotpotQA, TriviaQA, PopQA, 2WikiMultiHopQA, MuSiQue, Bamboogle, and MultihopRAG, and compares parallel versus sequential subsets.
- **Shortcut risk:** Medium–High. Separators, decomposition labels, search-count reward, query count, template compliance, and final EM can replace true subquery quality. Query length and lexical diversity are not independently evaluated.
- **Method effectiveness:** Strong for end-task efficiency and answer performance.
- **Capability isolation:** Limited.
- **Generalization evidence:** Medium for task structure; Limited for environment transfer.
- **A2/A6 boundary:** A2 Core with A6 secondary. Parallel decomposition is explicit, but search count and execution control are also optimized.
- **Main finding:** ParallelSearch is a high-discrimination example of structured information-need formulation. It teaches *which independent needs to express* more clearly than generic query rewriting, but it does not prove that the generated subqueries are individually high quality.
- **Decision:** Include Core.

### A2-5. StepSearch: Igniting LLMs Search Ability via Step-Wise Proximal Policy Optimization

- **Authors / year:** Xuhui Zheng, Kang An, Ziliang Wang, Yuhang Wang, Yichao Wu, 2025.
- **Venue / links:** EMNLP 2025 Main; [ACL Anthology](https://aclanthology.org/2025.emnlp-main.1106/) · [arXiv](https://arxiv.org/abs/2505.15107) · [code](https://github.com/Zillwang/StepSearch)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Sub-question-aligned query generation with process-level query/evidence reward.
- **Secondary capabilities:** Feedback-conditioned query refinement and information-gain-oriented search actions; these overlap with A6 but are not the primary label.
- **Information need target:** The current unresolved hop or sub-question in a multi-hop task, represented through gold search keys and supporting documents.
- **Query action:** The model generates a search-key query, observes retrieved evidence, and may continue, reformulate, or answer.
- **Training mechanism:** StePPO, a PPO variant with step-wise reward, answer reward, query-key reward, information-gain reward, and redundancy penalty. MuSiQue-derived data are constructed with GPT-4o assistance.
- **Learning target:** An end-to-end search policy that formulates queries aligned with the current sub-question and retrieves novel supporting evidence.
- **Training signal:** Final answer F1; gold search-key token F1; marginal TF-IDF information gain against gold support documents; repeated-document penalty; format reward.
- **Training–capability alignment:** Medium. It is the clearest example in this pool of process-level query/evidence alignment, but gold keys and TF-IDF support similarity are generated or defined proxies rather than independent human query-quality judgments.
- **Retriever dependence:** High. The main training setup uses E5, fixed local corpora, top-k=3, and max turns=5.
- **Query reliance:** Not Tested. Reward and ablation evidence do not amount to query replacement, query shuffle, or fixed-evidence causal intervention.
- **Generalization evidence:** Medium for task/model transfer. It trains from MuSiQue and evaluates HotpotQA, 2WikiMultiHopQA, Bamboogle, Qwen 3B/7B, and Base/Instruct variants. Cross-retriever, tool, budget, language, and dynamic-environment transfer are not tested.
- **Shortcut risk:** High. Gold-key lexical overlap, max-over-query aggregation, TF-IDF support similarity, fixed XML tags, top-k, max turns, and GPT-4o-generated keys/support documents can reward phrase copying, corpus exploitation, and answer leakage.
- **Method effectiveness:** Medium.
- **Capability isolation:** Limited.
- **Generalization evidence:** Medium for task/model transfer; Limited for environment transfer.
- **A2/A6 boundary:** A2 Core with A6 secondary. The query/evidence process is trained jointly with continuation and stopping, but the distinctive contribution is step-level query/evidence credit.
- **Main finding:** StepSearch is the strongest representative candidate for moving beyond global answer reward toward query-level and process-level credit assignment. Its central unresolved issue is whether the reward measures semantic information need or only lexical overlap with generated keys and support documents.
- **Decision:** Include Core.

### A2-6. Understanding the Behaviors of Environment-aware Information Retrieval

- **Authors / year:** Ruifeng Yuan, Chaohao Yuan, David Dai, Yu Rong, Hong Cheng, Hou Pong Chan, Chenghao Xiao, 2026.
- **Venue / links:** ACL 2026 Main Long Paper; [ACL Anthology](https://aclanthology.org/2026.acl-long.2013/) · [arXiv](https://arxiv.org/abs/2606.16817) · [code](https://github.com/LCO-Embedding/Envs-aware-Information-Retrieval)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Retriever-aware query formulation and query rewriting.
- **Secondary capabilities:** Two-turn query refinement; environment-conditioned query style adaptation; cross-retriever transfer diagnosis.
- **Information need target:** The user’s underlying intent expressed in a query style that matches a target retriever and corpus.
- **Query action:** Single-turn rewrite; optional multi-query decomposition; in two-turn settings, a second rewrite conditioned on top-document snippets.
- **Training mechanism:** GRPO with black-box retriever feedback, format reward, and one- or two-turn branching rollouts.
- **Learning target:** Maximize target-retriever nDCG@10 for generated queries.
- **Training signal:** Ground-truth passage-ID nDCG@10 plus format reward; no final answer reward or human query-quality label.
- **Training–capability alignment:** Medium–Strong for retriever-side utility; Medium for general information-need formulation. The signal directly measures ranking utility for a specific retriever but not semantic sufficiency or environment-independent query quality.
- **Retriever dependence:** Strong and explicitly diagnosed. Experiments cover BM25, Contriever, MiniLM, and Qwen3-Embedding, but source-retriever policies often fail to transfer to target retrievers because query dialect and structure differ.
- **Query reliance:** Partial and indirect. Cross-retriever differences and no-rewrite baselines show query effects, but no query-only causal intervention is provided.
- **Generalization evidence:** Medium. It includes RAGBench, BEIR zero-shot, FinAgentBench, four retrievers, and cross-domain settings. The most important result is negative: cross-retriever transfer is unstable.
- **Shortcut risk:** Medium–High. BM25 rewards keywords, dense retrievers prefer different statement/question styles, formatting is rewarded, and fixed corpora/templates can support retriever-specific exploitation. HyDE-like document-style outputs may benefit from corpus vocabulary overlap.
- **Method effectiveness:** Strong.
- **Capability isolation:** Medium.
- **Generalization evidence:** Medium overall, with strong negative evidence for environment-independent transfer.
- **A2/A6 boundary:** A2 Core. Two-turn refinement is A6-adjacent, but the main contribution is query formulation conditioned on retriever environment, not stopping or tool control.
- **Main finding:** This paper is essential because it shows both learnability and non-generality. LLMs can learn retriever-specific query dialects, but a query policy that works for one retriever may not transfer to another. This is the clearest evidence in the current A2 set that query quality is environment-dependent.
- **Decision:** Include Core.

---

### A2-7. QREF: Improving DeepSearch Agents by Aligning Search with Reasoning via Query Refinement and Evidence Filtering

- **Authors / year:** Yujia Chen, Yuhui Wang, Junzhe Wang, Shihan Dou, Di Liang, Yujiong Shen, Yuhao Zhou, Zhiheng Xi, Ming Zhang, Senjie Jin, Jiayi Chen, Dingwei Zhu, Yajie Yang, Yuan Hua, Chenyao Lu, Tao Gui, Qi Zhang, Xuanjing Huang, 2026.
- **Venue / links:** arXiv:2610.21512, submitted 15 Oct 2026; [arXiv abstract](https://arxiv.org/abs/2610.21512) · [HTML paper](https://arxiv.org/html/2610.21512)
- **Role / contribution:** Core / Capability Learning
- **Primary capability:** Reasoning-conditioned query refinement with complementary batch coverage.
- **Secondary capabilities:** Evidence filtering and ineffective-query reduction; the filter is a separate Evidence Evaluation/Integration component, not part of the Query Policy itself.
- **Information need target:** The entities and evidence anchors required by the current reasoning step of a DeepSearch agent.
- **Query action:** Given the original question, current reasoning context, and a candidate query batch, the Query Refiner outputs a refined query batch. It compares original and refined batches against the same web-search environment and seeks complementary coverage.
- **Training mechanism:** SFT initialization followed by PPO in a real web-search environment. The Evidence Filter is a separate Qwen3-8B component.
- **Learning target:** Improve evidence coverage of the current reasoning step while preserving previously retrieved entities and reducing ineffective query batches.
- **Training signal:** Entity-level retrieval reward based on recovered, preserved, lost, and missed expected entities; invalid/empty-query penalty; SFT target behavior. The reward compares the refined batch against the original batch rather than relying only on final answer correctness.
- **Training–capability alignment:** Medium–Strong for retrieval coverage improvement; Medium for semantic intent preservation. Entity hits in top-10 snippets are a useful fine-grained proxy, but they do not fully measure whether the rewritten query preserves user intent or retrieves reliable evidence.
- **Retriever dependence:** Strong. Training and evaluation use a Serper web-search API and a fixed search stack. The experiments vary two DeepSearch agent backbones, WebSailor-32B and TongyiDeepResearch, but do not establish cross-Retriever transfer.
- **Query reliance:** Medium. The reward directly compares original and refined query batches, and Query Refiner-only ablations improve both backbones. However, there is no query-only test that holds retrieved evidence fixed, and entity-hit reward is snippet/entity matching rather than a semantic query-quality judgment.
- **Generalization evidence:** Medium for cross-agent-backbone transfer and four information-seeking benchmarks: BrowseComp, BrowseComp-ZH, GAIA, and HLE. This is not cross-Retriever transfer; both backbones use the same web-search stack.
- **Shortcut risk:** Medium–High. Entity string matching in snippets, query-batch size, Serper-specific ranking, top-10 retrieval, Evidence Filter interaction, and benchmark/entity leakage may improve reward without guaranteeing intent preservation.
- **Method effectiveness:** Strong.
- **Capability isolation:** Medium.
- **Generalization evidence:** Medium for cross-agent-backbone and benchmark transfer; Limited for retriever/tool/environment transfer.
- **A2/A6 boundary:** A2 Core with A6-adjacent iterative refinement. The module refines queries at each search step, but the paper does not learn stopping, retry, tool switching, or verification as independent controls.
- **Main finding:** QREF is an important exception because it directly compares original and refined query batches and uses a fine-grained retrieval-grounded reward. Its cross-backbone results should be recorded as **Agent Backbone Transfer**, not Retriever Transfer.
- **Main limitation:** The entity-level reward establishes improved evidence coverage in a fixed web-search stack, but not semantic intent preservation, query-independent evidence quality, or transfer to an unseen retriever/tool.
- **Decision:** Include Core as a targeted A2 exception.

---

## Supporting and excluded candidates

### LESER: Learning to Expand via Search Engine-feedback Reinforcement in e-Commerce

LESER is a valuable business-domain example: SFT from GPT-4.1 distillation followed by GRPO uses production search feedback to optimize query-set relevance and unique-item coverage. However, the paper is tied to one proprietary e-commerce search engine, one internal relevance model, one domain, and one deployment environment. It lacks cross-domain/retriever transfer and query-reliance interventions. It should remain outside the core six-paper A2 map unless the survey later adds a small domain-application appendix. Its correct role is a **domain-specific supporting candidate**, not general A2 evidence.

### WebDancer: Towards Autonomous Information Seeking Agency

WebDancer is a useful A6 contrast, not an A2 core paper. It trains an end-to-end web-agent policy through trajectory SFT/RFT and DAPO, with query, click/visit, and answer actions. Its main reward is final-answer correctness, and it does not isolate query quality, retrieval gain, or information-need formulation. It should be cited when discussing how query generation is embedded in a broader adaptive search policy, but not used as evidence that a standalone Query Policy was learned.

---

## Cross-paper synthesis

### Three distinct meanings of query generalization

The A2 map should not treat Retriever Independence as the only desirable endpoint. It separates three properties:

1. **Semantic Intent Preservation:** does the query express the information actually needed by the task, even when surface wording changes?
2. **Backend Adaptation:** can the policy adjust expression to the lexical, structural, and ranking preferences of a particular retriever or search interface?
3. **Transferable Query Learning:** can the model preserve the underlying intent in a new environment and rapidly adapt its expression without relearning the entire policy?

Environment-aware IR primarily measures Backend Adaptation and exposes weak cross-retriever transfer. QREF mainly measures evidence coverage within one web-search stack and transfers across Agent Backbones. Neither establishes Semantic Intent Preservation with an independent human or model-agnostic evaluation, and neither proves broad Transferable Query Learning.

### Training signal

The seven papers expose a useful signal hierarchy. Query Rewriting uses pseudo-query supervision filtered by frozen-reader correctness. StepSearch adds direct query-string overlap, support-document information gain, and redundancy penalties, making it the closest to process-level query/evidence credit in the original six-paper map. QREF adds an important comparison-based signal: recovered, preserved, lost, and missed expected entities when replacing an original query batch with a refined batch. DeepRetrieval and Environment-aware IR directly optimize retrieval utility such as Recall/NDCG or passage ranking. ExpandSearch and ParallelSearch rely more heavily on final-answer reward or decomposition/search-count proxies.

No paper provides an unambiguous human-annotated standard for “the best expression of an information need.” ChatGPT pseudo-queries, GPT-4o-generated search keys, benchmark-derived decomposition labels, and passage-level relevance all provide useful supervision, but each is a proxy. They can show that a query behavior is learnable without proving that it is semantically complete, user-aligned, or robust to a new information environment.

Retrieval gain should not be treated as query quality. DeepRetrieval and Environment-aware IR show that retrieval metrics can be optimized directly, but the learned policy may exploit backend syntax, lexical statistics, field structure, or corpus-specific query dialect. The negative cross-retriever transfer result in Environment-aware IR is therefore as important as its in-environment improvement.

### Learned capability

The targeted A2 map supports five learned behaviors: single-query reformulation, multi-query expansion, structured parallel sub-question decomposition, retriever-aware query formulation, and reasoning-conditioned query refinement with batch-level evidence coverage. StepSearch and QREF both move beyond a single final-answer reward, but they optimize different proxies: StepSearch uses lexical search-key and support-document signals, whereas QREF compares entity coverage before and after refinement.

These behaviors are not equivalent. A model that generates more queries has not necessarily learned a better information need. A model that decomposes a comparison question has not necessarily learned whether each subquery is relevant. A query that retrieves a relevant passage may still be semantically incomplete, answer-leaking, or useless under another retriever.

### Generalization

Task and benchmark transfer is relatively common. Environment transfer is not. DeepRetrieval covers several tasks and backends, but mostly as separately bound training environments. Environment-aware IR explicitly demonstrates that query styles learned for one retriever may fail on another. QREF transfers across two Agent Backbones, but both use the same web-search stack, so this is not Retriever Transfer. The remaining papers are largely tied to fixed E5/Wikipedia, Bing, or a proprietary backend.

There is almost no systematic transfer to unseen web tools, APIs, dynamic corpora, budgets, languages, or retriever mixtures. Existing “generalization” claims should therefore be coded carefully: a new QA benchmark or a new Agent Backbone is not sufficient evidence of a transferable Query Policy unless query behavior itself is measured in the new environment.

### A2–A6 boundary

Query Rewriting and DeepRetrieval are primarily pre-retrieval, one-shot query policies. ParallelSearch and ExpandSearch include search count, multi-turn retrieval, or compression behavior, so their query gains are partly entangled with A6. StepSearch is the clearest original A2–A6 boundary case because it directly rewards query/evidence alignment while also learning feedback-conditioned continuation. QREF is A2-primary because Query Refinement is a separately trained module, but its per-step refinement still touches A6-style feedback. WebDancer belongs primarily to A6 because its query is simply one parameter of a complete web-agent action policy.

## Cross-paper synthesis

The original six-paper core map plus the targeted QREF exception is sufficient for A2 candidate collection. It covers the main training signals and capability forms without reopening a broad Search-RL crawl.

The defensible stage conclusion is:

> **LLMs can learn useful query behaviors through SFT, PPO, GRPO, and process-oriented rewards, but current evidence mostly demonstrates retriever- or benchmark-conditioned utility rather than a broadly transferable ability to formulate information needs.**

The central unresolved problem is not whether query generation can be improved. It is whether a model can learn a query policy whose quality remains meaningful when the task, retriever, tool, corpus, budget, and feedback regime change.

The most important missing evaluation is a controlled query-only protocol: hold the task, answer generator, **Retriever, Corpus, Top-k, and reasoning/search budget fixed**, allow the retrieved results to change naturally with the query, and replace, delete, or semantically perturb only the query. Then separately measure Semantic Intent Preservation, retrieval utility, evidence coverage, and downstream answer change. This should be combined with train-on-retriever-A/test-on-retriever-B, unseen-tool/environment, and rapid-adaptation tests.

A2 is therefore closed for broad candidate discovery, with QREF recorded as the one targeted exception. It is not closed as a research question. The next major capability axis is A3 Information Space Exploration. A6 should be studied afterward with explicit focus on feedback-driven query modification, retry, verification, stopping, and cost control.
