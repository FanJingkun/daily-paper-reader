---
title: On the Linear Speedup of Personalized Federated Reinforcement Learning with Shared Representations
title_zh: 共享表示下个性化联邦强化学习的线性加速
authors: "GUOJUN XIONG, Shufan Wang, Daniel Jiang, Jian Li"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=BfUDZGqCAu"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 异构环境下的联邦强化学习，通过共享表示学习个性化策略
tldr: 联邦强化学习通常在异构环境下训练单一策略，导致性能不佳。本文提出PFedRL-Rep，在智能体间学习共享特征表示并输出个性化策略，兼顾协作与异构性。理论分析证明了在异构环境下的线性加速，实验验证了算法的有效性与收敛速度。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有联邦RL在异构环境下使用单一策略导致性能下降。
method: 提出PFedRL-Rep，学习共享表示并允许智能体个性化策略。
result: 理论证明异构环境下实现线性加速，实验验证性能提升。
conclusion: 共享表示与个性化结合能有效应对联邦RL中的环境异构性。
---

## Abstract
Federated reinforcement learning (FedRL) enables multiple agents to collaboratively learn a policy without needing to share the local trajectories collected during agent-environment interactions. However, in practice, the environments faced by different agents are often heterogeneous, but since existing FedRL algorithms learn a single policy across all agents, this may lead to poor performance. In this paper, we introduce a personalized FedRL framework (PFedRL) by taking advantage of possibly shared common structure among agents in heterogeneous environments. Specifically, we develop a class of PFedRL algorithms named PFedRL-Rep that learns (1) a shared feature representation collaboratively among all agents, and (2) an agent-specific weight vector personalized to its local environment. We analyze the convergence of PFedTD-Rep, a particular instance of the framework with temporal difference (TD) learning and linear representations. To the best of our knowledge, we are the first to prove a linear convergence speedup with respect to the number of agents in the PFedRL setting. To achieve this, we show that PFedTD-Rep is an example of federated two-timescale stochastic approximation with Markovian noise. Experimental results demonstrate that PFedTD-Rep, along with an extension to the control setting based on deep Q-networks (DQN), not only improve learning in heterogeneous settings, but also provide better generalization to new environments.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- 联邦强化学习（FedRL）允许多个智能体协作学习策略，而无需共享本地交互轨迹，具有隐私保护和数据隔离优势。
- 现实环境中，不同智能体面临的环境往往是异构的（heterogeneous），例如不同机器人所处物理环境、奖励结构或动态特性不同。
- 现有 FedRL 算法通常在所有智能体间学习**单一策略**，在异构环境下会导致策略性能严重下降，无法适应各智能体的本地环境。
- 论文由此提出**个性化联邦强化学习框架（PFedRL）**，核心思想是：在异构环境中，智能体之间仍可能存在**共享的公共结构（common structure）**，可以加以利用，同时为每个智能体保留个性化部分，从而兼顾协作效率与环境适应性。

## 2. 方法论：核心思想、技术细节与算法流程

### 核心思想
- 提出一类名为 **PFedRL-Rep** 的个性化联邦强化学习算法，其关键设计是：
  1. **共享特征表示（shared feature representation）**：所有智能体协作学习一个公共的特征映射，用以捕捉异构环境间的共通结构。
  2. **智能体专属权重向量（agent-specific weight vector）**：每个智能体在共享表示的基础上，维护一个针对其本地环境的个性化线性权重。
- 最终策略输出为：\( \text{动作价值或策略} = f(\text{共享表示}, \text{个性化权重}) \)，实现“共享底层、个性化上层”的解耦。

### 技术细节与算法实例
- 论文分析了一个具体实例：**PFedTD-Rep**。
  - 使用**时序差分（TD）学习**和**线性函数近似（linear representations）**。
  - 在优化上，将其建模为**带马尔可夫噪声的联邦双时间尺度随机逼近（federated two-timescale stochastic approximation with Markovian noise）**问题。
  - 通过双时间尺度更新，分别对共享表示和个性化权重进行迭代优化。
- 此外，论文还将 PFedRL-Rep 扩展到**控制设置**，基于 **Deep Q-Networks（DQN）**实现了深度版本，以处理高维状态空间。

### 算法流程（文字描述）
1. 初始化共享特征表示参数和每个智能体的个性化权重。
2. 每个智能体在本地环境中采集轨迹，使用 TD 或 DQN 损失更新本地个性化权重（快时间尺度）。
3. 周期性将本地梯度或模型更新上传至中心服务器，服务器聚合所有智能体的更新来改进共享特征表示（慢时间尺度）。
4. 服务器将更新后的共享表示下发至各智能体，重复上述过程直至收敛。

## 3. 实验设计

- 由于论文提供的文本仅为摘要，未展示详细的实验章节，因此无法获得具体的实验设计细节。
- 从摘要中可以确认：
  - 实验场景包括**异构环境设置**下的强化学习任务。
  - 评估了 **PFedTD-Rep** 以及基于 **DQN 的扩展版本**。
  - **对比方法**：摘要未列出具体 baseline，但通常应与标准 FedRL 算法（如 FedAvg 结合的 RL 方法）和单智能体独立学习等方法对比。
  - 还评估了**对新环境的泛化能力（generalization to new environments）**，说明实验不仅关注训练性能，也关注迁移能力。
- 由于缺少实验设置的完整描述，无法确知具体使用的数据集或仿真环境名称（如 Gym、MuJoCo 等），也未给出 benchmark 细节。

## 4. 资源与算力

- 论文提供的文本中**未提及任何算力信息**，包括 GPU 型号、数量、训练时长、计算集群等。
- 如需了解训练资源，需查阅论文全文的实验部分或附录；但就当前材料而言，无法总结具体资源投入。

## 5. 实验数量与充分性

- 摘要仅概述了实验结果，未列出实验数量（如多少个环境、多少组消融）。
- 从摘要表述来看，实验至少包含：
  - 异构环境下的收敛性能比较；
  - PFedTD-Rep 与 DQN 扩展的验证；
  - 新环境泛化测试。
- 由于缺少具体实验图表和数据，**无法判断实验的充分性、客观性与公平性**。但论文在理论分析之外进行了实验验证，表明作者做了初步实证支持。
- 更深层的评估（如消融实验、超参数敏感性、异构度变化的影响等）是否覆盖，需论文全文确认。

## 6. 主要结论与发现

- **理论贡献**：首次在个性化联邦强化学习设置下，证明了**关于智能体数量的线性收敛加速（linear convergence speedup）**，即随着参与智能体数量增加，收敛所需迭代次数线性减少。
- 该理论结果的关键在于将 PFedTD-Rep 建模为带马尔可夫噪声的联邦双时间尺度随机逼近问题，并给出收敛分析。
- **实验贡献**：PFedTD-Rep 及其 DQN 扩展在异构环境中不仅提升了学习性能，还表现出**对新环境的更好泛化能力**。
- 总体结论：共享特征表示与个性化权重结合，能有效解决联邦强化学习中的环境异构性问题。

## 7. 优点

- **针对性强**：直击现有 FedRL 在异构环境下性能下降的痛点，提出个性化联邦学习框架，具有明确的实用动机。
- **方法论新颖**：通过共享表示 + 个性化权重的解耦设计，在协作与个性化之间取得平衡，思路清晰。
- **理论突破**：第一个在 PFedRL 设置中证明线性收敛加速的工作，且技术路线（联邦双时间尺度随机逼近 + 马尔可夫噪声分析）具有较高的理论深度。
- **扩展性好**：从线性 TD 扩展到深度 DQN，显示框架具备从简单到复杂任务的适用性。
- **强调泛化**：实验不仅关注训练任务，还验证了对新环境的泛化，增加了实际价值。

## 8. 不足与局限

- **实验细节缺失**：当前提供的内容中缺少数据集、环境、baseline、超参数等具体信息，无法全面评估实验的全面性和公平性。
- **算力资源未披露**：未说明训练成本，不利于可复现性和资源可及性评估。
- **理论假设范围**：线性收敛加速的证明基于共享线性表示假设，对于更复杂的非线性深度表示，理论上是否仍成立尚不明确。
- **异构性范围**：论文聚焦“共享表示 + 个性化权重”的结构，但真实异构环境可能包含更复杂的差异（如动作空间、观测空间不同），该框架是否适用仍需进一步讨论。
- **隐私与通信开销**：虽然不需要共享轨迹，但联邦过程中仍需通信共享表示参数，通信效率和隐私保护强度未在摘要中讨论。

（完）
