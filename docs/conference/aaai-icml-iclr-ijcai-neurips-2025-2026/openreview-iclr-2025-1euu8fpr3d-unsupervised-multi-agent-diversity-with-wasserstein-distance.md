---
title: Unsupervised Multi-Agent Diversity With Wasserstein Distance
title_zh: 基于Wasserstein距离的无监督多智能体多样性方法
authors: "Tianxu Li, Kun Zhu"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=1Euu8FPr3d"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 利用Wasserstein距离鼓励多智能体多样性，促进合作多智能体强化学习中的角色分化与探索。
tldr: 针对共享参数的合作型多智能体强化学习中智能体行为趋同、探索不足的问题，提出基于Wasserstein距离的多智能体多样性方法WMAD。通过最大化不同智能体轨迹分布之间的Wasserstein距离，促进行为分化和充分探索。与基于互信息的方法相比，WMAD能更有效地引导探索并避免局部最优，提升合作策略性能。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 共享参数的合作型多智能体强化学习易导致行为趋同，妨碍探索并产生局部最优。
method: 提出WMAD方法，最大化不同智能体轨迹分布之间的Wasserstein距离以鼓励多样性。
result: 与互信息方法相比，WMAD更能促进探索并提升合作策略性能。
conclusion: 基于Wasserstein距离的多样性约束可有效促进多智能体角色分化与探索。
---

## Abstract
In cooperative Multi-Agent Reinforcement Learning (MARL), agents sharing policy network parameters are observed to learn similar behaviors, which impedes efficient exploration and easily results in the local optimum of cooperative policies. In order to encourage multi-agent diversity, many recent efforts have contributed to distinguishing different trajectories by maximizing the mutual information objective, given agent identities. Despite their successes, these mutual information-based methods do not necessarily promote exploration. To encourage multi-agent diversity and sufficient exploration, we propose a novel Wasserstein Multi-Agent Diversity (WMAD) exploration method that maximizes the Wasserstein distance between the trajectory distributions of different agents in a latent representation space. Since the Wasserstein distance is defined over two distributions, we further extend it to learn diverse policies for multiple agents. We empirically evaluate our method in various challenging multi-agent tasks and demonstrate its superior performance and sufficient exploration compared to existing state-of-the-art methods.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义

- **研究背景**：在合作型多智能体强化学习（MARL）中，一种常见做法是让多个智能体共享策略网络参数以提升训练效率和可扩展性。然而，这种共享机制会导致一个严重问题——**行为趋同**（behavioral convergence），即所有智能体逐渐学习到相似甚至相同的策略。
- **核心问题**：
  - 行为趋同会严重削弱多智能体的协同能力，使智能体无法在合作任务中形成有效的角色分工。
  - 趋同化策略会缩小策略搜索空间，**阻碍对状态空间的充分探索**，从而容易使合作策略陷入**局部最优**。
- **研究现状与不足**：近年来，大量已有工作试图通过**最大化互信息（Mutual Information）**目标来区分不同智能体的轨迹，从而达到鼓励多样性的目的。然而，论文明确指出，这类基于互信息的方法**本质上并不必然促进探索行为**，即使轨迹得到区分，智能体仍可能缺乏发现新策略的动力。
- **研究意义**：该工作旨在从"分布差异"的角度出发，提出一种能同时实现**行为多样性**与**充分探索**的多智能体训练框架，为合作型 MARL 中共享参数带来的固有缺陷提供更有效的解决方案。

---

### 2. 方法论：WMAD（Wasserstein Multi-Agent Diversity）

- **核心思想**：
  - 与基于互信息的方法（在信息论空间中测量变量相关性）不同，WMAD 将多样性问题转化为**分布距离最大化**问题。
  - 核心做法：在**潜在表征空间（latent representation space）**中，最大化不同智能体的**轨迹分布**之间的 **Wasserstein 距离**（也称推土机距离/EM距离）。
- **Wasserstein 距离的优势**：
  - 相比 KL 散度或 JS 散度，Wasserstein 距离在分布不重叠或低重叠时仍能提供**连续且可导的梯度信号**，因此能更平稳、更有效地驱动策略向多样化的方向优化。
  - 它度量的是分布间的"几何最优传输代价"，比互信息更具几何直观性，也与探索行为之间的关联更直接。
- **多智能体扩展**：
  - Wasserstein 距离本身定义在两个分布之间，而多智能体场景中通常有 N 个智能体。论文提出进一步**扩展该度量以支持多个智能体**，使所有智能体两两之间或全局整体上的轨迹分布差异都能被有效最大化。
  - 这一扩展需要克服多分布之间的均衡优化问题，确保每个智能体都能获得独特的、具有互补性的行为。
- **整体算法流程**（文字说明）：
  1. 每个智能体在环境中执行策略，收集轨迹数据。
  2. 使用一个编码器将原始轨迹映射到潜在表征空间。
  3. 在潜在空间内计算不同智能体轨迹分布之间的 Wasserstein 距离（或经过多智能体扩展后的多分布距离）。
  4. 将该 Wasserstein 距离作为**多样性奖励信号**（或策略正则项），与任务奖励共同用于更新策略网络。
  5. 通过最大化 Wasserstein 距离，驱动智能体朝彼此不同的行为方向发展，并在新区域进行探索。

---

### 3. 实验设计

- **实验场景/Benchmark**：
  - 论文使用了"多种具有挑战性的多智能体任务"进行评测，但摘要文本中**未列出具体的环境名称**（如 SMAC、MPE、Hanabi 等均未提及）。
- **对比方法**：
  - 论文声明将 WMAD 与**现有最先进方法（state-of-the-art methods）**进行了比较，但**未具体列出对比算法的名称**。
  - 从论文背景推断，对比对象至少包含**基于互信息的多智能体多样性方法**，这是本文的核心对照基线。
- **评测指标**：
  - 主要指标为**合作策略的性能**（即任务收益），同时在摘要中特别强调了**探索充分性**方面的评估。
- **评估方式**：
  - 论文提到了"empirically evaluate"和"demonstrate its superior performance"，说明进行了完整的实验验证，但摘要中未给出定量数值。

---

### 4. 资源与算力

- **计算资源说明**：论文摘要及文本中**未提供任何计算资源/算力信息**。
- **具体缺失信息**：
  - 未说明使用的 GPU 型号或数量。
  - 未说明训练时长（小时/天）。
  - 未说明实验的运行次数、随机种子数或计算集群配置。
- **结论**：无法从本文元数据或摘要中获取任何算力信息，需要阅读论文全文才能补充。

---

### 5. 实验数量与充分性

- **实验数量**：
  - 摘要声称进行了"多种任务上的大量实验"，但**没有给出具体实验组数**。
  - **未提及消融实验**（ablation study），例如没有说明是否验证了 Wasserstein 距离替代互信息的单独贡献、多智能体扩展方式的有效性、潜在空间维度的影响等。
- **充分性评估**：
  - **优点**：涉及多个任务场景，说明作者考虑了跨任务的一般性验证。
  - **不足**：由于没有提供具体实验详情，无法评估：
    - 任务类型是否覆盖充分（如仅覆盖离散动作空间还是同时覆盖连续控制）。
    - 与基线方法的对比是否在相同超参数、相同计算预算下进行。
    - 是否存在多次独立重复实验及方差分析。
- **客观性风险**：摘要中只提供"性能优越"的定性结论，缺少**数值表、学习曲线或统计显著性检验**，因此实验的客观性和可信度在当前文本范围内**暂不充分**。

---

### 6. 主要结论与发现

- 共享参数的合作型 MARL 中，基于互信息的多样性方法虽然能区分轨迹，但**不足以促进充分探索**。
- 通过在潜在表征空间最大化不同智能体轨迹分布之间的 **Wasserstein 距离**，WMAD 能够在鼓励多样性的同时**更有效地引导探索**，从而避免合作策略陷入局部最优。
- 实验结果表明，WMAD 在多种多智能体合作任务上**优于现有最先进方法**，展示了基于分布距离（而非互信息）的多样性约束在促进探索和提升合作性能方面具有更大潜力。

---

### 7. 优点

- **问题切中要害**：直接瞄准共享参数 MARL 中行为趋同与探索不足的核心矛盾，具有较强的理论动机。
- **方法论创新**：引入 **Wasserstein 距离**替代常用的互信息/JS散度来衡量多智能体多样性，利用其几何特性和连续梯度优势，理论上有更强的引导探索能力。
- **分布视角**：从"分布差异"而非"信息相关性"切入，更贴近策略探索的几何本质。
- **可扩展性**：显式考虑了从二分布到多分布的扩展问题，说明方法设计考虑了 N 智能体的一般化场景，增强了实用性。
- **探索与多样性统一**：在同一个目标函数中同时处理角色分化和探索需求，避免了多目标之间的权衡困难。

---

### 8. 不足与局限

- **信息不足带来的验证局限**：
  - 当前文本中**没有提供任务环境的名称、对比方法列表、性能数值**，无法独立判断方法的实际改善幅度。
  - **缺少消融实验的描述**：无法确认每个组件（如潜在空间选择、Wasserstein 距离计算方式、多智能体扩展策略）的具体贡献。
- **潜在偏差风险**：
  - 仅以"性能优于基线"作为结论，未说明是否在**所有**任务上都优于基线，也未提及是否存在效果变差的任务场景。
  - 未讨论 Wasserstein 距离计算带来的**额外计算开销**，在多智能体场景下，该开销随智能体数量增加可能呈平方级增长。
- **应用限制**：
  - 该方法的有效性高度依赖于**潜在表征空间**的设计质量，表征选择的敏感性值得讨论。
  - 缺少与 **异构策略参数化方法**、环境结构化先验等替代方案的对比，适用范围（如离散 vs 连续控制任务、大规模智能体数）尚不明确。
  - 摘要未说明该方法对于**非合作/Mixed-motive**场景的适用边界。
- **复现性局限**：由于未提供超参数配置、网络架构和代码信息，从当前文本无法评估复现的可行性。

---

（完）
