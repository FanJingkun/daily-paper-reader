---
title: Factored Value Functions for Graph-Based Multi-Agent Reinforcement Learning
title_zh: 基于图的多智能体强化学习中的因子化值函数
authors: "Ahmed Rashwan, Keith Briggs, Chris Budd, Lisa Maria Kreusser"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9c09c14ab1c232f08751f788010f3922db325a91.pdf"
tags: ["query:mcd"]
score: 9.0
evidence: 基于图结构的因子化值函数用于多智能体信用分配
tldr: 在大规模多智能体系统中，信用分配是核心挑战，尤其是在图结构局部交互的设定下。该论文提出漫扩散值函数，通过将奖励沿影响图进行时间折扣和空间衰减，为每个智能体分配一个价值分量。DVF具有良好的定义并满足贝尔曼方程，可作为全局或局部评分函数的替代。实验表明该方法在结构化多智能体环境中显著改善每个智能体的学习信号，提升整体性能，为图马尔可夫决策过程中的信用分配提供了新工具。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 全局价值函数对个体智能体学习信号弱，局部构造难以估计且在无限时域下不稳定。
method: 通过图上的时间-空间奖励扩散定义每个智能体的扩散值函数，并推导其贝尔曼方程。
result: 在结构化的多智能体任务中验证DVF能提供更好估计并提升学习性能。
conclusion: DVF是一种可扩展且良定义的信用分配值函数，适用于大规模图结构MARL。
---

## Abstract
Credit assignment is a core challenge in multi-agent reinforcement learning (MARL), especially in large-scale systems with structured, local interactions.
Graph-based Markov decision processes (GMDPs) capture such settings via an influence graph, but standard critics are poorly aligned with this structure: global value functions provide weak per-agent learning signals, while existing local constructions can be difficult to estimate and ill-behaved in infinite-horizon settings.
We introduce the Diffusion Value Function (DVF), a factored value function for GMDPs that assigns to each agent a value component by diffusing rewards over the influence graph with temporal discounting and spatial attenuation.
We show that DVF is well-defined, admits a Bellman fixed point, and decomposes the global discounted value via an averaging property.
DVF can be used as a drop-in critic in standard RL algorithms and estimated scalably with graph neural networks.
Building on DVF, we propose Diffusion A2C (DA2C) and a sparse message-passing actor, Learned DropEdge GNN (LD-GNN), for learning decentralised algorithms under communication costs.
Across the firefighting benchmark and three distributed computation tasks (vector graph colouring and two transmit power optimisation problems), DA2C consistently outperforms local and global critic baselines, improving average reward by up to 11%.

---

## 论文详细总结（自动生成）

好的，我将按照您要求的八个要点，对这篇论文（基于其提供的摘要和元数据）进行详细、客观的中文总结。需要注意的是，提供的文本材料仅限于标题、作者、元数据标签、TLDR和摘要，并未包含论文的正文、方法细节、实验设置和具体结果图表。因此，以下分析和总结将严格基于现有信息，并明确标注出信息缺失的部分。

---

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：论文聚焦于大规模多智能体强化学习（MARL）中的**信用分配（Credit Assignment）** 难题，特别是在具有**图结构局部交互（Graph-Based Local Interactions）** 的动态系统中。
- **研究背景与动机**：
    - 现实世界中的许多多智能体系统（如无线通信网络、分布式计算、消防调度）都可以建模为**图马尔可夫决策过程（GMDP）**，其中智能体之间的影响通过一张“影响图（Influence Graph）”来定义。
    - 然而，现有的强化学习评论家（Critic）模型的结构与这种图结构不匹配。
    - **全局价值函数**（Global Value Function）：虽然理论上完美，但在大规模系统中为每个智能体提供的学习信号过于稀疏和微弱，难以指导个体策略更新。
    - **局部构造的价值函数**（Local Constructions）：虽然试图解决信号弱的问题，但在无限时域（Infinite-Horizon）设定下难以估计且表现不佳（ill-behaved）。
- **整体含义**：论文旨在提出一种新型的、在结构上与图模型匹配的因子化价值函数（Factored Value Function），从根本上改善大规模图结构MARL中的信用分配与学习效率。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**扩散值函数（Diffusion Value Function, DVF）**。其核心概念是将全局奖励信号沿着影响图（Influence Graph）进行“扩散”，在扩散过程中结合**时间折扣（Temporal Discounting）** 和**空间衰减（Spatial Attenuation）**，从而为每个智能体计算出一个专用的价值分量。
- **关键技术细节（基于摘要推导）**：
    - **空间扩散机制**：每个智能体的值函数不仅取决于自身状态和奖励，还通过图结构（影响图）接收来自邻居智能体（以及更远距离智能体，但衰减）的状态和奖励信息，从而在局部价值中耦合全局影响。
    - **数学性质（理论贡献）**：
        - **良定义性（Well-defined）**：论文证明了DVF在数学上是良定义的，即对于任何给定的策略和问题实例，该值函数都存在且唯一。
        - **贝尔曼不动点（Bellman Fixed Point）**：证明了DVF满足贝尔曼方程，存在贝尔曼不动点，这意味着它可以通过标准的时序差分（TD）学习算法进行迭代求解。
        - **平均性质（Averaging Property）**：DVF具有一个“平均性质”，即所有智能体的DVF的某种聚合（如平均或求和）可以**分解（Decompose）** 出全局折扣价值函数。这是它是一种“因子化”价值函数的关键，保证了它与全局最优目标的一致性。
    - **实现与集成**：
        - **即插即用**：DVF可以作为标准RL算法（如A2C）中的评论家（Critic）直接替换使用。
        - **可扩展估计**：DVF可以**通过图神经网络（GNN）进行可扩展地估计**，利用GNN的消息传递机制天然地实现奖励在影响图上的扩散和聚合。
- **配套算法**：
    - **扩散A2C（Diffusion A2C, DA2C）**：将DVF集成到标准的A2C（Advantage Actor-Critic）算法框架中。
    - **Learned DropEdge GNN（LD-GNN）**：针对通信成本有限的情况，提出了一种稀疏的消息传递Actor网络，旨在学习在通信受限下如何选择性地与关键邻居通信，从而学习分散式算法。
- **公式或算法流程（文字描述）**：
    - 由于原文未提供具体公式，根据摘要描述推断流程为：在每个时间步，计算全局奖励并为其添加时间折扣。随后，将这个带折扣的奖励信号在影响图上进行多跳扩散，每一跳（对应一次空间传播）都乘以一个小于1的空间衰减系数。每个智能体累积其接收到的“扩散奖励”信号，结合其局部状态，共同构成其DVF值。该DVF值作为基准（Baseline）用于计算优势函数（Advantage），进而更新Actor网络（LD-GNN）。

### 3. 实验设计

- **使用的数据集和场景（Benchmark）**：
    - **消防调度基准（The Firefighting Benchmark）**：经典的、结构化的多智能体协作任务，用于评估在动态变化环境中的协调能力。
    - **分布式计算任务（Distributed Computation Tasks）**：共三个任务，具体为：
        - **向量图着色（Vector Graph Colouring）**：一个典型的组合优化问题，需要智能体协调决策以满足图上的约束。
        - **两个发射功率优化问题（Two Transmit Power Optimisation Problems）**：这两个问题模拟了无线通信网络中的干扰管理，是典型的具有局部交互特性的大规模优化问题。
- **对比方法（Baselines）**：
    - **局部评论家（Local Critic Baselines）**：使用仅基于局部信息构造价值函数的方法。
    - **全局评论家（Global Critic Baselines）**：使用标准的全局价值函数作为基准的方法。
- **评论**：实验场景覆盖了物理调度（消防）、经典图论难题（着色）和工程应用（功率控制），具有一定的代表性，能够验证方法在各类图结构MARL问题上的泛化性。

### 4. 资源与算力

- **明确说明：原文信息缺失。** 在提供的材料中，作者并未提及实验所使用的具体GPU型号、数量、训练时长、框架（如PyTorch/TensorFlow）或总计算资源预算。因此，无法对算力开销进行量化评估。

### 5. 实验数量与充分性

- **实验数量**：
    - 从摘要来看，作者至少在**4个不同的任务**上进行了实验（1个消防 + 1个图着色 + 2个功率优化）。
    - **推断**：考虑到这是一个标准RL论文，主体实验很可能在这4个任务上对比了DA2C、局部Critic、全局Critic三种算法（以及可能的消融变体）。为了验证DVF的泛化性，很可能还有额外的消融实验（如测试不同衰减系数的影响、LD-GNN与普通GNN的对比等）。
    - **信息缺失**：摘要中未提及具体图表数量、消融研究的详细设置、统计显著性检验（如多次随机种子的标准差）等关键信息。
- **充分性与客观性**：
    - **充分性存疑**：虽然覆盖了4个任务，但缺少对消融实验的描述（例如，DVF中的空间衰减系数如何影响性能？LD-GNN的DropEdge策略是否优于固定随机DropEdge？），也缺少关于算法收敛性和鲁棒性的深入分析。
    - **可能存在偏差风险**：4个实验任务中，有3个是新提出的（或基于标准问题构建的），且都来源于可能有利的领域。整体结果是否依赖特定的GNN架构或A2C超参数，尚不明确。不过，对比了局部和全局两类基线，这使得对比结果较为全面，提升了结论的可靠性（实验结果提升幅度高达11%提供了有力的定量证据）。

### 6. 论文的主要结论与发现

- **理论层面**：证明了DVF是良定义的，满足贝尔曼方程，并且其通过平均性质可以忠实于全局价值函数，为图结构MARL提供了一种理论上坚实的信用分配工具。
- **实践层面**：在消防和分布式计算任务上，将DVF作为Critic的DA2C算法**一致性地优于**（consistently outperforms）基于局部和全局评论家的基线方法，并且在平均奖励上取得了**最高11%（up to 11%）** 的提升。
- **核心发现**：**图结构的因子化价值函数（DVF）是规模化和良定义的，且能显著改善每个智能体的学习信号**。同时，LD-GNN证明了可以在有限的通信开销下学习有效的分散式策略。

### 7. 优点

- **理论创新性与严谨性**：该工作不仅提出一个启发式的算法，还为其核心价值函数构建了完整的数学基础（良定义性、贝尔曼不动点、平均性质）。这在多智能体RL领域尤为重要，增加了方法的可信度和可推广性。
- **结构-算法对齐（Structure-Algorithm Alignment）**：DVF的核心思想（通过图结构扩散奖励）与GMDP的底层结构高度对齐，这是它能有效解决“全局信号弱、局部信号难估计”这一矛盾的关键。
- **实用性与兼容性**：DVF是即插即用的，可以非常方便地嵌入到A2C等流行RL框架中，而不是需要设计全新的Actor-Critic架构。使用GNN估计DVF，也顺应了当前处理图结构数据的先进方法。
- **对现实问题的关切**：通过引入LD-GNN考虑**通信成本（Communication Costs）**，使得该方法不仅适用于集中训练分布执行（CTDE）的设定，也向通信受限的真实分布式系统迈进了一步。

### 8. 不足与局限

- **实验细节缺失（基于当前信息）**：最大的局限性在于提供的材料不完整。缺少对DVF公式的详细推导、对GNN架构的具体描述、对LD-GNN训练细节的说明。没有这些信息，无法完全复现或深入评估其技术细节的合理性。
- **评估维度有限**：摘要仅报告了“平均奖励”（Average Reward）的提升。未提及是否评估了算法的**样本复杂度（Sample Efficiency）**、**通信开销与性能的权衡（Trade-off）**、**对超参数（如空间衰减系数）的敏感性**，以及在大规模（如数千个智能体）下的性能表现。
- **实验场景的偏向性**：虽然任务多样，但这些任务均为相对抽象化或简化后的模型（如简化的功率控制模型），未涉及像SMAC（星际争霸多智能体挑战赛）或MAMuJoCo这样更复杂、高维或具挑战性的标准3D渲染MARL基准。因此，结论在视觉或物理引擎驱动的复杂环境中的泛化性尚未得到验证。
- **对比基线的覆盖面**：对比仅限于局部和全局的Critic。未提及与VDN、QMIX等经典深度MARL算法（但这些算法多基于连续或离散动作空间的合作任务，可能不完全适用于本文中的连续控制问题），或针对通信学习的专用方法（如TarMAC、IC3Net等）进行对比。这使得比较的外部有效性（External Validity）相对受限。
- **模型的不确定性**：作为一篇理论结合实验的论文，未讨论DVF估计器的方差（Variance）或偏差（Bias）在GNN近似过程中是否会引入新的误差。

（完）
