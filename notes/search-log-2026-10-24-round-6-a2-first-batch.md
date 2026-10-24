# Search Log — Round 6A — A2 Information Need Formulation

## Fixed scope

A2 studies whether LLMs can learn to express and structure information needs through query generation, query reformulation, query expansion, sub-question decomposition, and search-plan construction.

A1 concerns whether information acquisition is necessary or worthwhile. A6 concerns feedback-driven continuation, query modification, verification, retry, tool switching, and stopping. A query emitted inside an adaptive loop is not automatically an A2 contribution; the primary label follows the capability isolated by the training signal and evaluation.

## Six core papers

1. **Query Rewriting in Retrieval-Augmented Large Language Models** — EMNLP 2023. Historical SFT+PPO single-step query-rewriting anchor; query policy is measured through a fixed Bing/BM25 reader pipeline.
2. **DeepRetrieval: Hacking Real Search Engines and Retrievers with Large Language Models via Reinforcement Learning** — COLM 2025. Direct PPO retrieval-gain query learning across medical APIs, BM25/dense retrieval, evidence seeking, and SQL; strong in-environment evidence with backend exploitation and answer-injection risks.
3. **Beyond the limitation of a single query: Train your LLM for query expansion with Reinforcement Learning** — arXiv 2025 / ICLR 2026 submission. Multi-query expansion learned mainly from final answer EM; indispensable low-alignment contrast.
4. **ParallelSearch: Train your LLMs to Decompose Query and Search Sub-queries in Parallel with Reinforcement Learning** — arXiv 2025. Parallel sub-question decomposition and search-plan construction; decomposition is directly rewarded, but individual query quality is not.
5. **StepSearch: Igniting LLMs Search Ability via Step-Wise Proximal Policy Optimization** — EMNLP 2025. Gold search-key overlap, support-document information gain, redundancy penalty, and answer reward provide the closest first-batch example of query/evidence process credit.
6. **Understanding the Behaviors of Environment-aware Information Retrieval** — ACL 2026 Main Long Paper. GRPO optimizes retriever-specific nDCG and explicitly shows that query dialects may not transfer across retrievers; the strongest negative environment-generalization anchor.

## Supporting/excluded candidates

- **LESER: Learning to Expand via Search Engine-feedback Reinforcement in e-Commerce** is a valuable domain-specific supporting candidate. It uses SFT distillation and GRPO with production search relevance and unique-item coverage rewards, but it is tied to one proprietary e-commerce environment and lacks cross-domain/retriever transfer and query-reliance controls. It is not in the core six.
- **WebDancer: Towards Autonomous Information Seeking Agency** is an A6-dominant supporting contrast. It trains an end-to-end web-agent policy with trajectory SFT/RFT and DAPO; query is only one action parameter and there is no standalone query-quality or retrieval-gain objective.

## Cross-paper findings

### Measurement

No paper provides an unambiguous human-annotated standard for the best expression of an information need. Pseudo-queries, GPT-generated search keys, benchmark-derived decomposition labels, and passage relevance are all useful proxies but can be disconnected from semantic completeness and user intent.

Query-only causal evaluation is absent. The papers do not jointly provide fixed-task, fixed-retriever, fixed-evidence-budget query replacement/deletion, semantic-perturbation, query-intent, evidence-coverage, and downstream-answer measurements. Therefore final QA improvement and retrieval gain must not be treated as sufficient evidence of query capability.

### Learning

SFT/PPO/GRPO can learn single-query rewriting, multi-query expansion, parallel sub-question decomposition, retriever-aware query style, and query/evidence process behavior. The signal alignment differs sharply:

- Query Rewriting: pseudo-query labels plus reader EM/F1 and answer-hit;
- DeepRetrieval and Environment-aware IR: direct retrieval utility such as Recall/NDCG, but backend-conditioned;
- StepSearch: query-key overlap plus information gain and redundancy signals, but GPT-generated and lexical proxies;
- ExpandSearch: mainly final answer EM and format reward;
- ParallelSearch: answer EM plus decomposition/search-count labels, without per-query relevance or coverage.

The closer the signal is to final answer reward, the harder it is to attribute gains to query quality. The closer it is to backend retrieval gain, the greater the risk of retriever-specific exploitation.

### Generalization

Task and benchmark transfer is relatively common. Transfer to unseen retrievers, tools, APIs, dynamic corpora, budgets, or languages is rare. Environment-aware IR provides the most important counterexample to broad generalization: a query policy trained for one retriever may learn a lexical or structural dialect that fails on another. DeepRetrieval covers multiple backends but mostly trains and evaluates separately within each environment.

## First-round closure

The six-paper core map is sufficient for the first A2 candidate-discovery round. Do not add minor Query-RL variants unless they provide one of the missing conditions: query-only causal intervention, human/independent query-quality supervision, train-on-retriever-A/test-on-retriever-B transfer, unseen tool/API, dynamic corpus, budget transfer, or cross-language behavior transfer.

A2 is closed for this first candidate round, not closed as a research question. The next capability axis is A3 Information Space Exploration; A6 follows with explicit feedback-driven search control, verification, retry, stopping, and cost trade-offs.
