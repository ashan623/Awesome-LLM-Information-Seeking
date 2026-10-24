# Round 9 — Claim–Evidence Matrix

## 1. Purpose

Cross-Paper Matrix answers **what each paper establishes**. This matrix answers a different question:

> **Which cross-capability claims are supported, weakened, or still not adjudicated by the current evidence?**

每条 Claim 都拆成四种证据角色：

- **Supporting Evidence:** 论文结果直接支持该 Claim 在某个明确范围内成立；
- **Counterevidence:** 论文提供了与 Claim 相反或更强的正面证据，说明 Claim 不能被写成普遍断言；
- **Partial Mitigation:** 论文缓解了该问题的一部分，或在一个受限维度上提供了更直接的证据，但不能完全关闭 Claim；
- **Not Established:** 论文结果与 Claim 相关，但没有完成判断该 Claim 所需的行为隔离/迁移实验。

因此，`Supporting` 不等于“这篇论文的方法失败”，`Counterevidence` 也不等于“这篇论文解决了泛化问题”，`Partial Mitigation` 也不等于“问题已被解决”。它们只说明该论文在 Claim 的证据结构中扮演什么角色。

## 2. Claim–Evidence Matrix

| Claim ID | Cross-capability claim under test | Supporting Evidence | Counterevidence | Partial Mitigation | Not Established | Required adjudicating test | Current scope-limited conclusion |
|---|---|---|---|---|---|---|---|
| **C1 Objective–Capability Misalignment** | **Training signals often improve a behavior that overlaps with the claimed capability but do not fully measure the capability itself.** | **A2-02 DeepRetrieval** directly improves Recall/NDCG/SQL execution but does not test semantic intent; **A2-05 StepSearch** uses search-key F1 and TF-IDF support gain but lacks query-only intervention; **A2-06 Environment-aware IR** optimizes passage-ID nDCG and shows retriever dialect dependence; **A2-07 QREF** improves recovered/preserved entity coverage but does not establish intent preservation; **A4-06 EAPO** couples evidence utility to answer reward; **A5-08 BRIEF** uses reader-likelihood proposition selection; **A6-07 AutoSearch** uses Gold-derived optimal depth; **A6-11 TRACE-turn** uses Gold-answer state value; **A6-12 SSPO** uses privileged Evidence Anchors. These are positive results whose optimized object is narrower than the high-level capability claim. | **A1-12 CoBRA** provides paired no-tool/tool margins that directly target tool-use boundary; **A1-13 Calibrate-Then-Act** explicitly conditions action on cost and calibrated priors; **A4-05 RLSeek** directly trains source-span support/refute behavior; **A5-06 Source-balancing** directly measures both accepting correct external evidence and resisting false evidence; **A6-05 Beneficial Reasoning Behaviors** labels verification, authority evaluation and error recovery before RL. These show that alignment can be improved when the behavior is explicitly defined and measured, even though transfer remains incomplete. | **A2-07 QREF**, **A1-12 CoBRA**, **A4-02 S2G-RAG**, **A4-05 RLSeek**, **A6-05 Beneficial Reasoning Behaviors**: explicit query, boundary, sufficiency, verification or behavior labels move supervision closer to the intended object, but proxy dependence and transfer gaps remain. | **A2-03 ExpandSearch**, **A2-04 ParallelSearch**, **A6-02 Search-R1**, **A6-03 R1-Searcher**, **A5-01 ClashEval**, and **A5-07 RAFT** show useful endpoint or output changes, but their results alone cannot decide whether the intended semantic, causal, or control capability was acquired. | For every target behavior, measure the signal’s intended object and plausible shortcuts separately: semantic intent vs backend utility; useful coverage vs diversity; support vs citation format; evidence reliance vs answer change; feedback-conditioned action vs search depth. | The defensible claim is not “fine-grained signals fail.” It is: **more structured signals improve observability or local alignment, but capability acquisition requires a behavior-level test beyond the optimized proxy.** |
| **C2 Outcome–Behavior Confounding** | **An improved final answer or system score does not by itself identify which information-seeking behavior or system component caused the gain.** | **Retrieval, Reward, and Training Protocols** shows corpus completeness, tool-call format and training protocol can exceed credit-algorithm differences; **A6-02 Search-R1** and **A2-03 ExpandSearch** use end-task reward for full search-and-answer loops; **A5-04 GuarantRAG** updates evidence/fusion components and reports better answers without necessary-evidence tests; **A5-14 Memory-R1** obtains downstream QA reward for memory operations without operation-level correctness; **A3-04 MARAG-R1** combines tool coordination, document coverage and reasoning; **A1-10 s3** depends on frozen generator and judge. These results support the need for module and behavior attribution. | **A1-12 CoBRA** fixes the query and compares paired no-tool/tool branches; **A2-07 QREF** compares original and refined query batches; **A4-04 RAGChecker** separates retrieval, support and noise metrics; **A5-01 ClashEval** and **A5-06 Source-balancing** replace answer-bearing assertions; **A3-01 InfoMosaic-Bench** separates tool selection/use failures; **A6-08 AnySearch** reports search count, token use, early stopping and scaffold removal; **A6-13 controlled comparison** fixes backbone, interface, corpus and protocol. These are counterevidence to the stronger statement that all existing studies are endpoint-only. | **A2-07 QREF**, **A4-04 RAGChecker**, **A5-01 ClashEval**, **A5-06 Source-balancing**, **A6-08 AnySearch**, and **A6-13 controlled comparison** add behavior, module, replacement, budget or protocol controls; each isolates only a subset of the full causal loop. | A higher EM/F1 on a new benchmark, a lower average turn count, a component ablation without a matched intervention, or a better citation score remains insufficient. Examples include **A2-01 Query Rewriting**, **A2-04 ParallelSearch**, **A5-08 BRIEF**, **A5-09 CARE**, **A6-09 CriticSearch**, and **A6-10 CW-GRPO** when interpreted only through final QA. | Use paired potential outcomes and module-fixed controls: no-information/actual/oracle state; query-only replacement; source removal; necessary-evidence deletion; fixed-history feedback swap; retriever/generator/reader controls; and variance-aware multi-sample comparisons. | The evidence supports: **endpoint gains are often compatible with several causal explanations**. The matrix also contains genuine partial counterexamples, so the Survey should say “under-isolated in many settings,” not “never isolated.” |
| **C3 Hindsight–Deployment Gap** | **A training signal may improve policy learning while relying on Gold Answer, future trajectory, oracle evidence, or realized branch outcomes unavailable when the deployed policy must act.** | **A6-09 CriticSearch** labels turns using complete future trajectories and Gold Answer; **A6-07 AutoSearch** defines optimal depth by earliest Gold match; **A6-11 TRACE-turn** scores Gold-answer prefix readiness; **A6-12 SSPO** uses Evidence Anchors and student errors for teacher supervision; **A1-11 InfoReasoner** uses Gold-answer posterior shifts; **A1-10 s3** uses post-trajectory Gain Beyond RAG; **A1-12 CoBRA** trains on realized no-tool/tool margins; **A5-07 RAFT** uses oracle documents and generated reasoning targets. These all demonstrate useful learning signals but leave information parity as a separate question. | **A1-01 When to Retrieve** deploys a current-query `<RET>` trigger; **A1-13 Calibrate-Then-Act** uses a pre-action cost/prior interface; **A6-08 AnySearch** removes explicit budget scaffolding and tests unseen budgets; **A4-02 S2G-RAG** trains a context-only sufficiency/gap judge; **A6-05 Beneficial Reasoning Behaviors** explicitly primes observable behaviors before RL. These show that training can be closer to deployment-visible state or can remove part of the scaffold. | **A1-01 When to Retrieve**, **A1-13 Calibrate-Then-Act**, **A4-02 S2G-RAG**, **A6-05 Beneficial Reasoning Behaviors**, and **A6-08 AnySearch** are closer to deployment-visible state or remove part of a scaffold. **A1-12 CoBRA** uses ex-post branch margins without giving them as direct action input. | A method that reports a deployment-time policy, a no-gold ablation, or a new benchmark without changing the information available to the policy does not by itself close the gap. In particular, **A6-09 CriticSearch**, **A6-11 TRACE-turn**, and **A6-12 SSPO** retain privileged information in training even when the deployed policy does not receive it. | Separate train-time and deployment-time inputs; run no-privilege and state-only variants; fix completed history and swap only current feedback; record next-action distributions, false stops, unnecessary continuation and failed recovery—not only final accuracy. | The defensible claim is: **hindsight supervision is useful for credit assignment, but parity and online policy conversion require direct evidence.** This is a diagnostic boundary, not a blanket rejection of privileged training. |
| **C4 Environment-conditioned Learning vs. Transferable Capability** | **A policy that succeeds in one retriever, corpus, tool inventory, feedback format or cost regime should not be called a transferable information-seeking capability without behavior-level tests under changed environments.** | **A2-06 Environment-aware IR** provides the clearest negative cross-retriever evidence: learned query dialects fail to transfer reliably; **A2-01**, **A2-03**, **A2-05**, **A2-07** remain tied to Bing/Serper/E5/Wikipedia or a fixed web stack; **A3-04 MARAG-R1** and **A3-05 DDPO** keep a fixed tool/retriever/corpus family; **A6-07 AutoSearch**, **A6-09 CriticSearch**, **A6-10 CW-GRPO**, and **A6-11 TRACE-turn** mostly vary tasks or benchmarks under fixed search ecology; **A4/A5** methods usually keep source ecology, reader and evidence format fixed. These patterns support the need to separate environment-conditioned utility from transfer. | **A1-02 KBM** tests multiple target LLMs and knowledge settings; **A2-07 QREF** transfers across Agent Backbones; **A3-06 Transferable Tool Selection** transfers across tool repositories and BM25/COLT; **A6-08 AnySearch** transfers from training budgets up to five to unseen budgets six through eight; **A1-12 CoBRA** includes an initial vertical music-agent setting; **A3-04 MARAG-R1** transfers to unseen QA tasks; **A5-04 GuarantRAG** reports several retrievers and model scales. These are real transfer results that prevent the claim from becoming “nothing transfers.” | **A2-07 QREF** covers Agent Backbone Transfer, **A6-08 AnySearch** covers unseen Budget, **A3-04 MARAG-R1** covers task transfer, and **A3-06 Transferable Tool Selection** covers tool-repository/retriever transfer; each is a partial mitigation on one axis, not a general environment proof. | New benchmark names, new backbone sizes, multiple datasets under the same retriever, or unseen budget alone are not sufficient for C4. **A2-07 QREF** is Agent Backbone Transfer, not Retriever Transfer; **A6-08 AnySearch** is Budget Generalization, not Tool/Environment OOD; **A3-06 Transferable Tool Selection** is strong tool discovery transfer, but not information acquisition. | Use a transfer grid: train environment A and test environment B across task, domain, retriever, tool, corpus, feedback format, budget and temporal state; re-measure the target behavior in B. Distinguish `Task Transfer — Final Outcome Only` from `Behavior Transfer`. | The current evidence supports a cautious claim: **task and budget transfer are common enough to be meaningful, while Retriever/Tool/Corpus/Behavior Transfer remains sparse and capability-specific.** |

## 3. Role assignment by claim

| Paper / row ID | C1 Objective–Capability | C2 Outcome–Behavior | C3 Hindsight–Deployment | C4 Environment–Transfer | Why it is useful |
|---|---|---|---|---|---|---|---|
| A2-05 StepSearch | Supporting | Supporting | Supporting | Supporting | Fine-grained query/evidence signal with lexical and gold-support proxy limits |
| A2-07 QREF | Partial Mitigation | Partial Mitigation | Supporting | Partial Mitigation | Original-versus-refined query comparison is the strongest current A2 approximation, but transfer is backend-limited |
| A4-02 S2G-RAG | Partial Mitigation | Partial Mitigation | Partial Mitigation | Supporting | Context-only sufficiency and missing-gap supervision, bounded by gold-title/Wikipedia structure |
| A5-06 Source-balancing | Counterevidence | Partial Mitigation | Partial Mitigation | Supporting | Bidirectional correct/incorrect external assertion test; synthetic-domain scope remains |
| A6-08 AnySearch | Counterevidence | Partial Mitigation | Partial Mitigation | Counterevidence for Budget only; Not Established for Retriever/Tool Transfer | Strongest unseen-budget evidence in the current pool |
| A6-09 CriticSearch | Supporting | Supporting | Supporting | Supporting | Cleanest hindsight credit versus online-control distinction |
| A6-11 TRACE-turn | Supporting | Partial Mitigation | Supporting | Supporting | Controlled turn-credit ablations plus Gold-conditioned signal |
| A6-12 SSPO | Partial Mitigation | Supporting | Supporting | Supporting | Step-level self-distillation and Evidence Anchor parity case |
| A6-13 Controlled Training Protocols | Supporting | Supporting | Partial Mitigation | Supporting | Controls corpus, reward, optimizer and protocol simultaneously |
| A1-12 CoBRA | Partial Mitigation | Counterevidence locally | Partial Mitigation | Partial Mitigation | Strong paired boundary isolation, but not full VOI transfer |
| A3-06 Transferable Tool Selection | Counterevidence to “nothing transfers” | Partial Mitigation | Partial Mitigation | Counterevidence for tool-repository transfer, not information acquisition | Important boundary contrast |

## 4. What the matrix does and does not establish

### Established with relatively strong support

1. Post-training and inference-time methods can change **specific observable information-seeking behaviors**: retrieval triggering, query dialect, evidence-span support, stopping depth, budget allocation, source balancing, or memory operations.
2. More structured signals can improve **method effectiveness, behavior observability, or credit assignment** in controlled settings.
3. Some transfer exists: task OOD, model/backbone transfer, vertical/domain shifts, and unseen-budget transfer are not absent.
4. Environment variables—especially corpus coverage, retriever dialect, tool inventory, reward format and budget—can materially change the apparent result.

### Not established by the current matrix

1. A general, calibrated A1 value-of-information capability that predicts retrieval benefit before acting;
2. A semantic-intent-preserving A2 query policy that also adapts to unseen retrievers;
3. A3 policy that discovers useful evidence regions under unfamiliar source inventories or knowledge structures;
4. A4 source-reliability/conflict judgment policy that survives source replacement, time shift and verifier changes;
5. A5 process-level causal evidence reliance that survives necessary-evidence deletion and environment change;
6. An A6 policy that changes its next action correctly under a fixed-history current-feedback intervention;
7. A full-life-cycle A5.4 memory policy with operation-level correctness and causal future-utility attribution.

These are evidence boundaries, not claims that the underlying capabilities are impossible or that current methods are ineffective.

## 5. Traceability rule for the synthesis chapter

The Cross-Capability Synthesis must cite row IDs from this file. It should not write “the literature shows” without specifying whether the cited evidence is:

```text
Supporting Evidence
Counterevidence
Partial Mitigation
Not Established
Counterevidence
Evidence Not Sufficient
```

The chapter should therefore make bounded statements such as:

> **QREF establishes a stronger original-versus-refined coverage signal than final-answer-only query training, but its fixed Serper stack and lack of semantic-intent intervention leave transferable query learning unestablished.**

rather than:

> **QREF proves that LLMs learn general query formulation.**
