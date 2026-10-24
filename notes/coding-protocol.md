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
- **A4 Evidence Evaluation:** judging evidence quality before using it. Code four sub-directions: **A4.1 Relevance & Support** (does the evidence support the question or claim?), **A4.2 Source Reliability** (is the source credible, authoritative, current, and traceable?), **A4.3 Evidence Sufficiency** (is the set of evidence complete enough, and what is missing?), and **A4.4 Conflict & Robustness** (can the model detect contradiction, noise, or misleading evidence?).
- **A5 Evidence Integration:** correctly using evaluated evidence to update reasoning, memory, beliefs, generation, or decisions. Do not count successful evidence use as A4 unless the paper separately measures the quality judgment itself.
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

## A4-specific diagnostic fields

For A4 papers, add three fields that prevent relevance, support, sufficiency, and source authority from collapsing into one score:

- **Judgment Target:** record whether the model judges a `Query–Document`, `Claim–Evidence`, `Evidence Set`, or `Source`.
- **Ground Truth Quality:** record whether the target is `Human-verified`, `Expert`, `Synthetic`, `LLM-generated`, or `Weak Label`, and whether it is independent of the final answer or downstream reward.
- **Controlled Perturbation:** record whether the evaluation controls source authority, conflict ratio, evidence order, temporal change, retrieval noise, formatting, citation fabrication, or answer leakage. If none are controlled, write `Not Tested` rather than inferring robustness from a new dataset.

For A4, a high final QA score cannot compensate for weak ground truth or missing controlled perturbations. Source authority must be separated from factual correctness: a famous source can be outdated or wrong, while an obscure first-party source can be accurate.

## A5-specific capability boundaries and diagnostic fields

A5 asks whether the model faithfully integrates acquired information into reasoning, beliefs, generation, and decisions. Keep the following distinctions explicit:

- **A5.1 Knowledge Integration & Belief Revision:** can external evidence correct erroneous parametric knowledge without causing blind obedience to incorrect evidence?
- **A5.2 Multi-evidence Composition:** can the model combine multiple passages, hops, or partial facts into a valid inference?
- **A5.3 Faithful Evidence Utilization:** does the reasoning and conclusion actually depend on the provided evidence rather than merely quoting or co-occurring with it?
- **A5.4 Persistent Information Integration & Memory Updating:** can acquired information be selectively written, revised, forgotten, and reused across interactions without stale, conflicting, or irrelevant state accumulation?

For A5 papers, add three fields:

- **Integration Target:** `Answer`, `Reasoning Chain`, `Belief`, `Memory`, or `Decision`.
- **Evidence Reliance:** split the diagnosis into three levels: `Output Reliance` (does the final answer change appropriately after key evidence is replaced?), `Compositional Reliance` (does deleting a necessary passage break the corresponding multi-hop conclusion?), and `Process-level Reliance` (does the reasoning process itself depend on the key evidence rather than producing a plausible post-hoc explanation?). Output changes establish behavioral sensitivity, but alone do not prove causal faithfulness of the reasoning chain.
- **Parametric–External Balance:** record whether the model can revise wrong internal knowledge, resist false external evidence, and explain conflicts rather than following either source mechanically.

A5.4 papers should be cross-coded along four dimensions:

- **A5.4.1 Memory Formation:** whether the system learns what information is worth writing rather than storing everything.
- **A5.4.2 State Revision:** whether new information correctly overwrites, updates, reconciles, or expires old state under temporal change and conflict.
- **A5.4.3 Future Memory Utility:** whether stored information measurably improves later tasks, decisions, or reasoning rather than merely being retrieved.
- **A5.4.4 Memory Policy Learning:** whether SFT, RL, feedback, or another learning signal teaches transferable write, update, delete, no-op, and utilization behavior.

For A5.4, also record **Persistence Target** (`External Memory`, `Context State`, `Policy`, or `LLM Parameters`), **Temporal Consistency** under stale information, preference changes, and conflict, and **Future Utility Attribution** distinguishing memory-caused gains from fresh retrieval, context leakage, or parametric knowledge. Use failure labels `Write Failure`, `Update Failure`, `Retrieval Failure`, and `Utilization Failure` rather than collapsing all errors into memory retrieval.

Evidence Access is not Evidence Utilization, and Evidence Utilization is not Belief Update. A context that contains the answer does not prove that the model used it in its reasoning.

## Current search priority and stop rule

The current search should focus on four connected pools:

1. **P0 — Memory and State Update**, including A5.4 Persistent State Integration
2. **P1 — Capability Evaluation and Generalization**, including behavior transfer, causal evidence reliance, and credit assignment
3. **P1 — Domain applications**, only after a capability-level gap is established
4. **P2 — Targeted A5 follow-up**, only for genuinely new controlled interventions or process-level transfer evidence

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
Judgment Target
Ground Truth Quality
Controlled Perturbation
Integration Target
Evidence Reliance
Parametric–External Balance
Output Reliance
Compositional Reliance
Process-level Reliance
Persistence Target
Temporal Consistency
Future Utility Attribution
Memory Failure Type
Epistemic Objective
Update Target
Limitation
Research Opportunity
Role: Core / Supporting / Background / Evaluation
```
