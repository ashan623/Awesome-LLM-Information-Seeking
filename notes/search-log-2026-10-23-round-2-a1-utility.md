# Search Log — Round 2B — A1 Retrieval Utility

## Scope

This sub-round narrows A1 from **when to retrieve** to **when information acquisition is worthwhile**. It only investigates retrieval utility, cost-sensitive evidence acquisition, value of information, and LLM-oriented retrieval evaluation.

The organizing quantity is:

\[
V_{\mathrm{retrieve}}(x) =
\mathbb{E}[U(y)\mid x,\mathrm{retrieve}]
-
\mathbb{E}[U(y)\mid x,\mathrm{no\ retrieve}]
- \lambda C_{\mathrm{retrieve}}(x).
\]

This is an analysis framework. It does not imply that the papers below all optimize this objective.

## Papers verified

- **LLM-Specific Utility for Retrieval-Augmented Generation** — core evaluation of model-specific, counterfactual passage utility.
- **When Knowledge Is Not Free: Cost-Aware Evidence Selection in Retrieval-Augmented Generation** — core evaluation of explicit evidence access cost, budget, and stopping.
- **Value of Information: A Framework for Human–Agent Communication** — supporting decision-theoretic reference; it studies user clarification rather than document retrieval.
- **Redefining Retrieval Evaluation in the Era of LLMs** — core evaluation framework for LLM-consumer utility and distraction effects.

## Classification decision

The first and second papers directly contribute to retrieval-utility analysis. The fourth contributes an LLM-oriented utility metric and evaluation framework. The third should remain supporting theory rather than being classified as a retrieval-trigger post-training method, because its information source is user clarification and not a document or search environment.

None of the four papers directly demonstrates post-training of a general LLM information-acquisition policy with a calibrated counterfactual utility reward. This is an observation to verify against broader literature, not a final claim of a new research gap.

## Main findings

Retrieval value is not only query-dependent. It is also model-dependent, evidence-dependent, task-dependent, and cost-dependent. The same passage may help one generator and distract another. More retrieval budget may be non-monotonic or harmful. A high uncertainty score does not necessarily imply that retrieval will be beneficial, and a confident answer may still benefit from corrective evidence.

The strongest current evidence is evaluation-oriented: counterfactual with/without-evidence comparison, model-specific utility sets, budget sweeps, explicit no-retrieval baselines, and utility-aware ranking. The main missing connection is from these measurements to a trainable policy with delayed reward and transfer evaluation.

## Next check before closing A1

Search specifically for methods that use counterfactual downstream utility, expected information value, or explicit cost-sensitive benefit as SFT targets, preference labels, process rewards, or RL rewards. The check should cover post-training and agentic search, but should stop if new results merely repeat answerability, uncertainty, or invocation-frequency proxies.
