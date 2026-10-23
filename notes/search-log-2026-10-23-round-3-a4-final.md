# Search Log — Round 3B — A4 Second Batch and Closure

## Scope

This batch followed the first six A4 anchors and verified four gap-focused works. The purpose was to distinguish current literature coverage from actual post-training research gaps.

The A4 boundary remains:

- A4.1 Relevance & Support
- A4.2 Source Reliability
- A4.3 Evidence Sufficiency
- A4.4 Conflict & Robustness

A5 remains evidence integration: using already-evaluated evidence to update reasoning, beliefs, memory, generation, or decisions.

## Four verified papers

- **AuthorityBench** — direct evaluation of domain/entity/source authority perception; labels are largely PageRank and popularity proxies, with no post-training.
- **Resolving Conflicting Evidence in Automated Fact-Checking / CONFACT** — source credibility and support/refute evidence under naturally occurring conflict; credibility prediction exists, but no end-to-end verifier policy training.
- **Sufficient Context** — independent human/expert pair-level query–context sufficiency judgments, separating insufficient evidence from sufficient evidence that the generator fails to use.
- **ConfRAG** — realistic multi-source Web conflict benchmark with human-adjudicated viewpoint clusters and reason coverage; evaluates organization and coverage, not truth selection or conflict resolution.

## New A4 coding fields

Every second-batch entry records:

1. **Judgment Target:** Query–Document, Claim–Evidence, Evidence Set, or Source.
2. **Ground Truth Quality:** Human-verified, Expert, Synthetic, LLM-generated, or Weak Label, including whether it is independent of final-answer reward.
3. **Controlled Perturbation:** source authority, conflict ratio, evidence order, temporal change, retrieval noise, formatting, citation fabrication, and answer leakage controls.

These fields prevent a benchmark from receiving strong A4 credit simply because it introduces a new label or reports higher final QA accuracy.

## Final ten-paper map

The first six anchors plus these four papers provide at least one representative paper for every A4 sub-direction:

- **A4.1 Relevance & Support:** CRAG, RAGTruth, RAGChecker, RLSeek, EAPO.
- **A4.2 Source Reliability:** AuthorityBench, CONFACT.
- **A4.3 Evidence Sufficiency:** S2G-RAG, Sufficient Context, RAGChecker.
- **A4.4 Conflict & Robustness:** RAGTruth, CONFACT, ConfRAG, RAGChecker.

## Final A4 conclusion

A4 measurement is ahead of A4 post-training. Relevance, claim support, sufficiency, authority perception, and conflict organization now have evaluation resources. RLSeek and EAPO provide early evidence-grounded policy learning for A4.1; S2G-RAG provides process supervision for A4.3. In contrast, direct post-training evidence for source reliability and robust conflict evaluation remains limited.

The remaining research questions are:

1. **Measurement:** Do labels genuinely distinguish relevance, support, sufficiency, authority, and conflict, or do they collapse into answer correctness and verifier proxies?
2. **Learning:** Does SFT/RL teach evidence judgment itself, or does it teach the model to imitate external evaluators, rules, citation formats, or reward models?
3. **Generalization:** Does the learned behavior survive source replacement, evidence reordering, time shift, conflict changes, domain/tool changes, and verifier/reward gaming?

A4 is closed for broad collection at ten papers. Further A4 additions should be made only for controlled perturbation, held-out verifier robustness, or direct cross-environment post-training transfer. The next capability axis is A5 Evidence Integration.
