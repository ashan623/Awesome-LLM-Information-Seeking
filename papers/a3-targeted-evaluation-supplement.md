# A3 — Targeted Evaluation Supplement

本补充收录两篇直接服务于 A3 `Information Space Exploration` 的论文，不新增能力分支；GISA补强真实信息寻求行为评价，SearchOS-V1提供覆盖状态与失败恢复的系统边界证据。

## A3-08 GISA: A Benchmark for General Information-Seeking Assistant

- **Authors / year:** Yutao Zhu, Xingshuo Zhang, Maosen Zhang, Jiajie Jin, Liancheng Zhang, Xiaoshuai Song, Kangzhi Zhao, Wencong Zeng, Ruiming Tang, Han Li, Ji-Rong Wen, Zhicheng Dou, 2026.
- **Venue / links:** arXiv:2602.08543v2; [abstract](https://arxiv.org/abs/2602.08543v2) · [HTML](https://arxiv.org/html/2602.08543v2) · [code](https://github.com/RUC-NLPIR/GISA).
- **Role / contribution:** Evaluation / A3 behavioral anchor.
- **Primary sub-direction:** A3.2 Exploration and useful coverage; secondary A3.3 unseen/realistic information-seeking evaluation.
- **What it measures:** 373 human-crafted information-seeking queries, four structured answer formats, a live subset, and complete human search trajectories. The benchmark exposes planning, broad aggregation, query refinement, browsing depth, answer organization and efficiency failures.
- **Evidence value:** GISA gives A3 a more realistic evaluation target than query diversity alone. Its trajectory comparison shows that current agents can issue more search queries while browsing fewer pages than humans; higher tool-call counts do not guarantee higher information coverage or answer quality.
- **Capability isolation:** Strong for measuring search behavior and coverage-related failure modes; it is not a post-training intervention and does not isolate a learned exploration policy from the backbone, prompt, tools or search backend.
- **Generalization:** Includes stable/live questions and multiple model/product families, but does not establish transfer to an unseen search backend or information-space structure.
- **Boundary:** The benchmark establishes that useful exploration, deep browsing and broad aggregation are measurable and remain difficult. It does not prove that any particular training signal teaches transferable A3 exploration.
- **Survey placement:** Section 3 A3 after `Agentic Search in the Wild`, as a benchmark that operationalizes useful coverage and trajectory-level exploration. In the cross-capability matrix, classify as `Evaluation`, not `Learning`.

## A3-09 SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration

- **Authors / year:** Yuyao Zhang, Junjie Gao, Zhengxian Wu, Jiaming Fan, Jin Zhang, Shihan Ma, Yao Yao, Weiran Qi, Chuyan Jin, Guiyu Ma, Xingzhong Xu, Kai Yang, Ji-Rong Wen, Zhicheng Dou, 2026.
- **Venue / links:** arXiv:2607.15257v1; [abstract](https://arxiv.org/abs/2607.15257v1) · [HTML](https://arxiv.org/html/2607.15257v1) · [code](https://github.com/antins-labs/SearchOS).
- **Role / contribution:** Boundary / system-state evidence.
- **Primary sub-direction:** A3.2 coverage-state navigation; secondary A6 feedback recovery and stopping.
- **What it contributes:** SearchOS externalizes search progress through a Frontier Task, Evidence Graph, Coverage Map and Failure Memory. Its analyses show that explicit unresolved-region tracking and middleware interventions can restore progress after repetitive search loops; schema planning is also adapted to the discovered information topology.
- **Evidence value:** It supplies concrete operationalizations for `useful coverage`, `unresolved region`, `false saturation` and `failure recovery`, strengthening the evaluation protocol proposed for A3. It also demonstrates that some apparent exploration gains can arise from external state management, scheduling and skills rather than from a changed LLM search policy.
- **Capability isolation:** Limited for learned A3 capability. The reported gains are system-level and combine state representation, multi-agent scheduling, middleware governance, skills and model behavior. The ablations do not establish that the underlying LLM learned transferable exploration.
- **Generalization:** Evaluated on WideSearch and GISA and includes repeated/stalled trajectories, but does not provide train-on-one/test-on-unseen information-space or tool-inventory behavior transfer for a learned policy.
- **Boundary:** Strong system evidence that explicit coverage state and failure memory matter; not evidence that post-training alone creates an A3 policy.
- **Survey placement:** Section 3 A3 after GISA as a system-level boundary contrast. In the cross-capability matrix, classify as `Boundary`, not `Learning`.

## Synthesis

These two papers sharpen the A3 argument without expanding its scope:

> **Useful exploration is measurable at the trajectory/state level, and external coverage state can improve search systems; however, neither observation establishes that post-training has produced a transferable policy for discovering necessary information regions in unfamiliar spaces.**

They should therefore support the A3 evaluation and boundary discussion, while MARAG-R1 and DDPO remain the main learning anchors.
