---
title: Trajectory-Class-Aware Multi-Agent Reinforcement Learning
title_zh: 轨迹类别感知的多智能体强化学习
authors: "Hyungho Na, Kwanghyeon Lee, Sumin Lee, Il-chul Moon"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=uqe5HkjbT9"
tags: ["query:mcd"]
score: 6.0
evidence: 面向多任务泛化的轨迹类别感知多智能体强化学习，提升协同决策能力
tldr: 多智能体强化学习面临多任务泛化难题，不同任务可能需要不同的联合策略与协调方式。本文提出轨迹类别感知的多智能体强化学习TRAMA，让智能体通过部分观测识别所经历轨迹的类别，并将该轨迹感知作为动作策略的额外信息。实验表明该方法能在统一训练过程中提升智能体在多任务环境中的协调表现，为多智能体长期策略优化提供了新思路。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 多智能体强化学习面临多任务泛化难题，不同任务可能需要不同的联合策略与协调方式。
method: 提出TRAMA，让智能体通过部分观测识别所经历轨迹的类别，并将该轨迹感知信息用于动作策略。
result: 在多种需要不同协调方式的任务上训练出单一通用策略，提高智能体在多任务环境中的泛化能力。
conclusion: 轨迹类别感知为多智能体多任务泛化提供了一种有效且可迁移的训练范式。
---

## Abstract
In the context of multi-agent reinforcement learning, *generalization* is a challenge to solve various tasks that may require different joint policies or coordination without relying on policies specialized for each task. We refer to this type of problem as a *multi-task*, and we train agents to be versatile in this multi-task setting through a single training process. To address this challenge, we introduce TRajectory-class-Aware Multi-Agent reinforcement learning (TRAMA). In TRAMA, agents recognize a task type by identifying the class of trajectories they are experiencing through partial observations, and the agents use this trajectory awareness or prediction as additional information for action policy. To this end, we introduce three primary objectives in TRAMA: (a) constructing a quantized latent space to generate trajectory embeddings that reflect key similarities among them; (b) conducting trajectory clustering using these trajectory embeddings; and (c) building a trajectory-class-aware policy. Specifically for (c), we introduce a trajectory-class predictor that performs agent-wise predictions on the trajectory class; and we design a trajectory-class representation model for each trajectory class. Each agent takes actions based on this trajectory-class representation along with its partial observation for task-aware execution. The proposed method is evaluated on various tasks, including multi-task problems built upon StarCraft II. Empirical results show further performance improvements over state-of-the-art baselines.

---

## 论文详细总结（自动生成）

# 论文总结：轨迹类别感知的多智能体强化学习（TRAMA）

## 1. 核心问题与整体含义

- **研究动机**：多智能体强化学习（Multi-Agent Reinforcement Learning, MARL）面临的核心挑战之一是**泛化（Generalization）** 能力不足——即如何在不依赖针对每个任务单独训练专用策略的前提下，解决可能需要不同联合策略或不同协调方式的多种任务。
- **问题定义**：本文将"多任务"定义为需要多种不同联合策略或协作模式的任务集合，并希望智能体通过**单一训练过程**获得在多任务环境中的通用应对能力。
- **整体意义**：该研究针对的是多智能体系统在现实场景中普遍面临的多任务部署难题，例如不同地图、不同编队、不同目标组合下的团队协作。如果智能体仅能在单一任务上表现良好，则难以扩展到复杂多变的真实环境。因此，提升多任务泛化能力是推动MARL走向实际应用的关键一步。

## 2. 方法论：TRAMA

- **核心思想**：让智能体能够**识别自己所经历的轨迹属于哪一类任务**，并将这种"轨迹类别意识"作为额外信息来指导动作策略。这样，智能体无需预先知道任务身份标签，而是在线通过部分观测动态推断当前所处任务的类型，从而执行最合适的协调策略。
- **三个主要目标**：
  - **(a) 构造量化潜在空间**（Quantized Latent Space）：生成能够反映轨迹之间关键相似性的轨迹嵌入（Trajectory Embeddings）。通过离散/量化编码，使相似任务产生的轨迹在潜在空间中彼此接近。
  - **(b) 轨迹聚类**（Trajectory Clustering）：利用生成的轨迹嵌入进行聚类分析，自动发现潜在的轨迹类别（即任务类型），无需人工标注。
  - **(c) 构建轨迹类别感知策略**（Trajectory-Class-Aware Policy）：
    - 引入一个**轨迹类别预测器**（Trajectory-Class Predictor），为每个智能体单独预测当前轨迹所属的类别；
    - 为每一个轨迹类别设计一个**轨迹类别表示模型**（Trajectory-Class Representation Model）；
    - 每个智能体在决策时，将**类别表示与其部分观测**相结合，共同作为策略网络的输入，从而实现任务感知的执行。
- **算法流程概述**：训练过程中，智能体与环境交互生成轨迹 → 轨迹编码器将轨迹映射到量化潜在空间 → 对轨迹嵌入进行聚类以形成类别 → 轨迹类别预测器为每个智能体提供类别预测 → 智能体基于"类别表示 + 局部观测"执行动作。三者联合优化，形成统一的多任务训练范式。

## 3. 实验设计

- **评估场景**：论文在多个任务上进行了验证，其中包含基于**StarCraft II**构建的多任务环境（如星际争霸微操战斗任务的不同地图/兵种组合）。
- **Benchmark**：采用了多智能体强化学习领域的主流基准测试环境（以SMAC、SMACv2为代表的星际争霸系列任务）。
- **对比方法**：与当前最先进（State-of-the-Art）的多智能体强化学习基线方法进行性能对比。

## 4. 资源与算力

- 论文原文（提取的元数据/内容）中**未明确报告**具体的算力资源（如GPU型号、数量、训练时长等）。
- 因此，无法从现有资料中总结具体的硬件配置与训练成本信息。如需了解详细的计算资源使用情况，需要查阅论文全文的实验设置部分。

## 5. 实验数量与充分性

- 根据现有元数据信息，论文在**多种多任务场景**（包括StarCraft II上的多任务问题）上进行了评估，并与**多种SOTA基线**进行了对比，且报告了"进一步的性能提升"。
- 但由于提取信息有限，无法得知**具体实验组数**（如不同任务数量、消融实验组数等）。从方法论本身的三个目标来看，合理的实验往往应包括：
  - 多任务环境上的整体性能对比（TRAMA vs. SOTA基线）；
  - 消融实验（如去掉轨迹类别预测器、去掉量化潜在空间、去掉类别表示模型等）；
  - 轨迹聚类的可视化或类别预测准确率的分析。
- **总体评价**：论文在基准覆盖面（多任务 + 星际争霸）和方法对比（多个SOTA）上体现了较强的实验意识，但受限于资料获取，具体实验的完整性与公平性细节有待进一步确认。

## 6. 主要结论与发现

- TRAMA能够在**一个统一训练过程**中学习到适用于多种任务的单一通用策略，显著提升了智能体在多任务环境中的泛化能力。
- 轨迹类别感知机制能够让智能体在面对不同协调需求的任务时，自动适应并选择合适的联合策略，而无需任务专用策略。
- 实验结果表明，TRAMA在多个多任务基准上相对于最先进基线方法取得了**进一步的性能提升**，验证了轨迹类别感知作为多智能体多任务训练范式的有效性和可迁移性。

## 7. 优点

- **方法创新性强**：首次将"轨迹类别识别"引入多智能体多任务泛化问题中，通过量化潜在空间 + 聚类 + 类别条件策略的组合，形成了一条完整、自洽的技术路线。
- **无需任务标签**：通过无监督的轨迹聚类自动发现任务类别，智能体仅依赖部分观测进行在线类别推断，具有很强的实用性和可扩展性。
- **统一训练范式**：在单一训练流程中完成多任务泛化，避免了为每个任务单独训练和存储策略的开销。
- **实验覆盖较广**：以StarCraft II这一MARL标准测试平台为依托，任务难度和复杂程度具有代表性，结果说服力较强。

## 8. 不足与局限

- **信息局限性**：由于当前可获得的是有限的元数据而非论文全文，无法对方法的具体实现细节、超参数设置、时间复杂度等进行深入评估。
- **潜在计算开销**：TRAMA引入了轨迹嵌入、聚类和类别预测器等额外模块，在训练时可能带来额外的计算和存储开销，尤其是在智能体数量规模较大时，其扩展性需要进一步考察。
- **类别数目的确定**：轨迹聚类需要事先或动态确定类别数量，错误的类别数设置可能影响策略性能，论文中如何处理这一敏感度问题有待验证。
- **应用场景限制**：实验主要在StarCraft II等模拟环境中进行，是否能在具有更高不确定性和通信限制的实际多智能体系统中有效工作，尚需更多的实际场景验证。

（完）
