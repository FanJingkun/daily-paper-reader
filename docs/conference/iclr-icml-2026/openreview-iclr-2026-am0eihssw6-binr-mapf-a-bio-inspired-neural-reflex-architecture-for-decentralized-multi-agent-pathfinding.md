---
title: "BINR-MAPF: A Bio-Inspired Neural-Reflex Architecture for Decentralized Multi-Agent Pathfinding"
title_zh: BINR-MAPF：面向分散式多智能体路径搜索的生物启发神经反射架构
authors: "Zhang Honglin, Jing Chen, rundong Li"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Am0EIHSSW6"
tags: ["query:maspd"]
score: 9.0
evidence: 分散式多智能体路径搜索，结合反射式避障与进化策略
tldr: 多智能体路径搜索（MAPF）在实时场景中面临可扩展性和冲突协调挑战。本文将蟑螂神经反射与群体动力学引入分散式MAPF，提出BINR-MAPF：智能体采用反应式向量场进行目标吸引与避障，通过有限状态机切换行为以应对拥塞。实验在网格地图上表明，该方法相比基线增强了可扩展性、实时性能并降低碰撞率，为生物启发式MAPF提供了新方向。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 分散式多智能体路径搜索需要实时避障和拥塞适应，现有方法在扩展性和冲突率上不足。
method: 构建生物启发神经反射架构，使用反应向量场与有限状态机，并采用集中式进化策略优化参数。
result: 在网格地图上实现更好的扩展性、实时性能和更低的碰撞率。
conclusion: BINR-MAPF为分散式MAPF提供了高效的生物启发式求解框架。
---

## Abstract
This paper presents a novel bio-inspired algorithmic framework for decentralized multi-agent path finding (BINR-MAPF), which integrates decentralized neuro-reflex behavioral models into the MAPF problem. Inspired by cockroach nervous system responses and group dynamics, we design a system where each agent employs reactive vector fields for goal attraction and collision avoidance. A finite state machine (FSM) governs behavior switching, enabling agents to adapt to local congestion and blockages. The system integrates centralized evolution strategies to optimize reflex parameters and role assignments. Experiments on grid-based maps demonstrate enhanced scalability, real-time performance, and reduced collision rates compared to baseline reactive and learning-based methods. This work bridges bio-neurological modeling and scalable swarm path finding under limited communication.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- 多智能体路径搜索（Multi-Agent Pathfinding, MAPF）在实时场景中面临**可扩展性**与**冲突协调**两大挑战。
- 现有分散式方法通常在动态、拥挤环境中难以兼顾**实时避障**与**全局效率**，且冲突率较高。
- 论文从**蟑螂神经反射系统**和**群体动力学**中获得灵感，试图将生物神经反射机制引入分散式 MAPF，从而在**有限通信**条件下实现可扩展、实时的路径搜索。
- 整体意义在于：为 MAPF 提供一种**生物启发的神经反射架构**，弥补传统反应式方法和学习式方法在鲁棒性、可扩展性上的不足。

## 2. 论文提出的方法论

### 核心思想
- 提出 **BINR-MAPF**（Bio-Inspired Neural-Reflex Architecture for Decentralized Multi-Agent Pathfinding）。
- 将**蟑螂神经反射**机制抽象为智能体的局部行为规则，结合**群体动力学**实现去中心化协同。

### 关键技术细节
- **反应式向量场（Reactive Vector Fields）**：
  - 每个智能体利用向量场实现两个基本功能：
    - **目标吸引**：生成指向目标的吸引力向量，引导智能体前进；
    - **碰撞避免**：生成排斥力向量，避开其他智能体和障碍物。
- **有限状态机（FSM）**：
  - 用于管理智能体的行为状态；
  - 根据局部环境信息（如拥堵程度、阻塞情况）在不同行为之间切换，例如“朝目标移动”“避让”“等待”等，从而适应局部拥堵和堵塞。
- **集中式进化策略（Centralized Evolution Strategies）**：
  - 在系统层面集中优化反射参数（如向量场权重、阈值）和角色分配；
  - 通过进化算法自动搜索最优参数组合，提升整体性能。

### 算法流程（文字描述）
1. 初始化一组智能体，每个智能体配置初始反射参数和状态；
2. 在每个决策时刻，智能体根据自身感知计算目标吸引力和避障排斥力；
3. 根据有限状态机判断当前行为状态，决定是否切换行为；
4. 执行运动，更新位置；
5. 在集中式训练阶段，使用进化策略评估种群表现，选择并演化参数；
6. 重复上述过程直至达到目标或满足终止条件。

## 3. 实验设计

- **场景/数据集**：
  - 实验在**基于网格的地图（grid-based maps）** 上进行。
  - 未明确具体数据集名称或地图来源，也未提及地图尺寸、智能体数量等细节。
- **基准（Benchmark）**：
  - 未明确指出使用的 MAPF 标准基准库（如 Moving AI Lab 的 MAPF 基准），仅说明使用网格地图。
- **对比方法**：
  - 基线包括：
    - **反应式方法（reactive methods）**；
    - **基于学习的方法（learning-based methods）**。
  - 未给出具体方法名称或版本。
- **评估指标**：
  - **可扩展性**；
  - **实时性能**；
  - **碰撞率（collision rate）**。

## 4. 资源与算力

- 论文中**未明确说明**使用的算力资源，包括：
  - GPU 型号与数量；
  - 训练时长；
  - 运行环境（如 CPU/GPU、内存、分布式设置等）。
- 由于涉及进化策略训练，可能依赖一定的计算资源，但文本中未提及任何具体硬件信息。

## 5. 实验数量与充分性

- 摘要中仅给出**笼统的实验结论**（BINR-MAPF 相比基线提升了可扩展性、实时性能并降低碰撞率），但**没有提供具体的实验数量、数据表格或图表**。
- 未提及消融实验、参数敏感性分析、不同地图尺寸/障碍物比例的比较实验。
- 因此，**难以判断实验的充分性和客观性**：
  - 缺乏原始数据支撑；
  - 缺乏统计显著性检验；
  - 对比方法的选取不透明，可能存在选择偏差；
  - 未展示失败案例或局限性分析。

## 6. 主要结论与发现

- BINR-MAPF 框架成功将生物神经反射模型应用于分散式 MAPF，在网格地图环境中取得优于反应式和学习式基线的性能。
- 作为一种**去中心化、有限通信**的算法，它展示了良好的可扩展性和实时性，同时降低冲突率。
- 作者认为该工作为**生物神经建模与可扩展群体路径规划**之间搭建了桥梁。

## 7. 优点

- **创新性**：将蟑螂神经系统反应机制引入 MAPF，是生物启发方法的重要尝试。
- **去中心化设计**：智能体仅依赖局部信息决策，适用于通信受限场景，具有更强的可扩展潜力。
- **自适应行为切换**：通过有限状态机应对拥堵，提高在复杂环境下的鲁棒性。
- **集中式优化与分散式执行结合**：兼顾全局参数调优与局部实时响应的优势。

## 8. 不足与局限

- **信息不完整**：本文仅为摘要/扩展摘要级别，缺乏具体的算法公式、伪代码和参数设置。
- **实验不充分**：未提供实验数据、图表、对比细节，无法验证声称的改进是否显著；缺失消融实验与可重复性所需信息。
- **基准范围有限**：仅在网格地图上测试，未涉及更复杂或更真实的地图（如仓库、室内环境、非网格连续空间）。
- **对比方法模糊**：未明确基线方法的名称、实现版本或超参数，公平性存疑。
- **未报告算力**：无法评估方法的资源开销和实际部署成本。
- **应用限制**：真实机器人系统中存在感知噪声、运动不确定性、通信延迟等问题，该研究尚未覆盖这些现实挑战。

（完）
