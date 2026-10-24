# A6 — Adaptive Search Control: Bounded Search Plan

## Central question

> **Can LLMs learn feedback-conditioned search-control policies that adapt their actions, allocate resources, and stop appropriately across changing information environments?**

A6 begins after search has started. It is not a general survey of Search-Agent RL, and it is not a duplicate of A1 retrieval necessity or A2 query formulation.

## Boundary

- **A1 Epistemic Awareness:** whether acquisition is necessary or worthwhile before/at the start of search;
- **A2 Information Need Formulation:** how to express the information need as a query, sub-question, or plan;
- **A3 Information Space Exploration:** which source or unresolved information region to explore;
- **A6 Adaptive Search Control:** after observing search feedback, whether to Continue, Retry, Reformulate, Switch, Verify, Allocate budget, or Stop.

## First-round questions

1. **Feedback-conditioned adaptation:** Does a failed, repeated, insufficient, contradictory, or newly informative result change the next action appropriately?
2. **Budget and stopping:** Does the policy stop when another search has low expected value, continue when evidence is insufficient, and allocate different depths under different budgets?
3. **Long-horizon credit assignment:** Can training assign success/failure to the specific control actions that caused it rather than giving every turn the same outcome reward?
4. **Feedback dependence:** Is the learned action sensitive to the observed result, or does it follow a fixed step-count/template policy?
5. **Transfer:** Does the control rule survive changes in budget, Retriever, Tool, Corpus, domain, and feedback quality?

## First four candidates

- **AutoSearch:** search depth and stopping;
- **AnySearch:** budget-aware control and unseen-budget transfer;
- **CriticSearch:** retrospective turn-level credit assignment;
- **CW-GRPO:** contribution-aware per-round advantage weighting.

Existing Search-R1, StepSearch, and Beneficial Reasoning Behaviors in Agentic Search are reused as anchors and are not duplicated in this round.

## A6-specific coding fields

- **Control Action:** Continue / Retry / Reformulate / Switch / Verify / Allocate / Stop;
- **Observed Feedback:** empty, irrelevant, repeated, novel, sufficient, contradictory, failed tool call, budget state, or cost signal;
- **Feedback Dependence:** whether the same history with altered feedback produces a different appropriate next action;
- **Control Credit:** terminal, step-level, turn-level, hindsight, contribution-weighted, or counterfactual;
- **Deployment Availability:** whether the signal uses only current state or privileged future trajectory/gold answer during training;
- **Control Transfer:** budget, Retriever, Tool, Corpus, Domain, Environment, and Behavior Transfer;
- **Shortcut Risk:** fixed depth, fixed budget template, search-count imitation, answer leakage, or critic privileged information.

## Required controlled experiment

Prefer a fixed-history feedback intervention: hold the task and completed trajectory constant, replace only the current retrieval feedback, and test the next-action distribution. The desired directions are:

- sufficient and consistent evidence → higher Stop / Answer probability;
- empty or failed retrieval → Reformulate / Retry / Switch;
- repeated evidence → avoid identical search and seek verification or a new source;
- contradictory evidence → Verify or seek an independent source;
- larger remaining budget and unresolved information → Continue when marginal value is positive.

A paper without this intervention may still be useful, but its Feedback Dependence field must be marked Partial or Limited rather than inferred from final QA improvement.

## Stop rule

The first A6 pass closes after these four candidates are coded alongside existing Search-R1, StepSearch, and Beneficial Reasoning Behaviors anchors. Do not add generic Search-RL variants unless they introduce a new control object, a direct feedback intervention, or a new transfer condition.

After A6, stop longitudinal collection and begin the cross-capability matrix:

> **Training Signal × Learned Behavior × Capability Isolation × Generalization**

The detailed four-paper map is [`papers/a6-adaptive-search-control.md`](../papers/a6-adaptive-search-control.md). The verification record is [`notes/search-log-2026-10-24-round-6-a6-first-batch.md`](./search-log-2026-10-24-round-6-a6-first-batch.md).
