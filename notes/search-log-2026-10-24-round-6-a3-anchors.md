# Search Log — Round 6A — A3 Information Space Exploration Anchors

## Scope

This is a deliberately bounded A3 anchor pass. The objective is to test whether A3 has an independently meaningful capability definition and whether its existing literature already provides direct Post-training evidence. It is not a broad search over browser agents, tool use, or Search-RL variants.

## Verified anchors

### InfoMosaic-Bench

InfoMosaic-Bench is an ICLR 2026 evaluation benchmark for multi-source information seeking. It contains 621 tasks, 77 MCP tools, and six domains: medicine/biology, finance, maps, video, web, and multi-domain integration. Tasks are constructed with verified tool outputs, cross-source dependencies, and filters against trivial single-source lookup.

The benchmark separates tool usage errors, tool selection errors, invalid but successful results, and valid useful results. It reports that web-only search is often insufficient, domain tools provide selective benefits, and tool inventory size increases selection risk. Tool-call scaling also shows a plateau where additional calls can become redundant. This makes Source/Tool Selection and multi-source coverage measurable, but the paper is evaluation rather than direct capability Post-training.

Sources: [arXiv](https://arxiv.org/abs/2510.02271), [HTML](https://arxiv.org/html/2510.02271v1), [ICLR 2026 paper](https://proceedings.iclr.cc/paper_files/paper/2026/file/44f09d1973a88529b40029e6c3693ded-Paper-Conference.pdf), [code](https://github.com/DorothyDUUU/Info-Mosaic).

### Table-as-Search

Table-as-Search is a Findings of ACL 2026 system framework that reformulates long-horizon information seeking as table completion. Rows represent candidates, columns represent constraints or required information, filled cells record search history/evidence, and empty cells encode pending information regions. The framework unifies Deep Search, Wide Search, and DeepWide Search.

The key A3 contribution is not an intrinsic learned policy but an explicit state representation for unresolved coverage, row expansion, cell population, and global verification. It helps separate “the model found one plausible result” from “the required candidate–constraint space has been covered.” Its system improvements therefore provide useful operationalization of Exploration and Coverage, while causal attribution to Post-training remains open.

Sources: [ACL Anthology](https://aclanthology.org/2026.findings-acl.743/), [arXiv HTML](https://arxiv.org/html/2602.06724), [code/data](https://github.com/AIDC-AI/Marco-Search-Agent).

### Agentic Search in the Wild

Agentic Search in the Wild is a SIGIR 2026 behavioral analysis based on 14.44M search requests and 3.97M sessions from DeepResearchGym. It labels sessions as declarative, procedural, or reasoning and labels step transitions as specialization, generalization, exploration, or repetition. It also proposes Context-driven Term Adoption Rate, which measures lexical traceability of newly introduced query terms to previously retrieved evidence.

The paper reports that more than 90% of multi-turn sessions contain at most ten steps, fact-seeking sessions become increasingly repetitive, reasoning sessions sustain broader exploration, and about 54% of newly introduced query terms appear in accumulated evidence context on average. These are candidate behavior metrics and real-world descriptive patterns, not evidence that an agent was Post-trained to explore effectively.

Sources: [arXiv](https://arxiv.org/abs/2601.17617), [HTML](https://arxiv.org/html/2601.17617v3), [SIGIR-linked version/DOI](https://doi.org/10.1145/3805712.3809627), [logs](https://huggingface.co/datasets/cx-cmu/deepresearchgym-agentic-search-logs).

## Synthesis

The anchors support three distinct A3 objects:

1. **Source choice:** selecting a useful information channel from heterogeneous tools;
2. **Coverage state:** identifying unresolved candidate–constraint or evidence regions;
3. **Trajectory exploration:** moving to new aspects without repeating already explored information.

The first anchor set is much stronger on measurement and system representation than on Learning. This is precisely the diagnostic needed for the survey: A3 may be a real information-seeking capability while still lacking a mature Post-training literature.

## Decision and next step

Keep A3 as a provisional standalone capability. Do not force a final chapter decision yet. Search only for direct SFT/RL/preference-learning papers on Source Selection, Exploration Diversity, Coverage Completion, or Unseen-Environment Navigation. Require behavior-level evaluation or counterfactual source/coverage intervention.

If the next bounded pass yields fewer than 4–6 distinct direct-learning papers, or if papers only improve final QA within fixed search loops, merge A3 with A2/A6 as a cross-cutting navigation capability. If it yields distinct training signals and behavior transfer, retain A3 as an independent chapter.
