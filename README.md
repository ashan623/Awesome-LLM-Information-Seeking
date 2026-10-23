# Learning Information-Seeking in LLMs

This repository collects papers for the survey:

> **Beyond Retrieval: Learning Information-Seeking Capabilities in Large Language Models**

The survey is organized around three questions:

1. **What to Learn?** What information-seeking capabilities should a foundation model acquire?
2. **How to Learn?** How do pre-training, supervised fine-tuning, behavior learning, reinforcement learning, process supervision, distillation, and joint optimization teach these capabilities?
3. **Does It Generalize?** Does the model learn transferable information-seeking behavior, or only benchmark-specific tool-use patterns?

## Current taxonomy

Each paper is annotated with one or more of the following fields.

### Capability: what the model learns

- **A1 Epistemic Awareness** — recognizing missing knowledge, uncertainty, retrieval necessity, and value of information.
- **A2 Information Need Formulation** — query reformulation, question decomposition, query planning, and goal-conditioned search.
- **A3 Source Navigation** — source/tool selection, web navigation, multi-source exploration, and search-environment interaction.
- **A4 Evidence Evaluation** — relevance, authority, support, sufficiency, contradiction detection, and verification.
- **A5 Evidence Integration** — using evidence in reasoning, generation, memory, belief update, and downstream decisions.
- **A6 Adaptive Control** — deciding whether to continue, retry, verify, change source, stop, or trade off search quality against cost.

### Learning: how the capability is acquired

- **B1 Data / Trajectory Construction**
- **B2 Behavior Learning** — SFT, imitation learning, behavior priming, and trajectory distillation.
- **B3 Reward / Feedback Design** — outcome, process, evidence, preference, information-gain, cost, and risk feedback.
- **B4 Policy Optimization** — RL, GRPO/PPO/REINFORCE-style optimization, and test-time policy learning.
- **B5 Credit Assignment** — attributing downstream success or failure to multi-turn search actions.
- **B6 Joint Optimization** — retriever–agent, reasoner–retriever, verifier–policy, memory–policy, or system-level co-training.

### Generalization and auxiliary labels

- **C1 Task Transfer**
- **C2 Domain Transfer**
- **C3 Tool / Search-Engine Transfer**
- **C4 Corpus / Environment Shift**
- **C5 Budget Generalization**
- **C6 Behavioral Robustness**

- **D Epistemic Objective** — grounding/acquisition, verification/disambiguation, exploration/discovery, decision/action support, or learning/update.
- **E Update Target** — context/belief, plan/policy state, memory/index, retriever/verifier, LLM policy, or joint system.

A paper may have multiple capability and learning labels, but every entry should identify a **primary capability**, a **learning target**, and the strength of evidence for generalization.

## Repository structure

- [`papers/information-seeking-seed.md`](papers/information-seeking-seed.md): first-round seed papers and annotations.
- [`notes/search-log-2026-10-23-round-1.md`](notes/search-log-2026-10-23-round-1.md): scope, queries, inclusion decisions, and next search boundary.

## Search protocol

The literature collection is intentionally incremental:

1. Establish the field map with neighboring surveys.
2. Verify a small set of seed methods and evaluation papers.
3. Expand by capability × learning mechanism.
4. Snowball references and citing papers from high-value seeds.
5. Search for missing areas, especially memory, evidence evaluation, cross-tool transfer, and domain generalization.

The current round is **not** intended to be exhaustive. It establishes the initial map and the coding scheme before expanding the corpus.
