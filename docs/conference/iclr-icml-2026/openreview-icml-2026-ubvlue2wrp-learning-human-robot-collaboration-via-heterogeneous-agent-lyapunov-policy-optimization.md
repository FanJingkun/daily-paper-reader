---
title: Learning Human-Robot Collaboration via Heterogeneous-Agent Lyapunov Policy Optimization
title_zh: 基于异构智能体李雅普诺夫策略优化的人机协作学习
authors: "Hao Zhang, Yaru Niu, Yikai Wang, Ding Zhao, Eric H. Tseng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d07c060a2d8540bb431889515360269847e1cbfa.pdf"
tags: ["query:mcd"]
score: 7.0
evidence: 通过李雅普诺夫收缩稳定去中心化多智能体强化学习，用于人机协作
tldr: 人机协作中机器人与人类行为异构导致理性差距，独立策略更新易发散。本文提出HALO框架，在策略参数空间施加李雅普诺夫收缩，稳定去中心化多智能体强化学习。该方法解决了异构智能体联合优化中的振荡问题，提升人机协作的泛化性与鲁棒性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 人机异构导致的理性差距使独立策略更新振荡或发散，需要稳定的协作优化框架。
method: 提出HALO，通过李雅普诺夫收缩约束策略参数空间，稳定异构智能体的去中心化策略更新。
result: 在人机协作任务中验证了HALO能稳定训练并提升泛化与鲁棒性。
conclusion: 李雅普诺夫约束为异构多智能体协作学习提供稳定性保障，拓展了MARL在HRC中的应用。
---

## Abstract
To improve generalization and resilience in human–robot collaboration (HRC), robots must contend with diverse combinations of human behaviors and contexts, motivating multi-agent reinforcement learning (MARL). However, inherent heterogeneity between robots and humans creates a rationality gap (RG), where decentralized policy updates deviate from cooperative joint optimization. The resulting learning problem is a general-sum differentiable game, so independent policy-gradient updates can oscillate or diverge without added structure. We propose heterogeneous-agent Lyapunov policy optimization (HALO), a framework that stabilizes decentralized MARL by enforcing Lyapunov-based contraction in policy-parameter space. Unlike Lyapunov-based safe RL, which targets state/trajectory constraints in constrained Markov decision processes, HALO uses Lyapunov certification to stabilize decentralized policy learning. HALO rectifies decentralized gradients via optimal quadratic projections, ensuring monotonic contraction of RG and enabling effective exploration of open-ended interaction spaces. Extensive simulations and real-world humanoid-robot experiments show that this certified stability improves generalization and robustness in collaborative corner cases.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：人机协作（HRC）中，机器人需要应对多样化的人类行为与场景组合，以提升泛化性和鲁棒性，因此引入多智能体强化学习（MARL）成为自然选择。
- **核心难题**：机器人与人类在行为模式、目标函数上天然存在异构性，形成“理性差距”（Rationality Gap, RG）。在去中心化策略更新中，各智能体独立优化自身目标，偏离了整体联合最优，导致协作学习不稳定。
- **问题本质**：该问题可建模为一般和可微博弈（general-sum differentiable game），若无额外结构约束，独立策略梯度更新可能产生振荡甚至发散，无法收敛到有效的协作策略。
- **意义**：该研究旨在为异构多智能体协作学习提供稳定性保障，拓展 MARL 在 HRC 领域的适用范围。

## 2. 方法论
- **总体框架**：提出异构智能体李雅普诺夫策略优化（HALO），通过强制在策略参数空间中实现李雅普诺夫收缩，稳定去中心化 MARL 的训练过程。
- **关键区别**：与常见基于李雅普诺夫的安全强化学习（关注 CMDP 中的状态/轨迹约束）不同，HALO 将李雅普诺夫认证用于稳定策略学习本身，而非约束智能体的物理安全性。
- **核心技术**：
  - 在策略参数更新过程中引入李雅普诺夫收缩条件，作为去中心化更新的稳定性约束。
  - 通过最优二次投影（optimal quadratic projection）对去中心化梯度进行修正，使更新方向同时满足协作优化的需要。
  - 保证理性差距（RG）单调收缩，从而避免异构智能体联合优化中的振荡与发散。
  - 该稳定性约束允许智能体在开放式交互空间中更有效地探索，避免因不稳定更新而局限在次优策略。
- **算法流程（文字说明）**：每个智能体独立计算策略梯度 → 构建李雅普诺夫函数度量当前策略与联合最优策略的差距 → 对原始梯度进行二次投影，找到满足收缩条件的最近梯度和更新方向 → 按修正后的梯度更新策略参数，循环直至收敛（或达到稳定协作表现）。

## 3. 实验设计
- **实验场景**：论文提到进行了“大量仿真”和“真实世界人形机器人实验”，重点关注协作边界情况（collaborative corner cases）。
- **Benchmark / 数据集**：摘要中未明确列出具体的公开 benchmark、环境名称或数据集细节。
- **对比方法**：摘要中未具体指明对比了哪些基线方法（例如 MADDPG、QMIX、MAPPO 等未提及）。
- **评估指标**：主要报告泛化性和鲁棒性的提升，但具体量化指标（如成功率、累计奖励等）未被提供。

## 4. 资源与算力
- 论文摘要和元数据中**没有明确说明**使用的 GPU 型号、数量、训练时长、超参数规模等算力信息。
- 由于原文提取内容有限，无法获知训练成本或硬件配置。

## 5. 实验数量与充分性
- 摘要中仅描述为“extensive simulations”和“real-world humanoid-robot experiments”，但**未提供具体实验组数**，如不同场景数量、人类行为类型、消融实验组数等。
- 没有看到消融实验的细节（如移除李雅普诺夫约束、不同投影方式、不同收缩率的影响）。
- 因此，从当前信息看：实验初步验证了 HALO 的有效性，但**公开的细节不足以全面评估实验的充分性和公平性**，例如是否在多个随机种子下运行、是否对基线进行了同等调优、是否进行了统计显著性检验等均未知。

## 6. 主要结论与发现
- HALO 能够有效稳定异构智能体在去中心化 MARL 中的策略更新过程，避免振荡和发散。
- 通过李雅普诺夫收缩约束，理性差距得到单调控制，训练过程更加稳定。
- 这种认证的稳定性在仿真和真实机器人实验中均提升了人机协作的泛化能力和鲁棒性，尤其在协作边界情况下表现更好。

## 7. 优点
- **理论创新**：将李雅普诺夫稳定性理论引入策略参数空间，而非传统安全 RL 的状态/轨迹约束，角度新颖。
- **针对性强**：直接面向人机异构导致的理性差距，不回避联合优化与个体优化之间的冲突。
- **实用性强**：采用二次投影修正梯度，算法易于集成到现有去中心化策略梯度方法中。
- **应用价值**：在真实人形机器人上验证，表明其具备实际部署潜力。

## 8. 不足与局限
- **信息不全**：由于提取到的内容仅来自摘要，缺乏方法细节、网络结构、奖励设计等，限制了深入评估。
- **实验透明度不足**：没有报告具体场景数量、基线方法、评估指标和统计结果，难以判断实验的广度与公平性。
- **算力未披露**：无法评估训练成本或资源需求。
- **泛化性存疑**：仅在有限的协作 corner cases 上验证，未说明对人类行为多样性的覆盖程度，以及能否拓展到复杂开放真实环境。
- **理论假设**：李雅普诺夫函数的构造方式、理性差距的度量方法未公开，可能对设计者的先验知识有依赖。
- **对比缺失**：未与已有应对异构 MARL 的方法（如基于博弈论、平均场或通信机制的方法）比较，说服力有限。

（完）
