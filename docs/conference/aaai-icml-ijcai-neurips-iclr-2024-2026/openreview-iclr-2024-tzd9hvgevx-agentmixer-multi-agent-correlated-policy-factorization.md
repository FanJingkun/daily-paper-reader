---
title: "AgentMixer: Multi-Agent Correlated Policy Factorization"
title_zh: AgentMixer：多智能体相关策略分解
authors: "Zhiyuan Li, Wenshuai Zhao, Lijun Wu, Joni Pajarinen"
date: 2023-09-23
pdf: "https://openreview.net/pdf?id=tzD9HVgeVx"
tags: ["query:mcd"]
score: 9.0
evidence: 基于CTDE的相关策略分解以提升智能体协调
tldr: CTDE常用集中值函数来稳定部分可观测MARL，但现有方法假设各智能体独立作决策，缺乏相关联合策略。AgentMixer充分利用CTDE，通过策略修正模块在全局状态条件下组合局部策略，显式建模相关联合策略，并解决分布式执行与集中训练的失配问题。实验表明该方法增强了智能体间的协调能力，优于多种强基线。
source: ICLR-2024-Public
selection_source: conference_retrieval
motivation: 现有CTDE方法假设智能体独立决策，难以实现需要协调的相关联合策略。
method: 在全局状态条件下用策略修正模块组合局部策略，显式建模相关联合策略。
result: 在多个协作任务上提升了协调性能，验证了相关策略分解的有效性。
conclusion: 利用全局信息学习相关策略可显著增强部分可观测环境中的多智能体协调。
---

## Abstract
Centralized training with decentralized execution (CTDE) has been popularly employed to stabilize the partially observable multi-agent reinforcement learning (MARL) by learning a centralized value function. However, existing methods typically assume that agents make decisions based on their local observation independently, which could hardly lead to a correlated joint policy with sufficient coordination. In this paper, we propose AgentMixer which fully takes advantage of CTDE to learn correlated decentralized policies. Specifically, AgentMixer first explicitly models the correlated joint policy by a module named \textit{Policy Modifier} composing the partially observable individual policies conditioned on global state information. To overcome the mismatch problem caused by the asymmetric information when distilling the state-based joint policy into partially observable decentralized policies, we introduce \textit{Individual-Global-Consistency} (IGC) to maintain the mode consistent between them. The incorporation of these two novel modules enables learning correlated decentralized policies with restricted partial observability. We further theoretically prove that AgentMixer converges to $\epsilon$-approximate Correlated Equilibrium. The strong experimental performance on three MARL benchmarks also confirms the effectiveness of our method.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- 背景：CTDE（集中训练、分散执行）范式广泛用于部分可观测的多智能体强化学习（MARL），通过训练集中式价值函数来稳定学习过程。
- 核心问题：现有 CTDE 方法通常假设每个智能体独立地基于局部观测做决策，这难以形成具有充分协调性的**相关联合策略（correlated joint policy）**。
- 研究意义：解决该问题可提升多智能体在部分可观测环境下的协同能力，缩小集中训练与分散执行之间的语义鸿沟。

## 2. 方法论

- 核心思想：充分利用 CTDE 的全局信息，显式建模并学习相关（非独立）的分散策略，使智能体在执行时仍能保持协调。
- 关键模块一：**Policy Modifier（策略修正模块）**——在全局状态条件下组合各个局部可观测的个体策略，显式构建相关联合策略。
- 关键模块二：**Individual-Global-Consistency（IGC）**——用于保持全局状态策略与部分可观测分散策略之间的模式一致性，以解决集中训练与分散执行之间信息不对称导致的失配问题。
- 理论保证：作者证明了 AgentMixer 可以收敛到 **ε-近似相关均衡（ε-approximate Correlated Equilibrium）**，为方法的收敛性提供理论支撑。

## 3. 实验设计

- 场景与基准：在 **3 个 MARL 基准环境** 上进行了实验（具体环境名称在摘要中未列出，需查阅原文确认）。
- 对比方法：与多种强基线方法进行了对比（具体的基线名称在摘要中未列出）。
- 评估目标：验证 AgentMixer 在协作任务上是否能够提升智能体间的协调性能。

## 4. 资源与算力

- 该论文的摘要和元数据**未说明**具体使用的 GPU 型号、数量或训练时长等算力信息。
- 如需了解训练成本，需进一步查看原文的实验设置部分。

## 5. 实验数量与充分性

- 覆盖范围：3 个 MARL 基准环境，基本覆盖了常见的协作任务类型。
- 对比与验证：与多类强基线对比，并引入了理论证明（ε-近似相关均衡），实验与理论相结合，增加了结果的可信度。
- 总体评价：基于现有信息判断，实验设计较为完整；但消融实验的具体数量与结果、不同环境下的方差与显著性分析等信息在摘要中未体现，需阅读原文进一步确认其客观性与公平性。

## 6. 主要结论与发现

- AgentMixer 通过显式建模相关联合策略，显著增强了部分可观测环境中智能体间的协调能力。
- 在多个协作基准任务上，AgentMixer 优于多种强基线方法，验证了“利用全局信息学习相关分散策略”这一思路的有效性。

## 7. 论文的优点

- 问题定位准确：指出现有 CTDE 方法中“独立决策假设”的局限性，切中实际。 
- 方法设计新颖：Policy Modifier + IGC 的组合较有创新性，直接针对失配问题建模，而非仅依赖集中 value 函数的隐式引导。
- 理论部分扎实：给出收敛到 ε-近似相关均衡的证明，增强了方法的可信度。
- 实验覆盖多基准：在多个标准协作基准上验证了方法泛化性。

## 8. 不足与局限

- 实验细节不透明：摘要中未提供具体环境和基线名称，降低了初步评估的直观性。
- 算力与训练成本未披露：无法判断该方法在实际应用中的资源需求。
- 消融与敏感性分析未在摘要中体现：Policy Modifier 与 IGC 各自的贡献、全局状态噪声等影响尚不明确。
- 应用的广泛性有限：验证主要集中在协作场景，对竞争、混合场景的适用性需要进一步探索。
- 偏差风险：可能存在环境选择、超参数调整等对结果有利的潜在偏差，需待完整实验细节公开后进一步判断。

（完）
