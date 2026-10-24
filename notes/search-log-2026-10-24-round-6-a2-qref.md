# Search Log — Round 6A Exception — QREF

## Why this exception was checked

QREF was checked as the only targeted exception after the six-paper A2 map was closed. It is directly relevant because it trains a Query Refiner with SFT followed by PPO and constructs a retrieval-grounded reward by comparing an original query batch with a refined query batch. It also includes a separate Evidence Filter, so query-refiner gains must be separated from filtering gains.

## Verified identity and scope

- **Title:** QREF: Improving DeepSearch Agents by Aligning Search with Reasoning via Query Refinement and Evidence Filtering
- **Authors:** Yujia Chen et al.
- **Version:** arXiv:2610.21512 v1, submitted 15 October 2026
- **Primary role:** A2 Capability Learning
- **Secondary role:** Evidence filtering and ineffective-retrieval reduction
- **Primary capability:** Reasoning-conditioned Query Refinement with complementary batch coverage
- **Backbones:** WebSailor-32B and TongyiDeepResearch
- **Search stack:** Serper search API plus Jina Reader in the reported implementation
- **Benchmarks:** BrowseComp, BrowseComp-ZH, GAIA, and HLE

## Training signal

The Query Refiner first receives supervised initialization. PPO then operates on the InfoSeek dataset and uses approximately 100,000 queries. At each search step, the refiner receives the original question, reasoning context, and candidate query batch, and produces a refined batch.

The main retrieval reward is entity-level. Expected entities are annotated for the current reasoning step. The original and refined query batches are both executed against the search engine. Each expected entity is labeled as recovered, preserved, lost, or missed according to top-10 snippet matches. The weighted reward assigns positive weight to recovered and preserved entities, a strong penalty to lost entities, and a smaller penalty to missed entities. Empty or malformed query batches receive a hard penalty.

This is a meaningful improvement over a pure final-answer reward because it directly compares the retrieval consequences of the original and refined query batches. However, entity-string hits in snippets remain a retrieval-coverage proxy. They do not fully establish semantic intent preservation, evidence reliability, or general query quality.

## Query Refiner versus Evidence Filter

The paper evaluates Query Refiner and Evidence Filter as separate components. Query Refiner-only ablations improve both DeepSearch backbones, while the full system is stronger than either component alone on the reported settings. Therefore QREF should not be coded as an Evidence Filtering paper only. Its core A2 evidence comes from the learned query-refinement module and its original-versus-refined batch reward.

The Evidence Filter retains evidence relevant to both the current search batch and the reasoning objective. Its improvement should be recorded as a separate secondary contribution. Final Pass@1 improvements cannot be attributed entirely to query formulation.

## Generalization interpretation

QREF transfers across two Agent Backbones and four challenging benchmarks. This is useful evidence for **Agent Backbone Transfer** and task/benchmark transfer. It is not evidence for cross-Retriever transfer because both backbones use the same web-search stack. The paper does not test a train-on-retriever-A/test-on-retriever-B setting, an unseen tool/API, a dynamic corpus shift, a budget shift, or a language transfer beyond the specific English/Chinese benchmark pair.

A2 generalization should now be separated into:

1. **Semantic Intent Preservation:** whether the query continues to express the real task need;
2. **Backend Adaptation:** whether the query adapts to a particular retriever’s dialect;
3. **Transferable Query Learning:** whether intent is preserved and expression can be adapted in a new environment without complete retraining.

QREF provides evidence for learned evidence-coverage improvement and Agent Backbone Transfer. It does not establish the third property and does not independently measure the first.

## Query-only intervention requirement

The recommended future test must hold the task, answer generator, Retriever, Corpus, Top-k, and reasoning/search budget fixed. The changed query should alter retrieval results naturally. Evaluation should separately report Semantic Intent Preservation, retrieval utility, evidence coverage, and downstream answer change. Fixed-evidence tests can diagnose answer dependence, but they should not replace natural query-to-retrieval intervention.

## Decision

Include QREF as the seventh A2 Core paper and close broad A2 collection. Do not reopen a general Query-RL crawl. Add further A2 papers only if they provide independent semantic intent supervision, query-only causal intervention, train-on-retriever-A/test-on-retriever-B transfer, unseen tool or dynamic-environment validation, or operation-level credit assignment that materially changes the current conclusion.

## Sources

- https://arxiv.org/abs/2610.21512
- https://arxiv.org/html/2610.21512
- https://arxiv.org/pdf/2610.21512
