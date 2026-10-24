# A2 — Information Need Formulation: First-round Search Plan

## Central question

> **Can LLMs learn to formulate effective information needs and search queries that generalize across tasks and search environments?**

A2 is about expressing and structuring the information need: query generation, query reformulation, sub-question decomposition, search-plan construction, and selecting what to ask the information source. It is not primarily about deciding whether to search, adapting after failed feedback, or deciding when to stop.

## Boundary with A1 and A6

- **A1 Epistemic Awareness:** should the model seek external information, and is information acquisition worth its cost?
- **A2 Information Need Formulation:** given a need or knowledge gap, what query, sub-question, decomposition, or search plan should the model express?
- **A6 Adaptive Search Control:** after observing retrieval or tool feedback, should the model modify the query, continue, verify, retry, switch tools, or stop?

A paper may touch multiple capabilities, but its primary label should follow the learned or evaluated behavior rather than the overall system name. Query rewriting driven only by a fixed heuristic belongs to background unless it helps isolate a learned A2 capability.

## First-round questions

1. **Query Learning:** what does SFT, RL, preference optimization, distillation, or process supervision teach the model to generate? Candidate targets include query relevance, coverage, specificity, decomposed sub-questions, entity linking, ambiguity resolution, and information-gain-oriented queries.
2. **Training Alignment:** does the supervision or reward measure query quality itself, retrieval gain, evidence usefulness, downstream answer reward, or only final answer correctness? Can the signal distinguish a good information need from a lucky query or a retriever-specific shortcut?
3. **Generalization:** does the learned Query Policy transfer across tasks, domains, retrievers, search interfaces, corpora, query languages, budgets, and information environments?

For A2, generalization should be separated into three properties rather than reduced to Retriever Independence:

- **Semantic Intent Preservation:** the query continues to express the information actually needed by the task;
- **Backend Adaptation:** the policy adjusts expression to the lexical, structural, and ranking preferences of a particular retriever or interface;
- **Transferable Query Learning:** the model preserves intent in a new environment and can adapt expression without relearning the entire policy.

## Required coding fields

Every candidate should record:

- **Information Need Target:** missing fact, entity, relation, sub-question, hypothesis, evidence set, or search plan;
- **Query Action:** rewrite, expand, decompose, diversify, clarify, rank sub-questions, or formulate a new query;
- **Training Signal:** query labels, relevance labels, retrieval gain, evidence utility, answer reward, preference, process trace, or self-generated signal;
- **Training–Capability Alignment:** whether the signal directly supervises the query or only provides downstream proxy feedback;
- **Retriever Dependence:** fixed retriever, retriever-aware training, retriever transfer, or retriever-free evaluation;
- **Query Reliance:** whether changing the query changes the retrieved evidence in a controlled and useful way;
- **Generalization Evidence:** task, domain, retriever, tool, environment, language, and budget transfer;
- **Shortcut Risk:** memorized entities, benchmark-specific templates, retriever exploitation, query length, or answer leakage;
- **A2/A6 Boundary:** whether query generation is the capability being learned, or merely one action inside an adaptive search policy.

## Inclusion rule

Include a paper in the core map only if it helps define, train, measure, or bound a learned information-need formulation capability. Exclude generic RAG query rewriting, fixed prompt recipes, and retriever-only improvements unless they contain a clear A2 capability isolation or a useful training-alignment/generalization result.

## First-round size and stop rule

Start with 6–8 high-discrimination papers across query-learning SFT, query-learning RL, multi-query or sub-question decomposition, and retriever/environment transfer. Stop the first round when every major training signal and transfer condition has at least one representative, or when new papers repeat an existing capability–signal–evaluation pattern without adding a new generalization or alignment issue.

The next stage after A2 is A3 Information Space Exploration, followed by A6 Adaptive Search Control. The survey should preserve one common analysis axis throughout:

> **Training Signal → Learned Information-Seeking Capability → Generalization Evidence**

## First-round result

The first candidate-discovery round retained six core papers: single-step query rewriting, direct retrieval-gain RL, multi-query expansion, parallel sub-question decomposition, step-wise query/evidence credit, and retriever-aware query formulation. A targeted exception, **QREF**, was then verified and added as A2 Core because it combines SFT+PPO with an entity-level comparison between original and refined query batches. LESER is recorded as a domain-specific e-commerce supporting candidate; WebDancer is recorded as an A6-dominant supporting contrast.

The round is closed for broad candidate collection. The current evidence supports learnable query behaviors, but not a broadly transferable Query Policy. Most papers lack query-only causal interventions, independent semantic query-quality labels, train-on-retriever-A/test-on-retriever-B evaluation, unseen-tool or dynamic-environment transfer, and budget or language transfer. Cross-Agent-Backbone transfer, as in QREF, must not be labeled cross-Retriever transfer.

The required query-only intervention should hold the task, answer generator, Retriever, Corpus, Top-k, and reasoning/search budget fixed. The query may change the retrieved results naturally; the evaluation should separately measure Semantic Intent Preservation, retrieval utility, evidence coverage, and downstream answer change.

The detailed map is in [`papers/a2-information-need-formulation.md`](../papers/a2-information-need-formulation.md), and the verification record is in [`notes/search-log-2026-10-24-round-6-a2-first-batch.md`](./search-log-2026-10-24-round-6-a2-first-batch.md). The next capability stage is A3 Information Space Exploration.
