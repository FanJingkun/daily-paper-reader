---
title: Decentralized primal-dual actor-critic with entropy regularization for safe multi-agent reinforcement learning
title_zh: 面向安全多智能体强化学习的去中心化原始-对偶演员-评论家与熵正则化
authors: "Yifan Hu, Junjie Fu, Guanghui Wen"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=luS9zeDpeO"
tags: ["query:mcd"]
score: 5.0
evidence: 面向同构系统去中心化安全MARL，采用原始-对偶演员-评论家与熵正则化
tldr: 针对同构多智能体系统的安全强化学习问题，形式化定义带约束的齐次马尔可夫博弈，并证明策略共享可保持最优性。在此基础上提出去中心化原始-对偶演员-评论家算法，结合局部梯度更新与一致性更新，无需中心化训练器，同时最大化团队平均收益和策略熵并满足安全约束。该工作为同构系统的分布式安全决策提供了收敛性保证。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 同构多智能体系统需要在满足安全约束下协同优化团队收益，集中式训练不可行。
method: 提出去中心化原始-对偶演员-评论家算法，结合一致性更新与熵正则化。
result: 算法无需中心化训练器，且具有渐近收敛保证。
conclusion: 为同构多智能体系的安全策略学习提供了分布式求解方案。
---

## Abstract
We investigate the decentralized safe multi-agent reinforcement learning (MARL) problem based on homogeneous multi-agent systems, where agents aim to maximize the team-average return and the joint policy's entropy, while satisfying safety constraints associated to the cumulative team-average cost. A mathematical model referred to as a homogeneous constrained Markov game is formally characterized, based on which policy sharing provably preserves the optimality of our safe MARL problem. An on-policy decentralized primal-dual actor-critic algorithm is then proposed, where agents utilize both local gradient updates and consensus updates to learn local policies, without the requirement for a centralized trainer. Asymptotic convergence is proven using multi-timescale stochastic approximation theory under standard assumptions. Thereafter, a practical off-policy version of the proposed algorithm is developed based on the deep reinforcement learning training architecture. The effectiveness of our practical algorithm is demonstrated through comparisons with solid baselines on three safety-aware multi-robot coordination tasks in continuous action spaces.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：在多智能体强化学习（MARL）中，许多实际场景（如多机器人协同）要求智能体在满足安全约束的前提下最大化团队收益。同时，集中式训练（如中心化训练-去中心化执行）在某些分布式场景中不可行，需要完全去中心化的训练范式。
- **核心问题**：针对**同构多智能体系统**，如何在无中心化训练器的条件下，学习一种安全策略——既最大化团队平均回报和策略熵，又满足关于累积平均成本的约束。
- **整体含义**：论文形式化定义了“同构约束马尔可夫博弈”，并证明了策略共享在该安全MARL问题中不损失最优性，从而为分布式安全决策提供了理论依据和算法框架。

## 2. 论文提出的方法论

- **核心思想**：采用**原始-对偶（primal-dual）**框架处理安全约束，结合**演员-评论家（actor-critic）**结构进行策略学习，并引入**熵正则化**鼓励探索；通过**一致性更新（consensus updates）**实现去中心化训练。
- **数学建模**：
  - 定义“同构受约束马尔可夫博弈”（Homogeneous Constrained Markov Game），其中所有智能体具有相同的动力学和奖励/成本结构。
  - 证明在小队平均回报、熵和成本约束下，**共享策略**可保持最优性，这是去中心化策略学习的关键前提。
- **算法流程**：
  1. 每个智能体维护本地策略参数（actor）和价值/成本估计（critic）。
  2. 使用**局部梯度更新**优化自身策略，并在相邻智能体间进行**一致性更新**，使策略参数趋于一致。
  3. 利用拉格朗日乘子（对偶变量）将约束转化为无约束优化问题，在训练中同时更新乘子以满足安全约束。
  4. 基于**多时间尺度随机近似（multi-timescale stochastic approximation）**理论证明渐近收敛性。
- **技术扩展**：给出一个基于深度强化学习训练架构的**离策略（off-policy）**实用版本，以提升样本效率。

## 3. 实验设计

- **场景**：使用了**三个连续动作空间中的安全感知多机器人协调任务**。
- **Baseline**：与若干“扎实的基线方法”（solid baselines）进行了比较，但摘要中未列出具体基线名称。
- **评估指标**：未在摘要中明确给出，但结合问题设定，应包含团队平均回报、策略熵、安全约束满足情况（如累积成本上界）等。
- **说明**：由于论文正文未提供，具体任务细节（如机器人数量、环境复杂度）和基线定义无法从摘要得知。

## 4. 资源与算力

- **未明确说明**：摘要和元数据中没有提及任何算力信息，包括GPU型号、数量、训练时长、运行环境等。
- 仅能推断为深度强化学习训练架构，但具体计算资源未知。

## 5. 实验数量与充分性

- **实验组数**：摘要仅提及“三个任务上的对比实验”，未说明每个任务下的重复次数、消融实验或参数敏感性分析。
- **充分性评估**：
  - **优点**：覆盖了多个连续控制任务，能初步验证算法的有效性。
  - **不足**：缺少消融研究（如熵正则化的作用、一致性更新的影响、对偶变量的作用等），也未报告统计显著性、方差或收敛曲线，因而实验的客观性和全面性有限。
  - **公平性**：与“solid baselines”比较，但未列出具体方法，无法判断基线强度或是否公平对齐网络结构、超参数等。

## 6. 论文的主要结论与发现

- 实现了**无中心化训练器的去中心化安全MARL算法**，并证明其具有**渐近收敛性**。
- **策略共享**在同构安全MARL问题中是保持最优性的，这为智能体间参数一致性奠定了理论基础。
- 在连续动作空间的安全多机器人协调任务上，该算法优于对比基线，表明其实际有效性。
- 离策略版本进一步提升了实用性和训练效率。

## 7. 优点

- **理论贡献明确**：形式化建模并证明策略共享最优性，为去中心化安全MARL提供了严谨数学基础。
- **方法设计新颖**：将原始-对偶优化、演员-评论家、熵正则化和一致性更新有机结合，兼顾安全性、探索性和去中心化特性。
- **收敛性保证**：使用多时间尺度随机近似理论提供渐近收敛证明，增强了方法可信度。
- **实用性**：给出了离策略深度强化学习版本，可在复杂连续控制任务中应用。

## 8. 不足与局限

- **同构假设限制**：方法仅适用于同构智能体系统，无法直接扩展到异构系统（不同动力学、奖励或约束）。
- **实验细节缺失**：未提供具体任务描述、基线名称、超参数设置、计算资源等，可复现性和可比性受损。
- **实验范围较窄**：仅三个连续控制任务，且缺乏消融实验和规模扩展验证；对高维、大规模智能体系统的扩展性未知。
- **收敛性为渐近保证**：实际有限时间性能未分析，收敛速度未知。
- **摘要信息有限**：由于无法访问完整PDF，许多技术细节和实验数据不可得，上述总结基于摘要与元数据，可能不全面。

（完）
