# Search Log — Round 4A — A5 Evidence Integration First Batch

## Fixed boundary

A5 asks whether an LLM can faithfully integrate acquired information into reasoning, beliefs, generation, and decisions after the evidence has been retrieved and evaluated.

This round covers:

- **A5.1 Knowledge Integration & Belief Revision**
- **A5.2 Multi-evidence Composition**
- **A5.3 Faithful Evidence Utilization**

A5.4 Persistent State Integration is deferred to the memory-focused stage.

The core distinctions are:

> Evidence Access ≠ Evidence Utilization ≠ Belief Update

## Six verified papers

- **ClashEval** — controlled answer-bearing evidence replacement; direct bidirectional test of correct-prior/wrong-context and wrong-prior/correct-context behavior; evaluation-only.
- **FaithfulRAG** — inference-time self-fact mining and conflict reconciliation; useful output-level evidence integration but no parameter learning.
- **BRIEF / Belief Revision** — update-versus-maintain diagnostic on synthetic three-premise tasks; no parametric–external conflict test and no training.
- **GuarantRAG** — DPO-trained Refer-Answer model plus joint decoding; genuine but asymmetric evidence integration training, with no strong test against false external evidence.
- **Efficient Prior-Guided Reasoning** — GRPO/RLVR and prior dropout for robustness to noisy/conflicting retrieval; answer-F1 reward leaves faithful causal evidence use underdetermined.
- **How Large Language Models Balance Internal Knowledge with User and Document Assertions** — controlled answer/probability probe plus mixed LoRA SFT; strongest answer-level PAR+/SDR+ balance measurement, still synthetic and not reasoning-level.

## New A5 fields

Every A5 entry records:

1. **Integration Target:** Answer, Reasoning Chain, Belief, Memory, or Decision.
2. **Evidence Reliance:** whether controlled key-evidence changes appropriately alter the output and irrelevant changes leave it stable.
3. **Parametric–External Balance:** whether the model corrects wrong internal knowledge, resists false external evidence, and handles source conflicts.

## First-batch findings

A5.1 dominates this selected batch. A5.2 is represented mainly by GuarantRAG and Efficient Prior-Guided Reasoning. A5.3 is generally measured through answer accuracy, hallucination, entity precision, output probability, or noise robustness rather than causal reasoning-chain attribution.

ClashEval and the source-balancing study provide the clearest controlled evidence-reliance probes. Efficient Prior-Guided Reasoning adds no-gold and all-incorrect retrieval conditions; FaithfulRAG adds fact replacement and irrelevant contexts; GuarantRAG mainly tests irrelevant-document noise; BRIEF tests update versus maintenance after an added premise.

Only three works perform genuine parameter-updating interventions: GuarantRAG, Efficient Prior-Guided Reasoning, and the mixed LoRA SFT source-balancing study. None demonstrates persistent belief or memory update. The trained behaviors are answer-level or generation-level and do not yet establish transferable reasoning-chain faithfulness.

## Caution on the current diagnosis

Within this six-paper first batch, measurement appears more developed for answer-level parametric–external conflict than for learned, reasoning-level, transferable evidence utilization. This is a preliminary observation about the selected papers, not a field-wide conclusion. The next batch must test whether additional process-supervised, multi-hop, attribution-based, and counterfactual training work changes the diagnosis.

## Next batch boundary

The next A5 batch should focus on:

- Multi-hop evidence composition with explicit evidence-chain or intermediate-state supervision;
- Causal key-evidence deletion, leave-one-out, and irrelevant-evidence-invariance tests;
- Process-level evidence attribution rather than final-answer-only rewards;
- Parametric-wrong/external-true and parametric-right/external-false paired interventions;
- Generalization across source, domain, tool, time, and misleading evidence;
- Reward/citation copying and external-obedience failure modes.

A5.4 remains deferred until the memory and state-update stage.
