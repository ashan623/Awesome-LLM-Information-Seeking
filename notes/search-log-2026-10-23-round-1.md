# Search Log — Round 1 — 2026-10-23

## Purpose

This round establishes a small, verifiable field map for the survey **Beyond Retrieval: Learning Information-Seeking Capabilities in Large Language Models**. It intentionally does not attempt exhaustive searches across all six capability pools, memory, healthcare, e-commerce, agriculture, or every agentic-search method.

## Search boundary

The first round contains:

- three neighboring surveys: LLM-based Deep Search Agents, RL-based Agentic Search, and Deep Research;
- seven core method/evaluation papers: Self-RAG, Search-R1, R1-Searcher, ZeroSearch, Behavior Priming, Agentic-R, and Bright-Pro/reasoning-intensive retrieval.

The goal is to test whether the proposed A1–A6 capability taxonomy and B1–B6 learning taxonomy can be applied consistently to real papers.

## Source strategy

Search was performed using broad web queries, followed by direct reading of arXiv HTML, ACL Anthology, and official paper/project pages. Search snippets were not used as the sole evidence for paper claims.

Representative query families:

- `LLM based search agents survey paradigm optimization evaluation 2025`
- `agentic search survey reinforcement learning search agents`
- `Self-RAG Learning to Retrieve Generate and Critique`
- `Search-R1 R1-Searcher ZeroSearch reinforcement learning search LLM`
- `Agentic-R Learning to Retrieve for Agentic Search`
- `Rethinking Reasoning-Intensive Retrieval`
- `Beneficial Reasoning Behaviors in Agentic Search`

## Corrections made during verification

- The correct arXiv identifier for **ZeroSearch** is `2505.04588`; `2505.04589` is an unrelated mathematics paper.
- The neighboring survey found in the official arXiv record is titled **A Survey of LLM-based Deep Search Agents: Paradigm, Optimization, Evaluation, and Challenges**, arXiv `2508.05668`.
- **Agentic-R** has an official ACL Anthology page: `2026.findings-acl.785`.
- **Rethinking Reasoning-Intensive Retrieval** has an official ACL Anthology page: `2026.acl-long.1705`.

## Main observations

The neighboring surveys are primarily pipeline-, system-, or RL-centric. The proposed survey can differentiate itself by using **learned information-seeking capability** as the unit of analysis and by separating task success from behavior evidence and transfer evidence.

The seed methods already expose several distinct learning interfaces:

- Self-RAG: retrieval necessity and evidence critique through reflection-token supervision.
- Search-R1: outcome-based RL for interleaved multi-turn search and reasoning.
- R1-Searcher: staged learning of retrieval invocation and retrieval use.
- ZeroSearch: learning with a controllable simulated search environment.
- Behavior Priming: explicit search behavior supervision before RL.
- Agentic-R: iterative retriever–agent joint optimization using local relevance and global answer utility.
- Bright-Pro: evaluation of reasoning-intensive retrieval inside agentic workflows.

## What is not yet concluded

An empty capability × learning cell is not a research gap until citation snowballing and targeted search have been performed. In particular, this round does not yet establish the state of:

- epistemic uncertainty and value-of-information estimation;
- long-term memory retrieval and belief/state update;
- evidence sufficiency and contradiction resolution beyond QA;
- cross-tool and cross-retriever transfer;
- medical, scientific, enterprise, recommendation, e-commerce, and agricultural search;
- budget-generalization and calibrated stopping;
- adversarial or poisoned search environments.

## Next round proposal

The next round should target **A1 + A4**, because the first seed set suggests these capabilities are more directly measurable than generic “search capability.” The search should collect papers on retrieval necessity, uncertainty-aware retrieval, source reliability, evidence sufficiency, verification, contradiction handling, and citation/attribution. After that, expand into memory/state update and C1–C6 generalization.
