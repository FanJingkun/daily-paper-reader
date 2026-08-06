---
title: Heterogeneous Graph Transformers for Simultaneous Mobile Multi-Robot Task Allocation and Scheduling under Temporal Constraints
title_zh: 时序约束下基于异构图变换器的移动多机器人任务分配与调度
authors: "Batuhan Altundas, Shengkang Chen, Shivika Singh, Shivangi Deo, Minwoo Cho, Matthew Craig Gombolay"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=k1fbdnwjCH"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 基于图变换器的异构多机器人任务分配与调度
tldr: 大规模异构移动机器人团队的任务分配与调度存在可扩展性瓶颈，且现有启发式与优化方法常忽视任务依赖与智能体异构性。论文提出同时决策模型HM-MATAS，基于残差异构图表征Transformer对智能体能力、行驶时间与时空约束进行编码，并通过边级与节点级注意力实现任务分配与调度联合求解。该方法面向物流、制造与灾害响应等关键应用，具备更强的可扩展性和解质量。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有任务分配调度方法可扩展性差，且未充分利用智能体异构性与任务依赖。
method: 提出HM-MATAS模型，用残差异构图Transformer联合决策任务分配与调度。
result: 在大规模异构机器人任务中提升了可扩展性和调度最优性。
conclusion: 为异构多机器人任务分配与调度提供了一种可扩展的学习求解方法。
---

## Abstract
Coordinating large teams of heterogeneous mobile agents to perform complex tasks efficiently has scalability bottlenecks in feasible and optimal task scheduling, with critical applications in logistics, manufacturing, and disaster response. Existing task allocation and scheduling methods, including heuristics and optimization-based solvers, often fail to scale and overlook inter-task dependencies and agent heterogeneity. We propose a novel Simultaneous Decision-Making model for Heterogeneous Multi-Agent Task Allocation and Scheduling (HM-MATAS), built on a Residual Heterogeneous Graph Transformer with edge and node-level attention. Our model encodes agent capabilities, travel times, and temporospatial constraints into a rich graph representation and is trainable via reinforcement learning. Trained on small-scale problems (10 agents, 20 tasks), our model generalizes effectively to significantly larger scenarios (up to 40 agents and 200 tasks), enabling fast, one-shot task assignment and scheduling. Our simultaneous model outperforms classical heuristics by assigning 164.10\% more feasible tasks given temporal constraints in 3.83\% of the time, metaheuristics by 201.54\% in 0.01\% of the time and exact solver by 231.73\% in 0.03\% of the time, while achieving $20\times$-to-$250\times$ speedup from prior graph-based methods across scales.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- 论文针对大规模**异构移动多机器人团队**在复杂任务下的**任务分配与调度**问题，指出其存在可行解与最优解的可扩展性瓶颈。
- 现实应用包括物流、制造和灾害响应，这些场景通常要求同时考虑**任务间依赖关系**与**智能体异构性**，而现有方法往往忽略或简化这些因素。
- 传统的启发式算法、元启发式算法和基于优化的求解器在大规模场景下难以扩展，无法在合理时间内给出高质量解。

## 2. 方法论

- 提出 **HM-MATAS（Heterogeneous Multi-Agent Task Allocation and Scheduling）** 模型，实现任务分配与调度的**联合/同时决策**（Simultaneous Decision-Making）。
- 核心架构为**残差异构图 Transformer（Residual Heterogeneous Graph Transformer）**，结合**边级注意力**与**节点级注意力**。
- 模型将以下信息编码为丰富图结构：
  - 智能体能力（agent capabilities）
  - 行驶时间（travel times）
  - 时间-空间约束（temporospatial constraints）
- 训练方式：**强化学习（Reinforcement Learning）**，模型可端到端训练。
- 推理时一次性输出任务分配与调度结果（one-shot），无需迭代搜索。

## 3. 实验设计

- 训练场景：**小规模问题**，10个智能体、20个任务。
- 测试/泛化场景：**更大规模**，最多40个智能体、200个任务。
- 对比方法：
  - 经典启发式算法（classical heuristics）
  - 元启发式算法（metaheuristics）
  - 精确求解器（exact solver）
  - 先前的基于图的方法（graph-based methods）
- 主要评价指标：在时间约束下成功分配的可行任务数量、求解时间、求解速度。

## 4. 资源与算力

- **论文摘要和元数据中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。
- 仅提及模型在小型问题上训练、在大型问题上泛化，但未披露训练成本细节。

## 5. 实验数量与充分性

- 论文主要展示了一组核心对比实验（不同规模场景下与多种基线方法的比较），并报告了改进百分比和加速比。
- 未在摘要/元数据中列出详细的消融实验（如不同图结构、注意力机制、约束编码方式的消融），也未提及多个数据集或应用领域的独立验证。
- 总体而言，实验设置具有代表性，但**公开信息有限**，无法全面判断实验的完整性与公平性（例如基线调参细节、重复次数、方差等）。

## 6. 主要结论与发现

- HM-MATAS 在时间约束下可分配**164.10%**更多的可行任务，耗时仅为经典启发式的**3.83%**。
- 相比元启发式，可分配任务数量提升**201.54%**，耗时仅为其**0.01%**。
- 相比精确求解器，可分配任务数量提升**231.73%**，耗时仅为其**0.03%**。
- 与先前的基于图的方法相比，实现了**20倍到250倍**的加速。
- 模型具有良好泛化性：小规模训练即可推广到更大规模问题，兼顾解质量与速度。

## 7. 优点

- **联合决策**：同时求解任务分配与调度，避免两阶段方法的次优性。
- **异构性建模**：显式编码智能体能力和时空约束，更贴近实际场景。
- **可扩展性强**：小规模训练、大规模泛化，适合实际部署。
- **高效推理**：单次前向传播输出解，显著提升求解速度。
- **图Transformer架构**：利用边级与节点级注意力，能够捕捉复杂交互关系。

## 8. 不足与局限

- **算力信息缺失**：未说明训练所需的GPU资源与训练时长，难以评估复现成本。
- **实验细节有限**：提供的摘要中缺少数据集描述、基线实现细节、统计显著性检验等，实验充分性难以全面评估。
- **泛化范围有限**：仅验证到40个智能体、200个任务，更大规模或更复杂约束（如动态任务到达、通信限制）未涉及。
- **应用限制**：模型基于离线学习的策略，在环境分布显著变化或约束动态调整时可能需重新训练或微调。
- **与最优解差距**：尽管优于启发式和精确求解器，但缺乏与理论最优解在中小规模问题上的严格对比，无法量化最优性损失。

（完）
