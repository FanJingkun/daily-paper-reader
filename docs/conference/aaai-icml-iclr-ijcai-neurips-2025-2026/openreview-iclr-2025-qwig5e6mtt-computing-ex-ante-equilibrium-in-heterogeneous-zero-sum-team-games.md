---
title: Computing Ex Ante Equilibrium in Heterogeneous Zero-Sum Team Games
title_zh: 异构零和团队博弈中的事前均衡计算
authors: "Naming Liu, Mingzhi Wang, Xihuai Wang, Weinan Zhang, Yaodong Yang, Youzhi Zhang, Bo An, Ying Wen"
date: 2024-09-22
pdf: "https://openreview.net/pdf?id=QWIg5e6mtT"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 异构零和团队博弈，队友具有不同角色，团队内协作
tldr: 本文研究异构零和团队博弈中的事前均衡计算问题。作者指出，当队友扮演不同角色时，现有Team PSRO方法的联合策略空间无法覆盖全部团队策略空间，导致求解的事前均衡次优且可利用性显著提高。该工作揭示了异构角色对策略表达能力的深刻影响，也为后续设计角色感知的策略空间与均衡求解方法奠定了理论基础。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 异构团队中队友角色不同，现有Team PSRO策略空间表达不足，导致均衡质量次优。
method: 分析Team PSRO在异构团队博弈中的局限，提出需扩大策略空间以覆盖异构角色组合。
result: 发现Team PSRO在异构场景下可利用性显著更高，均衡求解不充分。
conclusion: 强调异构团队需要角色感知的策略空间设计，以改进均衡计算。
---

## Abstract
The \textit{ex ante} equilibrium for two-team zero-sum games, where agents within each team collaborate to compete against the opposing team, is known to be the best a team can do for coordination. Many existing works on \textit{ex ante} equilibrium solutions are aiming to extend the scope of \textit{ex ante} equilibrium solving to large-scale team games based on Policy Space Response Oracle (PSRO). However, the joint team policy space constructed by the most prominent method, Team PSRO, cannot cover the entire team policy space in heterogeneous team games where teammates play distinct roles. Such insufficient policy expressiveness causes Team PSRO to be trapped into a sub-optimal \textit{ex ante} equilibrium with significantly higher exploitability and never converges to the global \textit{ex ante} equilibrium. To find the global \textit{ex ante} equilibrium without introducing additional computational complexity, we first parameterize heterogeneous policies for teammates, and we prove that optimizing the heterogeneous teammates' policies sequentially can guarantee a monotonic improvement in team rewards. 
We further propose \textbf{Heterogeneous-PSRO} (\textbf{H-PSRO}), a novel framework for heterogeneous team games, which integrates the sequential correlation mechanism into the PSRO framework and serves as the first PSRO  framework for heterogeneous team games. 
We prove that H-PSRO achieves lower exploitability than Team PSRO in heterogeneous team games. 
Empirically, H-PSRO achieves convergence in matrix heterogeneous games that are unsolvable by non-heterogeneous baselines. 
Further experiments reveal that H-PSRO outperforms non-heterogeneous baselines in both heterogeneous team games and homogeneous settings.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- 论文研究**异构零和团队博弈（Heterogeneous Zero-Sum Team Games）**中的**事前均衡（Ex Ante Equilibrium）**计算问题。
- 在两人零和团队博弈中，同一团队内的多个智能体需要协作以对抗对手团队。事前均衡被认为是团队在协调能力上能达到的最优策略。
- 现有方法（如 Team PSRO）试图将事前均衡求解扩展到大规模团队博弈，但其构建的**联合团队策略空间**在队友扮演**不同角色**（异构）时，无法覆盖完整的团队策略空间。
- 这种策略表达能力不足会导致 Team PSRO 陷入**次优的事前均衡**，并且**可利用性（exploitability）**显著提高，无法收敛到全局事前均衡。
- 因此，论文的核心动机是：**揭示异构角色对策略表达能力的深刻影响，并提出一种能在异构团队中求解全局事前均衡的新方法**。

## 2. 提出的方法论：核心思想与关键技术细节

- **核心思想**：针对异构队友，显式参数化异构策略，并通过**顺序优化**机制保证团队奖励的单调提升，从而扩大策略空间覆盖范围。
- **技术路线**：
  1. 首先，对异构团队中的队友策略进行**异构参数化**，使每个角色拥有独立的策略表征。
  2. 证明**按顺序优化异构队友策略**可以保证团队奖励的**单调改进**。
  3. 将这种顺序相关机制（sequential correlation mechanism）集成到 PSRO 框架中，提出 **Heterogeneous-PSRO（H-PSRO）**。
  4. 理论证明 H-PSRO 在异构团队博弈中能够比 Team PSRO 获得**更低的可利用性**。
- **公式 / 算法流程说明**（根据摘要推断）：
  - 与 Team PSRO 类似，H-PSRO 也采用“策略空间响应”（Policy Space Response Oracle）迭代扩展策略集；
  - 区别在于，H-PSRO 在每次迭代中针对不同角色的队友分别计算最优响应，并采用顺序更新的方式，而非同步更新；
  - 最终在扩展后的异构策略空间上求解博弈均衡，从而逼近全局事前均衡。

## 3. 实验设计

- **场景 / 基准**：
  - **矩阵异构博弈（matrix heterogeneous games）**：用于验证算法能否收敛到非异构基线无法解决的场景。
  - **异构团队博弈（heterogeneous team games）**：测试方法在标准异构设置下的表现。
  - **同构设置（homogeneous settings）**：用于测试方法在非异构环境下的泛化能力。
- **对比方法**：
  - 非异构基线（主要是 Team PSRO 等基于联合策略空间的方法）。
- **评估指标**：
  - **可利用性（exploitability）**：衡量均衡求解的质量，越低越好。
  - **收敛性**：是否能收敛到全局事前均衡。
- **注意**：原文摘要未提供具体数据集名称、环境细节、奖励矩阵维度等，因此无法展开更详细的实验配置。

## 4. 资源与算力

- **文中未明确说明**所使用的 GPU 型号、数量、训练时长等计算资源信息。
- 从论文性质（博弈论 + 强化学习 + PSRO）推测，实验可能包含基于矩阵博弈的少量计算和相对轻量的团队博弈模拟，但**无法从提供的内容中确认**。

## 5. 实验数量与充分性

- **实验组数**：摘要中提到的实验主要包括：
  - 矩阵异构博弈上的收敛性验证；
  - 异构团队博弈上的性能对比；
  - 同构设置上的迁移对比。
- 整体来看，实验覆盖了**异构与同构**两类场景，但**缺乏消融实验、不同规模/复杂度团队的扩展实验、以及更复杂连续控制环境的验证**。
- 由于摘要未提供详细实验统计数据和误差条，**无法完全判断其统计显著性和公平性**。不过作者在理论层面给出了可利用性较低的证明，增强了可信度。

## 6. 主要结论与发现

- 异构团队中，队友拥有不同角色时，Team PSRO 的联合策略空间表达能力不足，导致均衡次优、可利用性高。
- 通过异构参数化和顺序优化机制，可以构造更完整的策略空间，从而求解**全局事前均衡**。
- 提出的 H-PSRO 是**首个面向异构团队博弈的 PSRO 框架**。
- 理论上，H-PSRO 的可利用性低于 Team PSRO；实验上，H-PSRO 在矩阵异构博弈中能收敛（基线无法解决），并在异构与同构设置中均优于非异构基线。

## 7. 优点

- **问题定位准确**：首次从“策略空间表达能力”角度系统性分析异构角色对均衡求解的影响。
- **理论贡献扎实**：给出了顺序优化保证单调改进的证明，以及 H-PSRO 可利用性更低的定理。
- **方法框架通用**：将顺序相关机制与 PSRO 框架结合，不额外引入计算复杂度，且在同构场景下也能适用。
- **实验设计有针对性**：选用矩阵异构博弈作为“无法求解”的算例，直观体现方法优势。

## 8. 不足与局限

- **实验规模与场景有限**：摘要中仅提及矩阵博弈和团队博弈，缺少大规模、高维连续控制或复杂部分可观察环境的验证。
- **消融与分析不足**：未提供关于“顺序相关机制”的消融实验，也没有分析不同角色数量、异构程度对性能的影响。
- **计算资源信息缺失**：无法评估算法的实际训练成本。
- **应用范围限制**：论文聚焦于零和团队博弈，对于更一般的合作-竞争混合场景或非零和博弈，方法的适用性有待验证。
- **摘要信息有限**：由于仅基于摘要与元数据，许多细节（如具体算法伪代码、超参数设置、收益计算方式）无法在总结中呈现。

（完）
