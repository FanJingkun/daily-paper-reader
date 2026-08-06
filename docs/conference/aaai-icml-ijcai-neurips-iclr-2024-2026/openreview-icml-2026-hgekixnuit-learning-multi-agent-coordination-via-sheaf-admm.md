---
title: Learning Multi-Agent Coordination via Sheaf-ADMM
title_zh: 通过Sheaf-ADMM学习多智能体协调
authors: "Jeffrey Seely, Bartłomiej Cupiał, Llion Jones"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e7062bee6e8dc764ce7a486714db3b30dab33e36.pdf"
tags: ["query:maspd"]
score: 8.0
evidence: 可微分协调框架，实验包括迷宫路径规划等任务
tldr: 该论文提出一种可微分的多智能体协调框架，将输入分解为重叠的局部视图，每个智能体通过参数化编码器求解凸子问题，并由细胞片层定义智能体间的一致性约束。通过开放迭代的ADMM进行联合训练，智能体可以在局部信息不足时相互协商产生正确的全局输出。在迷宫路径规划、图像分类和数独任务上的实验验证了该方法的有效性，表明其能通过学习协调解决需要全局一致性的多智能体决策问题。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体局部视角不足导致全局目标难以达成，需要可微分的协调机制。
method: 用细胞片层约束智能体间共识，通过可微ADMM联合训练神经编码器与子问题求解器。
result: 在迷宫路径规划、图像分类和数独上取得正确的全局输出。
conclusion: 该方法提供了一种通用的可微分协调范式，适用于需要全局一致性的多智能体任务。
---

## Abstract
We present a differentiable optimization framework for multi-agent coordination. An input is decomposed into overlapping local views, each processed by an agent that solves a convex subproblem parameterized by a neural encoder. Agents coordinate through the Alternating Direction Method of Multipliers (ADMM) with inter-agent constraints specified by a cellular sheaf. The sheaf specifies which aspects of neighboring solutions must agree, allowing for heterogeneous notions of global consensus. Backpropagating through the unrolled optimization jointly trains all components of the multi-agent system. We evaluate on maze pathfinding, image classification, and Sudoku, where agents with individually insufficient local views learn to coordinate to produce correct global outputs. On MNIST, the local-view decomposition yields improved robustness to distribution shifts relative to a standard CNN. On Sudoku, the optimization-derived structure yields markedly higher solve rates than parameter-matched MPNN baselines. Finally, the ADMM structure exposes distinct primal, consensus, and dual state variables, opening the coordination dynamics to direct analysis and intervention—a property unavailable in standard message-passing architectures.

---

## 论文详细总结（自动生成）

# 通过 Sheaf-ADMM 学习多智能体协调 —— 中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：在多智能体系统中，每个智能体通常只能获取局部的视图或信息，导致单个智能体无法独立得出全局一致的正确决策。传统方法缺乏可微分的协调机制，难以通过端到端训练学习复杂的协作策略。
- **核心问题**：如何设计一种通用的、可微分的多智能体协调框架，使局部信息不足的智能体通过相互协商，共同产生正确的全局输出。
- **整体意义**：论文将优化理论中的 ADMM 与细胞片层（cellular sheaf）结合，提出了一个可微分的协调范式，为多智能体协作提供了一种新的建模思路，并在多个典型任务上验证了有效性。

## 2. 论文提出的方法论

- **核心思想**：将输入分解为相互重叠的局部视图，每个智能体接收一个局部子问题；智能体之间通过 ADMM 进行迭代协商，协商过程中由细胞片层定义哪些解分量需要在相邻智能体之间达成一致。
- **关键技术细节**：
  - 每个智能体使用一个由神经编码器参数化的凸子问题求解器。
  - 协调过程基于 ADMM，包含原始更新、对偶更新和一致性更新。
  - 细胞片层（sheaf）提供灵活的“一致性约束”，允许多种异构的全局共识形式，而不要求所有智能体共享相同的状态空间。
  - 通过将 ADMM 迭代展开为计算图，梯度可以反向传播，从而联合训练神经编码器与所有子问题求解器。
- **算法流程**（文字描述）：
  1. 输入被切分为若干重叠的局部视图，分配给各智能体。
  2. 每个智能体用自己的编码器将局部视图映射为子问题参数。
  3. 进入 ADMM 循环：各智能体求解自己的凸子问题，根据片层约束与邻居交换所需的状态并更新对偶变量，反复迭代直到收敛或达到固定步数。
  4. 整个迭代过程作为“层”参与反向传播，更新编码器参数。
- **与消息传递架构的区别**：ADMM 显式维护原始变量、一致性变量和双变量，这些状态可供直接分析和干预，而标准消息传递架构不具备这种透明性。

## 3. 实验设计

- **任务/数据集**：
  - 迷宫路径规划：需要多个智能体协作找到全局可行路径。
  - 图像分类：使用 MNIST，重点考察分布偏移下的鲁棒性。
  - 数独（Sudoku）：需要全局一致性的约束满足问题，适合检验协调能力。
- **Benchmark**：数独任务上与参数匹配的 MPNN（消息传递神经网络）基线对比求解率；MNIST 上对比标准 CNN 的分布偏移鲁棒性。
- **对比方法**：标准 CNN、参数匹配的 MPNN 基线。

## 4. 资源与算力

- 论文提供的信息中**未明确提及** GPU 型号、数量、训练时长或具体算力配置。
- 因此无法量化计算资源消耗，只能说明该框架基于可微分的 ADMM 展开，训练开销主要由迭代展开的长度和子问题求解器决定。

## 5. 实验数量与充分性

- **实验组数**：共涉及三个任务（迷宫路径规划、MNIST 图像分类、数独）。
- **鲁棒性实验**：MNIST 上评估了分布偏移场景。
- **对比实验**：数独与 MPNN 对比，MNIST 与标准 CNN 对比。
- **消融研究**：元数据/摘要中未明确提及消融实验。
- **充分性评估**：
  - 三个任务覆盖了路径规划、感知和约束满足，具有一定代表性，但覆盖面仍有限；
  - 对比基线较少（仅 CNN 和 MPNN），缺少与其他多智能体协调方法的系统性比较；
  - 未明确报告运行时间、收敛性质等，公平性方面存在一些信息缺失。

## 6. 论文的主要结论与发现

- 智能体在局部信息不足时，可以通过学习到的协调机制产生正确的全局输出。
- 在 MNIST 上，基于局部视图分解的方法比标准 CNN 对分布偏移更具鲁棒性。
- 在数独上，由优化结构派生的方法比参数匹配的 MPNN 求解率显著更高。
- ADMM 结构暴露了原始、共识和对偶状态变量，使协调动态可以被直接分析和干预，这是标准消息传递架构不具备的特性。

## 7. 优点

- **理论结合学习**：将凸优化与神经表示结合，兼具优化收敛特性和表示学习能力。
- **灵活性**：细胞片层允许异构的共识约束，适应不同智能体状态维度不统一的场景。
- **可解释性**：ADMM 的显式状态变量提供了分析多智能体协调过程的窗口。
- **强泛化潜力**：局部视图共享机制可能带来更好的分布外鲁棒性。
- **任务多样性**：覆盖路径规划、图像分类和约束满足，展示了框架的通用性。

## 8. 不足与局限

- **资源信息缺失**：未提供计算环境与训练成本，难以评估实际可扩展性。
- **实验规模较小**：仅用 MNIST（简单分类），未在更复杂的图像数据集或更大规模多智能体环境中验证。
- **基线不够全面**：与 MPNN 的比较仅在数独任务，且缺少与领域最新方法的对比。
- **消融缺失**：未分析各组件（如片层结构、重叠比例、ADMM 步数）对性能的影响。
- **可扩展性疑问**：ADMM 展开后参与反向传播的迭代步数可能限制应用于大规模问题。
- **潜在偏差**：局部视图分解方式可能对特定任务结构有偏好，通用性仍需进一步证明。

（完）
