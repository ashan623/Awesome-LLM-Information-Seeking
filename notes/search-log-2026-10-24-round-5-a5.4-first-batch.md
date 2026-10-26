# Search Log — Round 5A — A5.4 Cross-turn Information Persistence & State Updating

## Fixed boundary

A5.4 is a cross-turn Information-Seeking capability, not a general Agent Memory survey. Memory is an implementation substrate and evaluation setting. The focus is whether LLM agents can learn to selectively persist, revise, forget, and reuse acquired information across interactions without accumulating stale, conflicting, or irrelevant state.

The four dimensions are:

- **A5.4.1 Selective Information Persistence**
- **A5.4.2 Cross-turn State Revision**
- **A5.4.3 Future Information Utility**
- **A5.4.4 Persistence Policy Learning**

## Six verified papers

- **LongMemEval** — evaluation benchmark for long-term interactive memory, knowledge update, temporal reasoning, and abstention.
- **MemoryAgentBench** — incremental multi-turn benchmark covering accurate retrieval, test-time learning, long-range understanding, and selective forgetting.
- **Memory-R1** — PPO/GRPO post-training of a Memory Manager and Answer Agent over ADD/UPDATE/DELETE/NOOP and memory utilization.
- **MemRL** — runtime non-parametric learning of external episodic-memory utility/Q values with a frozen LLM.
- **A-MEM** — NeurIPS 2025 external-memory system with prompted note linking and memory evolution.
- **Mem0** — ECAI 2025 external-memory system with prompted fact extraction, vector/graph memory, and ADD/UPDATE/DELETE/NOOP operations.

## Important type distinction

The six papers span four distinct contribution types:

- Evaluation resources: LongMemEval, MemoryAgentBench;
- Parameter-updating capability learning: Memory-R1;
- External-memory utility learning with frozen LLM: MemRL;
- Prompted external-memory systems: A-MEM, Mem0.

MemRL should not be described as LLM parameter post-training. A-MEM and Mem0 should not be described as learned Memory Policy Learning merely because their runtime systems perform memory operations.

## New A5.4 fields

Each entry records:

- **Persistence Target:** External Memory, Context State, Policy, or LLM Parameters;
- **Temporal Consistency:** response to stale information, preference changes, conflict, and time order;
- **Future Utility Attribution:** whether later gains are caused by stored memory rather than fresh retrieval, context leakage, or parametric knowledge;
- **Memory Failure Type:** Write Failure, Update Failure, Retrieval Failure, or Utilization Failure.

## First-batch findings

The complete No Persisted State → Actual Persisted State → Oracle State ladder is absent. LongMemEval is the closest approximation through full-history, actual retrieval, and oracle-evidence conditions, but retrieval, reader prompts, time cropping, context leakage, and parametric knowledge remain confounded.

Memory-R1 is the only first-batch work with clear LLM/Agent Policy parameter post-training. Its ADD/UPDATE/DELETE/NOOP action space is well aligned with memory lifecycle behavior, but sparse final QA reward cannot prove that individual memory actions are correct. MemRL learns external-memory utility and retrieval ranking from runtime feedback while freezing the LLM; it addresses future utility more directly but does not learn complete memory formation or state revision.

LongMemEval and MemoryAgentBench expose long-term memory failures but do not provide operation-level state traces. A-MEM and Mem0 provide practical external-memory organization and lifecycle operations through prompting, function calls, similarity, and rules, but no transferable memory policy is learned.

State Revision and temporal consistency are the thinnest evidence areas. Some papers include knowledge-update, temporal QA, newer-fact priority, graph invalidation, or UPDATE/DELETE/NOOP interfaces, but no common controlled suite covers key-memory replacement, stale/expired memory, preference changes, conflict ratio, time-order permutation, or selective forgetting.

Future utility is also only partially attributable. Downstream QA, Q-success correlation, held-out transfer, and oracle-evidence comparisons are useful, but no paper combines leave-one-memory-out, fresh-retrieval control, context-length matching, memory-only evaluation, and parameter-knowledge control.

## Failure-attribution rule

Do not collapse errors into “memory retrieval failure.” Code separately:

- **Write Failure:** should-have-stored information was not persisted, or irrelevant information was stored;
- **Update Failure:** old state was not overwritten, merged, invalidated, or expired correctly;
- **Retrieval Failure:** the correct memory exists but was not recalled;
- **Utilization Failure:** the memory was recalled but the downstream reader, reasoner, or decision policy did not use it.

## Scope closure

A5.4 is closed for active expansion. Add a paper only if it directly studies LLM Policy post-training, operation-level credit assignment, counterfactual persisted-state utility, or strict cross-environment generalization. Do not expand into a general Agent Memory architecture survey.
