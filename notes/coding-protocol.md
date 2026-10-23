# Converged Coding Protocol

This protocol fixes the survey line for subsequent search and annotation. It is intentionally conservative: a new paper should sharpen the central argument, not merely add another implementation variant.

## Fixed survey line

The survey studies:

> **What information-seeking capabilities can LLMs acquire, how well do training signals align with those capabilities, and whether the learned behavior transfers beyond the original task and environment?**

Every core paper should contribute evidence to at least one of the three links:

1. **Capability learning:** what behavior is learned or claimed to be learned?
2. **Training–capability alignment:** does the supervision or reward directly reflect that behavior?
3. **Capability transfer:** does the behavior survive a new task, domain, tool, environment, or budget?

Papers that only report a small implementation improvement over an existing Search-RL recipe should remain background unless they add new evidence for one of these links.

## Capability boundaries

The A1–A6 capability taxonomy remains stable, but two boundaries are explicit:

- **A1 Epistemic Awareness:** recognizing missing information, uncertainty, retrieval necessity, and expected value of information.
- **A2 Information Need Formulation:** translating a goal and knowledge gap into queries, sub-questions, or a search plan.
- **A3 Information Space Exploration:** discovering paths, sources, and new information regions; navigating across an information space.
- **A4 Evidence Evaluation:** judging relevance, authority, support, sufficiency, contradiction, reliability, and verifiability.
- **A5 Evidence Integration:** using evidence to update reasoning, memory, beliefs, generation, or decisions.
- **A6 Adaptive Search Control:** deciding when to search, how to modify the search, how to invoke or retry a tool, when to verify, and when to stop under a budget.

A tool call is not automatically A3. Search invocation and stopping belong to A6 unless the paper directly studies exploration or source discovery. A paper may be marked `Not Directly Trained` when its main contribution is infrastructure, retriever optimization, or evaluation.

## Orthogonal contribution type

Record exactly one primary contribution type, with secondary types when necessary:

- **Capability Learning** — trains or distills an information-seeking behavior into the policy model.
- **Training Infrastructure** — constructs a simulator, rollout environment, curriculum, or scalable interaction system.
- **Retriever Optimization** — changes the retriever, verifier, or retriever–agent coupling rather than directly training an LLM capability.
- **Evaluation** — introduces a benchmark, metric, annotation protocol, or workflow-level evaluation.

Contribution type must not be confused with A1–A6. For example, Agentic-R can be central to the survey while its primary contribution type is Retriever Optimization, not a directly trained A5 LLM capability.

## Three-dimensional evidence vector

Replace a single overall evidence score with three separate fields:

| Field | Strong evidence requires |
|---|---|
| **Method Effectiveness** | Meaningful baselines, ablations, and reproducible task-level improvement |
| **Capability Isolation** | Direct measurement of the target search behavior, not only final answer accuracy |
| **Generalization Evidence** | A deliberate test under a changed task, domain, tool, environment, behavior, or budget |

Each field takes one of: `Strong`, `Medium`, `Limited`, or `Not Tested`.

## Generalization regimes

Use the following labels rather than treating every new benchmark as transfer:

- **In-distribution:** training and test tasks, tools, and corpora are substantially aligned.
- **Task OOD:** new task type or reasoning form.
- **Domain OOD:** new knowledge domain.
- **Tool OOD:** different search interface, retriever, API, browser, database, or memory backend.
- **Environment OOD:** changing corpus, temporal knowledge, source noise/conflict, or simulator-to-real shift.
- **Behavior Transfer:** the target behavior itself is measured in a new setting.
- **Budget / Cost Generalization:** different token, latency, API-call, memory, or monetary budgets.

Behavior Transfer is the most important label for this survey. A benchmark transfer result should not be described as a transferable information-seeking capability unless the behavior is also measured.

## Training–Capability Alignment

For every method, record whether its signal directly matches the claimed capability:

| Training signal | Claimed capability | Alignment risk |
|---|---|---|
| Final answer reward | Search policy | Cannot localize useful search steps; may reward shortcuts |
| Retrieval invocation reward | Search capability | May encourage unnecessary search |
| Query relevance reward | Query formulation | Relevant queries may still fail to produce useful evidence |
| Behavior supervision | Verification or recovery | Labels may not fully represent evidence quality |
| Downstream utility reward | Evidence selection | Reasoner or generator differences may confound evidence utility |

Use `Direct`, `Partial`, `Indirect`, or `Not Applicable` in the paper table, with one sentence of justification.

## Second-round priority and stop rule

The next search should focus on four connected pools:

1. **P0 — A1 Search Necessity / Awareness**
2. **P0 — A4 Evidence Evaluation**
3. **P0 — Memory and State Update**
4. **P1 — Capability Evaluation and Generalization**, including behavior transfer and credit assignment

Only after these pools reveal a concrete gap should the search add a small number of medical, scientific, e-commerce, or other domain papers. Do not expand with large numbers of Search-R1-like variants. Stop a search branch when new papers repeat the same capability, signal, and evaluation pattern without adding a new transfer condition or a new alignment problem.

## Required annotation template

```text
Paper / Year / Link
Contribution Type
Primary Capability
Secondary Capabilities
Training Mechanism
Learning Target
Supervision / Reward
Training–Capability Alignment
Method Effectiveness
Capability Isolation
Generalization Evidence
Generalization Regime
Epistemic Objective
Update Target
Limitation
Research Opportunity
Role: Core / Supporting / Background / Evaluation
```
