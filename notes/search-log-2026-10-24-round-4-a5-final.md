# Search Log — Round 4B — A5.1–A5.3 Second Batch and Closure

## Scope

The second batch was deliberately restricted to the two weak areas from the first A5 batch:

- **A5.2 Multi-evidence Composition**
- **A5.3 Faithful Evidence Utilization**

A5.1 Knowledge Integration & Belief Revision was not broadly expanded. A5.4 Persistent State Integration remains deferred to the Memory and State Update stage.

## Five verified papers

- **RAFT: Adapting Language Model to Domain Specific RAG** — COLM 2024; SFT with golden/distractor documents and quote-bearing CoT; a strong A5.2 capability-learning anchor with limited process-faithfulness evidence. The RAG paper is distinct from the same-acronym EMNLP 2024 text-detector attack paper.
- **BRIEF: Bridging Retrieval and Inference for Multi-hop Reasoning via Compression** — Findings of NAACL 2025; SFT of a T5 compressor using multi-hop proposition targets selected by reader likelihood; strong output compression/composition behavior, without necessary-hop or process-faithfulness tests.
- **CARE: Improving Context Fidelity via Native Retrieval-Augmented Reasoning** — EMNLP 2025; LoRA SFT plus GRPO with retrieval-span and answer rewards; the strongest direct post-training anchor for this batch, but its context-match reward and answer F1 do not prove causal evidence use.
- **MIRAGE: Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation** — EMNLP 2024; inference-only CTI/CCI attribution; a useful A5.3 diagnostic, not a post-training method and not a multi-evidence composition method.
- **TRACE the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation** — Findings of EMNLP 2024; inference-time 2–4 hop evidence-chain construction; the strongest indirect composition behavior in this batch, without parameter learning or causal chain verification.

## Three-level reliance coding

The second batch uses:

1. **Output Reliance:** whether the answer changes appropriately after key evidence substitution.
2. **Compositional Reliance:** whether deleting a necessary hop removes the corresponding conclusion basis.
3. **Process-level Reliance:** whether the reasoning process itself depends on the key evidence rather than producing a plausible post-hoc rationale.

## Main findings

No paper completes the strongest intervention: delete or replace one necessary piece of evidence, rerun reasoning, and verify that the corresponding conclusion fails or changes appropriately. RAFT varies distractors and document count; BRIEF varies compression and context size; CARE uses counterfactual contexts; MIRAGE compares context-sensitive predictions; TRACE performs chain and component ablations. These are useful but not equivalent to matched necessary-evidence leave-one-out.

The evidence hierarchy remains Output Reliance > Compositional Reliance > Process-level Reliance. TRACE is the strongest indirect A5.2 behavior anchor; BRIEF, RAFT, and CARE provide multi-document or multi-hop behavior with partial training alignment; MIRAGE provides a process-adjacent attribution diagnostic. None provides a complete causal process-faithfulness protocol.

RAFT, BRIEF, and CARE perform parameter-updating interventions. Their signals supervise evidence selection, quote-bearing output, proposition compression, support-span generation, or answer utility. They do not directly supervise necessary evidence, step-level entailment, conflict handling, or causal reasoning dependence. MIRAGE and TRACE are inference-only.

Generalization is broader than in the first batch but still incomplete. The five papers vary tasks, readers, models, compression budgets, distractors, and limited context conflicts. None systematically demonstrates stable evidence dependence across domain, document order, controlled noise ratio, source replacement, temporal shift, and bidirectional parametric–external conflict.

## Closure decision

A5.1–A5.3 are closed for broad collection at an eleven-paper map. The defensible conclusion is deliberately bounded:

> The current representative literature does not yet provide strict evidence that post-training teaches necessary multi-evidence dependence or process-level faithful evidence utilization that survives controlled perturbations and environment changes.

This is not the claim that models cannot use evidence. Existing methods provide useful evidence-conditioned output behavior, multi-document compression, structured evidence chains, context-fidelity training, and internal attribution diagnostics. The unresolved issue is whether these behaviors are causal, calibrated, and transferable rather than benchmark-specific or reward-specific.

Further A5 additions should be limited to a paper that introduces a genuinely new controlled intervention, process-level supervision, held-out evidence-reliance transfer, or a direct test against citation/reward/external-obedience shortcuts. The next broad capability axis is A5.4 Persistent State Integration within the Memory and State Update stage.
