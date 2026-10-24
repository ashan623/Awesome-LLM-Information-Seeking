# A3 — Information Space Exploration: Bounded Search Plan

## Central question

> **Can LLMs learn to navigate and explore unfamiliar information spaces to acquire useful, non-redundant information?**

A3 is about choosing information regions and covering unresolved evidence space. It is not a general Browser Automation or Tool Use survey.

## Boundary

- **A2 Information Need Formulation:** how to express the information need as a query, sub-question, or search plan;
- **A3 Information Space Exploration:** which source, tool, candidate, subspace, or unexplored evidence region should be visited next, and whether the trajectory covers complementary information;
- **A6 Adaptive Search Control:** after feedback, whether to continue, modify, verify, retry, switch tools, or stop.

A browser or tool call enters A3 only when it contributes evidence about source selection, exploration breadth, coverage of unresolved information, or navigation in an unfamiliar information space.

## First anchor set

1. **InfoMosaic-Bench** — evaluation anchor for multi-source information seeking, heterogeneous tool selection, and cross-source dependencies;
2. **Table-as-Search** — system/state anchor for representing pending information regions and long-horizon coverage as table completion;
3. **Agentic Search in the Wild** — behavioral-analysis anchor from 14.44M real search requests, with exploration/repetition labels and CTAR.

## First-round questions

1. **A3.1 Source / Tool Selection:** Can the policy select a source likely to contain the required information instead of defaulting to Web Search?
2. **A3.2 Exploration / Coverage:** Can it acquire complementary, non-redundant evidence and identify what remains uncovered?
3. **A3.3 Unseen-Environment Navigation:** Can it discover useful paths in an unfamiliar corpus, source inventory, tool API, or knowledge structure?
4. **Training alignment:** Does the reward supervise source/exploration behavior directly, or only final answer correctness?
5. **Behavior transfer:** Is exploration measured after changing the task, source inventory, corpus, tool interface, or information structure?

## Coding fields

Every direct-learning candidate should record:

- Exploration Target;
- Source / Tool Choice;
- Coverage State or Unresolved Region;
- Exploration Diversity / Redundancy Signal;
- Training Mechanism;
- Training–Capability Alignment;
- Counterfactual Source or Coverage Intervention;
- A2/A3/A6 Boundary;
- Generalization across task, domain, source inventory, tool, environment, horizon, and budget;
- Shortcut risk from tool count, query count, context size, memorized source order, or final-answer reward.

## Bounded next search

After the three anchors, search only for direct SFT/RL/preference-learning papers that train source selection, exploration diversity, coverage completion, or unseen-environment navigation. A candidate must add behavior-level evidence beyond a final QA score.

Do not add generic browser automation, tool-use benchmarks, Web-Agent papers, or Search-R1 variants unless they isolate one of the A3 behaviors above.

## Stop / merge rule

If the next targeted papers are mostly evaluation or system representations, with no direct Post-training evidence or behavior-transfer test, do not force A3 into a standalone learning chapter. Merge A3 with A2/A6 as a cross-cutting capability of information-space navigation.

If at least 4–6 direct-learning papers provide distinct signals and behavior-level transfer, retain A3 as an independent chapter.

## Current state

The three anchors and four targeted candidates are verified. MARAG-R1 and DDPO provide direct information-space learning evidence. Training Small Language Models for Transferable Tool Selection and ToolSearcher provide strong tool-repository selection/transfer evidence but are boundary contrasts because they do not measure acquisition of task-relevant information.

Final decision: retain A3 as a **cross-capability dimension with a dedicated subsection**, not a full independent learning chapter and not a complete merge into A2/A6. The decisive distinction is:

> **Query Diversity ≠ Evidence Diversity ≠ Information Gain ≠ Useful Coverage.**

Stop broad A3 collection. Proceed to A6 Adaptive Search Control. The detailed map and decision are in [`papers/a3-information-space-exploration.md`](../papers/a3-information-space-exploration.md), and the targeted verification is in [`notes/search-log-2026-10-24-round-6-a3-learning.md`](./search-log-2026-10-24-round-6-a3-learning.md).
