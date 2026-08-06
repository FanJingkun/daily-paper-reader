---
title: Learning Human-Robot Collaboration via Heterogeneous-Agent Lyapunov Policy Optimization
title_zh: 基于异质智能体Lyapunov策略优化的人机协作学习
authors: "Hao Zhang, Yaru Niu, Yikai Wang, Ding Zhao, Eric H. Tseng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d07c060a2d8540bb431889515360269847e1cbfa.pdf"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 面向人机协作的异质智能体Lyapunov策略优化
tldr: 在人机协作中，机器人需应对多样的人类行为，但智能体异构导致理性差距，使分散式策略更新偏离联合优化并可能振荡。论文提出异质智能体Lyapunov策略优化（HALO），通过在策略参数空间施加Lyapunov收缩来稳定分散式多智能体强化学习。与仅考虑安全约束的Lyapunov方法不同，HALO直接处理一般和可微博弈的动态，提升泛化性与鲁棒性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 人机异构造成理性差距，独立策略梯度更新可能振荡或发散。
method: 提出HALO框架，在策略参数空间中施加Lyapunov收缩以稳定异构分散式MARL。
result: 稳定了人机协作中的多智能体学习过程，提升了泛化性与鲁棒性。
conclusion: 为异构智能体协作策略优化提供了李雅普诺夫理论支撑的新框架。
---

## Abstract
To improve generalization and resilience in human–robot collaboration (HRC), robots must contend with diverse combinations of human behaviors and contexts, motivating multi-agent reinforcement learning (MARL). However, inherent heterogeneity between robots and humans creates a rationality gap (RG), where decentralized policy updates deviate from cooperative joint optimization. The resulting learning problem is a general-sum differentiable game, so independent policy-gradient updates can oscillate or diverge without added structure. We propose heterogeneous-agent Lyapunov policy optimization (HALO), a framework that stabilizes decentralized MARL by enforcing Lyapunov-based contraction in policy-parameter space. Unlike Lyapunov-based safe RL, which targets state/trajectory constraints in constrained Markov decision processes, HALO uses Lyapunov certification to stabilize decentralized policy learning. HALO rectifies decentralized gradients via optimal quadratic projections, ensuring monotonic contraction of RG and enabling effective exploration of open-ended interaction spaces. Extensive simulations and real-world humanoid-robot experiments show that this certified stability improves generalization and robustness in collaborative corner cases.

---

## 论文详细总结（自动生成）

# 基于异质智能体Lyapunov策略优化的人机协作学习（HALO）论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：人机协作（HRC）中，机器人需要面对多样化的人类行为与环境组合，因此引入多智能体强化学习（MARL）以提升泛化性和鲁棒性。
- **核心问题**：机器人与人类之间的**内在异构性**造成了“理性差距”（Rationality Gap, RG），即分散式策略更新会偏离合作式的联合优化目标。
- **理论难点**：该问题本质上是一个**一般和可微博弈**（general-sum differentiable game），如果不对更新过程施加额外结构，独立策略梯度更新会导致**振荡或发散**。
- **整体意义**：论文旨在通过为分散式MARL增加稳定性保障，使机器人能在开放、不确定的人机交互空间中有效学习并应对协作中的边界情况。

## 2. 方法论

- **核心思想**：提出**异质智能体Lyapunov策略优化（Heterogeneous-Agent Lyapunov Policy Optimization, HALO）**框架，在**策略参数空间**中强制施加基于Lyapunov的收缩性，从而稳定分散式MARL的学习过程。
- **与安全RL的区别**：不同于传统Lyapunov安全强化学习（针对约束马尔可夫决策过程中的状态/轨迹约束），HALO利用Lyapunov认证来**稳定策略学习本身**，而非仅保证安全性。
- **关键技术细节**：
  - 通过**最优二次投影**（optimal quadratic projections）对分散式策略梯度进行修正，使更新方向满足Lyapunov收缩条件。
  - 保证理性差距（RG）的**单调收缩**，避免独立更新带来的发散与振荡。
  - 该机制允许智能体在开放式交互空间中有效探索，同时保持学习过程的稳定性。

## 3. 实验设计

- **实验场景**：
  - 大量**仿真实验**（simulations）
  - **真实世界人形机器人实验**（real-world humanoid-robot experiments）
- **具体数据集/基准**：摘要中未提及具体数据集名称或标准benchmark。
- **对比方法**：摘要中未列出具体基线方法，但从动机推断可能对比了朴素独立策略梯度、安全RL/Lyapunov类方法等，论文正文中应有详细对比。

## 4. 资源与算力

- 论文摘要**未明确说明**使用的GPU型号、数量、训练时长等算力资源信息。
- 仅在元数据中显示来源为ICML-2026-Accepted，但无计算资源细节。若要了解具体算力配置，需查阅论文全文的实验设置部分。

## 5. 实验数量与充分性

- 摘要提到“大量的仿真实验”和“真实世界人形机器人实验”，说明实验覆盖了**从仿真到实机**的多个层面。
- 但由于摘要篇幅有限，**未列出实验组数、消融实验数量、统计显著性检验**等细节，无法从摘要直接判断实验的充分性与公平性。
- 从元数据看，论文获得9.0分（满分10），具有一定评审认可度，但实验具体质量需阅读全文评估。

## 6. 主要结论与发现

- HALO框架通过Lyapunov收缩稳定了异构分散式MARL的学习过程，有效缓解了理性差距带来的发散/振荡问题。
- 实验结果表明，这种认证稳定性能够**提升协作边界情况下的泛化能力与鲁棒性**。
- 为异构智能体协作策略优化提供了一种基于李雅普诺夫理论的新框架。

## 7. 优点

- **理论新颖**：将Lyapunov稳定性思想从安全约束领域引入到策略学习稳定性领域，角度独特。
- **通用性强**：直接处理一般和可微博弈动态，而非局限于特定安全约束，因此适用于更广泛的人机协作场景。
- **实践价值**：同时进行仿真与真实机器人实验，验证方法在实际系统中的可行性。
- **针对性强**：专门针对异构智能体间的理性差距问题，切中HRC中分散式MARL的关键痛点。

## 8. 不足与局限

- **实验细节缺失**：摘要中未提供具体实验配置、对比基线和量化指标，无法验证其声称的泛化性提升幅度。
- **算力信息不明**：未披露训练资源，难以评估方法的计算开销。
- **应用范围限制**：目前以人机协作为核心场景，对更广泛的异构多智能体系统（如多机器人协作）是否同样有效尚需验证。
- **理论假设**：Lyapunov收缩依赖于可微博弈假设，若策略更新不可微或模型不连续，方法可能受限。
- **真实实验规模**：摘要仅提到“humanoid-robot experiments”，未说明人类参与者数量、任务复杂度等，外部效度有待考察。

（完）
