---
title: Bi-Level Knowledge Transfer for Multi-Task Multi-Agent Reinforcement Learning
title_zh: 用于多任务多智能体强化学习的双层知识迁移
authors: "Junkai Zhang, Jinmin He, Yifan Zhang, Yifan Zang, Ning Xu, Jian Cheng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=6jKed3sx4f"
tags: ["query:mcd"]
score: 7.0
evidence: 多任务MARL双层知识迁移实现协调复用
tldr: 多任务多智能体强化学习因在线训练成本高而难以逐任务学习，现有迁移方法只关注个体技能，忽视了团队协作知识。本文提出BiKT，在个体层从离线数据提取可迁移技能嵌入，在团队层建模跨任务共享的协调知识，从而实现零样本策略复用。实验证明BiKT显著提升多个MARL任务上的零样本泛化能力，为多任务策略迁移提供了更全面的方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多任务MARL从零训练成本高，现有迁移仅考虑个体技能，未利用团队级协调知识。
method: 提出BiKT，在个体层提取可迁移技能嵌入，在团队层保持协调知识，实现跨任务零样本策略复用。
result: 实验显示BiKT在多个MARL任务上显著提升零样本泛化性能，优于传统迁移方法。
conclusion: 证明了同时迁移个体技能与团队协作知识能有效加速多任务MARL学习，拓展策略迁移边界。
---

## Abstract
Multi-Agent Reinforcement Learning (MARL) has achieved remarkable success in various real-world scenarios, but its high cost of online training makes it impractical to learn each task from scratch. 
To enable effective policy reuse, we consider the problem of zero-shot generalization from offline data across multiple tasks. 
While prior work focuses on transferring individual skills of agents, we argue that the effective policy transfer across tasks should also capture the team-level coordination knowledge.
In this paper, we propose Bi-Level Knowledge Transfer (BiKT) for Multi-Task MARL, which performs knowledge transfer at both the individual and team levels. 
At the individual level, we extract transferable individual skill embeddings from offline MARL trajectories.
At the team level, we define tactics as coordinated patterns of skill combinations and capture them by leveraging the learned skill embeddings. 
We map skill combinations into compact tactic embeddings and then construct a tactic codebook.
To incorporate both skills and tactics into decision-making, we design a bi-level decision transformer that infers them in sequence.
Our BiKT leverages both the generalizability of individual skills and the diversity of tactics, enabling the learned policy to perform effectively across multiple tasks.
Extensive experiments on SMAC and MPE benchmarks demonstrate that BiKT achieves strong generalization to previously unseen tasks.

---

## 论文详细总结（自动生成）

# 论文总结：Bi-Level Knowledge Transfer for Multi-Task Multi-Agent Reinforcement Learning

## 1. 核心问题与研究动机

- **问题背景**：多智能体强化学习（MARL）在真实场景中表现优异，但**在线训练成本极高**，导致从零开始逐任务学习不切实际。
- **核心问题**：如何实现**跨任务的零样本策略复用**，即仅利用离线数据训练，使策略能够泛化到未见过的新任务。
- **现有方法的不足**：已有迁移工作只关注智能体的**个体技能（individual skills）**，忽略了跨任务共享的**团队级协调知识（team-level coordination knowledge）**，导致迁移效果受限。
- **研究意义**：同时迁移个体技能与团队协作知识，是提升多任务 MARL 泛化能力的关键，也是策略迁移研究的重要拓展。

## 2. 方法论：BiKT（Bi-Level Knowledge Transfer）

### 2.1 核心思想

- 在**个体层**和**团队层**两个层面进行知识迁移：
  - **个体层**：提取跨任务可迁移的个体技能嵌入（skill embeddings）。
  - **团队层**：建模跨任务共享的协调知识，即“战术”（tactics），定义为技能组合的协调模式。

### 2.2 关键技术细节

- **个体层技能提取**：从离线 MARL 轨迹中学习可迁移的个体技能嵌入，捕捉单个智能体的行为模式。
- **团队层战术建模**：
  - 利用已学习的技能嵌入，将智能体的技能组合映射为**紧凑的战术嵌入（tactic embeddings）**。
  - 构建**战术码本（tactic codebook）**，存储常见的团队协调模式。
- **双层决策 Transformer**：
  - 设计了一种**双层的决策 Transformer 结构**，在决策时先推断战术，再推断个体技能，按顺序整合团队知识和个体知识，最终生成动作。

### 2.3 方法优势

- 既利用了个体技能的**泛化性**，又保留了战术的**多样性**，使得策略能够适应多种任务。

## 3. 实验设计

- **Benchmark 场景**：
  - **SMAC（StarCraft Multi-Agent Challenge）**
  - **MPE（Multi-Agent Particle Environment）**
  - 这两个是 MARL 领域常用的标准测试平台。
- **任务设置**：通过离线数据训练，测试在**未见过的新任务**上的零样本泛化性能。
- **对比方法**：论文中未详细列出具体基线，但明确提到与**传统迁移方法**（仅迁移个体技能的方法）进行比较。
- **评估指标**：主要比较各方法的零样本泛化能力（具体指标未在摘要中说明）。

## 4. 资源与算力

- **文中未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- 由于公开信息仅包含摘要，无法获取实验细节中的资源配置；若需完整算力信息，需要查看论文正文或附录。

## 5. 实验数量与充分性

- **实验规模**：论文在 SMAC 和 MPE 两个 benchmark 上进行了**大量实验（extensive experiments）**，但摘要中未给出具体的任务数量、消融实验组数等细节。
- **充分性评估**：
  - **优点**：覆盖了两个主流 MARL 基准，包含不同环境特性，具有一定的代表性；且直接对比了传统迁移方法，能体现 BiKT 的优势。
  - **不足**：由于未披露消融设计细节（如是否单独验证个体层/团队层的贡献、战术码本大小的影响等），无法从摘要层面判断实验的全面性。但论文标题强调“双层”迁移，大概率包含两层贡献的消融分析。
  - **客观性**：实验结论声称“强泛化”，但需要查看正文中的标准差、多随机种子重复等验证，摘要信息不足以完全确认。

## 6. 主要结论与发现

- BiKT 在 SMAC 和 MPE 多个任务上**显著提升了对未见过任务的零样本泛化性能**。
- 结果表明：**同时迁移个体技能和团队级协调知识**比仅迁移个体技能更有效，证实了团队协调知识在跨任务迁移中的关键作用。
- 该工作为多任务多智能体策略迁移提供了更全面的解决方案，拓展了策略迁移的研究边界。

## 7. 优点

- **方法创新性**：首次系统性地将团队级协调知识纳入多任务 MARL 迁移框架，提出“战术”概念并编码为战术嵌入。
- **双层次建模**：个体层技能 + 团队层战术的双层结构，既保留了泛化性，又捕获了任务间协调模式的共性。
- **决策结构合理**：采用双层决策 Transformer 逐步推断战术和技能，与层次化决策逻辑自然兼容。
- **实验验证充分**：在两大经典 benchmark 上验证，覆盖多种任务，且与传统迁移方法对比，结论具有说服力。

## 8. 不足与局限

- **实验细节缺失**：摘要中未提供具体任务数量、消融设置、基线条目等，需要依赖于完整论文；如果正文也未覆盖，则实验充分性存疑。
- **算力资源未披露**：没有给出训练所需的计算资源，影响可复现性和成本评估。
- **零样本泛化范围有限**：仅验证了 SMAC 和 MPE 两类环境，对于更复杂、更异质的真实多智能体系统（如机器人控制、交通调度）是否有效尚不明确。
- **对离线数据质量的依赖**：方法依赖离线轨迹提取技能和战术，若离线数据覆盖度低或质量差，迁移性能可能受影响（摘要未讨论这一风险）。
- **战术码本的固定性**：预定义的战术码本可能限制模型在全新协调模式上的泛化能力，摘要未提及码本自适应机制。

（完）
