---
title: Solving Homogeneous and Heterogeneous Cooperative Tasks with Greedy Sequential Execution
title_zh: 基于贪婪顺序执行的同质与异质合作任务求解
authors: "Shanqi Liu, Dong Xing, Pengjie Gu, Xinrun Wang, Bo An, Yong Liu"
date: 2024-01-16
pdf: "https://openreview.net/pdf?id=hB2hXtxIPH"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 同质与异质合作任务的MARL、价值分解、角色
tldr: 合作多智能体强化学习中，价值分解方法擅长同质任务但难以处理异质任务，而个性化观测或角色分配方法又牺牲同质性性能。本文提出贪婪顺序执行（GSE）方法，在统一框架下同时解决同质与异质合作任务。该方法通过顺序决策生成非对称策略，兼顾两类任务的需求，实验表明其效果优于专门化方法。该研究为同质/异质混合团队协作提供了新思路。
source: ICLR-2024-Accepted
selection_source: conference_retrieval
motivation: 价值分解方法在同质任务表现好但对异质任务适应性差，角色分配方法则反之，二者难以兼得。
method: 提出贪婪顺序执行框架，通过顺序决策生成非对称策略，统一处理同质与异质合作任务。
result: 实验证明该方法在两类任务上均取得较好性能，兼顾策略多样性与协调性。
conclusion: 为同时处理同质与异质智能体合作任务的算法设计提供了新思路。
---

## Abstract
Cooperative multi-agent reinforcement learning (MARL) is extensively used for solving complex cooperative tasks, and value decomposition methods are a prevalent approach for this domain. However, these methods have not been successful in addressing both homogeneous and heterogeneous tasks simultaneously which is a crucial aspect for the practical application of cooperative agents. 
On one hand, value decomposition methods demonstrate superior performance in homogeneous tasks. Nevertheless, they tend to produce agents with similar policies, which is unsuitable for heterogeneous tasks. On the other hand, solutions based on personalized observation or assigned roles are well-suited for heterogeneous tasks. However, they often lead to a trade-off situation where the agent's performance in homogeneous scenarios is negatively affected due to the aggregation of distinct policies. An alternative approach is to adopt sequential execution policies, which offer a flexible form for learning both types of tasks. However, learning sequential execution policies poses challenges in terms of credit assignment, and the limited information about subsequently executed agents can lead to sub-optimal solutions, which is known as the relative over-generalization problem. To tackle these issues, this paper proposes Greedy Sequential Execution (GSE) as a solution to learn the optimal policy that covers both scenarios. In the proposed GSE framework, we introduce an individual utility function into the framework of value decomposition to consider the complex interactions between agents. 
This function is capable of representing both the homogeneous and heterogeneous optimal policies. Furthermore, we utilize greedy marginal contribution calculated by the utility function as the credit value of the sequential execution policy to address the credit assignment and relative over-generalization problem. We evaluated GSE in both homogeneous and heterogeneous scenarios. The results demonstrate that GSE achieves significant improvement in performance across multiple domains, especially in scenarios involving both homogeneous and heterogeneous tasks.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- 合作多智能体强化学习（MARL）广泛用于求解复杂合作任务，价值分解（Value Decomposition）是一类主流方法。
- 现有方法难以同时高效处理同质（homogeneous）与异质（heterogeneous）合作任务，而这对于实际应用至关重要。
- 价值分解方法在同质任务中性能优越，但容易产生策略相似的智能体，不适合异质任务。
- 基于个性化观测或角色分配的方法适合异质任务，但会导致智能体策略差异过大，反而损害同质场景下的性能。
- 顺序执行策略（sequential execution policies）是一种灵活的策略形式，理论上可同时适应两类任务，但其面临信用分配（credit assignment）困难，且后续执行智能体的信息有限，容易导致相对过度泛化（relative over-generalization）问题，从而得到次优解。
- 因此，论文致力于设计一种能够统一解决同质与异质合作任务的算法框架。

## 2. 方法论

- **核心思想**：提出贪婪顺序执行（Greedy Sequential Execution, GSE）框架，通过顺序决策生成非对称策略，在统一框架下同时学习同质与异质任务的最优策略。
- **关键技术细节**：
  - 在价值分解框架中引入**个体效用函数（individual utility function）**，用于刻画智能体之间的复杂交互。
  - 该效用函数能够同时表示同质和异质场景下的最优策略。
  - 利用效用函数计算的**贪心边际贡献（greedy marginal contribution）** 作为顺序执行策略的信用值（credit value），以解决顺序策略中的信用分配问题。
  - 通过上述信用值设计，缓解相对过度泛化问题，避免因信息不足导致的次优行为。
- **算法流程（文字说明）**：
  1. 智能体按顺序执行动作。
  2. 每个智能体根据当前状态和已执行智能体的动作，通过个体效用函数评估自身对整体任务的边际贡献。
  3. 使用贪心方式选择使边际贡献最大化的动作，并将该贡献作为该智能体的信用值用于训练。
  4. 所有智能体完成顺序决策后，联合优化价值分解目标，从而生成既适用于同质又适用于异质任务的最优策略。

## 3. 实验设计

- 论文摘要中仅说明在**同质和异质场景**下进行评估，并提到结果在**多个领域（multiple domains）** 中取得了显著改进。
- 但当前提供的材料中**并未明确列出**具体使用的数据集、基准环境（例如 SMAC、MPE、GRF 等）以及对比方法（如 QMIX、VDN、角色分配方法等）。
- 根据元数据推断，该方法属于合作 MARL 的 value decomposition 与角色/顺序执行方向，实验可能涵盖标准 MARL 测试平台和异质任务定制场景，但具体细节需查阅原文。

## 4. 资源与算力

- 提供的论文摘要与元数据中**未提及任何计算资源信息**，包括 GPU 型号、数量、训练时长等。
- 由于本文仅给出摘要级内容，无法获取训练硬件配置和算力消耗细节。

## 5. 实验数量与充分性

- **实验数量**：摘要仅陈述了实验结果，未给出具体实验组数、消融实验数量或统计显著性信息。
- **充分性与客观性**：
  - 从现有材料看，无法判断实验是否覆盖多种典型同质与异质任务，也无法判断是否进行了充分的消融分析（如对效用函数设计、边际贡献计算方式等关键组件的验证）。
  - 摘要声称效果优于现有方法，但缺少具体的数值比较和误差范围，因此**不足以独立评估实验的充分性与公平性**。
  - 需要阅读论文全文才能确认对比方法、超参数设置、实验重复次数等关键细节。

## 6. 主要结论与发现

- GSE 框架能够同时处理同质与异质合作任务，避免了价值分解方法在异质任务中策略趋同的问题，也避免了角色分配方法在同质任务中的性能损失。
- 通过个体效用函数和贪心边际贡献机制，GSE 有效解决了顺序执行策略中的信用分配与相对过度泛化问题。
- 实验表明，GSE 在多个领域上均取得了显著的性能提升，尤其是在同时包含同质与异质任务的环境中优势明显。
- 论文为设计兼顾同质与异质智能体合作的 MARL 算法提供了新思路。

## 7. 优点

- **统一框架**：首次在单一算法中同时适配同质与异质任务，避免了“二选一”的权衡。
- **方法创新**：将顺序决策与价值分解结合，引入个体效用函数表述复杂交互，并用贪心边际贡献作为信用分配依据，针对性强。
- **解决关键难题**：直接应对顺序执行策略中的信用分配与相对过度泛化两大痛点，理论动机清晰。
- **应用价值**：对实际中混合同质/异质团队协作场景（如多机器人系统、人机协作）具有较强参考意义。

## 8. 不足与局限

- **信息不足**：提供的材料仅有摘要，无法验证实验细节，包括具体基准、对比方法、参数设置和统计结果。
- **实验覆盖不明**：摘要未说明异质任务的具体性质（如能力差异、角色差异或目标差异），也未说明同质任务是否覆盖多种经典场景，普适性尚不明确。
- **潜在效率问题**：顺序执行策略通常假设智能体按序决策，实际执行时可能需要串行化，可能带来通信延迟或决策时延；摘要未讨论该方法的计算复杂度和可扩展性。
- **依赖效用函数设计**：个体效用函数的形式和质量对算法性能影响较大，但摘要未说明该函数是手工设计还是可学习，以及在不同任务间的通用性。
- **局限性说明不足**：未讨论 GSE 在智能体数量较多、部分可观测或非平稳环境下的表现，也未分析可能失败的情形。

（完）
