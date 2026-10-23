# Search Log — Round 2A — A1 Search Necessity

## Scope

This sub-round searched only **A1 Epistemic Awareness / Search Necessity**. The target was not generic RAG improvement, but work that asks whether an LLM should acquire external information and how that decision is trained or evaluated.

The search intentionally excluded the existing seed papers Self-RAG, Search-R1, R1-Searcher, ZeroSearch, Behavior Priming, Agentic-R, and Rethinking Reasoning-Intensive Retrieval. It also avoided expanding into A4 Evidence Evaluation, Memory, or domain applications.

## Candidate discovery

The candidate discovery stage returned six papers. Five were retained for the A1 core/supporting pool:

- **When to Retrieve: Teaching LLMs to Utilize Information Retrieval Effectively** — explicit `<RET>` supervision for retrieval necessity.
- **KBM: Delineating Knowledge Boundary for Adaptive Retrieval in Large Language Models** — a separate knowledge-boundary model for known/unknown decisions.
- **DRAGIN: Dynamic Retrieval Augmented Generation based on the Real-time Information Needs of Large Language Models** — inference-time information-need detection and query formation.
- **SeaKR: Self-aware Knowledge Retrieval for Adaptive Retrieval Augmented Generation** — hidden-state and multi-sample uncertainty for retrieval triggering and evidence selection.
- **Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home** — evaluation of self-knowledge, uncertainty estimation, retrieval quality, and cost.

**Adaptive-RAG: Learning to Adapt Retrieval-Augmented Large Language Models through Question Complexity** was not included in the A1 core. Its question-complexity routing is useful as a supporting baseline, but it does not directly identify a knowledge boundary, uncertainty, or expected information value.

## Mainline findings

The retained papers form three signal families: answerability/weak labels, internal uncertainty, and evaluation-first self-knowledge measurement. They are complementary and should not be collapsed into a single “adaptive retrieval” category.

The common limitation is that retrieval necessity is represented through proxies. Answer correctness, entropy, hidden-state variation, and task complexity can all be useful signals, but none directly measures the counterfactual quantity: how much external information will improve the answer or decision relative to its cost.

The strongest survey-level gap is therefore **training–capability alignment**, not another retrieval-triggering heuristic. A direct A1 benchmark should separate trigger calibration, query quality, evidence quality, downstream utility, cost, and behavior transfer.

## Evidence discipline

Final QA improvement is not counted as proof of epistemic awareness. The A1 coding records separately mark method effectiveness, capability isolation, and generalization evidence. Cross-dataset evaluation is coded by its exact regime and is not automatically treated as behavior transfer.

## Next step

Before moving to A4 or Memory, the next A1 search should target the missing piece: value-of-information, counterfactual retrieval benefit, cost-sensitive retrieval triggering, and calibrated behavior transfer. Stop if new papers repeat the same answerability or uncertainty proxy without introducing a new training signal, evaluation protocol, or transfer condition.
