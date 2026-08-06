---
title: Sparse Topology-Aware Pairwise Scoring for Large-Scale Multi-Agent Reinforcement Learning
title_zh: 面向大规模多智能体强化学习的稀疏拓扑感知成对评分
authors: "Zhibo Deng, Feng Liang, Yong Zhang, Xiaoxi Zhang, Xiping Hu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/77ae3202268ce25e8cfac85ca989a381906d6866.pdf"
tags: ["query:mcd"]
score: 9.0
evidence: 面向大规模多智能体强化学习的可扩展通信的稀疏拓扑成对评分方法
tldr: 针对大规模多智能体通信中智能体间交互数量爆炸、可扩展性与任务适应性难以兼顾的问题，提出SOPS通信方案。该方法将通信限制在指数图骨干上，通过稀疏拓扑感知的成对评分实现任务自适应的链路分配，既保证可扩展性又保持适应性。实验验证了SOPS在大规模多智能体强化学习中的通信效率与性能优势。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 大规模多智能体通信中成对交互数量爆炸，可扩展性与任务自适应性难以兼顾。
method: 提出SOPS通信方案，利用指数图骨干约束通信范围，并通过稀疏拓扑感知成对评分实现任务自适应的链路分配。
result: 实验证明SOPS在大规模场景中显著降低通信开销并保持良好的任务表现。
conclusion: 将可扩展性与任务自适应解耦是大规摸多智能体通信设计的有效原则。
---

## Abstract
In multi-agent reinforcement learning (MARL), communication enables agents to mitigate partial observability and stochasticity through information sharing, but large-scale systems inherently lead to a rapidly growing number of pairwise interactions. Previous studies often struggle to simultaneously achieve scalability and task adaptivity in large-scale multi-agent communication. To address this challenge, we propose a scalable communication scheme for large-scale MARL, termed $\textit{Sparse tOpology-aware Pairwise Scoring}$ (SOPS). We argue that scalable MARL communication requires decoupling scalability from task-adaptive link allocation. To ensure scalability, we constrain communication to an exponential-graph backbone with a small diameter, which preserves rapid potential information mixing while keeping per-agent candidates logarithmic. On top of this constraint, we learn a task-conditioned probabilistic subgraph distribution via a pairwise scoring network over agent states and edge-type embeddings to allocate sparse links for maximizing return, optimized end-to-end through differentiable Gumbel-Sigmoid reparameterization. Evaluation results show that SOPS significantly outperforms existing state-of-the-art methods across cooperative benchmarks of diverse scales and exhibits robust zero-shot transfer capabilities.

---

## 论文详细总结（自动生成）

## 论文总结：Sparse Topology-Aware Pairwise Scoring for Large-Scale Multi-Agent Reinforcement Learning

### 一、核心问题与研究动机

- **背景**：在多智能体强化学习（MARL）中，智能体通常处于部分可观测环境中，通信是缓解不确定性和提升协作能力的关键手段。
- **核心问题**：随着智能体数量增大，智能体间的成对交互数量呈平方级增长，导致通信开销爆炸；现有研究难以同时兼顾**可扩展性（scalability）** 与**任务自适应性（task adaptivity）**。
- **研究含义**：本文旨在设计一种大规模 MARL 通信方案，使通信结构既能随智能体数量线性扩展，又能根据具体任务动态调整通信链路，从而在降低通信成本的同时保持甚至提升任务性能。

### 二、方法论：SOPS 通信方案

- **核心思想**：将**可扩展性**与**任务自适应的链路分配**解耦，作为大规模 MARL 通信设计的基本原则。
- **关键技术步骤**：
  1. **指数图骨干（exponential-graph backbone）**：将通信限制在一种指数图结构上，该结构直径小，保证信息快速混合，同时每个智能体的候选通信对数数量保持在 `O(log N)` 级别，确保通信规模可扩展。
  2. **稀疏拓扑感知成对评分网络**：在指数图骨干之上，学习一个任务条件下的概率性子图分布。该网络以智能体状态和边类型嵌入（edge-type embeddings）为输入，通过成对评分函数为每条候选边打分，进而分配稀疏通信链路。
  3. **端到端优化**：通过**可微的 Gumbel-Sigmoid 重参数化**方法，使离散的子图采样过程可微，从而支持端到端梯度优化，以最大化累积回报。

### 三、实验设计

- **Benchmark 场景**：使用了多个不同规模的协作型多智能体强化学习基准任务（cooperative benchmarks）。
- **对比方法**：与现有的最新方法（state-of-the-art）进行对比，具体包括多种已有的多智能体通信方法，作者未在摘要中逐一列出名称，但明确表示 SOPS 在各项基准上显著优于这些方法。
- **额外实验**：包含**零样本迁移（zero-shot transfer）** 实验，用于验证方法在新场景或更大规模场景下的泛化能力。

### 四、资源与算力

- 论文提供的 Markdown 元数据与摘要中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- 也未提及具体的训练轮数、运行时间或硬件环境配置。

### 五、实验数量与充分性

- 从摘要和元数据来看，实验覆盖了**多个规模不同的协作基准任务**，并进行了**与现有方法的对比实验**以及**零样本迁移实验**。
- **充分性评估**：
  - 优点：覆盖多种规模场景，验证了方法的可扩展性和泛化能力；对比了 SOTA 方法，具备一定说服力。
  - 不足：由于可用信息有限，**无法确认是否进行了系统的消融实验**（如仅去掉指数图骨干、仅去掉评分网络等情况），也未说明是否在非协作或混合博弈场景中测试。因此，实验的全面性有待完整论文进一步验证。

### 六、主要结论与发现

- SOPS 在大规模多智能体强化学习场景中**显著降低通信开销**，同时保持甚至提升任务表现。
- 方法展现出**较强的零样本迁移能力**，说明学习到的通信策略具有一定的场景泛化性。
- 作者提出：**将可扩展性与任务自适应链路分配解耦，是大规模多智能体通信设计的有效原则**。

### 七、优点与亮点

- **结构创新**：将通信限制在指数图骨架上，从理论上把每个智能体的通信候选数压缩到对数级别，具有明显的可扩展性优势。
- **任务自适应性**：通过状态与边类型嵌入进行成对评分，使通信链路分配能够随任务变化而动态调整，兼顾效率与性能。
- **端到端可训练**：使用 Gumbel-Sigmoid 重参数化解决离散采样不可微的问题，保证了整个通信策略可以联合优化。
- **通用性**：方法设计不依赖特定任务，具备迁移至多类大规模协作场景的潜力。

### 八、不足与局限

- **算力信息缺失**：未报告 GPU、训练时长等资源配置，难以评估方法的训练成本与复现难度。
- **实验细节有限**：摘要层面未列出详细的对比方法名称、具体任务设置、评价指标及数值结果，实验的透明度和可复现性需要完整论文支撑。
- **消融分析不明确**：未明确说明是否对指数图骨干、评分网络、Gumbel-Sigmoid 等关键组件进行消融研究，因而难以判断各组件对整体性能的独立贡献。
- **适用范围局限**：实验集中在协作场景，尚未验证在竞争性或混合动机场景下的表现；此外，零样本迁移虽被提及，但迁移跨度与边界条件尚未明确。
- **偏差风险**：若评测仅基于特定类型的环境，结论可能对任务分布敏感，存在一定的选择偏差风险。

（完）
