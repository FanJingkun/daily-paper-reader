---
title: Encouraging metric-aware diversity in contrastive representation space
title_zh: 在对比表示空间中鼓励度量感知的多样性
authors: "Tianxu Li, Kun Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DghZGQDTaD"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 通过对比Wasserstein距离促进多智能体策略多样性，支持角色特化
tldr: 在共享策略网络参数的协作多智能体强化学习中，智能体常学到相似行为，阻碍探索并导致次优策略。为此提出Wasserstein对比多样性(WCD)方法，通过最大化不同智能体轨迹分布在潜在表示空间中的Wasserstein距离来显式鼓励策略差异。实验表明该方法能有效提升多智能体多样性，并改善协作任务的学习效果，为无需显式角色标签的行为分化提供了可行的探索机制。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 共享参数的MARL智能体易产生同质化行为，抑制探索并导致次优协作策略。
method: 提出WCD探索，在潜在表示空间最大化轨迹分布间的Wasserstein距离以提升策略多样性。
result: 实验显示WCD能有效缓解策略相似问题，带来更富多样性的协作策略。
conclusion: 为协作MARL提供了一种无需角色标签的多样性驱动探索机制，促进行为分化。
---

## Abstract
In cooperative Multi-Agent Reinforcement Learning (MARL), agents that share policy network parameters often learn similar behaviors, which hinders effective exploration and can lead to suboptimal cooperative policies. Recent advances have attempted to promote multi-agent diversity by leveraging the Wasserstein distance to increase policy differences. However, these methods cannot effectively encourage diverse policies due to ineffective Wasserstein distance caused by the policy similarity. To address this limitation, we propose Wasserstein Contrastive Diversity (WCD) exploration, a novel approach that promotes multi-agent diversity by maximizing the Wasserstein distance between the trajectory distributions of different agents in a latent representation space. To make the Wasserstein distance meaningful, we propose a novel next-step prediction method based on Contrastive Predictive Coding (CPC) to learn distinguishable trajectory representations. Additionally, we introduce an optimized kernel-based method to compute the Wasserstein distance more efficiently. Since the Wasserstein distance is inherently defined for two distributions, we extend it to support multiple agents, enabling diverse policy learning. Empirical evaluations across a variety of challenging multi-agent tasks demonstrate that WCD outperforms existing state-of-the-art methods, delivering superior performance and enhanced exploration.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在协作多智能体强化学习（MARL）中，当智能体共享策略网络参数时，它们往往学习到相似的行为，导致探索效率低下，最终收敛到次优的协作策略。
- **研究背景**：
  - 近年已有方法尝试利用 **Wasserstein 距离** 来增加策略差异，以促进多智能体多样性。
  - 但这些方法面临一个关键困境：因为策略本身高度相似，生成的轨迹分布之间差异很小，导致计算出的 **Wasserstein 距离缺乏区分度**，无法有效推动策略分化。
- **整体含义**：该研究旨在解决共享参数场景下多智能体行为同质化的问题，通过一种新的“度量感知”多样性机制，实现无需显式角色标签的行为分化，从而提升协作任务的最终表现。

### 2. 论文提出的方法论（WCD 探索机制）

- **核心思想**：提出 **Wasserstein Contrastive Diversity (WCD)** 探索方法，在**潜在表示空间（latent representation space）**中最大化不同智能体轨迹分布之间的 Wasserstein 距离，从而显式地鼓励策略差异。
- **关键技术细节**：
  - **可区分的轨迹表示学习**：为克服“策略相似导致 Wasserstein 距离失效”的问题，提出了一种基于 **对比预测编码（Contrastive Predictive Coding, CPC）** 的下一步预测方法，学习具有高度区分度的轨迹表示，使 Wasserstein 距离在度量智能体行为差异时变得有意义。
  - **高效 Wasserstein 距离计算**：引入了一种**基于核的优化方法**，以更高效的方式计算 Wasserstein 距离，降低计算开销。
  - **多智能体扩展**：由于 Wasserstein 距离天然是为两个分布定义的，作者将其扩展以支持多个智能体，从而在全体智能体之间协同地促进多样化的策略学习。
- **算法流程概要**：
  1. 各智能体在环境中交互，产生各自的轨迹数据。
  2. 通过 CPC 模块学习轨迹的潜在表示，使不同行为模式的表示相互可区分。
  3. 在表示空间中，计算智能体两两之间的 Wasserstein 距离，并利用基于核的方法加速。
  4. 将最大化该距离作为多样性奖励或正则化项，与任务奖励共同优化策略。

### 3. 实验设计

- **基准场景**：在“一系列具有挑战性的多智能体任务”（a variety of challenging multi-agent tasks）上进行评估。提供的文本中未具体列出仿真环境名称（如 SMAC、MPE、Hanabi 等）。
- **对比方法**：与 **现有最先进方法（state-of-the-art methods）** 进行对比。提供的文本中未具体列出对比方法的名称（如 QMIX、MAVEN、CDS 等）。
- **评估指标**：主要关注**协作任务的最终性能**（回报/胜率）以及**探索效率**和**策略多样性**。
- **说明**：由于文本仅包含摘要，实验的具体环境、任务类型、基准方法的详细列表在提供的内容中未能体现。

### 4. 资源与算力

- **文中未明确说明**：在提供的论文文本（摘要）中，**没有提供**关于 GPU 型号、GPU 数量、训练时长、参数量级等计算资源与算力的任何信息。因此无法基于现有内容总结具体的资源消耗情况。

### 5. 实验数量与充分性

- **实验数量**：文本中提及进行了“跨多种多智能体任务的实证评估”，暗示了多个实验场景的设置，但未提供具体数量或消融实验的细节。
- **充分性与客观性评估**：
  - 从摘要看，实验涵盖了多个任务，并对比了现有 SOTA 方法，初步具备一定的说服力。
  - 但是，在提供的文本中**未提及消融实验**（如去除 CPC 表示学习的影响、去除 Wasserstein 多样性项的影响、不同核方法的对比等）。
  - 由于缺少基线方法的具体名称、环境细节和统计显著性分析（标准差、多次随机种子等），**无法完全判断实验的公平性与全面性**。需要在完整论文中核实。

### 6. 论文的主要结论与发现

- WCD 方法能有效缓解共享参数多智能体策略相似的问题，带来更富多样性的协作策略。
- 实验结果表明，WCD 在多种多智能体任务上**优于现有最先进方法**，展现出优越的性能和更强的探索能力。
- 为协作 MARL 提供了一种**无需显式角色标签**的多样性驱动探索机制，有效促进智能体的行为分化。

### 7. 优点

- **动机明确**：精准指出了现有 Wasserstein 距离方法在多智能体场景中“因策略相似而度量失效”的根本缺陷。
- **方法新颖**：将对比预测编码（CPC）与 Wasserstein 距离结合在潜在空间中，形成了一种“度量感知”的多样性信号，使距离计算在策略演化过程中保持有效。
- **工程效率考量**：提出了基于核的优化方法来计算 Wasserstein 距离，兼顾了理论效果与计算可行性。
- **无需角色标签**：不依赖预先定义的角色分工，让智能体在训练中自发分化，具备较强的泛化能力。
- **多智能体扩展**：将两两分布的 Wasserstein 距离扩展到了多智能体场景，适应协作任务的整体需求。

### 8. 不足与局限

- **实验细节缺失**：提供的文本中未列出具体的环境、基线和消融实验，难以从摘要层面判断实验的深度与广度。
- **表示学习的依赖性**：WCD 的效果很大程度上依赖于 CPC 所学轨迹表示的质量，如果表示学习失败或难以区分，多样性信号可能仍然会退化。
- **计算开销**：尽管提出核方法优化，但在智能体数量较多或轨迹长度较大时，计算轨迹 Wasserstein 距离仍可能带来较高的训练成本。
- **通用性尚待验证**：实验仅覆盖了论文作者选择的任务，未说明在非协作场景、大规模智能体群（如 >100 个智能体）或部分可观测极端场景下的表现。
- **缺乏对多样性-收敛性权衡的分析**：文本未讨论多样性最大化是否可能干扰团队收敛或任务奖励最优化等潜在副作用。
- **资源信息缺失**：全文未报告训练所需的算力资源，不利于研究者评估方法的可复现性与应用门槛。

（完）
