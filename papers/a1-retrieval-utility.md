# A1 Extension — Retrieval Utility, Value of Information, and Cost

This sub-pool extends A1 from **“When should the model retrieve?”** to the stricter question:

> **When is acquiring external information worthwhile for the target model, task, evidence state, and retrieval cost?**

The distinction is important:

- A model can be uncertain while retrieval is unlikely to help.
- A model can be confident while being wrong and therefore benefit substantially from retrieval.
- Retrieval can improve answer quality while the gain is too small to justify latency, tokens, API calls, or monetary cost.

A useful analysis target is:

\[
V_{\mathrm{retrieve}}(x) =
\mathbb{E}[U(y)\mid x,\mathrm{retrieve}]
-
\mathbb{E}[U(y)\mid x,\mathrm{no\ retrieve}]
- \lambda C_{\mathrm{retrieve}}(x).
\]

Here, \(U\) is downstream task utility, \(C\) is retrieval cost, and \(\lambda\) controls the cost–quality trade-off. This equation is an organizing framework for the survey, not a claim that all papers below optimize it directly.

The final verification of utility-aware policy-learning methods is maintained in [`a1-utility-policy-learning.md`](./a1-utility-policy-learning.md). That sub-pool confirms that direct training methods already exist and focuses the remaining question on ex-ante calibration and cross-environment transfer.

## Core and supporting papers

### U1. LLM-Specific Utility for Retrieval-Augmented Generation

The first arXiv version used the title **LLM-Specific Utility: A New Perspective for Retrieval-Augmented Generation**. The current arXiv record is version 4.

- **Authors / year:** Hengran Zhang et al., 2025; current arXiv v4 revised 2026; CIKM 2026
- **Links:** [arXiv v4](https://arxiv.org/abs/2510.11358) · [current HTML](https://arxiv.org/html/2510.11358v4) · [code](https://github.com/Trustworthy-Information-Access/LLM_specific_utility) · [DOI](https://doi.org/10.1145/3799682.3840666)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary relation to A1:** Retrieval Utility and model-dependent evidence value
- **Utility definition:** For target LLM \(L\), query \(q\), and passage \(d\), the paper defines utility by the marginal answer-accuracy change \(u(d;L,q)=Acc(L(q,d))-Acc(L(q,\emptyset))\). A passage is useful for that LLM when the evidence-conditioned answer improves over the same model without the passage.
- **Counterfactual retrieval benefit:** **Strong.** The same LLM is compared with and without evidence, and the paper constructs LLM-specific utilitarian evidence sets.
- **Cost awareness:** **Limited.** The study does not include a latency, token, API, or monetary cost objective.
- **Training / evaluation:** Evaluation and benchmark construction. It evaluates Llama and Qwen generators, model-specific utility labels, human evidence, retrieved evidence, and utility-aware selection/ranking. It does not train a retriever or LLM retrieval policy.
- **Training–capability alignment:** No policy is trained. The paper provides a direct measurement target that could supervise future retriever or trigger learning, but current utility judgments are not themselves a learned search capability.
- **Generalization regimes:** Multiple generator models, QA datasets, model-specific evidence sets, cross-LLM evidence transfer, and known-versus-unknown queries. Retriever-family and non-QA environment transfer remain limited.
- **Key finding:** The same passage can have different utility for different LLMs. Each generator generally performs best with its own model-specific utilitarian evidence, and evidence that is relevant in a traditional retrieval sense can still hurt a model’s answer. This directly supports the claim that retrieval value is **model-dependent** and **evidence-dependent**, not only query-dependent.
- **Main limitation:** Utility is mainly binary and pointwise, based on answer accuracy. It does not model passage synergy, calibration, attribution, robustness, latency, or cost. It also does not train an end-to-end personalized retrieval policy.
- **Research opportunity:** Use model-specific counterfactual utility as supervision for a cost-sensitive retrieval policy, then test whether the policy transfers to new models, tools, domains, and changing corpora.

### U2. When Knowledge Is Not Free: Cost-Aware Evidence Selection in Retrieval-Augmented Generation

- **Authors / year:** Mingyan Wu, Han Yang, Omer Ben-Porat, Yftah Ziser, 2026
- **Links:** [arXiv v1](https://arxiv.org/abs/2606.02245v1) · [code](https://github.com/Mignonmy/Cost-Aware)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary relation to A1:** Cost-sensitive evidence acquisition and stopping
- **Utility definition:** Candidate evidence has relevance value and an access cost. The system selects evidence under a budget using fixed, knapsack, redundancy-aware, MMR, or prompted agent policies; downstream answer quality is measured against the cost of the acquired evidence.
- **Counterfactual retrieval benefit:** **Strong.** The study includes no-retrieval baselines, fixed retrieval budgets, varying budgets, and per-query comparisons between answer quality and acquired evidence cost.
- **Cost awareness:** **Strong.** It explicitly models source/access tiers and budget constraints, and shows that more retrieval budget can be non-monotonic or harmful.
- **Training / evaluation:** Inference-time evaluation only. No retriever-policy training, reward-model training, RL, or end-to-end fine-tuning is reported. The prompted agent decides whether to retrieve, which tier to access, and when to answer.
- **Training–capability alignment:** No learned policy is trained. The paper diagnoses the capability and provides a cost-sensitive evaluation setting rather than demonstrating post-training acquisition of that capability.
- **Generalization regimes:** Open-domain QA, medical QA, two backbone families, multiple budgets, shared versus per-query budgets, and alternative cost mappings. Tool and corpus diversity remain limited.
- **Key finding:** No fixed selector dominates across datasets, models, and budgets. A zero-shot agent can sometimes improve the quality–cost trade-off by stopping early, while another model may spend substantially more without gains. This makes retrieval utility explicitly **model-dependent**, **task-dependent**, and **cost-dependent**.
- **Main limitation:** Access costs are simulated domain-level tiers rather than observed latency, licensing, token, or API costs. The candidate pool and original query are mostly fixed; query rewriting, multi-query search, verification, and learned policy optimization are not central.
- **Research opportunity:** Convert the cost-sensitive setting into a post-training environment with per-action rewards, counterfactual evidence utility, delayed credit assignment, and explicit transfer across source tiers and search tools.

### U3. Value of Information: A Framework for Human–Agent Communication

- **Authors / year:** Yijiang River Dong et al., ACL 2026
- **Links:** [ACL Anthology](https://aclanthology.org/2026.acl-long.1987/) · [arXiv](https://arxiv.org/abs/2601.06407) · [code](https://github.com/dong-river/VOI_communication)
- **Role:** Supporting / Theory
- **Contribution type:** Evaluation / decision-theoretic framework
- **Primary relation to A1:** Expected Value of Information as a decision rule for active information acquisition
- **Utility definition:** For a candidate question \(q\), the paper defines the expected increase in downstream action value after possible answers as \(VoI(q)=V_{post}(b,q)-V(b)\), and uses net value \(NetVoI(q)=VoI(q)-c\) to decide whether another question is worth asking.
- **Counterfactual retrieval benefit:** **Limited.** The paper explicitly computes a counterfactual information gain, but the acquired information is a user clarification answer rather than a retrieved passage or search result.
- **Cost awareness:** **Strong.** Communication cost is explicit, and the policy stops asking when the expected gain no longer exceeds the cost.
- **Training / evaluation:** Inference-time decision framework for clarify-or-commit behavior. It does not train an LLM retrieval policy or retriever.
- **Training–capability alignment:** Not a direct retrieval training signal. It is useful as a theoretical template for future retrieval rewards: replace user intent and clarification answers with evidence state, search results, and retrieval cost.
- **Generalization regimes:** Multiple tasks and risk levels, multiple communication costs, and two LLM backbones. It does not test open-web retrieval, document evidence, search failure, latency, token, or API costs.
- **Key finding:** The agent should ask more when task risk or ambiguity is high and stop earlier when communication is expensive or the expected action improvement is small. The principle is directly relevant to calibrated stopping and value-sensitive information acquisition.
- **Main limitation:** It studies what to ask a user, not what to retrieve from an information environment. The answer space is comparatively closed, and the belief update and user response model are simulated.
- **Research opportunity:** Treat this as a theoretical supporting reference, then derive a retrieval analogue with document-level uncertainty, evidence quality, search cost, and multi-step query actions.

### U4. Redefining Retrieval Evaluation in the Era of LLMs

- **Authors / year:** Giovanni Trappolini et al., EACL 2026
- **Links:** [ACL Anthology](https://aclanthology.org/2026.eacl-long.391/) · [arXiv](https://arxiv.org/abs/2510.21440) · [code](https://github.com/GiovanniTRA/UDCG)
- **Role:** Core / Evaluation
- **Contribution type:** Evaluation
- **Primary relation to A1:** LLM-consumer utility evaluation
- **Utility definition:** The paper’s UDCG estimates passage utility for a target LLM by combining passage relevance with the model’s probability of refusing or failing to answer from that passage. It aggregates positive utility and distraction effects across a context and compares the ranking with downstream RAG answer accuracy.
- **Counterfactual retrieval benefit:** **Limited.** It evaluates LLM-consumer effects and compares evidence-conditioned behavior, but it does not define a full same-query, same-budget, with-retrieval versus no-retrieval utility difference for every retrieval action.
- **Cost awareness:** **Limited.** It discusses computational complexity and scoring cost but does not optimize monetary, latency, token, or API cost.
- **Training / evaluation:** Primarily an evaluation metric. UDCG is mostly training-free; UDCGθ learns metric weights to correlate with downstream answer rankings, but it does not train the retriever or LLM policy.
- **Training–capability alignment:** Indirect. It provides a useful LLM-oriented utility signal that could guide retriever or policy training, but the paper does not demonstrate capability learning.
- **Generalization regimes:** Multiple QA datasets, six instruction-tuned LLMs, several context sizes, and cross-dataset evaluation. Sparse/dense retriever diversity, non-QA tasks, and environment shift remain limited.
- **Key finding:** LLM-oriented utility correlates with downstream RAG answer accuracy better than classical relevance metrics in the tested settings. The key shift is from “is this passage relevant?” to “does this passage help this LLM answer, or does it distract it?”
- **Main limitation:** The utility is still passage-level and QA-centric. It does not model evidence synergy, temporal reliability, retrieval cost, or a learned search policy; the learned metric may need retraining for different context sizes.
- **Research opportunity:** Combine UDCG-style LLM-consumer utility with U1’s no-evidence counterfactual and U2’s explicit cost to form a trainable, budget-aware retrieval policy objective.

---

## Cross-paper synthesis

These papers establish a sharper decomposition of A1. The first A1 sub-pool studied **whether the model is uncertain or likely to know the answer**. This extension studies whether **acquiring information actually improves downstream utility enough to justify its cost**.

The evidence supports four dependencies:

- **Model-dependent:** the same passage can help one generator and hurt another.
- **Evidence-dependent:** utility depends on the specific passage, its distraction effect, and its interaction with the generator.
- **Task-dependent:** retrieval benefit differs across factual QA, multi-hop reasoning, medical QA, and ambiguous decision tasks.
- **Cost-dependent:** additional retrieval is not monotonically beneficial; more evidence can increase cost and introduce distractors.

The four papers do not by themselves establish that LLMs can learn a calibrated cost-sensitive acquisition policy. The follow-up verification found that direct utility-aware training methods do exist; therefore, the remaining question is not whether utility-aware training exists, but whether observed gains can become an ex-ante, cost-calibrated, transferable value-of-information policy.
