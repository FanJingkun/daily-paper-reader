---
title: Learning Human-Robot Collaboration via Heterogeneous-Agent Lyapunov Policy Optimization
title_zh: 通过异构智能体李雅普诺夫策略优化学习人机协作
authors: "Hao Zhang, Yaru Niu, Yikai Wang, Ding Zhao, Eric H. Tseng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d07c060a2d8540bb431889515360269847e1cbfa.pdf"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 通过李雅普诺夫策略优化实现人机协作中的异构智能体强化学习
tldr: 本文针对人机协作中机器人与人类智能体异构性导致的理性差距问题，提出异构智能体李雅普诺夫策略优化（HALO）框架，通过在策略参数空间施加李雅普诺夫收缩来稳定分布式多智能体强化学习。该方法将学习问题建模为一般和可微博弈，缓解独立策略梯度更新的振荡或发散。实验表明HALO能有效提升异构人机协作的泛化性与鲁棒性，为异构多智能体系统的稳定训练提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 人机协作中机器人需应对多变的人类行为，但异构性导致理性差距，使分布式策略更新偏离联合最优。
method: 提出HALO框架，利用李雅普诺夫收缩在策略参数空间施加约束，稳定异构智能体的分布式多智能体强化学习。
result: 在异构人机协作场景中，HALO比现有方法更稳定，泛化性和鲁棒性更强。
conclusion: 李雅普诺夫约束能有效解决异构多智能体协作中的理性差距问题，提升学习稳定性。
---

## Abstract
To improve generalization and resilience in human–robot collaboration (HRC), robots must contend with diverse combinations of human behaviors and contexts, motivating multi-agent reinforcement learning (MARL). However, inherent heterogeneity between robots and humans creates a rationality gap (RG), where decentralized policy updates deviate from cooperative joint optimization. The resulting learning problem is a general-sum differentiable game, so independent policy-gradient updates can oscillate or diverge without added structure. We propose heterogeneous-agent Lyapunov policy optimization (HALO), a framework that stabilizes decentralized MARL by enforcing Lyapunov-based contraction in policy-parameter space. Unlike Lyapunov-based safe RL, which targets state/trajectory constraints in constrained Markov decision processes, HALO uses Lyapunov certification to stabilize decentralized policy learning. HALO rectifies decentralized gradients via optimal quadratic projections, ensuring monotonic contraction of RG and enabling effective exploration of open-ended interaction spaces. Extensive simulations and real-world humanoid-robot experiments show that this certified stability improves generalization and robustness in collaborative corner cases.

---

## 论文详细总结（自动生成）

# 论文总结：通过异构智能体李雅普诺夫策略优化学习人机协作

## 1. 核心问题与整体含义

- **研究动机**：人机协作（HRC）中，机器人需要应对多样且多变的人类行为与场景组合。为了提高泛化性和鲁棒性，研究者引入多智能体强化学习（MARL）。然而，机器人与人类在决策机制上存在天然**异构性**，导致出现“**理性差距**”（Rationality Gap, RG）：各智能体独立进行策略更新时，会偏离联合最优的协作方向。
- **问题本质**：该学习问题可建模为**一般和可微博弈**（general-sum differentiable game），在缺乏额外结构约束时，独立的策略梯度更新容易产生振荡甚至发散，无法稳定收敛到良好协作策略。
- **整体含义**：本文旨在解决异构多智能体（尤其是人机异构）在分布式策略优化中的稳定性问题，为开放交互场景下的协作学习提供一种可认证的稳定训练框架。

## 2. 方法论

- **核心思想**：提出**异构智能体李雅普诺夫策略优化**（Heterogeneous-Agent Lyapunov Policy Optimization, HALO）框架。其核心是在**策略参数空间**中施加基于李雅普诺夫（Lyapunov）的收缩约束，从而稳定分布式的多智能体强化学习过程。
- **与安全强化学习的区别**：不同于传统的基于李雅普诺夫的安全强化学习（通常针对受限马尔可夫决策过程中的状态/轨迹约束），HALO 将李雅普诺夫验证用于**稳定化策略学习过程本身**，而非约束系统的安全性。
- **关键技术细节**：
  - 利用李雅普诺夫认证（certification）来保证策略更新过程的单调收缩。
  - 通过**最优二次投影**（optimal quadratic projections）对分布式梯度进行修正，使每个智能体的更新方向朝向降低理性差距（RG）的方向。
  - 该方法能够在开放交互空间中进行有效探索，同时规避独立策略梯度更新带来的振荡/发散。
- **算法流程**（根据摘要推断，论文内可能未给出完整伪代码）：
  - 初始化各异构智能体策略参数；
  - 各智能体在环境中进行交互采样，计算独立策略梯度；
  - 构造李雅普诺夫函数并验证当前参数状态；
  - 将原始梯度投影到满足李雅普诺夫收缩条件的可行方向（二次规划问题）；
  - 执行投影后的梯度更新，迭代直至收敛（理性差距单调递减）。

> 注：论文提供的摘要内容有限，详细的公式推导、李雅普诺夫函数构造方式、具体投影算法等在本次提取文本中**未明确展示**。

## 3. 实验设计

- **实验场景**：
  - **大规模仿真实验**（extensive simulations）——涵盖多种异构人机协作任务。
  - **真实世界类人机器人实验**（real-world humanoid-robot experiments）——验证方法在实际机器人上的有效性。
- **Benchmark**：摘要未明确列出具体基准环境名称，仅说明是在“协作边界情形”（collaborative corner cases）下评估，强调对泛化性和鲁棒性的测试。
- **对比方法**：摘要未明确列出基线方法，但通常应包含标准独立策略梯度方法、其他稳定化 MARL 算法（如 MADDPG、MAPPO、安全 RL 方法等），具体列表在本次文本中**未给出**。

## 4. 资源与算力

- 论文提供的文本（Abstract 及元数据）中**未明确提及**任何计算资源信息，如 GPU 型号、数量、训练时长、集群规模等。
- 仅能推断实验包含仿真和实体机器人两部分，但具体算力消耗无数据。

## 5. 实验数量与充分性

- **实验数量**：摘要提及“大量仿真”和“真实世界机器人实验”，但**未给出具体实验组数、任务数量、消融实验细节**。
- **充分性评估**：
  - 优点：同时包含仿真和真机实验，能一定程度上证明方法的实际可行性，且针对“协作边界情形”进行验证，符合人机协作的泛化性目标。
  - 不足：由于缺少详细的实验配置、对比基线和消融分析，难以从摘要层面完全判断实验的全面性与公平性（如是否覆盖多种人类行为模型、是否控制变量等）。

## 6. 主要结论与发现

- HALO 通过执行最优二次投影修正分布式梯度，能确保理性差距（RG）在李雅普诺夫意义下单调收缩，从而稳定异构智能体的分布式多智能体学习。
- 该框架赋予策略学习“认证的稳定性”（certified stability），显著提升了人机协作中的**泛化能力**和**鲁棒性**，尤其在难以预料的协作边界情形下表现突出。
- 实验结果表明，相比现有方法，HALO 在异构人机协作任务中更加稳定、泛化更强。

## 7. 优点

- **问题切入精准**：聚焦于智能体“异构性”带来的理性差距，这是人机协作中实际且未被充分解决的关键难点。
- **理论框架具有通用性**：将李雅普诺夫收缩从状态空间扩展到策略参数空间，为一般和博弈中的分布式优化提供了新的稳定化机制。
- **兼顾安全与探索**：不同于安全 RL 的保守约束，HALO 通过认证的收缩方向引导策略更新，既能保证稳定，又允许在开放交互空间中有效探索。
- **验证方式可信度高**：同时进行仿真和实体机器人实验，增加了结果的说服力。

## 8. 不足与局限

- **信息缺失**：本次提取的论文文本仅包含摘要和元数据，**缺少**完整的算法细节、理论证明、实验设置、超参数、对比基线和消融实验，无法进行深入的技术评估。
- **实验覆盖不透明**：未明确说明仿真场景的具体类型、人类行为模型的多样性、任务难度等级、实验重复次数等，可能影响结论的普适性。
- **算力与资源未披露**：无法判断方法在大规模、高维环境中的可扩展性和训练成本。
- **潜在偏差风险**：若仅针对特定的人形机器人平台和少数协作任务，方法在不同机器人形态、不同人类群体的有效性仍需验证。
- **应用限制**：李雅普诺夫函数的构造本身可能具有难度，对于复杂高维策略空间，找到合适的 Lyapunov 函数可能不直观，此点论文文本中未讨论。

---

（完）
