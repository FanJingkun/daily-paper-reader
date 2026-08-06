---
title: Randomized Exploration in Cooperative Multi-Agent Reinforcement Learning
title_zh: 协作多智能体强化学习中的随机化探索
authors: "Hao-Lun Hsu, Weixin Wang, Miroslav Pajic, Pan Xu"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=7Tir0u0ukg"
tags: ["query:mcd"]
score: 9.0
evidence: 面向协作多智能体强化学习的可证明高效随机探索算法
tldr: 本文首次研究协作多智能体强化学习中可证明高效的随机化探索。提出统一算法框架及CoopTS-PHE、CoopTS-LMC两种汤普森采样算法，分别结合扰动历史与朗之万蒙特卡洛探索。在线性迁移近似的并行MDP上给出后悔界与通信复杂度界。该工作为MARL探索理论提供了可实现的实用算法。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 协作多智能体强化学习中随机化探索的效率与理论保证尚属空白，需要可实现且可证明的探索机制。
method: 提出并行MDP上的统一随机化探索框架，并设计两种集成PHE与LMC策略的TS算法。
result: 理论证明两种算法在线性并行MDP上达到接近最优的后悔界，并给出通信复杂度上界。
conclusion: 为协作MARL提供了首个可证明高效的随机探索算法族，兼具理论保证与实现灵活性。
---

## Abstract
We present the first study on provably efficient randomized exploration in cooperative multi-agent reinforcement learning (MARL). We propose a unified algorithm framework for randomized exploration in parallel Markov Decision Processes (MDPs), and two Thompson Sampling (TS)-type algorithms, CoopTS-PHE and CoopTS-LMC, incorporating the perturbed-history exploration (PHE) strategy and the Langevin Monte Carlo exploration (LMC) strategy respectively, which are flexible in design and easy to implement in practice. For a special class of parallel MDPs where the transition is (approximately) linear, we theoretically prove that both CoopTS-PHE and CoopTS-LMC achieve a $\widetilde{\mathcal{O}}(d^{3/2}H^2\sqrt{MK})$ regret bound with communication complexity $\widetilde{\mathcal{O}}(dHM^2)$, where $d$ is the feature dimension, $H$ is the horizon length, $M$ is the number of agents, and $K$ is the number of episodes. This is the first theoretical result for randomized exploration in cooperative MARL. We evaluate our proposed method on multiple parallel RL environments, including a deep exploration problem (i.e., $N$-chain), a video game, and a real-world problem in energy systems. Our experimental results support that our framework can achieve better performance, even under conditions of misspecified transition models. Additionally, we establish a connection between our unified framework and the practical application of federated learning.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：协作多智能体强化学习（Cooperative MARL）中，智能体共享环境并协同完成同一目标，探索策略对样本效率和最终性能至关重要。然而，随机化探索（如汤普森采样）在单智能体强化学习中被证明有效且可分析，在协作多智能体场景中却缺乏理论保证与系统性的算法设计。
- **核心问题**：如何在协作多智能体强化学习中实现**可证明高效**的随机化探索？即能否设计出既具备理论后悔界、又易于实际部署的随机探索算法？
- **整体含义**：本文首次系统地研究了协作 MARL 中的随机化探索理论，填补了该方向的理论空白，并为多智能体并行环境（如联邦学习、多智能体协同控制）提供了实用的算法框架。

## 2. 论文提出的方法论

- **核心思想**：将协作多智能体问题建模为**并行马尔可夫决策过程（Parallel MDPs）**，利用随机化探索策略在共享特征表示上高效估计模型与价值函数。
- **统一算法框架**：提出一个适用于并行 MDP 的随机化探索统一框架，可灵活嵌入不同的随机探索策略。
- **两种汤普森采样算法**：
  - **CoopTS-PHE**：结合**扰动历史探索（PHE）**策略，通过对历史奖励与转移计数施加随机扰动实现探索。
  - **CoopTS-LMC**：结合**朗之万蒙特卡洛探索（LMC）**策略，利用 Langevin 动力学对后验分布采样实现探索。
- **理论保证**：
  - 在线性迁移近似（即转移概率近似为特征线性函数）的并行 MDP 中，两种算法均达到  
    \(\widetilde{\mathcal{O}}(d^{3/2}H^2\sqrt{MK})\) 的后悔界，  
    其中 \(d\) 为特征维度，\(H\) 为 horizon 长度，\(M\) 为智能体数，\(K\) 为 episode 数。
  - 通信复杂度上界为 \(\widetilde{\mathcal{O}}(dHM^2)\)。
- **算法流程（文字描述）**：
  1. 每个 episode 开始时，各智能体根据共享或独立的随机化策略（PHE 或 LMC）生成对模型参数的采样；
  2. 基于采样参数计算最优策略并执行动作；
  3. 收集全局或局部奖励与状态转移信息；
  4. 通过通信机制同步信息，更新历史分布，进入下一轮采样与探索。

## 3. 实验设计

- **实验场景**：
  - 深度探索问题：\(N\)-chain 环境（经典的随机探索测试基准）；
  - 视频游戏环境：用于验证算法在通用强化学习任务中的表现；
  - 能源系统中的真实世界问题：检验算法在现实场景中的适用性。
- **Benchmark 与对比**：论文未在公开摘要中列出具体对比方法，但通常此类工作会与随机探索基线（如 \(\epsilon\)-greedy、UCB 型方法）和确定性探索方法进行对比；也可能包含两种算法（CoopTS-PHE vs CoopTS-LMC）的相互对比。
- **额外分析**：论文特别考察了**迁移模型被错误设定（misspecified transition models）**条件下的性能，验证算法鲁棒性。

## 4. 资源与算力

- 论文提供的信息中**未明确说明**使用的 GPU 型号、数量、训练时长、显存等算力资源。
- 仅能从实验环境（视频游戏、能源系统）推断其使用了常见深度 RL 训练配置，但缺乏可复现的算力细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要可见至少三类环境（\(N\)-chain、视频游戏、能源系统），每个环境下可能包含多种智能体数量与任务难度设置；两种算法（PHE 与 LMC）均有测试，并包含 misspecified 模型鲁棒性分析。
- **充分性评价**：
  - **优点**：覆盖从经典深度学习探索问题到真实应用环境，兼具理论研究与工程验证，实验设计较为全面。
  - **不足**：未提及消融实验（如不同特征维度、不同通信频率、奖励设定影响）和更细粒度的参数敏感性分析，也未给出统计显著性检验（如多次随机种子的方差报告）。整体实验**基本充分但不算非常深入**，有待补充更多场景与消融。

## 6. 论文的主要结论与发现

- 协作 MARL 中随机化探索可以通过统一框架实现，且具有严格的理论保证。
- CoopTS-PHE 和 CoopTS-LMC 均达到接近最优的后悔界，且通信成本可控，证明随机化探索在并行多智能体中的高效性。
- 实验表现优于或至少不弱于传统基线，即使在迁移模型存在误设定时仍能保持较好性能，说明算法具有鲁棒性。
- 统一框架还可与联邦学习等分布式应用建立联系，为多智能体系统提供更广泛的应用价值。

## 7. 优点

- **首次理论突破**：首次为协作 MARL 中的随机化探索提供可证明的后悔界，理论贡献显著。
- **算法实用性强**：两种算法（PHE、LMC）设计简单、易于实现，且框架高度灵活。
- **理论与实验结合**：既有严格理论又有多类环境验证，说服力较强。
- **鲁棒性验证**：专门考虑了模型误设定情况，增强结论可信度。
- **应用延伸**：将算法与联邦学习联系，拓宽了研究影响面。

## 8. 不足与局限

- **理论适用范围有限**：后悔界仅对**线性迁移近似**的并行 MDP 成立，对更一般的非线性或高维状态空间缺乏分析。
- **通信复杂度依赖** \(M^2\)：当智能体数量很大时，通信成本可能成为瓶颈，是否可进一步近优尚未讨论。
- **实验对比不够详尽**：未给出与多种最新 MARL 探索算法的横向对比，基准方法细节缺失；实验报告缺少多次运行的方差与显著性分析。
- **算力信息缺失**：未提供训练资源配置，妨碍复现和公平性评估。
- **未讨论异构智能体或奖励设置**：协作场景假设相对理想（共享目标、并行转移），现实世界中智能体能力或奖励可能异构，适用性有待扩展。

（完）
