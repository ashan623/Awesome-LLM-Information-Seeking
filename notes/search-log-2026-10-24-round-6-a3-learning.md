# Search Log — Round 6B — A3 Targeted Learning Check

## Scope

This pass verified exactly four user-specified candidates and did not expand to generic Tool Learning or Browser Agent literature. The checks were: whether exploration is directly rewarded, whether exploration is independently measurable, and whether behavior transfers across Retriever, Tool, Corpus, or information-space changes.

## Results

### MARAG-R1 — Core A3 learning evidence

MARAG-R1 equips the policy with semantic retrieval, keyword retrieval, document filtering, and aggregation tools. It uses GPT-4-generated/rejection-filtered trajectories for SFT and RLOO RL. The reward combines answer F1, document-ID F1 against gold supporting documents, and a tool-call reward relative to expert trajectory length.

The document-coverage reward is direct evidence for useful evidence acquisition: it rewards necessary supporting-document recovery and penalizes irrelevant retrieval. Tool ablations and reward ablations support the role of multi-tool coordination. However, the tool inventory is predefined and fixed, and the tool-call reward measures amount/efficiency rather than novelty or source discovery. Unseen 2WikiMultiHopQA and HotpotQA provide task transfer, but not unseen-tool, unseen-retriever, or unseen information-structure transfer.

**Decision:** Include Core for A3.1/A3.2, with Medium capability isolation and Limited unseen-environment transfer.

Sources: [arXiv](https://arxiv.org/abs/2510.27569), [HTML](https://arxiv.org/html/2510.27569v1).

### DDPO — Core A3 learning evidence

Restoring Exploration via Diversity-Driven Policy Optimization in Search-Based LLM Agents is an online/early-access article in Information Processing & Management, Volume 64 Issue 2 Part A, article 105104, with issue metadata dated February 2027. It trains search agents with GRPO and an adaptive-gated log-determinant reward over query semantic diversity and retrieved-document coverage diversity.

The paper explicitly distinguishes pseudo-exploration from meaningful retrieval diversity. It evaluates seven single- and multi-hop QA benchmarks and reports search-step count, diversity/coverage signals, and Gold Passage Recall in addition to EM. The reported 7B model reaches 52.1% average EM; NQ reaches 44.8% EM with 1.01 average search steps, and MuSiQue Gold Passage Recall improves by 6.3 points.

DDPO is the strongest direct A3.2 paper in this pass. Nevertheless, Query Diversity, Evidence Diversity, Information Gain, and Useful Coverage are not equivalent. The available evaluation does not establish counterfactual source removal or systematic transfer to unseen Retriever, Corpus, Tool, or source inventory conditions.

**Decision:** Include Core for A3.2, with Medium capability isolation and Limited environment transfer. Record the publication status as online/early access with a 2027 issue date.

Sources: [ScienceDirect abstract](https://www.sciencedirect.com/science/article/abs/pii/S0306457326004942), [full article](https://www.sciencedirect.com/science/article/pii/S0306457326004942).

### Training Small Language Models for Transferable Tool Selection — Boundary contrast

This 22 October 2026 arXiv preprint trains Qwen3 1.7B/4B/8B models with SFT followed by RL for selecting gold API tool sets in ToolBench G3. It reports transfer to ToolBench G2, Gorilla, CRAFT, and TaskBench and swaps BM25 for COLT at inference without additional training. This is strong transfer evidence across tool corpora, domains, and retrievers.

The scope mismatch is decisive: the target is functional API selection, not acquisition of task-relevant information. Exact-match selection of a gold tool set does not measure execution, evidence coverage, unresolved information regions, or useful new information. It is therefore a valuable transfer baseline for A3.1-adjacent behavior, but not core A3 information-space exploration.

**Decision:** Include as boundary contrast, not core.

Sources: [arXiv abstract](https://arxiv.org/abs/2610.31483), [HTML](https://arxiv.org/html/2610.31483).

### ToolSearcher — Boundary contrast

ToolSearcher is a September 2026 arXiv preprint for large-scale tool-repository search. It uses category-constrained tool discrimination, event-level search modeling, and trajectory-aligned credit allocation to optimize discovery of target tools during multi-turn search. Its ablations replace event-level rewards with trajectory-level final-outcome supervision.

This is valuable evidence that event-level credit can train search over a large candidate space. But the explored space is a tool repository and the target is a gold tool set. The available evidence does not show that selected tools are executed to acquire new information, nor that the trajectory covers complementary evidence or resolves a knowledge gap. It is general tool discovery rather than information-space exploration.

**Decision:** Include as boundary contrast, not core A3.

Sources: [arXiv abstract](https://arxiv.org/abs/2609.30906), [HTML](https://arxiv.org/html/2609.30906), [code/data](https://github.com/zhenlongDai/ToolSearcher).

## Cross-paper synthesis

The four candidates produce a useful hierarchy:

- **Query Diversity:** DDPO’s semantic query diversity;
- **Evidence Diversity:** DDPO’s retrieved-document coverage diversity;
- **Information Gain:** the intended novelty/coverage improvement, only partially captured by the above proxies;
- **Useful Coverage:** MARAG-R1’s gold-document coverage reward;
- **Tool Discovery:** the target-tool discovery objective in the two boundary papers.

These must not be collapsed. More diverse queries can retrieve redundant or irrelevant evidence. Selecting the correct API can be transferable without proving that the agent acquired new information. Document-ID coverage can be precise but depends on gold support-document annotations and a fixed tool family.

## Final decision

A3 should not be merged entirely into A2/A6 because source selection and unresolved-region coverage are distinct from query wording and feedback control. However, A3 should not yet become a full independent learning chapter: only MARAG-R1 and DDPO directly optimize information-space exploration, while the two stronger transfer papers target general tool selection.

For the current survey, retain A3 as a **cross-capability dimension with a dedicated subsection**, and compare it across A1–A6 using the common Training Signal → Learned Capability → Generalization matrix. Stop broad A3 collection. Proceed to A6 Adaptive Search Control.

## Sources

- https://arxiv.org/abs/2510.27569
- https://www.sciencedirect.com/science/article/pii/S0306457326004942
- https://arxiv.org/abs/2610.31483
- https://arxiv.org/abs/2609.30906
