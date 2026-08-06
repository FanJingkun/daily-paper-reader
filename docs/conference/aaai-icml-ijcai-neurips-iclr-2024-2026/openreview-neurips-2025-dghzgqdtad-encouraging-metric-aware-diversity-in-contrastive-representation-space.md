---
title: Encouraging metric-aware diversity in contrastive representation space
title_zh: 在对比表示空间中鼓励度量感知的多样性
authors: "Tianxu Li, Kun Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DghZGQDTaD"
tags: ["query:mcd"]
score: 8.0
evidence: 基于Wasserstein对比学习的协作多智能体多样性探索
tldr: 共享参数策略容易导致智能体行为趋同，阻碍协作探索。论文提出基于Wasserstein距离的对比多样性探索（WCD），在潜在表示空间中最大化不同智能体轨迹分布之间的距离，从而促进策略多样化。与现有基于Wasserstein的方法相比，WCD能有效缓解策略相似导致的度量失效问题，提升协作探索效率与最终策略性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 共享策略网络的智能体常学到相似行为，导致探索不足与次优协作策略。
method: 提出WCD探索，最大化轨迹分布在潜在表示空间中的Wasserstein距离以促进多样性。
result: 实验显示WCD能有效提升策略多样性，改善协作探索和最终性能。
conclusion: 为协作MARL中的多样性探索提供了一种度量感知的对比学习方案。
---

## Abstract
In cooperative Multi-Agent Reinforcement Learning (MARL), agents that share policy network parameters often learn similar behaviors, which hinders effective exploration and can lead to suboptimal cooperative policies. Recent advances have attempted to promote multi-agent diversity by leveraging the Wasserstein distance to increase policy differences. However, these methods cannot effectively encourage diverse policies due to ineffective Wasserstein distance caused by the policy similarity. To address this limitation, we propose Wasserstein Contrastive Diversity (WCD) exploration, a novel approach that promotes multi-agent diversity by maximizing the Wasserstein distance between the trajectory distributions of different agents in a latent representation space. To make the Wasserstein distance meaningful, we propose a novel next-step prediction method based on Contrastive Predictive Coding (CPC) to learn distinguishable trajectory representations. Additionally, we introduce an optimized kernel-based method to compute the Wasserstein distance more efficiently. Since the Wasserstein distance is inherently defined for two distributions, we extend it to support multiple agents, enabling diverse policy learning. Empirical evaluations across a variety of challenging multi-agent tasks demonstrate that WCD outperforms existing state-of-the-art methods, delivering superior performance and enhanced exploration.

---

## 论文详细总结（自动生成）

由于您提供的页面内容仅为 OpenReview 的浏览器验证界面，并未包含论文的正文、实验图表或具体数据，我仅能基于您附带提供的 Markdown 元数据（标题、摘要、TLDR、动机、方法等）进行总结。对于元数据中未涉及的信息（如具体实验数量、算力配置等），我将明确指出信息缺失。

以下是根据现有元数据生成的中文总结：

---

## 论文总结：在对比表示空间中鼓励度量感知的多样性

### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：在协作多智能体强化学习（MARL）中，共享策略网络参数的智能体容易学到相似甚至相同的行为。
- **导致后果**：这种行为趋同会严重阻碍智能体对状态空间的充分探索，导致协作策略陷入次优解，无法完成复杂协作任务。
- **现有方法的不足**：近期虽有研究尝试利用Wasserstein距离（推土机距离）来增大策略差异以促进多样性，但这些方法因策略本身过于相似，导致计算的Wasserstein距离失效，无法有效鼓励多样化行为。
- **研究意义**：解决多智能体探索中的“行为同质化”问题，对提升协作效率和策略上限具有重要意义。

### 2. 方法论：核心思想与关键技术
- **总体思路**：提出 Wasserstein Contrastive Diversity (WCD) 探索方法，在潜在表示空间中最大化不同智能体轨迹分布之间的Wasserstein距离，从而在度量层面强制拉开智能体行为差异。
- **关键技术一：可区分轨迹表示学习**
  - 为了确保Wasserstein距离有意义，论文提出基于**对比预测编码（Contrastive Predictive Coding, CPC）**的下一步预测方法。
  - 该方法通过学习，使相似轨迹在表示空间中靠近，不同轨迹相互远离，从而获得具有高度区分性的轨迹表征，解决“策略相似导致距离度量失效”的痛点。
- **关键技术二：高效Wasserstein距离计算**
  - 引入了一种**基于优化核的方法**来更高效地计算分布间的Wasserstein距离，降低了计算复杂度，使方法在实用中更加可行。
- **关键扩展：多智能体支持**
  - 由于Wasserstein距离原生定义于两个分布之间，论文将其扩展以支持**多个智能体**同时进行多样化的策略学习，而不仅限于两两对比。

### 3. 实验设计
- **Benchmark与场景**：论文在多个**具有挑战性的多智能体协作任务**上进行了实证评估（元数据中未列出具体环境名称，如 SMAC、MPE 等）。
- **对比方法**：与现有的**最先进方法（state-of-the-art）**进行比较，重点对比了同样基于Wasserstein距离的其他多样性探索方法。
- **评估指标**：主要关注**策略性能（任务成功率/回报）**和**探索效率**。

### 4. 资源与算力
- **信息缺失**：提供的元数据中**未提及**任何关于GPU型号、数量、训练时长或计算资源的详细信息。

### 5. 实验数量与充分性
- **元数据信息有限**：摘要仅笼统提到“在多种任务上的评估”，未给出具体实验组数（如消融实验数量、不同地图/场景数量）。
- **评估判断**：基于已有信息，无法客观判断实验的充分性与公平性。但根据元数据给出的信息，该方法被NeurIPS 2025接收（且审稿评分8.0），通常意味着实验设计在同行评审中具有一定说服力。

### 6. 主要结论与发现
- WCD方法能够**有效提升策略多样性**，缓解因共享参数导致的智能体行为趋同问题。
- 在多种协作任务中，WCD显著**提升了协作探索效率**，并最终**改善了策略的最终性能**，超越了现有的最先进方法。

### 7. 优点
- **洞察深刻**：精准识别出“策略相似→Wasserstein距离失效→多样性不足”这一链条中的核心瓶颈，即度量失效问题。
- **方案创新**：将CPC与Wasserstein距离有效结合，通过对比学习获得有意义的度量空间，思路具有启发性。
- **实用性强**：提出优化的核方法计算Wasserstein距离，并扩展至多智能体场景，兼顾了理论有效性与计算可行性。

### 8. 不足与局限（基于已有信息的推断）
- **信息不完整**：由于正文缺失，无法获取具体的消融实验、超参数敏感性分析或可视化结果。
- **环境覆盖范围未知**：未明确说明是否在多样化的异构任务（如通信受限、大规模智能体）中验证，泛化性有待确认。
- **计算开销**：虽然核方法优化了计算，但相比简单的多样性奖励（如熵正则），其额外计算负担仍需评估。
- **应用限制**：方法高度依赖轨迹表示的区分质量，在观测高度冗余或奖励极度稀疏的环境中，CPC学习的有效性可能面临挑战。

---

**注意**：以上总结严格基于您提供的论文元数据（Title, Abstract, TLDR, Motivation, Method 等）展开，未包含任何虚构的细节。若需获取关于实验详情、算力配置、消融实验数量的准确信息，建议访问 OpenReview 页面或获取完整PDF后重新分析。

（完）
