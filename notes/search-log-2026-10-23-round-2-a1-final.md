# Search Log — Round 2C — A1 Final Utility-aware Training Verification

## Scope

This was the final narrow A1 check. It verified six explicitly identified works spanning utility-aware RL, semantic information-gain reward, counterfactual tool-use margins, cost-aware exploration, knowledge-boundary-aware RL, and historical self-knowledge gating.

The purpose was to test whether the survey could still claim that utility-aware search training is missing. It cannot make that claim in its broad form.

## Verified works

- **s3: You Don’t Need That Much Data to Train a Search Agent via RL** — GBR binds searcher training to observed gain beyond naive RAG.
- **Optimizing Agentic Reasoning with Retrieval via Synthetic Semantic Information Gain Reward** — InfoReasoner uses observed semantic posterior gain plus final answer reward in GRPO.
- **CoBRA: Learning Tool-Use Boundaries via Counterfactual Margins** — paired internal/external branches and cost-adjusted margins train tool-use boundaries.
- **Calibrate-Then-Act: Cost-Aware Exploration in LLM Agents** — calibrated priors and explicit costs guide retrieve/answer or verify/commit decisions, with prompting and RL variants.
- **IKEA: Reinforced Internal-External Knowledge Synergistic Reasoning** — answer correctness and retrieval-count shaping train internal/external knowledge routing.
- **Self-Knowledge Guided Retrieval Augmentation for Large Language Models** — historical paired-outcome gating baseline; the target LLM is not updated.

## Corrected A1 conclusion

Utility-aware training is already present. The strongest direct examples are CoBRA, s3, InfoReasoner, IKEA, and CTA-RL. However, most signals remain ex-post observed gain, semantic information gain after retrieval, coarse answer/call-count shaping, or externally supplied calibration priors.

The defensible survey claim is:

> Retrieval necessity is evolving from proxy-based triggering toward utility-aware policy learning. However, optimizing observed retrieval gains does not necessarily imply that an LLM can predict, calibrate, and transfer the value of information acquisition before taking action.

This is a research direction and a structured evidence gap, not yet a claim that no prior work exists.

## Three remaining open problems

1. **Ex-post reward versus ex-ante value:** Can a policy predict before tool invocation whether acquisition will help, rather than receive reward only after a retrieval trajectory succeeds?
2. **Counterfactual utility estimation:** Can paired or multi-sample potential outcomes separate retrieval benefit from generator capability, retriever quality, expert differences, judge noise, and sampling randomness?
3. **Cost-calibrated generalization:** Does the same policy remain calibrated when the LLM, retriever, tool, corpus, domain, time, or token/latency/API-price budget changes?

## Decision

A1 is sufficiently mapped for the current survey stage. No more broad searches for utility-aware or uncertainty-trigger variants should be conducted now. The next stage is A4 Evidence Evaluation, with the explicit interface:

- A1: whether to acquire information and whether its expected benefit justifies its cost.
- A4: whether acquired evidence is relevant, reliable, sufficient, non-contradictory, and safe to use.

The A1 and A4 axes may share paired trajectories and cost logs, but their capabilities and evidence scores must remain separate.
