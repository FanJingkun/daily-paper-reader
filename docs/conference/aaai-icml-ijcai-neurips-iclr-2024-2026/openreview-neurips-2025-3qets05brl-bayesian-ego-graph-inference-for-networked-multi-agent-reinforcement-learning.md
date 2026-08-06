---
title: Bayesian Ego-graph Inference for Networked Multi-Agent Reinforcement Learning
title_zh: 网络化多智能体强化学习中的贝叶斯自我图推断
authors: "Wei Duan, Jie Lu, Junyu Xuan"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3qeTs05bRL"
tags: ["query:mcd"]
score: 9.0
evidence: 针对网络化MARL提出随机图策略，基于局部邻域通信与观测进行决策
tldr: 针对网络化多智能体强化学习中静态邻域假设不适应动态异构环境的问题，提出随机图策略与分布式Actor-Critic框架BayesG。每个智能体基于局部物理邻域采样子图来条件化决策，避免全局状态依赖。该方法使去中心化智能体能够在局部可观测和受限通信下灵活适应环境变化。这为网络化MARL中的动态图建模提供了贝叶斯方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 网络化MARL中静态邻域假设限制了动态/异构环境的适应性，集中式动态图又依赖全局状态。
method: 提出随机图策略，智能体基于局部物理邻域的采样子图决策，并引入分布式Actor-Critic框架BayesG进行贝叶斯图推断。
result: 在保持去中心化的同时提升对动态环境的适应性。
conclusion: 贝叶斯自我图推断能在局部信息下实现灵活的网络化多智能体协作。
---

## Abstract
In networked multi-agent reinforcement learning (Networked-MARL), decentralized agents must act autonomously under local observability and constrained communication over fixed physical graphs. Existing methods often assume static neighborhoods, limiting adaptability to dynamic or heterogeneous environments. While centralized frameworks can learn dynamic graphs, their reliance on global state access and centralized infrastructure is impractical in real-world decentralized systems. We propose a stochastic graph-based policy for Networked-MARL, where each agent conditions its decision on a sampled subgraph over its local physical neighborhood. Building on this formulation, we introduce \textbf{BayesG}, a decentralized actor–critic framework that learns sparse, context-aware interaction structures via Bayesian variational inference. Each agent operates over an ego-graph and samples a latent communication mask to guide message passing and policy computation. The variational distribution is trained end-to-end alongside the policy using an evidence lower bound (ELBO) objective, enabling agents to jointly learn both interaction topology and decision-making strategies.
BayesG outperforms strong MARL baselines on large-scale traffic control tasks with up to 167 agents, demonstrating superior scalability, efficiency, and performance.

---

## 论文详细总结（自动生成）

### 论文详细总结

#### 论文核心问题与整体含义（研究动机与背景）
- **核心问题**：在网络化多智能体强化学习（Networked-MARL）中，去中心化智能体需在局部可观测性与固定物理图上的通信约束下自主决策。现有方法普遍依赖**静态邻域假设**，难以应对动态或异构环境；而集中式动态图方法又依赖全局状态访问和集中式基础设施，在实际去中心化系统中难以部署。
- **研究动机**：探索一种能在局部信息条件下灵活建模智能体间交互结构、同时保持完全去中心化运行的方法。
- **整体含义**：该工作为网络化 MARL 中的动态图建模提供了一种**贝叶斯方案**，使智能体能够在局部可观测和受限通信下自适应环境变化，兼具可扩展性与实际部署可行性。

#### 论文提出的方法论
- **核心思想**：提出**随机图策略（Stochastic Graph-based Policy）**，每个智能体基于其局部物理邻域上的**采样子图**来条件化决策，而非依赖全局状态或固定邻域结构。
- **关键技术细节**：
  - 每个智能体在其**自我图（Ego-graph）** 上操作，采样一个**潜通信掩码（Latent Communication Mask）**，用于引导消息传递和策略计算。
  - 引入**贝叶斯变分推断**学习稀疏、上下文感知的交互结构。
  - 构建**分布式 Actor-Critic 框架 BayesG**，将变分分布与策略进行**端到端联合训练**。
- **公式/算法流程（文字说明）**：
  - 目标函数采用**证据下界（ELBO）**，将交互拓扑的学习与决策策略的学习统一在同一优化框架中。
  - 训练过程中，智能体同时学习“与谁通信”（交互拓扑）和“如何决策”（策略），形成协同优化。

#### 实验设计
- **任务场景**：大规模**交通控制任务**，智能体数量最高达 **167 个**。
- **Benchmark**：以强 MARL 基线方法作为对比基准（具体基线名称原文未列出）。
- **对比方法**：原文仅提及“outperforms strong MARL baselines”，未给出具体方法名称。

#### 资源与算力
- 原文及元数据中**未明确说明**使用的 GPU 型号、数量或训练时长等算力信息。
- 仅能从“大规模任务（167 个智能体）”推断其实验规模较大，但具体资源开销无法得知。

#### 实验数量与充分性
- 原文摘要仅报告了单一场景（交通控制）下的实验结果，**未提及消融实验、多场景对比或模型组件分析**。
- 实验设计上，验证了该方法在大规模任务上的**优越性、可扩展性和效率**，但从公开信息看，**实验覆盖面有限**，缺乏对方法各模块贡献的系统性验证。
- 由于缺少具体基线名称和详细实验设置，**公平性与客观性难以完全评估**，需以论文全文为准。

#### 论文的主要结论与发现
- **BayesG 在大型交通控制任务中显著优于强 MARL 基线**，展示了卓越的可扩展性、效率与性能。
- 贝叶斯自我图推断能够在**局部信息**下实现灵活的网络化多智能体协作，兼顾去中心化与动态环境适应性。

#### 优点
- **方法创新性强**：将贝叶斯变分推断引入网络化 MARL 的图结构学习，突破了静态邻域假设。
- **高度去中心化**：不依赖全局状态，适合真实世界分布式系统。
- **端到端可训练**：交互拓扑与策略的联合优化提升了整体协调性。
- **可扩展性突出**：在 167 个智能体的大规模场景中表现优异。

#### 不足与局限
- **实验场景单一**：仅覆盖交通控制任务，缺乏多领域泛化验证。
- **缺少消融实验**：未验证潜通信掩码、变分推断等关键组件各自的贡献。
- **算力信息缺失**：未报告资源开销，难以评估方法在实际部署中的成本。
- **对比基线不明**：未列出具体基线方法，弱化了客观比较的可信度。
- **潜在偏差风险**：若测试场景中的智能体拓扑规律性较强，可能对贝叶斯图学习方法有利，需更多异构/动态场景验证。

（完）
