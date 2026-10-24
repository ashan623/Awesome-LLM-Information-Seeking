# Search Log — Round 8 Learning Mechanism Verification

## Scope

本轮只核验三个 P0 候选，不重新搜索 A1–A6，不扩展通用 OPD、Memory、RAG 或 Search-RL 变体。每篇按六个字段记录：Learning Signal、Optimizer、Update Locus、Credit Assignment、Deployment Parity、Capability Evidence。

## Sources checked

- [SSPO arXiv abstract](https://arxiv.org/abs/2608.12764) and [SSPO full text](https://arxiv.org/html/2608.12764)
- [TRACE arXiv abstract](https://arxiv.org/abs/2607.13988) and [TRACE full text](https://arxiv.org/html/2607.13988v1)
- [Retrieval, Reward, and Training Protocols arXiv abstract](https://arxiv.org/abs/2605.27881) and [v2 full text](https://arxiv.org/html/2605.27881v2)

## Decisions

### SSPO — Core Mechanism

SSPO is direct evidence that teacher–student disagreement can be converted into step-level advantage weighting inside GRPO for deep search. The method is mechanism-relevant because it separates outcome-defined update direction from teacher-defined step magnitude and explicitly analyzes Evidence Anchors as privileged information. Capability evidence remains partial: the main results are final QA, average turns, learning efficiency, and token/step/distillation ablations. No controlled query/evidence/control intervention establishes transferable behavior-level capability.

### TRACE — Core Mechanism

TRACE is direct evidence that Gold-answer prefix state values from a frozen reference model can produce turn-level TD credit for long-horizon search-agent RL. It uses controlled baselines, credit-form ablations, turn-weight and TD-horizon ablations, and closed-web to open-web evaluation. The key limitation is Deployment Information Parity: Gold Answer is available to the training-time reference probe but not to the deployed policy. Better training and final performance do not by themselves establish online feedback-conditioned control.

### Retrieval, Reward, and Training Protocols — Core Mechanism / Controlled Comparison

This paper is not a new optimizer. Its value is causal discipline: it reimplements several reward/credit methods in a shared GRPO setup and varies corpus completeness, reward design, data diversity, off-policy degree, format, and budget. It finds that missing corpus evidence can create reward noise and that corpus correction can exceed algorithmic differences; simple outcome reward remains a strong baseline and process credit can over-correct or induce over-search. This is core evidence for the survey’s environment-dependence and outcome-confounding claims.

## Preference / DPO check

No new search-specific Preference/DPO paper was added. Existing GuarantRAG is retained as an A5 supporting case because its DPO primarily trains evidence preference and has asymmetric external-evidence reliance; it does not provide direct Search Policy behavior isolation. The mechanism taxonomy therefore remains sufficient without adding another DPO variant.

## Stop decision

Round 8 is closed after three papers. The four How-to-Learn dimensions are sufficient:

1. Data and Learning Signals;
2. Policy Learning and Optimization;
3. Learning Targets and Optimization Loci;
4. Long-horizon Credit Assignment.

Deployment Information Parity and Generalization remain cross-capability diagnostics. The next phase is Cross-Capability Synthesis and Survey writing, not more longitudinal paper collection.

For the detailed six-field coding and evidence assessment, see [`round8-learning-mechanism-verification.md`](../papers/round8-learning-mechanism-verification.md).
