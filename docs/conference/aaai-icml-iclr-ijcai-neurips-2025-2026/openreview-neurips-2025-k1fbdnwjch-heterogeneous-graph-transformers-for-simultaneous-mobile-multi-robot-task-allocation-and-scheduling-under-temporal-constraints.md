---
title: Heterogeneous Graph Transformers for Simultaneous Mobile Multi-Robot Task Allocation and Scheduling under Temporal Constraints
title_zh: 面向时间约束的移动多机器人任务分配与调度的异构图Transformer
authors: "Batuhan Altundas, Shengkang Chen, Shivika Singh, Shivangi Deo, Minwoo Cho, Matthew Craig Gombolay"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=k1fbdnwjCH"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 异构多机器人时间约束下的任务分配与调度
tldr: 异构移动机器人团队的任务分配与调度在物流、制造等领域至关重要，但传统方法难以在大规模场景下兼顾任务依赖和智能体差异。本文提出HM-MATAS，一个基于残差异构图Transformer的同步决策模型，同时处理任务分配与时间约束下的调度，利用节点与边注意力编码能力、旅行时间和时空信息。实验表明该方法在可扩展性和任务完成质量上优于现有求解器与启发式方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大规模异构移动多机器人任务分配与调度存在可扩展瓶颈，传统方法难以处理任务依赖与异构能力。
method: 提出HM-MATAS，基于残差异构图Transformer同时决策任务分配与调度，编码能力、旅行时间和时间约束。
result: 在合成与真实场景任务上，HM-MATAS在求解质量与扩展性上优于启发式和优化求解器。
conclusion: 为异构多机器人任务规划提供了一种可学习的同步决策模型，支持复杂时间约束场景应用。
---

## Abstract
Coordinating large teams of heterogeneous mobile agents to perform complex tasks efficiently has scalability bottlenecks in feasible and optimal task scheduling, with critical applications in logistics, manufacturing, and disaster response. Existing task allocation and scheduling methods, including heuristics and optimization-based solvers, often fail to scale and overlook inter-task dependencies and agent heterogeneity. We propose a novel Simultaneous Decision-Making model for Heterogeneous Multi-Agent Task Allocation and Scheduling (HM-MATAS), built on a Residual Heterogeneous Graph Transformer with edge and node-level attention. Our model encodes agent capabilities, travel times, and temporospatial constraints into a rich graph representation and is trainable via reinforcement learning. Trained on small-scale problems (10 agents, 20 tasks), our model generalizes effectively to significantly larger scenarios (up to 40 agents and 200 tasks), enabling fast, one-shot task assignment and scheduling. Our simultaneous model outperforms classical heuristics by assigning 164.10\% more feasible tasks given temporal constraints in 3.83\% of the time, metaheuristics by 201.54\% in 0.01\% of the time and exact solver by 231.73\% in 0.03\% of the time, while achieving $20\times$-to-$250\times$ speedup from prior graph-based methods across scales.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与研究动机

- **核心问题**：大规模异构移动多机器人团队在时间约束下，如何同时进行任务分配与调度，以获得可行且高效的任务执行计划。
- **研究背景**：物流、制造、灾害响应等关键应用场景中，需要协调大量异构移动智能体完成复杂任务，但可行且最优的任务调度存在严重的**可扩展性瓶颈**。
- **现有方法缺陷**：
  - 传统启发式方法难以保证解的质量，且难以处理复杂的任务依赖关系。
  - 基于优化的精确求解器在大规模场景下计算代价过高，难以实时应用。
  - 已有方法往往**忽略任务间的相互依赖**以及**智能体的异构能力**差异。
- **研究动机**：设计一种能够同时决策任务分配与调度、可扩展到大规模场景、且能编码智能体异构性与任务依赖关系的同步决策模型。

## 2. 方法论

- **模型名称**：HM-MATAS（Heterogeneous Multi-Agent Task Allocation and Scheduling）。
- **核心思想**：基于**残差异构图 Transformer**（Residual Heterogeneous Graph Transformer），利用**节点级与边级注意力机制**，将任务分配与调度构建为一个**同步决策问题**，并通过**强化学习**进行训练。
- **关键技术细节**：
  - **图表示**：将多机器人任务分配与调度问题编码为异构图的丰富表示，节点和边分别携带智能体能力、任务属性以及智能体-任务之间的交互信息。
  - **时间与空间信息编码**：显式编码**旅行时间（travel times）** 与**时空约束（temporospatial constraints）**，使模型能够感知任务执行的时间窗口与空间位置关系。
  - **残差结构**：引入残差连接，提升深层Transformer的训练稳定性与表征能力。
  - **同步决策**：区别于传统先分配后调度的级联式方法，模型在一次前向传播中同时输出任务分配和调度方案，实现端到端联合优化。
  - **训练范式**：采用强化学习训练，在**小规模问题（10个智能体、20个任务）** 上训练。
- **算法流程**（文字描述）：输入智能体能力特征、任务特征及环境时空信息 → 构建异构图结构（智能体节点、任务节点及边） → 经残差异构图Transformer进行节点与边的消息传递与注意力聚合 → 输出任务分配策略与执行顺序（调度方案） → 通过奖励信号进行策略梯度优化。

## 3. 实验设计

- **训练规模**：在小规模问题（10 个智能体、20 个任务）上训练。
- **测试规模**：可泛化至高达 **40 个智能体、200 个任务**的大规模场景。
- **Benchmark**：合成场景与真实场景任务。
- **对比方法**：
  - **经典启发式方法（classical heuristics）**；
  - **元启发式方法（metaheuristics）**；
  - **精确求解器（exact solver）**；
  - **基于图的方法（prior graph-based methods）**。
- **评估指标**：
  - 在时间约束下可分配/可执行的**可行任务数量**；
  - **求解耗时**（计算效率）；
  - 不同规模下的**加速比**。

## 4. 资源与算力

- 论文提供的信息中**未明确说明**所使用的 GPU 型号、数量、训练时长等具体算力配置，仅强调模型在**小规模问题（10 agents、20 tasks）上训练**，即可泛化到大规模场景，体现了较好的样本效率和训练成本可控性。

## 5. 实验数量与充分性

- **实验规模跨度**：从 10 智能体/20 任务到 40 智能体/200 任务，涵盖多个规模档位，验证了模型的**可扩展性**。
- **对比维度**：与三类代表性基线（经典启发式、元启发式、精确求解器）进行对比，同时与**已有基于图的方法**进行速度对比，实验维度较为全面。
- **客观性评估**：
  - 论文报告了量化的性能提升数据（可行任务量提升百分比和加速比），结果描述清晰。
  - 受限于提供的材料仅为 abstract 和元数据，**无法确认是否包含消融实验**（如对各组件的贡献分析）、不同时间约束密度下的对比、以及多次随机种子下的方差报告等详细信息。

## 6. 主要结论与发现

- **效果领先**：在时间约束下，HM-MATAS 比经典启发式方法多分配 **164.10%** 的可行任务，同时仅花费 **3.83%** 的时间。
- **对元启发式方法**：可行任务量提升 **201.54%**，耗时仅为 **0.01%**。
- **对精确求解器**：可行任务量提升 **231.73%**，耗时仅为 **0.03%**。
- **速度优势**：相比之前的基于图的方法，在多个规模上实现了 **20× 到 250×** 的加速。
- **泛化能力**：小规模训练即可实现大规模场景的快速一次性分配与调度，说明模型学习到了可迁移的组合优化策略。

## 7. 方法亮点与优点

- **同步决策机制**：同时解决任务分配与调度，避免了级联式方法中局部最优和误差累积的问题。
- **异构建模能力**：显式建模 agent 的能力差异，利用异构图 Transformer 节点/边级注意力，精确刻画异构智能体与任务之间的匹配关系。
- **时间与空间约束的融合**：将旅行时间和时空约束直接编码进图表示中，模型输出天然满足时间可行性的调度方案。
- **出色的可扩展性与推理速度**：小规模训练 + 大规模泛化 + 单次前向推理输出结果，具备极强的实时部署潜力。
- **强化学习框架**：无需标注数据，奖励函数即可引导模型学习复杂约束下的调度策略，适应性强。

## 8. 不足与局限

- **实验信息不完整**：受限于可获取的材料（abstract 和元数据），无法确认是否进行了组件消融实验（如残差结构、边注意力、时空编码的各自贡献），以及是否在更多真实场景上验证。
- **泛化边界未充分讨论**：模型在 10 agents/20 tasks 训练后扩展到 40 agents/200 tasks 已获验证，但更极端规模（如数百个 agent、上千个任务）的表现仍属未知。
- **时间约束类型覆盖**：摘要中提及时间约束，但未明确区分硬时间窗、软时间窗、任务截止期等不同类型的约束，对复杂约束的适应能力有待进一步考察。
- **训练复杂度**：异构图的规模随智能体与任务数量增长，构建完整图表示的显存开销和训练时间成本未被讨论。
- **部署风险**：小规模训练模型在真实物理复杂环境（如通信延迟、动态障碍、机器人故障等）中的鲁棒性缺乏分析，存在**sim-to-real gap** 的风险。

---

（完）
