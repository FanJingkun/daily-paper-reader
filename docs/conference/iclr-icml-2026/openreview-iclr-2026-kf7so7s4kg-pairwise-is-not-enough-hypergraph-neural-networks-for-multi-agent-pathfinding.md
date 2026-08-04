---
title: "Pairwise is Not Enough: Hypergraph Neural Networks for Multi-Agent Pathfinding"
title_zh: 成对交互并不够：用于多智能体寻路的超图神经网络
authors: "Rishabh Jain, Keisuke Okumura, Michael Amir, Pietro Lio, Amanda Prorok"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=KF7sO7S4kG"
tags: ["query:maspd"]
score: 9.0
evidence: 面向多智能体寻路的高阶超图神经网络协调
tldr: 多智能体寻路（MAPF）是NP难的多智能体协调问题，现有GNN方法局限于成对消息传递，在密集环境下易出现注意力稀释和次优行为。本文提出基于超图神经网络的方法，对超过两个智能体的群组交互进行显式建模。该方法在密集协调场景中改善了学习型MAPF的性能，缓解了成对交互带来的局限。研究揭示高阶交互是多智能体路径规划学习中的重要因素。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: MAPF问题NP难，现有基于GNN的方法仅支持成对消息传递，在密集场景下导致注意力稀释和次优协调。
method: 提出基于超图神经网络的方法，显式建模多个智能体之间的高阶群组交互，实现超越成对的协作信息传递。
result: 在密集环境中缓解了注意力稀释问题，改进了学习型MAPF方法的协调表现。
conclusion: 证明高阶交互建模对密集多智能体寻路至关重要，为MAPF学习提供了新架构。
---

## Abstract
Multi-Agent Path Finding (MAPF) is a representative multi-agent coordination problem, where multiple agents are required to navigate to their respective goals without collisions. Solving MAPF optimally is known to be NP-hard, leading to the adoption of learning-based approaches to alleviate the online computational burden. Prevailing approaches, such as Graph Neural Networks (GNNs), are typically constrained to *pairwise* message passing between agents. However, this limitation leads to suboptimal behaviours and critical issues, such as attention dilution, particularly in dense environments where group (i.e. beyond just two agents) coordination is most critical. Despite the importance of such higher-order interactions, existing approaches have not been able to fully explore them. To address this representational bottleneck, we introduce HMAGAT (Hypergraph Multi-Agent Attention Network), a novel architecture that leverages attentional mechanisms over directed hypergraphs to explicitly capture group dynamics. Empirically, HMAGAT establishes a new state-of-the-art among learning-based MAPF solvers: e.g., despite having just 1M parameters and being trained on 100$\times$ less data, it outperforms the current SoTA 85M parameter model. Through detailed analysis of HMAGAT's attention values, we demonstrate how hypergraph representations mitigate the attention dilution inherent in GNNs and capture complex interactions where pairwise methods fail. Our results illustrate that appropriate inductive biases are often more critical than the training data size or sheer parameter count for multi-agent problems.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 多智能体寻路（MAPF）是一类经典的多智能体协调问题，要求多个智能体在无碰撞前提下分别到达各自目标点。
- 该问题在最优求解意义下是 NP 难的，因此研究界转向基于学习的求解器以缓解在线计算负担。
- 现有基于图神经网络（GNN）的学习型 MAPF 方法通常只支持**成对（pairwise）消息传递**，即每个智能体只与单个其他智能体交互。
- 这种成对限制在**密集环境**中会导致严重的“注意力稀释”（attention dilution）现象：当多个智能体同时处于冲突区域时，模型无法有效聚焦于真正关键的群体冲突，从而产生次优行为。
- 论文指出，**超过两个智能体的群组级（高阶）交互**在密集协调中至关重要，但已有方法未能充分建模这种交互。
- 整体研究含义在于：为 MAPF 引入高阶交互建模是提升学习型求解器能力的关键方向，而不仅仅是扩大模型规模或数据量。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- 核心思想：使用**超图（hypergraph）**而非普通图来显式建模多个智能体之间的群组交互，从而突破成对消息传递的表示瓶颈。
- 提出架构：**HMAGAT（Hypergraph Multi-Agent Attention Network）**，即“超图多智能体注意力网络”。
- 关键技术细节：
  - 在**有向超图**上设计注意力机制，每条超边可连接超过两个智能体，从而表示一个群体级别的交互。
  - 智能体之间的信息传递不再局限于“两两之间”，而是允许一个智能体同时接收来自一组智能体的聚合信息。
  - 利用注意力权重来动态衡量不同群组交互的重要性，从而在密集场景中避免注意力被均匀稀释。
- 算法流程（文字描述）：
  1. 将 MAPF 状态编码为超图结构，其中节点为智能体，超边为可能发生群体冲突或需要协调的智能体集合。
  2. 通过多层超图注意力网络，在每层中：对每条超边内的所有节点进行聚合，计算各节点对超边的注意力权重；随后更新节点表示。
  3. 最终将更新后的节点表示用于决策（如选择下一动作），以完成路径规划。
- 论文中未给出详细数学公式，但从摘要看，其核心是超图上的注意力消息传递机制。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- 数据集/场景：论文未在摘要中列出具体地图名称或场景类型，但强调实验覆盖了**密集环境**，因为该场景最能体现高阶交互价值。
- Benchmark：以**多智能体寻路（MAPF）标准任务**为基准，衡量求解成功率、碰撞规避能力或路径代价等指标（详细指标未在摘要中列出）。
- 对比方法：
  - 主要对比当前学习型 MAPF 求解器中的 **SoTA（State-of-the-Art）模型**，该对比模型拥有 **85M 参数**。
  - 同时与基于 GNN 的成对消息传递方法进行对比，以说明超图建模的优越性。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- 摘要中**未明确说明**实验所用的 GPU 型号、数量或训练时长。
- 仅知所提出的 HMAGAT 模型参数量为 **1M**，且训练数据量比 SoTA 模型少 **100 倍**。
- 由于缺少具体算力信息，无法评估训练成本对比；但作者强调“合适的归纳偏置比数据量和参数量更重要”，暗示其方法在有限资源下也能取得优异效果。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- 从摘要可见的实验内容：
  - 与当前 SoTA（85M 参数模型）的性能对比。
  - 对 HMAGAT 注意力值的详细分析，用于展示超图表示如何缓解注意力稀释。
  - 对比成对方法在高阶交互场景中的失效案例。
- 但摘要中**未列出完整的实验数量、消融实验设置**（如去掉超图结构、替换为普通图的消融）、**不同密度/地图规模的测试矩阵**等。
- 总体判断：实验设计方向合理，且包含注意力机制的可解释性分析，但在摘要层面尚不足以判断实验的全面性和公平性。不过，作者声称是在“训练数据少 100 倍、参数少 85 倍”的情况下超越 SoTA，这一对比在资源公平性上具有较强说服力。

## 6. 论文的主要结论与发现

- 基于超图神经网络的 HMAGAT 在密集 MAPF 任务中**建立了学习型求解器的新 SOTA**。
- 尽管 HMAGAT 只有 **1M 参数**，且训练数据仅为对比模型的 **1/100**，其性能仍优于 **85M 参数**的现有最佳模型。
- 通过分析注意力权重，论文验证了**超图表示能有效缓解 GNN 中的注意力稀释**问题，并能捕获成对方法失败的高阶交互模式。
- 重要启示：对于多智能体问题，**合适的归纳偏置（如高阶交互建模）往往比数据规模和参数量更为关键**。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法创新性强**：将超图神经网络引入 MAPF，显式建模多智能体群组交互，弥补了 GNN 成对机制的表示瓶颈。
- **注意力机制与超图结合自然合理**：利用有向超图注意力可以灵活处理不同大小的群组，且可解释性较好。
- **实验对比具有策略性优势**：在参数规模（1M vs 85M）和数据量（1/100）均大幅劣势的情况下取得更好结果，强有力地证明了高阶归纳偏置的有效性。
- **包含机制分析**：不仅报告性能，还对注意力值进行深入分析，帮助理解超图如何解决注意力稀释问题，具有较好的可解释性贡献。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验细节缺失**：摘要中未提供具体地图、场景数量、成功率等量化结果，也未说明是否包含不同规模（智能体数量）的测试。
- **消融实验不明确**：未明确是否对比了“不使用超图”或“使用普通图注意力”的基线，无法完全归因性能提升仅来自超图结构。
- **算力信息未知**：未报告训练资源、时间，难以评估方法的实际工程成本。
- **可能的应用限制**：有向超图的构造本身可能需要额外计算或启发式生成超边，在超大规模动态环境中可能引入新的开销；此外，方法可能更适用于密集场景，在稀疏场景下的优势未能从摘要中判断。
- **偏差风险**：论文结论强调“归纳偏置比数据/参数更重要”，这一论断虽然基于对比实验，但可能因未披露完整实验设置而存在选择性报告的风险。

（完）
