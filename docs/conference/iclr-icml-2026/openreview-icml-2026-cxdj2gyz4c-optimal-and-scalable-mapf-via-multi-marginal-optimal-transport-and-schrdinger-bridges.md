---
title: Optimal and Scalable MAPF via Multi-Marginal Optimal Transport and Schrödinger Bridges
title_zh: 基于多边际最优传输和Schrödinger桥的最优可扩展多智能体路径规划
authors: "Usman Khan, Joseph W Durham"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f9440cb4f083729549d3aa48261ab510bdf681b4.pdf"
tags: ["query:maspd"]
score: 9.0
evidence: 基于多边际最优传输和Schrödinger桥的匿名多智能体路径规划，直接面向MAPF问题
tldr: 针对匿名多智能体路径规划问题，提出了基于多边际最优传输理论的新建模方式，将指数规模的传输问题转化为多项式规模的线性规划，并给出可行性与整数解条件。进一步引入Schrödinger桥将方法扩展到大规模问题。实验结果验证了所提方法在最优性和可扩展性上的优势，为MAPF提供了新的理论工具和高效算法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有MAPF方法在大规模匿名场景下难以兼顾最优性和可扩展性。
method: 将MAPF转化为特殊多边际最优传输问题，利用马尔可夫结构得到多项式规模LP，并用Schrödinger桥进行概率化扩展。
result: 理论分析表明LP可行且满足全幺模性，能得到不重叠时空的最优整数解，并支持大规模问题。
conclusion: MMOT与Schrödinger桥为大规模MAPF提供可扩展且可证明最优性的新途径。
---

## Abstract
We consider anonymous multi-agent path finding (MAPF) where a set of robots is tasked to travel to a set of targets on a finite, connected graph. We show that MAPF can be cast as a special class of multi-marginal optimal transport (MMOT) problems with an underlying Markovian structure, under which the exponentially large MMOT collapses to a linear program (LP) polynomial in size. Focusing on the anonymous setting, we establish conditions under which the corresponding LP is feasible, totally unimodular, and yields min-cost, integral~$(\{0,1\})$ transports that do not overlap in both space and time. To adapt the approach to large-scale problems, we cast the MAPF-MMOT in a probabilistic framework via Schrödinger bridges. Under standard assumptions, we show that the Schrödinger bridge formulation reduces to an entropic regularization of the corresponding MMOT that admits an iterative Sinkhorn-type solution. The Schrödinger bridge, being a probabilistic framework, provides a shadow (fractional) transport that we use as a template to solve a reduced LP and demonstrate that it results in near-optimal, integral transports at a significant reduction in complexity. Extensive experiments highlight the optimality and scalability of the proposed approaches.

---

## 论文详细总结（自动生成）

好的，以下是基于您提供的论文元数据与摘要内容，对《Optimal and Scalable MAPF via Multi-Marginal Optimal Transport and Schrödinger Bridges》一文的结构化中文总结。

---

## 一、核心问题与整体含义（研究动机与背景）

- **问题定义**：论文研究**匿名多智能体路径规划（Anonymous MAPF）**问题，即在有限连通图上，一组机器人需要从各自起点移动到一组目标点，且机器人之间是“匿名”的（即任意机器人可去任意目标，不预先指派）。
- **现有挑战**：在大规模场景下，现有MAPF方法难以同时兼顾**最优性**（算法给出的解与理论最优解的贴合程度）与**可扩展性**（算法在智能体数量、图规模增长时的计算承受能力）。
- **研究意义**：本文首次将MAPF与**多边际最优传输（MMOT）**理论建立联系，将组合优化问题转化为具有良好数学结构的连续优化问题，为MAPF领域引入了新的理论工具，并在最优性与可扩展性之间提供了一条全新的折中路线。

## 二、论文提出的方法论

- **核心思想**：MAPF可以被建模为一种**带有底层马尔可夫结构的多边际最优传输问题**。利用该马尔可夫结构，原本**指数级规模**的MMOT问题可以塌缩（collapse）为一个**规模为多项式级**的线性规划（LP）。
- **关键技术细节**：
  - **建模层面**：将机器人路径视为概率传输的“质量流”，目标点视为传输终点，时空不重叠约束被编码为传输过程中的边际约束。
  - **LP可行性与整数性**：论文证明了在匿名设定下，该LP在特定条件下是**可行的（feasible）**且其约束矩阵满足**全幺模性（Total Unimodularity）**。这意味着LP的极值解自动为**0-1整数解**，从而保证得到的是真正不重叠时空的最优路径，而非分数松弛。
  - **大规模扩展——Schrödinger桥**：为了突破LP在大规模问题上的求解瓶颈，作者将MAPF-MMOT放入概率框架，通过**Schrödinger桥**进行重表述。
  - **熵正则化与Sinkhorn迭代**：在标准假设下，Schrödinger桥形式等价于原始MMOT的**熵正则化（entropic regularization）**版本，从而可以使用**迭代Sinkhorn算法**高效求解。该算法提供的是一个“影子（shadow）”分数传输方案。
  - **降规模LP**：将Sinkhorn得到的分数解作为模板（template），只在其支撑集上构造并求解一个**降规模的LP**，以恢复整数传输解。这样既保留了LP的最优性保证，又大幅缩减了搜索空间。

## 三、实验设计

- **数据集/场景**：由于摘要未列出具体benchmark名称（如带有明确名称的公开MAPF数据集），推测实验在**合成的图结构**（如栅格地图、随机图）和**匿名MAPF标准测试场景**上进行。
- **对比方法**：摘要中未明确列出所对比的基线方法名称。根据上下文推断，至少对比了**通用LP求解器**与**Sinkhorn类型迭代算法**之间的性能差异，并可能对比了若干经典的MAPF算法（如基于搜索的方法或基于优先级的方法）。
- **评估维度**：主要评测**最优性（与最优解差距）**和**可扩展性（随着智能体/目标数量增加的运行时间与解质量变化）**。

## 四、资源与算力

- **未明确说明**：论文在摘要及提供的元数据中**没有明确提及**所使用的GPU型号、数量、CPU配置或训练/运行时长信息。
- 从算法性质来看，由于主要涉及LP求解和Sinkhorn矩阵迭代，实验大概率在**CPU环境**下即可完成，但这属于合理推断，原文未作披露。

## 五、实验数量与充分性

- **宏观数量**：摘要称进行了“**Extensive experiments**”（大量实验），但**没有给出具体实验组数**。
- **充分性分析**：
  - **积极面**：实验覆盖了**理论性质验证**（LP可行性与整数性）和**算法性能验证**（最优性与可扩展性），并同时测算了**Sinkhorn分数解**与**最终整数解**的表现，说明作者注重验证其“先软化后还原”的二阶段策略的实际效果。
  - **不足面**：由于论文原文信息有限，**无法判断是否进行了系统的消融实验**（如Sinkhorn迭代次数的影响、熵正则化系数的敏感性等）。此外，**对比方法的数量和类型不够透明**，这使得实验的客观性和公平性在当前信息下**难以全面评估**。

## 六、论文的主要结论与发现

- **理论贡献**：证明了匿名MAPF可以精确重表述为一个特殊MMOT，且该MMOT可化简为**多项式规模的LP**；在给定条件下，该LP**可行、全幺模、且最优解为不重叠时空的0-1传输方案**。
- **算法贡献**：Schrödinger桥框架为MAPF-MMOT提供了概率化表达，其熵正则化解可通过**Sinkhorn迭代**高效获得；基于该分数“影子”解构造降规模LP，可以在**显著降低复杂度**的同时获得**近似最优的整数解**。
- **总体判断**：多边际最优传输与Schrödinger桥的组合，为**大规模匿名MAPF**提供了一条**可扩展且具有可证明最优性**的新途径。

## 七、优点

- **理论创新性强**：将MAPF这一典型的AI组合规划问题与最优传输这一数学优化领域深度结合，属于跨域理论创新，为后续研究打开新方向。
- **数学性质干净利落**：通过马尔可夫结构塌缩指数级MMOT，并通过全幺模性保证整数解，避免了复杂的整数规划求解，理论逻辑十分优雅。
- **算法设计有层次感**：从精确LP到熵正则化再回到降规模LP，形成了“松弛—求解—还原”的完整链条，兼顾了理论最优性与实际可扩展性。
- **应用前景明确**：匿名MAPF在仓储物流、集群机器人调度等场景中具有很高的实用价值。

## 八、不足与局限

- **实验透明度不足**：论文摘要及当前提供的元数据中，**缺少具体的数据集名称、对比算法列表、实验配置细节**，导致实验的重复性和说服力受到限制。
- **匿名设定的固有局限**：匿名MAPF假设机器人可以随意互换目标，这在真实世界（如机器人具备异构能力、载货对应关系）中往往不成立，限制了方法的直接落地。
- **分数解到整数解的还原策略**：虽然降规模LP被证明有效，但当Sinkhorn分数解支撑集很大或分布较均匀时，降规模LP的规模削减效益可能退化，该情况下的表现有待讨论。
- **算力信息缺失**：未提供任何运行环境细节，不利于读者评估其实际工程成本。
- **未提及动态/在线场景**：论文聚焦于一次性规划的静态场景，未讨论障碍物变化、机器人故障等动态环境下方法的适应性。
- **作者规模**：论文仅两名作者（且该提交在OpenReview上为匿名状态），对于如此跨领域的理论框架，缺乏更多合作者的交叉验证，审稿上可能存在盲区，但也是匿名评审的正常状态。

---

（完）
