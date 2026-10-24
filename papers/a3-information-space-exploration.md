# A3 — Information Space Exploration

## Central question

> **Can LLMs learn to navigate and explore unfamiliar information spaces to acquire useful, non-redundant information?**

A3 is deliberately narrower than generic Browser Automation or Tool Use. It concerns the choice and coverage of information regions: which source, tool, database, website, candidate, subspace, or unexplored evidence area should be visited next? A3 should capture exploration breadth, source complementarity, coverage of unresolved information gaps, and navigation in unfamiliar information environments.

A2 asks how to express an information need as a query or sub-question. A3 asks where to explore and whether the agent is covering new information regions. A6 asks how to react to feedback by continuing, modifying, retrying, verifying, switching tools, or stopping.

## First-round sub-directions

- **A3.1 Source / Tool Selection:** Can the agent choose the source or tool that is likely to contain the needed information rather than defaulting to generic Web Search?
- **A3.2 Exploration and Coverage:** Can the agent acquire complementary, non-redundant evidence and cover unresolved regions of the information space rather than producing many semantically similar queries?
- **A3.3 Unseen-Environment Navigation:** Can the agent discover effective search paths when the knowledge structure, source inventory, tool interface, or corpus is unfamiliar?

The first round is an anchor map, not yet a final A3 core set. The main question is whether there is enough independent capability-learning evidence to keep A3 as a standalone chapter.

---

## Anchor papers

### A3-1. InfoMosaic-Bench: Evaluating Multi-Source Information Seeking in Tool-Augmented Agents

- **Authors / year:** Yaxin Du et al., 2025 arXiv version; published as an ICLR 2026 conference paper.
- **Links:** [arXiv HTML](https://arxiv.org/html/2510.02271v1) · [arXiv abstract](https://arxiv.org/abs/2510.02271) · [ICLR paper](https://proceedings.iclr.cc/paper_files/paper/2026/file/44f09d1973a88529b40029e6c3693ded-Paper-Conference.pdf) · [code](https://github.com/DorothyDUUU/Info-Mosaic)
- **Role / contribution:** Evaluation / A3.1 Source and Tool Selection; secondary A3.2 multi-source coverage.
- **Core capability measured:** Whether agents can combine general Web Search with domain-specific tools and select useful tools for the current information need.
- **Environment:** 621 synthesized tasks, 77 MCP tools, and six domains: medicine/biology, finance, maps, video, web, and multi-domain integration.
- **Task structure:** InfoMosaic-Flow grounds tasks in verified tool outputs, enforces cross-source dependencies, and filters out trivial single-source shortcuts. The tasks therefore test whether the agent can discover and combine complementary sources rather than merely call a familiar Web Search tool.
- **A3 signal / metric:** Tool selection correctness, tool-use error, invalid-but-successful results, valid useful results, cross-source task accuracy, and failure-mode analysis. It is a measurement paper, not a Post-training paper.
- **Key findings:** Web-only search is insufficient on many tasks. Domain tools provide selective but inconsistent benefit. Incorrect tool usage or selection accounts for a substantial portion of failures. Larger tool inventories increase selection risk, and additional tool calls plateau or become redundant after an effective capacity threshold.
- **Why it matters for A3:** It turns Source/Tool Selection into an independently measurable information-seeking capability and makes the environment more realistic than a single fixed retriever.
- **A2 boundary:** Query quality may influence a tool call, but the central failure is often choosing the wrong information source or failing to combine sources; this is not reducible to Query Formulation.
- **A6 boundary:** More calls do not automatically constitute A6; the benchmark primarily evaluates whether the right sources are selected and integrated. However, call-budget curves create an A6-adjacent cost/control dimension.
- **Training–capability alignment:** Not Applicable for direct learning; it provides evaluation signals and failure labels for future Source Selection training.
- **Generalization evidence:** Stronger environment diversity than the current A2 map because domains and tool types vary, but the benchmark does not itself establish that a learned policy transfers to unseen tools or tool APIs.
- **Main limitation:** Tool availability is benchmark-defined, tool-output correctness is constructed from verified sources, and the evaluation does not separate source selection from later evidence integration in every score.
- **Decision:** Include as A3 Evaluation Anchor.

### A3-2. Table-as-Search: Agentic Information Seeking is Table Completion

- **Authors / year:** Tian Lan et al., Findings of ACL 2026.
- **Links:** [ACL Anthology](https://aclanthology.org/2026.findings-acl.743/) · [arXiv HTML](https://arxiv.org/html/2602.06724) · [code and data](https://github.com/AIDC-AI/Marco-Search-Agent)
- **Role / contribution:** Supporting / System and state representation; secondary A3.2 Exploration and Coverage.
- **Core mechanism:** Reformulates long-horizon information seeking as table completion. Rows represent discovered or potential candidates; columns represent constraints or required information; filled cells record evidence and empty cells explicitly represent pending information needs.
- **Exploration behavior:** The structured table exposes unresolved cells and supports row expansion, cell population, global verification, and targeted search. It unifies Deep Search, Wide Search, and DeepWide Search.
- **Learning status:** The framework changes the search state representation and planning interface. It is not direct evidence that Post-training teaches a general exploration policy; the core contribution is system-level state management and structured planning.
- **A3 signal / metric:** Completion of required table cells, candidate coverage, long-horizon answer quality, efficiency, and robustness under large search histories. The empty-cell representation is a useful operational proxy for an uncovered information region.
- **Why it matters for A3:** It provides a concrete representation of exploration coverage: the agent is not done when it has found one plausible answer; it must populate unresolved candidate–constraint cells.
- **A2 boundary:** Table schema construction may involve decomposition and sub-query generation, but the key contribution is tracking which regions remain unvisited and coordinating their coverage.
- **A6 boundary:** The framework supports iterative planning and search, but the paper does not isolate learned continuation, retry, or stopping policy as the primary capability.
- **Training–capability alignment:** Indirect. The structured state makes exploration behavior observable and controllable, but does not by itself prove that the LLM has learned a transferable exploration policy.
- **Generalization evidence:** The framework evaluates Deep, Wide, and DeepWide settings and multiple benchmark/system baselines; this is task-structure and horizon robustness, not necessarily unseen-environment transfer.
- **Main limitation:** State representation and external database support can improve performance without changing the model’s intrinsic exploration policy. Causal attribution between table state, planner behavior, source choice, and coverage remains open.
- **Decision:** Include as A3 Supporting/System Anchor.

### A3-3. Agentic Search in the Wild: Intents and Trajectory Dynamics from 14M+ Real Search Requests

- **Authors / year:** Jingjie Ning et al., SIGIR 2026.
- **Links:** [arXiv abstract](https://arxiv.org/abs/2601.17617) · [arXiv HTML](https://arxiv.org/html/2601.17617v3) · [anonymized logs](https://huggingface.co/datasets/cx-cmu/deepresearchgym-agentic-search-logs)
- **Role / contribution:** Behavioral Analysis / A3.2 Exploration and Coverage; secondary A3–A6 behavioral boundary.
- **Data:** 14.44 million search requests from 3.97 million sessions collected through DeepResearchGym.
- **Behavior labels:** Session-level intents are classified as declarative, procedural, or reasoning. Step-wise query transitions are labeled as specialization, generalization, exploration, or repetition.
- **Exploration signal:** The paper distinguishes horizontal exploration from vertical specialization/generalization and repetition. It also proposes Context-driven Term Adoption Rate (CTAR), measuring whether newly introduced query terms are traceable to previously retrieved evidence.
- **Key findings:** Most multi-turn sessions have at most ten steps; fact-seeking sessions exhibit increasing repetition, while reasoning sessions sustain broader exploration. On average, 54% of newly introduced query terms appear in accumulated evidence context, including evidence from earlier steps.
- **Why it matters for A3:** It provides large-scale behavioral definitions and candidate metrics for exploration breadth, redundancy, and cross-step evidence-conditioned navigation in realistic search sessions.
- **Learning status:** This is log analysis and measurement, not Post-training. LLM-based annotation is used to label behavior; that does not mean the agent learned the labels.
- **A2 boundary:** Query reformulation is observed, but the Exploration label is explicitly horizontal movement across aspects or related entities, not merely a better expression of the same information need.
- **A6 boundary:** Repetition and session length relate to stopping and budget control, but the main contribution is descriptive behavioral measurement rather than an adaptive-control policy.
- **Training–capability alignment:** Not Applicable for direct learning; strong as a source of candidate process signals and failure diagnostics.
- **Generalization evidence:** The data are large and real-world, but collected through one shared retrieval backend and external agent clients; this is ecological validity, not transfer to unseen tools or environments.
- **Main limitation:** LLM-based behavior labels and lexical CTAR do not establish useful semantic exploration or evidence causality. The same new term may be copied from retrieved text without representing a genuinely new information region.
- **Decision:** Include as A3 Behavioral Measurement Anchor.

---

## First-round synthesis

These anchors establish that A3 is not simply “more query diversity.” They expose three distinct objects of study:

1. **Source choice:** selecting the right information channel among heterogeneous tools;
2. **Coverage state:** identifying which candidate–constraint or evidence regions remain unresolved;
3. **Trajectory exploration:** moving horizontally to new aspects without repeating already explored information.

The current literature is much stronger on measurement and system support than on Post-training. InfoMosaic-Bench gives a realistic source/tool-selection benchmark. Table-as-Search provides an explicit state representation for pending information and long-horizon coverage. Agentic Search in the Wild provides real-trajectory labels and candidate metrics for exploration, repetition, and evidence-conditioned query evolution.

The key interim conclusion is:

> **A3 has an independently meaningful capability definition, but the first anchor set does not yet demonstrate that SFT, RL, or preference optimization has taught a generalizable exploration policy.**

This makes A3 a useful test of the survey’s central hypothesis. If the next targeted papers only optimize final answer reward in fixed search loops, then A3 will be mostly an evaluation/system chapter and may need to be merged with A2 and A6. If we find direct training of Source Selection, Exploration Diversity, Coverage Completion, or Unseen-Environment Navigation with behavior-level transfer tests, A3 should remain an independent capability chapter.

## Next bounded search

Only search for direct learning papers that add at least one of the following:

- SFT or RL for source/tool selection;
- rewards for non-redundant evidence coverage or unexplored-region discovery;
- explicit exploration-state or coverage-state policy learning;
- transfer to unseen tools, corpora, source inventories, or information structures;
- counterfactual source removal or coverage intervention that isolates exploration behavior from final answer quality.

Do not add generic browser automation, web navigation, tool-calling, or Search-R1 variants unless they isolate one of these A3 behaviors.
