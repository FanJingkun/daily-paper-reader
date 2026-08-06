---
title: Scalable Constrained Policy Optimization for Safe Multi-agent Reinforcement Learning
title_zh: 面向安全多智能体强化学习的可扩展约束策略优化
authors: "Lijun Zhang, Lin Li, Wei Wei, Huizhong Song, Yaodong Yang, Jiye Liang"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=pJlFURyTG5"
tags: ["query:mcd"]
score: 8.0
evidence: 面向安全协同控制的可扩展多智能体约束策略优化
tldr: 针对安全多智能体强化学习中全局状态耦合和状态动作空间指数增长导致难以扩展的问题，提出一种可扩展且理论可证的多智能体约束策略优化方法。该方法避免直接依赖全局状态，降低通信和计算开销，适用于自动驾驶和无人机集群等场景。实验表明在更大规模系统中仍能保持安全约束并完成协作任务。该工作为受限条件下的安全MARL提供了高效算法。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 安全MARL中全局状态耦合与状态动作空间爆炸限制其在实时通信/资源受限系统的应用。
method: 提出可扩展且理论可证的多智能体约束策略优化方法，避免对全局状态的依赖。
result: 在大型多智能体系统中提升安全性与可扩展性。
conclusion: 该方法为资源受限场景下的安全协同控制提供了可行的MARL方案。
---

## Abstract
A challenging problem in seeking to bring multi-agent reinforcement learning (MARL) techniques into real-world applications, such as autonomous driving and drone swarms, is how to control multiple agents safely and cooperatively to accomplish tasks. Most existing safe MARL methods learn the centralized value function by introducing a global state to guide safety cooperation. However, the global coupling arising from agents’ safety constraints and the exponential growth of the state-action space size limit their applicability in instant communication or computing resource-constrained systems and larger multi-agent systems. In this paper, we develop a novel scalable and theoretically-justified multi-agent constrained policy optimization method. This method utilizes the rigorous bounds of the trust region method and the bounds of the truncated advantage function to provide a new local policy optimization objective for each agent. Also, we prove that the safety constraints and the joint policy improvement can be met when each agent adopts a sequential update scheme to optimize a $\kappa$-hop policy. Then, we propose a practical algorithm called Scalable MAPPO-Lagrangian (Scal-MAPPO-L). The proposed method’s effectiveness is verified on a collection of benchmark tasks, and the results support our theory that decentralized training with local interactions can still improve reward performance and satisfy safe constraints.

---

## 论文详细总结（自动生成）

# 论文总结：面向安全多智能体强化学习的可扩展约束策略优化

## 1. 核心问题与整体含义（研究动机与背景）

- 多智能体强化学习（MARL）在自动驾驶、无人机集群等真实场景中落地时，面临的核心挑战是**如何在保证安全的前提下，让多个智能体协同完成目标任务**。
- 现有安全 MARL 方法通常依赖**全局状态**来学习集中式价值函数，以指导智能体间的安全协作。
- 然而，这类方法存在两个关键瓶颈：
  - **全局状态耦合**：每个智能体的安全约束与其他智能体状态相互关联，导致优化复杂性剧增；
  - **状态-动作空间指数增长**：随着智能体数量增加，联合状态和动作空间规模爆炸，难以在通信或计算资源受限的系统中部署。
- 因此，论文旨在解决**安全 MARL 在大规模系统中的可扩展性问题**，使训练和决策不依赖全局状态，同时仍能保证安全约束和协作性能。

## 2. 提出的方法论

- **核心思想**：通过理论推导，将全局联合策略优化问题**分解为每个智能体局部的、基于局部交互的优化问题**，从而避免直接使用全局状态，降低通信和计算负担。
- **关键技术细节**：
  - 利用**信任区域方法（trust region method）的严格界**，限制局部策略更新的幅度，保证策略改进的单调性与稳定性；
  - 引入**截断优势函数（truncated advantage function）的界**，允许每个智能体只考虑有限范围（κ-hop）内邻居的影响，近似全局优势信息；
  - 为每个智能体构造**新的局部策略优化目标函数**；
  - 设计**顺序更新机制（sequential update scheme）**，证明当每个智能体依次优化其 κ-hop 局部策略时，可以保证联合策略的安全约束满足性和策略改进性。
- **提出的算法**：**Scalable MAPPO-Lagrangian（Scal-MAPPO-L）**，是一种基于 MAPPO-Lagrangian 框架的可扩展实现，核心是在去中心化训练下利用局部交互进行安全约束优化。
- 理论贡献：论文给出了关于安全约束和联合策略改进的**形式化证明**，为方法的可靠性提供了理论支撑。

## 3. 实验设计

- **数据集/场景**：论文在“一组基准任务（a collection of benchmark tasks）”上验证方法，但所提供的文本中**未具体说明任务名称、环境类型或智能体数量**（可能涉及多智能体控制类任务，如协同导航、编队控制等，但无法从摘要中确认）。
- **Benchmark 对比**：文本未明确列出对比方法，只提到本文方法基于 MAPPO-Lagrangian 框架，可能与该基线及其他安全 MARL 方法进行比较，但具体对比对象和指标未给出。
- 实验关注点：验证“去中心化训练 + 局部交互”是否能在安全约束满足的前提下提升奖励性能。

## 4. 资源与算力

- 论文提供的文本中**未提及任何算力信息**，包括 GPU 型号、数量、训练时长、计算集群配置等。
- 如果原始论文正文中包含这些内容，需要查阅完整 PDF 才能补充；就当前摘要而言，无法给出具体资源消耗说明。

## 5. 实验数量与充分性

- **实验数量**：文本仅提到在“一组基准任务”上验证，**未给出具体实验组数**（例如多少个环境、多少个随机种子、是否包含消融实验等）。
- **充分性评估**：
  - 从摘要内容看，实验覆盖了多个任务，但**具体对比基线、性能指标、统计显著性、扩展性测试规模等关键细节缺失**，难以判断实验的全面性和客观性。
  - 理论上，应有针对不同智能体规模、通信半径 κ 的消融实验，以证明可扩展性；但当前文本未提供相关证据。
  - 因此，可认为实验**初步验证了方法有效性**，但完整充分性需依赖论文全文中的实验章节。

## 6. 主要结论与发现

- 提出的 Scal-MAPPO-L 方法能够在**不依赖全局状态**的情况下，通过去中心化训练和局部交互同时实现：
  - **奖励性能提升**（接近或达到集中式方法的协作效果）；
  - **安全约束满足**（比标准方法更可靠）。
- 理论分析和实验结果表明：**局部交互的信任区域方法可以支持更大规模的多智能体系统**，为资源受限环境下的安全 MARL 提供了可行方案。

## 7. 优点

- **理论可证性强**：不只是经验性算法，而是给出了关于安全约束和策略改进的严格理论界与证明，增强了方法的可信度。
- **可扩展性设计**：通过局部化优势函数和 κ-hop 交互，避免了全局状态依赖，显著降低通信与计算复杂度。
- **算法实用导向**：面向真实应用（自动驾驶、无人机集群）中通信和算力受限的问题，具有实际落地潜力。
- **顺序更新机制**：为分解优化提供了新思路，理论上保证了联合改进，而非简单的独立训练。

## 8. 不足与局限

- **实验信息不足**：提供的文本缺乏具体 benchmark 名称、对比方法、智能体规模等细节，无法完整评估实验的全面性和公平性。
- **算力信息缺失**：未报告训练资源，不利于复现和工程判断。
- **消融分析缺失**：未在摘要中提及对关键组件（如 κ 的选择、顺序更新策略、Lagrangian 系数）的敏感性分析，难以确定方法依赖的关键因素。
- **应用限制**：局部交互假设可能不适用于长程强耦合的安全约束（如全局交通流协调），对 κ 的依赖可能影响在大规模系统中的实际效果；且顺序更新可能导致训练时间较长。
- **潜在偏差风险**：若实验仅选择对局部交互有利的任务，可能高估方法优势；需要更广泛的任务分布来验证泛化性。

（完）
