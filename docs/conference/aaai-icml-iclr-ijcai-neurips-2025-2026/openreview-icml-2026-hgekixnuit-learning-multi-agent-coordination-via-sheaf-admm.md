---
title: Learning Multi-Agent Coordination via Sheaf-ADMM
title_zh: 通过Sheaf-ADMM学习多智能体协调
authors: "Jeffrey Seely, Bartłomiej Cupiał, Llion Jones"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e7062bee6e8dc764ce7a486714db3b30dab33e36.pdf"
tags: ["query:hetero-marl"]
score: 7.0
evidence: Sheaf-ADMM通过异构一致性约束协调多智能体，应用于迷宫路径规划等任务。
tldr: 本文提出Sheaf-ADMM可微优化框架，将输入分解为重叠局部视图，各智能体通过神经网络编码器求解凸子问题，并借助细胞束指定的异构一致性约束进行ADMM协调。该框架支持异构的全局共识概念，能联合训练整个多智能体系统。在迷宫路径规划、图像分类和数独等任务中，局部信息不足的智能体通过协调产生正确的全局输出，展示了异构协调方法的通用性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体各自拥有不足的局部信息，需要异构的一致性约束来协调全局输出。
method: 提出Sheaf-ADMM可微优化框架，用细胞束定义智能体间一致性要求，并通过ADMM和反向传播联合训练。
result: 在迷宫路径规划、图像分类、数独等任务上，智能体学会协调并正确输出全局结果。
conclusion: 异构一致性约束的ADMM协调能有效实现多智能体全局协同，且框架可微可学习。
---

## Abstract
We present a differentiable optimization framework for multi-agent coordination. An input is decomposed into overlapping local views, each processed by an agent that solves a convex subproblem parameterized by a neural encoder. Agents coordinate through the Alternating Direction Method of Multipliers (ADMM) with inter-agent constraints specified by a cellular sheaf. The sheaf specifies which aspects of neighboring solutions must agree, allowing for heterogeneous notions of global consensus. Backpropagating through the unrolled optimization jointly trains all components of the multi-agent system. We evaluate on maze pathfinding, image classification, and Sudoku, where agents with individually insufficient local views learn to coordinate to produce correct global outputs. On MNIST, the local-view decomposition yields improved robustness to distribution shifts relative to a standard CNN. On Sudoku, the optimization-derived structure yields markedly higher solve rates than parameter-matched MPNN baselines. Finally, the ADMM structure exposes distinct primal, consensus, and dual state variables, opening the coordination dynamics to direct analysis and intervention—a property unavailable in standard message-passing architectures.

---

## 论文详细总结（自动生成）

## 论文总结：Sheaf-ADMM——通过细胞束与ADMM学习多智能体协调

### 1. 核心问题与整体含义

- **研究动机**：在多智能体系统中，单个智能体往往仅能获取全局信息的一个局部视图，这些局部信息本身是不充分的。如何让这些智能体通过协调，整合局部信息以产生正确的全局输出，是核心挑战。
- **关键问题**：传统方法通常假设智能体间存在同质的“全局共识”，即所有智能体必须就完全相同的变量达成一致。然而，现实任务中不同智能体可能需要就不同的方面（或不同维度）相互协调，即需要**异构的一致性约束**。
- **整体含义**：本文提出一种称为 **Sheaf-ADMM** 的可微优化框架，将多智能体协调问题建模为带约束的凸优化问题，利用**细胞束（Cellular Sheaf）** 这一数学工具灵活指定智能体间异构的协商要求，从而克服了传统消息传递范式在异构性和可分析性上的局限。

### 2. 方法论

- **核心思想**：将单输入分解为多个**重叠的局部视图**；每个视图由一个智能体处理，该智能体求解一个由神经网络编码器参数化的**凸子问题**；智能体之间通过 **ADMM（交替方向乘子法）** 迭代协调，协调方式由**细胞束**指定的约束决定。
- **细胞束的作用**：细胞束定义了相邻智能体局部解决方案中哪些部分必须达成一致，从而支持“异构的全局共识”——不同的智能体可以对共享变量施加不同维度的保真度要求。
- **可微性与联合训练**：通过**展开（unrolled）** ADMM 迭代并进行**反向传播**，可以联合训练整个多智能体系统，包括各智能体的神经编码器以及协调过程中的各类参数。
- **算法流程（文字描述）**：
  1. 将原始输入划分为重叠的局部视图，分配给各智能体。
  2. 每个智能体利用其神经编码器生成局部凸优化问题的参数化描述。
  3. 循环执行 ADMM 迭代：对每个智能体独立求解局部子问题（primal 更新）；根据细胞束定义的约束，将局部解投影到共识空间（consensus 更新）；更新对偶变量（dual 更新）。
  4. 迭代收敛后，将输出用于端到端损失计算，并通过计算图反向传播更新所有可学习参数。
- **状态变量**：ADMM 结构显式暴露了三类状态变量——**原始变量（primal）**、**共识变量（consensus）** 和**对偶变量（dual）**，这使得协调过程可以直接被分析和干预，这是标准消息传递架构不具备的性质。

### 3. 实验设计

- **任务与数据集**：
  1. **迷宫路径规划**：智能体各自观察迷宫局部区域，需要协调以找到从起点到终点的全局路径。
  2. **图像分类（MNIST）**：将图像分解为重叠局部块，各智能体独立处理局部块并协调输出全局类别判断。
  3. **数独（Sudoku）**：每个智能体处理数独棋盘的一个局部，需协调以生成满足全局约束的正确填充。
- **Benchmark 与对比方法**：
  - **MNIST**：与标准 CNN 对比，评估在**分布偏移**下的鲁棒性。
  - **数独**：与**参数匹配的 MPNN（消息传递神经网络）** 基线对比，考察求解率。

### 4. 资源与算力

- 论文摘要及提供的元数据中**未明确说明**具体使用的 GPU 型号、数量或训练时长等算力信息。
- 也未提及与计算资源相关的消融分析（如不同规模模型的资源消耗对比）。

### 5. 实验数量与充分性

- **实验组数**：共覆盖三大类任务（迷宫、MNIST 图像分类、数独），每个任务下均进行了核心对比实验：
  - MNIST：与标准 CNN 的分布偏移鲁棒性对比。
  - 数独：与 MPNN 基线的求解率对比。
- **充分性评估**：
  - **优点**：任务跨度较大，既包含结构化推理（迷宫、数独），也包含感知类任务（图像分类），能较好地展示框架的多功能性。
  - **不足**：
    - 缺乏**消融实验**，如细胞束结构设计选择的影响、ADMM 迭代步数的影响、重叠视图设计的影响等未在提供材料中体现。
    - 对比基线较少（每个任务仅对比一种基线），且 MNIST 的对比并非针对“多智能体协调”的基线，而是标准 CNN，对比维度略显单薄。
    - 未报告多次运行的方差或统计显著性信息，无法完全判断结果的稳定性。

### 6. 主要结论与发现

- 具有异构一致性约束的 **Sheaf-ADMM 框架**能够有效实现多智能体协调，使拥有不足局部信息的智能体通过协商产生**正确的全局输出**。
- 在 MNIST 上，局部视图分解方式相比标准 CNN 对**分布偏移具有更好的鲁棒性**。
- 在数独上，由优化结构推导出的框架相比参数匹配的 MPNN 基线具有**显著更高的求解率**。
- ADMM 结构暴露的 primal/consensus/dual 状态变量为协调过程提供了**直接分析和干预**的可能性，这优于标准消息传递架构。

### 7. 优点

- **理论清晰**：将多智能体协调明确建模为凸优化问题，数学基础严谨，ADMM 求解具有良好的收敛保证。
- **异构性创新**：利用细胞束指定异构共识约束，突破了传统多智能体方法中“全局一致”的刚性假设，更贴近现实任务需求。
- **端到端可学习**：通过展开优化过程实现联合训练，各组件（编码器、协调约束）可以统一优化，无需手工设计协调策略。
- **可解释性与可干预性**：ADMM 显式的状态变量使得系统内部协调动态透明，便于理论分析和实际干预。
- **通用性强**：在路径规划、图像分类、组合推理等多种类型任务上均有良好表现。

### 8. 不足与局限

- **算力信息披露不足**：未说明训练所需的 GPU 资源与时间，复现成本未知。
- **消融实验缺乏**：未分析各设计组件（如细胞束结构、ADMM 迭代次数、视图重叠比例）对性能的影响。
- **对比基线有限**：未与更丰富的多智能体通信方法（如 attention-based communication、VBC、QMIX 等）进行对比，难以全面定位方法在文献中的相对优势。
- **扩展范围疑问**：当前实验任务均为结构化或低维图像输入，对于高维、复杂动态环境的适用性尚未验证。
- **凸子问题约束**：每个智能体求解的子问题被限制为凸优化形式，这可能限制神经编码器的表达能力，对于某些非凸特性明显的任务可能构成性能瓶颈。
- **统计严谨性**：没有给出多次实验的均值和方差信息，难以评估结果稳定性。

（完）
