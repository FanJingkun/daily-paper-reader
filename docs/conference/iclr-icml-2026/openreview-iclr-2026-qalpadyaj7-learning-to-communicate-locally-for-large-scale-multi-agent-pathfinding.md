---
title: Learning to Communicate Locally for Large-Scale Multi-Agent Pathfinding
title_zh: 学习面向大规模多智能体路径查找的局部通信
authors: "Alsu Sagirova, Anton Andreychuk, Yuri Kuratov, Konstantin Yakovlev, Aleksandr Panov, Alexey Skrynnik"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=QaLpADYaJ7"
tags: ["query:maspd"]
score: 10.0
evidence: 面向大规模多智能体路径查找的可学习求解器结合局部通信
tldr: 本文将大规模多智能体路径查找建模为从单智能体视角的Dec-POMDP，在强化/模仿学习基础上引入局部通信机制，使智能体能够交换受限邻居信息，从而在共享环境中高效协调避免冲突。方法面向物流和搜索救援等应用，在可扩展性和求解质量上优于现有可学习方法，展示了局部通信对可扩展MAPF的价值。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 大规模MAPF是NP难问题，现有可学习方法缺乏智能体间通信，难以协调避免冲突。
method: 在Dec-POMDP框架下采用局部通信增强的去中心化RL/IL求解器，让智能体共享局部观测信息。
result: 在多种大尺度MAPF任务上取得优于现有去中心化方法的成功率与可扩展性。
conclusion: 局部通信是提升大规模MAPF学习求解器协调能力的关键。
---

## Abstract
Multi-agent pathfinding (MAPF) is a widely used abstraction for multi-robot trajectory planning problems, where multiple homogeneous agents move simultaneously within a shared environment. Although solving MAPF optimally is NP-hard, scalable and efficient solvers are critical for real-world applications such as logistics and search-and-rescue. To this end, the research community has proposed various decentralized suboptimal MAPF solvers that leverage machine learning. Such methods frame MAPF (from a single agent perspective) as Dec-POMDP when at each time step an agent has to decide an action based on the local observation and typically solve the problem via reinforcement learning or imitation learning. We follow the same approach but additionally introduce a learnable communication module tailored to increase the level of cooperation between the agents via efficient feature sharing. We present the Local Communication for Multi-agent Pathfinding (LC-MAPF), the method that applies multi-round communication between the neighboring agents to exchange information and improve their coordination. Our experiments show that the introduced method outperforms the existing learning-based MAPF solvers, including IL and RL based approaches, across diverse metrics in a diverse range of (unseen) test scenarios. Remarkably, the introduced communication mechanism does not compromise the scalability LC-MAPF, which is a common bottleneck for communication-based MAPF solvers.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义
- **研究动机**：多智能体路径查找（MAPF）是多机器人轨迹规划的核心抽象，广泛应用于物流、搜索救援等真实场景。MAPF 最优求解是 NP-hard 问题，因此需要可扩展且高效的次优求解器。
- **背景问题**：现有基于学习的去中心化 MAPF 求解器通常将单智能体视角的 MAPF 建模为 Dec-POMDP，智能体仅依赖局部观测决策，**缺乏智能体间通信**，导致在共享环境中协调不足、冲突频发。
- **核心问题**：如何在不牺牲可扩展性的前提下，通过学习机制引入智能体间信息共享，提升大规模 MAPF 的协调能力和求解质量。

## 2. 方法论
- **核心思想**：在 Dec-POMDP 框架下，为基于强化学习（RL）或模仿学习（IL）的去中心化求解器增加一个**可学习的局部通信模块**，使相邻智能体能够在决策前交换有限信息，从而增强协作、避免冲突。
- **方法名称**：LC-MAPF（Local Communication for Multi-agent Pathfinding）。
- **关键技术细节**：
  - 智能体在每一步基于局部观测和接收到的邻居信息进行动作决策。
  - 通信采用**多轮交互**机制，在邻居之间多次交换特征，逐步融合更高阶的邻近状态信息。
  - 通信模块端到端可学习，与策略网络联合训练，不依赖手工设计的消息协议。
  - 设计上刻意控制通信范围与特征维度，以**避免通信开销成为可扩展性瓶颈**——这是已有通信式 MAPF 求解器的常见问题。
- **问题建模**：延续单智能体视角的 Dec-POMDP 建模方式；每个时间步智能体依据局部观测（及通信得到的特征）选择动作，目标是通过 RL/IL 学习近似最优去中心化策略。

## 3. 实验设计
- **数据集/场景**：在**多种大规模、未见过的测试场景**上评估，覆盖多样化地图布局（具体地图类型、尺寸、智能体数量在摘要中未明确列出）。
- **Benchmark**：采用 MAPF 领域的常规测试协议，衡量成功率、扩展性等多样指标（具体指标名称未在摘要中逐一列出）。
- **对比方法**：
  - 现有基于学习（learning-based）的 MAPF 求解器；
  - 包括**模仿学习（IL）** 和**强化学习（RL）** 两类方法；
  - 特别强调与已有**通信式 MAPF 求解器**进行对比，以验证其可扩展性优势。
- **评估方式**：在训练时未见过的测试场景中进行泛化测试，强调方法的泛化能力而非仅拟合训练分布。

## 4. 资源与算力
- 论文摘要和元数据中**未明确说明**具体使用的 GPU 型号、数量、训练时长或计算资源预算。
- 仅能推断该方法涉及深度 RL/IL 训练，需要一定规模的并行环境采样，但具体算力细节缺失。

## 5. 实验数量与充分性
- **实验组数**：从摘要描述看，至少包含：
  - 与 IL 方法对比；
  - 与 RL 方法对比；
  - 与通信式方法对比（用于验证可扩展性）；
  - 在**多种**不同测试场景上的性能测试；
  - 通信机制的可扩展性分析（通信不会损害扩展能力）。
- **充分性评估**：
  - **积极方面**：对比了多类代表性方法，并覆盖多样化场景，能初步证明方法的优越性和泛化性；专门评估了可扩展性，针对通信式方法的瓶颈进行了验证，实验设计有针对性。
  - **不足**：摘要中未报告具体实验数量、地图类型、智能体规模范围、指标数值、消融实验（如通信轮数、通信范围、特征维度的敏感性）等细节。缺少消融分析来单独验证通信模块的贡献程度（尽管与无通信方法对比可间接体现）。
  - **公平性**：无法从摘要判断超参数调优、训练预算对齐、计算资源是否一致等公平性问题，需要阅读全文确认。

## 6. 主要结论与发现
- LC-MAPF 在多种未见过的测试场景中，**在多项指标上优于现有基于 IL 和 RL 的 MAPF 求解器**。
- 引入局部通信能显著提升智能体间的协调能力，从而更有效地避免冲突。
- 该方法**不牺牲可扩展性**，打破了“通信机制导致计算瓶颈”的常见局限，使其适用于大规模 MAPF 问题。

## 7. 优点
- **方法层面**：
  - 将通信机制与 Dec-POMDP 框架有机结合，问题建模清晰。
  - 采用多轮局部通信，融合邻居信息，提升协作能力，同时限制通信范围以保持可扩展性。
  - 通信模块可学习，无需手动设计消息格式，具备灵活性。
- **实验层面**：
  - 对比了 IL、RL 及通信式方法，形成多维度比较。
  - 在未见场景上测试，关注泛化能力。
  - 专门检验可扩展性，回应了通信式求解器的核心痛点。

## 8. 不足与局限
- **实验覆盖有限**：摘要未给出具体场景配置、智能体数量、地图规模、成功率等量化结果，无法全面评估方法的实际性能边界。
- **消融不足**：未明确报告对通信模块本身的消融实验（如移除通信、改变轮数、邻居数量、消息维度等），难以精确量化通信对性能的贡献。
- **偏差风险**：仅对比学习方法，未与传统经典 MAPF 求解器（如优先级搜索、基于冲突的搜索 CBCA 等）比较，也无法确定与最先进最优求解器的质量差距。
- **应用限制**：方法假设通信可靠、无延迟、无噪声，未考虑真实机器人通信受限、带宽有限或动态拓扑等复杂情况；仅涉及同质智能体，未讨论异构场景。
- **算力信息披露不足**：训练成本、硬件需求不透明，影响可复现性和实际部署评估。

（完）
