---
title: Federated Natural Policy Gradient and Actor Critic Methods for Multi-task Reinforcement Learning
title_zh: 面向多任务强化学习的联邦自然策略梯度与Actor-Critic方法
authors: "Tong Yang, Shicong Cen, Yuting Wei, Yuxin Chen, Yuejie Chi"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=DUFD6vsyF8"
tags: ["query:mcd"]
score: 8.0
evidence: 联邦多智能体强化学习，结合邻居通信的分散式策略优化
tldr: 针对多个分布式智能体在不共享本地数据轨迹情况下协作决策的问题，提出联邦自然策略梯度与熵正则化Actor-Critic方法。各智能体拥有私有奖励函数并共享环境转移核，仅需与图上邻居通信即可分散式地学习全局最优策略。理论分析与实验表明该方法能最大化所有智能体的折扣总收益，同时保护数据隐私。该工作为联邦多智能体强化学习提供了可收敛的优化框架。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 多个分布式智能体需在不共享本地轨迹的前提下协同决策，各自拥有私有奖励函数。
method: 提出联邦自然策略梯度和熵正则化Actor-Critic方法，智能体仅与图上邻居通信，以分散方式学习全局最优策略。
result: 在共享转移核的多任务环境中，所提方法能有效最大化所有智能体折扣总回报。
conclusion: 联邦通信机制可在保护数据隐私的条件下实现多智能体全局最优策略学习。
---

## Abstract
Federated reinforcement learning (RL) enables collaborative decision making of multiple distributed agents without sharing local data trajectories. In this work, we consider a multi-task setting, in which each agent has its own private reward function corresponding to different tasks, while sharing the same transition kernel of the environment. Focusing on infinite-horizon Markov decision processes, the goal is to learn a globally optimal policy that maximizes the sum of the discounted total rewards of all the agents in a decentralized manner, where each agent only communicates with its neighbors over some prescribed graph topology.

We develop federated vanilla and entropy-regularized natural policy gradient (NPG) methods in the tabular setting under softmax parameterization, where gradient tracking is applied to estimate the global Q-function to mitigate the impact of imperfect information sharing. We establish non-asymptotic global convergence guarantees under exact policy evaluation, where the rates are nearly independent of the size of the state-action space and illuminate the impacts of network size and connectivity. To the best of our knowledge, this is the first time that global convergence is established for federated multi-task RL using policy optimization. We further go beyond the tabular setting by proposing a federated natural actor critic (NAC) method for multi-task RL with function approximation, and establish its finite-time sample complexity taking the errors of function approximation into account.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 论文关注**联邦强化学习（Federated Reinforcement Learning, FRL）**中的多任务协作决策问题。  
- 研究背景：多个分布式智能体在**不共享本地数据轨迹**的前提下，需要协同完成决策任务，同时各自拥有**私有奖励函数**（对应不同任务），并共享相同的环境转移核。  
- 目标：在无限时域马尔可夫决策过程（MDP）中，以分散方式学习一个**全局最优策略**，最大化所有智能体折扣总奖励之和。  
- 关键约束：每个智能体只能与**图上邻居通信**，不能集中式收集数据，因此需要设计隐私友好的分布式优化算法。  
- 整体含义：该研究为“联邦多智能体强化学习”提供了理论可收敛的优化框架，拓展了强化学习在隐私保护、多任务协作场景中的适用性。

---

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

- **核心思想**：利用联邦通信机制，在保护本地数据隐私的前提下，通过策略优化方法逼近全局最优策略。  
- **方法总览**：
  - 提出**联邦 vanilla 自然策略梯度（NPG）** 方法和**联邦熵正则化 NPG**方法，均基于**softmax参数化**的表格（tabular）设置。
  - 进一步扩展到函数近似场景，提出**联邦自然 Actor-Critic（NAC）**方法。
- **关键技术细节**：
  - 使用**梯度跟踪（gradient tracking）**技术来估计全局 Q 函数，从而缓解信息不完全共享带来的偏差。
  - 智能体之间仅通过预设图拓扑与邻居交换信息，而非共享原始轨迹。
  - 在表格设置下，建立了**精确策略评估**条件下的非渐近全局收敛保证。
  - 在函数近似设置下，给出了考虑函数近似误差的有限时间样本复杂度分析。
- **算法流程（文字说明）**：
  1. 每个智能体基于本地策略与环境交互，收集奖励和状态转移数据。
  2. 本地估计 Q 函数或价值函数，并通过梯度跟踪与邻居通信，迭代更新全局 Q 函数估计。
  3. 依据自然策略梯度方向更新本地策略参数（softmax策略）。
  4. 重复上述过程直至收敛，各智能体最终得到一致的全局最优策略。
- **理论贡献点**：首次在联邦多任务 RL 中，使用策略优化方法（而非仅价值方法）建立全局收敛保证；收敛速率几乎不依赖于状态-动作空间大小，并揭示了网络规模与连通性对收敛的影响。

---

## 3. 实验设计：数据集、场景与 benchmark

- 论文提供的元数据中没有包含**具体的实验数据集、仿真环境或基准方法**的描述。
- 根据摘要推测，实验可能涉及：
  - 表格型 MDP 的多任务环境（如小型网格世界或随机 MDP 实例）。
  - 带有函数近似的连续控制或高维状态任务（可能使用标准 RL benchmark，但无法确认）。
- 对比方法可能包括：集中式 NPG、独立学习的 NPG、无梯度跟踪的联邦策略梯度变体等（未在提供材料中明确）。
- **注意**：由于 OpenReview 页面提取内容仅为验证页，缺失实验章节，无法提供具体的 benchmark 和对比方法细节。

---

## 4. 资源与算力

- 在提供的文本中，**没有明确说明使用的 GPU 型号、数量、训练时长或任何算力资源**。
- 论文更侧重于**理论分析**，实验部分可能以仿真为主，但具体配置无法从现有内容判断。
- 若读者需要算力信息，需查阅论文原文的“实验设置”部分或补充材料。

---

## 5. 实验数量与充分性

- 由于实验章节内容缺失，无法直接统计实验组数、消融实验数量或进行公平性评估。
- 从理论贡献看，作者很可能提供了多个算法变体（vanilla 与熵正则化 NPG，表格与函数近似 NAC）的验证实验，并可能包含网络拓扑变化、智能体数量变化等消融。
- **客观评价**：在缺少完整实验细节的情况下，无法断定实验是否充分、公平。建议获取原论文全文后再做评估。

---

## 6. 论文的主要结论与发现

- 在共享转移核的多任务环境中，所提出的联邦 NPG 和熵正则化 NPG 方法能够**在保护数据隐私的条件下，有效最大化所有智能体的折扣总回报**。
- 建立了**非渐近全局收敛保证**，且收敛速率与状态-动作空间大小几乎无关，说明方法具有良好的**可扩展性**。
- 收敛速率受**网络规模与连通性**影响，为设计联邦拓扑结构提供了理论指导。
- 提出联邦 NAC 方法并给出了有限时间样本复杂度，证明了在函数近似下算法依然有效。
- 总体结论：联邦通信机制可以在不共享本地轨迹的前提下，实现多智能体全局最优策略学习，为联邦多任务 RL 提供了可收敛的优化框架。

---

## 7. 优点

- **理论创新性强**：首次在联邦多任务 RL 中建立策略优化方法的全局收敛性，填补了该方向的空白。
- **隐私保护**：仅通过邻居通信而非共享数据轨迹，有效保护各智能体的本地数据隐私。
- **适用性广**：同时覆盖表格设置和函数近似设置，兼顾理论与实际应用。
- **算法设计巧妙**：引入梯度跟踪技术估计全局 Q 函数，解决了信息不共享带来的偏差问题。
- **收敛速率可解释**：揭示了网络规模与连通性对收敛速度的影响，对实际部署有指导意义。
- **框架清晰**：分别提出 vanilla、熵正则化 NPG 以及 NAC 方法，形成可选的算法族。

---

## 8. 不足与局限

- **实验细节缺失**：从提供内容看，无法获取具体实验环境、基准对比和消融实验，难以验证方法在复杂任务上的实际表现。
- **理论假设较强**：依赖“共享转移核”的假设，限制了在更一般环境（各智能体转移核不同）下的适用性。
- **精确策略评估假设**：表格设置下的收敛保证基于精确策略评估，实际中只能近似实现，需要进一步分析误差传播。
- **函数近似误差**：虽然 NAC 方法考虑了函数近似误差，但样本复杂度可能对近似误差较为敏感，实际性能受函数逼近器质量影响。
- **通信限制**：算法仅依赖邻居通信，但收敛速度可能受网络拓扑连通性制约，在稀疏图中收敛可能变慢。
- **未提及实际算力与部署成本**：缺少对大规模环境下的计算和通信开销的讨论，可能限制工程落地的参考价值。

---

（完）
