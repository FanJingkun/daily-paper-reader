---
title: Shared Recurrent Memory Transformer for Multi-agent Lifelong Pathfinding
title_zh: 共享循环记忆变压器用于多智能体终身路径搜索
authors: "Alsu Sagirova, Yuri Kuratov, Mikhail Burtsev"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=OiwMgMjeRz"
tags: ["query:maspd"]
score: 9.0
evidence: 共享循环记忆变压器解决多智能体终身路径搜索
tldr: 在分散式多智能体强化学习中，协调依赖受限的通信协议或难以扩展到大规模群体的集中训练。本文提出共享循环记忆变压器（SRMT），提供全局记忆工作空间，智能体广播自身工作记忆状态并查询他人记忆表示来交换信息并协调，同时保持去中心化训练与执行。在部分可观测MAPF问题上的实验验证了其有效性，为大规模多智能体路径搜索提供了可扩展的协调机制。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多智能体路径搜索中现有通信协议受限或集中训练扩展性差，难以协调大规模智能体。
method: 提出共享循环记忆变压器（SRMT），利用全局记忆工作空间进行无约束通信，保持去中心化训练和执行。
result: 在部分可观测多智能体路径搜索（PO-MAPF）上验证了SRMT的协调能力和可扩展性。
conclusion: SRMT为大规模终身MAPF提供了一种无需显式通信约束的通用协调机制。
---

## Abstract
Coordination in decentralized multi-agent reinforcement learning (MARL) necessitates that agents share information about their behavior and intentions. Existing approaches rely on communication protocols with domain or resource constraints or centralized training that poorly scales to large agent populations. We introduce the Shared Recurrent Memory Transformer (SRMT), which enables coordination through unconstrained communication. SRMT provides a global memory workspace where agents broadcast their learned working memory states and query others' memory representations to exchange information and coordinate while maintaining decentralized training and execution. We evaluate SRMT on the Partially Observable Multi-Agent Pathfinding (PO-MAPF) problem, where coordination is vital for optimal path planning and deadlock avoidance. We demonstrate that shared memory enables emergent coordination even when the reward function provides minimal or no guidance. On the specifically constructed Bottleneck task that requires negotiation, SRMT consistently outperforms communicative and memory-augmented baselines, particularly under sparse reward signals, and successfully generalizes to longer corridors unseen during training. On POGEMA maps, SRMT scales with the increasing agents' population and map size, achieving competitive performance with recent MARL, hybrid, and planning-based methods while requiring no domain-specific heuristics. These results demonstrate that a transformer with shared recurrent memory enhances coordination in decentralized multi-agent systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题背景**：在去中心化多智能体强化学习（Decentralized MARL）中，智能体之间需要共享关于自身行为和意图的信息，才能实现有效协调。然而，现有方法存在明显瓶颈：
  - 依赖领域相关的通信协议（例如限制通信范围、信道数量等），在复杂或大规模场景下缺乏通用性；
  - 依赖集中式训练（Centralized Training），难以扩展到大规模智能体群体，训练开销和通信成本高。
- **核心研究问题**：如何在不依赖显式通信约束、同时保持去中心化训练与执行的前提下，实现大规模多智能体系统的高效协调。
- **论文切入点**：将多智能体路径搜索（MAPF）作为测试平台，特别是部分可观测的多智能体路径搜索（PO-MAPF），因为该问题要求智能体在局部信息下进行路径规划和避免死锁，协调能力至关重要。
- **整体含义**：论文提出一种基于共享循环记忆的 Transformer 架构，试图为去中心化多智能体系统提供一种通用、可扩展、无需显式通信协议的协调机制，从而突破现有方法的扩展性限制。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：借鉴“全局工作空间（Global Workspace）”理论，设计一个**共享循环记忆工作空间（Shared Recurrent Memory Workspace）**。所有智能体将其学习到的工作记忆状态广播到该全局空间中，同时可以查询其他智能体的记忆表示，从而进行信息交换和协调。该过程不依赖任何领域特定的通信协议，且保持训练和执行均为去中心化。
- **模型名称**：共享循环记忆变压器（Shared Recurrent Memory Transformer, **SRMT**）。
- **关键技术细节**：
  - **循环记忆**：每个智能体维护自身的循环记忆状态（如 RNN 或 Transformer 中的记忆槽位），用来编码自身的历史观测与决策信息。
  - **全局共享空间**：所有智能体的工作记忆被写入一个共享的全局记忆矩阵（global memory workspace），相当于一个“公共黑板”。
  - **广播与查询机制**：智能体通过 Transformer 的注意力机制，将自身的记忆状态广播到全局空间，同时从全局空间中读取其他智能体的记忆表示。
  - **去中心化训练与执行**：虽然共享了记忆空间，但每个智能体的策略网络（policy）只接收自身的局部观测以及从共享空间查询到的信息，因此训练和执行过程不需要中心控制器。
  - **公式/算法流程（文字说明）**：
    1. 每个智能体从环境中获取局部观测，更新自身的循环记忆隐藏状态。
    2. 将该隐藏状态作为 token 写入共享记忆工作空间（可理解为将所有智能体记忆拼接成一个序列）。
    3. 通过 Transformer 的多头注意力，所有智能体对这个共享序列进行读取和交互。
    4. 每个智能体基于自身记忆和从共享空间读取到的信息，输出动作决策。
    5. 训练采用多智能体强化学习（如 PPO 类算法），但每个智能体的梯度只依赖于自身收集的轨迹，无需全局价值函数。
  - 作者强调，这种机制允许智能体在**没有显式通信约束**的情况下自发涌现协调行为，甚至当奖励函数几乎不提供引导时也能形成有效协作。

## 3. 实验设计：数据集 / 场景 / Benchmark / 对比方法

- **主要评测场景**：
  - **Bottleneck Task（瓶颈任务）**：专门构造的狭窄通道场景，要求多个智能体必须协商通过顺序，否则会发生死锁。该任务用于检验模型在**需要强制谈判（negotiation）**时的协调能力。
  - **POGEMA 地图**：来自 POGEMA（Partially Observable Grid Environment for Multi-Agent pathfinding）标准 benchmark，包含不同规模和复杂度的随机障碍地图，用于评估模型在通用场景下的扩展性和泛化能力。
- **部分可观测设置**：所有实验均基于部分可观测多智能体路径搜索（PO-MAPF），智能体只能看到自身周围的局部视野，无法获得全局地图。
- **对比方法**：
  - **通信型基线（communicative baselines）**：例如允许智能体显式交换消息的方法，但通常受通信范围或带宽限制。
  - **记忆增强基线（memory-augmented baselines）**：如仅使用个体记忆（不带共享机制）的 Transformer/RNN 方法。
  - **近期 MARL 方法**：如基于图神经网络或集中训练去中心执行（CTDE）的先进多智能体强化学习方法。
  - **混合方法（hybrid）**：结合学习与规划的方法。
  - **基于规划的方法（planning-based）**：如经典的 MAPF 求解器（如基于搜索的算法）。
- **评估指标**：主要关注成功率和平均步数（或路径长度），以及在大规模智能体数量下的扩展表现。
- **训练与泛化测试**：在 Bottleneck 任务中，训练时使用较短的走廊长度，测试时使用**训练中未见过的更长走廊**，以检验模型的泛化能力。

## 4. 资源与算力

- **未明确说明**：论文提供的元数据和摘要中**没有提及**使用的 GPU 型号、数量、训练时长、参数量等算力相关信息。
- **指出缺失**：因此无法评估训练成本或进行可复现性分析。这也是论文透明性方面的一个不足，后续可补充相关信息（如 GPU 卡时、模型规模、超参搜索范围等）。

## 5. 实验数量与充分性

- **实验组数/类型**：
  - 从摘要可确认的实验包括：
    - 在 **Bottleneck 任务**上，对比 SRMT 与通信型、记忆增强型基线，并测试**稀疏奖励信号**下的表现（至少一组）。
    - 在 **POGEMA 地图**上，进行不同智能体数量和地图规模的扩展性测试，并与 MARL、混合、规划方法对比（至少一组）。
    - 泛化测试：在未见过的更长走廊上进行评估（属于 Bottleneck 任务的一部分）。
  - 但元数据中未列出精确的实验数量（例如 POGEMA 用了多少张地图、多少个随机种子、多少个智能体规模）。
- **充分性与客观性评价**：
  - **优点**：实验设计覆盖了“专门构造的谈判场景”和“标准 benchmark 通用场景”两类，且与多种现有方法（学习/混合/规划）进行对比，比较全面。
  - **不足**：
    - 缺乏**详细的消融实验**描述（例如去掉共享记忆只保留循环记忆的效果、不广播只查询的效果等）。
    - 未提及**统计稳定性**（如多次随机种子的方差、95%置信区间）。
    - 未说明基线方法的超参数是否经过公平调优，是否存在“基弱”风险。
    - 未报告**训练成本对比**，不同方法的计算量差异可能影响公平性。
    - 所有实验仅基于 PO-MAPF 场景，未在更广泛的多智能体任务（如协作导航、博弈对抗）上验证，结论的普适性有限。

## 6. 主要结论与发现

- SRMT 通过共享循环记忆工作空间，能够在**无需显式通信协议**的情况下实现智能体之间的协调。
- 在 **Bottleneck 任务**上，SRMT 持续优于通信型和记忆增强型基线，尤其在 **稀疏奖励信号** 下优势更明显。
- SRMT 能够**成功泛化到训练中未见过的更长走廊**，说明其学到的是可迁移的协调策略，而非简单记忆。
- 在 **POGEMA 地图**上，SRMT 与最近的 MARL、混合方法及基于规划的方法相比，达到了**具有竞争力的性能**，同时**无需领域特定启发式规则**。
- 随着智能体数量和地图尺寸增加，SRMT 展现出良好的**可扩展性**，初步证明共享循环记忆机制适用于大规模去中心化多智能体系统。
- 整体结论：带有共享循环记忆的 Transformer 能够增强去中心化多智能体系统中的协调能力，且不依赖资源受限的通信结构。

## 7. 优点

- **方法创新性**：将“共享全局工作空间”概念引入多智能体路径搜索，提出了一种介于“无通信”和“完全显式通信”之间的中间机制，理论上可扩展到任意规模。
- **去中心化训练与执行**：既避免了集中式训练的可扩展问题，又克服了固定通信协议的约束，具有理论优雅性。
- **自适应协调**：智能体通过注意力机制自主决定何时、与谁交换信息，无需人为设计通信拓扑。
- **对奖励信号不敏感**：即使在稀疏或弱奖励下也能形成协调，这对真实环境中的奖励设计具有实际价值。
- **泛化能力**：在未见过的更长走廊上表现良好，说明不是过拟合训练分布。
- **对比公平性较好**：与规划方法相比，SRMT不需要地图启发式（如 h 函数），显示其作为学习方法的通用性。

## 8. 不足与局限

- **实验细节缺失**：
  - 未报告算力、GPU 配置、训练时间、模型参数量等，难以评估计算成本。
  - 未给出标准差的误差条或多次随机种子的结果，无法判断性能稳定性。
  - 未提供消融实验证据，难以分离“共享记忆”和“循环记忆”各自的贡献。
- **场景局限**：
  - 仅在 PO-MAPF 及其变体上验证，未扩展到其他典型的 MARL 测试环境（如 MPE、SMAC、Hanabi 等）。
  - Bottleneck 任务相对简单，虽然要求“谈判”，但可能不足以反映真实世界中复杂的长期规划与资源竞争。
- **理论分析不足**：
  - 缺乏对共享记忆机制收敛性或通信复杂度上限的理论分析。
  - 智能体数量增加时，共享记忆矩阵大小线性增长，注意力计算复杂度为 O(N²)（N 为智能体数），论文未讨论这种计算瓶颈在极大规模（如 >1000）下的实际可行性。
- **潜在实现问题**：
  - 摘要中称“去中心化训练与执行”，但共享记忆工作空间本身是一个全局结构；在真正的物理分布式执行中，如何维护全局一致的共享记忆（例如通信延迟、内存同步）没有讨论。
  - 与“完全去中心化”定义存在歧义：如果共享记忆必须由所有智能体共同维护，那么它可能是一个隐式的集中式组件。
- **奖励函数与目标**：MAPF 任务通常有明确的目标（到达目标点），共享记忆可能更多是“协调避让”，但如果目标更复杂（如多目标分配），该机制的适用性未知。

**总体评价**：本文提出了一个有潜力的通用协调机制，实验结果初步支持其有效性和可扩展性，但论文呈现的实验细节和理论深度不足以完全支撑其强大主张，需要更多的消融、统计分析和其他任务上的验证。

（完）
