# Targeted Claim–Evidence Audit for A1, A4, and A5

> Round 10 targeted audit. The purpose is not to add a large number of papers. It is to align the strength of each prose claim with the strength of the evidence actually reported.

## Audit rule

For each capability, separate four levels:

1. **Observed behavior:** what changed in the reported experiment;
2. **Local capability evidence:** whether the changed behavior matches a narrow target;
3. **Capability-level claim:** whether the result supports the broader A1/A4/A5 capability definition;
4. **Transfer claim:** whether the behavior survives a changed model, task, source, tool, corpus, cost, or feedback condition.

A paper can provide strong evidence at level 1 and weak or absent evidence at level 4. The survey should not collapse these levels.

---

## A1 — Epistemic Awareness: calibrated retrieval utility

### Core claim under test

> Post-training and decision methods can improve retrieval triggering and utility-aware search behavior, but the current evidence does not establish a calibrated, transferable **ex-ante** value-of-information predictor.

| Evidence role | Papers | What was actually tested | What remains unestablished |
|---|---|---|---|
| Supporting | When to Retrieve, KBM, DRAGIN, SeaKR | Triggering, knowledge-boundary or uncertainty-conditioned retrieval behavior | Expected net benefit before acting; cross-environment calibration |
| Supporting | LLM-Specific Utility, Cost-Aware Evidence Selection, UDCG | Downstream utility/cost-sensitive evaluation of retrieved evidence | A policy that predicts instance-level value before retrieval |
| Partial mitigation | CoBRA | Paired no-tool/tool branches and counterfactual margins provide stronger local boundary isolation | Single realized branch differences still mix expert, retriever, judge and sampling effects; ex-ante prediction and transfer remain open |
| Partial mitigation | Calibrate-Then-Act | Cost-aware actions improve when an explicit calibrated prior/answerability estimate is supplied | The external prior is part of the decision interface; RL improvement is not proof that the LLM learned calibration intrinsically |
| Supporting but ex-post | s3, InfoReasoner, IKEA | Relative downstream gain, semantic posterior shift, and answer-plus-call-count rewards improve search behavior | Rewards are computed after observing retrieval outcomes; they do not establish pre-action value prediction |
| Not established | All current A1 methods | Some task/domain/cost variation exists | Stable calibration under new LLM, retriever, corpus, tool and cost distributions |

### Required prose distinction

Do not write “the model learns when retrieval is useful” solely from improved answer accuracy or fewer search calls. Write instead:

> The literature demonstrates improved **retrieval decisions under particular utility signals and environments**. It has not yet established a calibrated estimate of the counterfactual net benefit of acquiring information before action.

### Recommended A1 subsection order

1. Trigger and uncertainty proxies;
2. Utility measurement;
3. Ex-post utility-aware policy learning;
4. Cost-aware decision interfaces;
5. Ex-ante value prediction and calibration gap;
6. Model/evidence/task/cost dependence.

---

## A4 — Evidence Evaluation: reliable judgment rather than relevance matching

### Core claim under test

> Evidence evaluation is becoming measurable at the levels of support, authority, sufficiency, and conflict, but direct post-training evidence remains narrow and does not yet establish a transferable policy that distinguishes relevance from reliability.

| Evidence role | Papers | What was actually tested | What remains unestablished |
|---|---|---|---|
| Supporting measurement | CRAG, RAGTruth, RAGChecker | Query–document relevance, unsupported spans, claim support, entailment, noise and diagnostic decomposition | Source credibility, typed conflict handling and deployment-transfer policy |
| Supporting narrow learning | S2G-RAG | Context-only sufficiency/gap judgment and iterative continuation/stopping | Independent set-level truth, source replacement, temporal change and conflict robustness |
| Supporting narrow learning | RLSeek, EAPO | Source-span support/refute, quotation/citation behavior, evidence utility reward | General source reliability, process-independent support, verifier-robust transfer |
| Counterevidence | What Evidence Do Language Models Find Convincing? | ConflictingQA sensitivity and counterfactual analyses; relevance dominates while scientific references and neutral tone are underused | Post-training does not follow from the evaluation; source reliability remains an unlearned/generalization question |
| Partial mitigation | AuthorityBench, CONFACT | Authority/credibility perception and source-aware fact-checking under conflict | Popularity/TLD shortcuts, majority-source bias, static labels, and lack of separate conflict attribution/resolution |
| Partial mitigation | (D)RAGged Into a Conflict | Conflict taxonomy, desired behavior by conflict type, expert-annotated benchmark, conflict-aware prompting | Prompting is not transferable policy learning; unseen conflict types, source replacement and time shift remain open |
| Partial mitigation | Sufficient Context, ConfRAG | Set-level sufficiency and viewpoint/reason coverage | Sufficiency is not minimal evidence; coverage is not truth resolution |
| Not established | A4 as a whole | Local support and conflict-related behaviors are observable | General evidence judgment under source perturbation, freshness shift and held-out verifier conditions |

### Required prose distinction

Do not write “LLMs can evaluate evidence” as a general claim. Write:

> Models can be trained or prompted to perform **narrow evidence judgments**—for example, support-span detection, sufficiency estimation, or conflict-aware organization. The evidence does not yet show that these behaviors form a general, source-robust reliability policy.

### Recommended A4 subsection order

1. Relevance versus support;
2. Source authority and convincingness;
3. Sufficiency and missing evidence;
4. Typed conflicts and desired responses;
5. Measurement resources versus learned judgment;
6. Source/order/time/verifier perturbation gap.

---

## A5 — Evidence Integration: from evidence-conditioned outputs to causal reliance

### Core claim under test

> Post-training improves evidence-conditioned generation and some local evidence-reliance behaviors, but current evidence does not establish necessary, process-level, transferable causal evidence integration.

| Evidence role | Papers | What was actually tested | What remains unestablished |
|---|---|---|---|
| Supporting local output reliance | ClashEval, Source-balancing | Correct external assertions can change answers; false external assertions can sometimes be resisted; source/order controls are stronger than endpoint QA | Process-level reliance and realistic multi-document integration |
| Supporting evidence-conditioned generation | RAFT, BRIEF, CARE, GuarantRAG | Distractor filtering, proposition compression, retrieval spans, joint decoding and context-conditioned answer improvements | Necessary-hop deletion, evidence entailment, source replacement and causal reasoning dependence |
| Supporting structured composition | TRACE the Evidence, BRIEF | Explicit multi-hop chains or complementary proposition coverage | A chain can be structurally grounded yet post-hoc; necessary evidence and cross-environment reliance remain untested |
| Diagnostic separation | FRANQ | Factuality and faithfulness are annotated and estimated separately; factually correct but retrieval-unfaithful outputs are not treated identically to false outputs | FRANQ is a diagnostic, not evidence that the generator learned causal evidence use |
| Counterevidence to attribution-as-faithfulness | Evaluation of Attribution Bias in Generator-Aware RAG | Counterfactual authorship metadata changes attribution quality by 3–18% and induces human-authorship bias | Citation/attribution can be a shortcut; attribution quality alone cannot establish content-based evidence reliance |
| Diagnostic evidence | MIRAGE | Context sensitivity and token/document attribution through post-hoc internal diagnostics | Saliency is not causal process reliance; no necessary-evidence re-generation test |
| Not established | Current A5 map | Output changes and context-conditioned behavior are increasingly measurable | Necessary-evidence leave-one-out, irrelevant-evidence invariance, process-level causal reliance, and behavior transfer |

### Required prose distinction

A5 should explicitly separate:

1. **Factuality:** is the answer true?
2. **Faithfulness:** is the answer entailed by the retrieved context?
3. **Output reliance:** does the answer react to evidence replacement?
4. **Attribution sensitivity:** does citation change for content reasons rather than metadata?
5. **Compositional reliance:** does removing a necessary hop remove the corresponding conclusion?
6. **Process-level reliance:** does reasoning causally depend on the evidence?

The safe conclusion is:

> Existing work supports evidence-conditioned output behavior and increasingly precise diagnostics. It does not yet prove that post-training creates a generalizable policy whose reasoning causally depends on necessary evidence rather than answer shortcuts, context matching, or source metadata.

### Recommended A5 subsection order

1. Parametric–external conflict and belief revision;
2. Multi-evidence composition;
3. Factuality versus faithfulness;
4. Attribution and metadata shortcuts;
5. Output, compositional and process-level reliance;
6. Necessary-evidence and environment-transfer gap.

---

## Cross-section writing rule

Each A1/A4/A5 subsection should use the same four-paragraph pattern:

1. **Capability definition:** what the model would need to learn;
2. **Representative positive evidence:** what methods improve and what behavior is observed;
3. **Counterevidence and diagnostic limits:** what the strongest controls reveal or fail to reveal;
4. **Scope-limited conclusion:** the strongest statement supported by the current evidence, followed by the missing adjudicating test.

This structure is stronger than a method catalog because it prevents a benchmark gain from silently becoming a capability claim.

## References

[1]: https://aclanthology.org/2024.acl-long.403/ "What Evidence Do Language Models Find Convincing?"
[2]: https://arxiv.org/abs/2506.08500 "DRAGged into Conflicts: Detecting and Addressing Conflicting Sources in Search-Augmented LLMs"
[3]: https://aclanthology.org/2026.findings-acl.338/ "Faithfulness-Aware Uncertainty Quantification for Fact-Checking the Output of Retrieval-Augmented Generation"
[4]: https://aclanthology.org/2025.findings-acl.1087/ "Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models"
