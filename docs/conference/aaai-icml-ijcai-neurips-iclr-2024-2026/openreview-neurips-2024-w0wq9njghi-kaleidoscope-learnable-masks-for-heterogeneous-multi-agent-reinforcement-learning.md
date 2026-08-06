---
title: "Kaleidoscope: Learnable Masks for Heterogeneous Multi-agent Reinforcement Learning"
title_zh: 万花筒：面向异构多智能体强化学习的可学习掩码
authors: "Xinran Li, Ling Pan, Jun Zhang"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=W0wq9njGHi"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 通过可学习部分参数共享实现异构智能体协作
tldr: 在MARL中，完全参数共享虽能提高样本效率，却使智能体策略趋于同质，限制了多样化策略的潜力。Kaleidoscope提出一种自适应部分参数共享机制：保留一组公共共享参数，同时为不同智能体学习多组可学习掩码来控制参数共享范围。该方法在提升策略网络多样性的同时保持高样本效率，并在协作MARL基准中取得优异成绩，为异构策略训练提供了一种简洁有效的新方案。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 完全参数共享会导致智能体策略同质化，限制策略多样性带来的性能增益。
method: Kaleidoscope在共享参数外为各智能体学习可微掩码，自适应决定参数共享范围，促进异构策略。
result: 在多个MARL基准上策略多样性提升且样本效率保持，性能优于完全共享基线。
conclusion: 可学习掩码为异构多智能体策略训练提供了简单高效的通用方案。
---

## Abstract
In multi-agent reinforcement learning (MARL), parameter sharing is commonly employed to enhance sample efficiency. However, the popular approach of full parameter sharing often leads to homogeneous policies among agents, potentially limiting the performance benefits that could be derived from policy diversity. To address this critical limitation, we introduce \emph{Kaleidoscope}, a novel adaptive partial parameter sharing scheme that fosters policy heterogeneity while still maintaining high sample efficiency. Specifically, Kaleidoscope maintains one set of common parameters alongside multiple sets of distinct, learnable masks for different agents, dictating the sharing of parameters. It promotes diversity among policy networks by encouraging discrepancy among these masks, without sacrificing the efficiencies of parameter sharing. This design allows Kaleidoscope to dynamically balance high sample efficiency with a broad policy representational capacity, effectively bridging the gap between full parameter sharing and non-parameter sharing across various environments. We further extend Kaleidoscope to critic ensembles in the context of actor-critic algorithms, which could help improve value estimations. Our empirical evaluations across extensive environments, including multi-agent particle environment, multi-agent MuJoCo and StarCraft multi-agent challenge v2, demonstrate the superior performance of Kaleidoscope compared with existing parameter sharing approaches, showcasing its potential for performance enhancement in MARL. The code is publicly available at \url{https://github.com/LXXXXR/Kaleidoscope}.

---

## 论文详细总结（自动生成）

# 论文总结：《Kaleidoscope: Learnable Masks for Heterogeneous Multi-agent Reinforcement Learning》

## 1. 核心问题与整体含义

- **研究背景**：在多智能体强化学习（MARL）中，参数共享（parameter sharing）是提升样本效率（sample efficiency）的常用手段，被广泛应用于各类协作任务。
- **核心问题**：目前最主流的**完全参数共享**方法虽然高效，但会导致所有智能体的策略趋于同质化（homogeneous policies），从而限制了从策略多样性（policy diversity）中获得的性能增益。
- **研究动机**：作者试图解决“**样本效率**”与“**策略多样性**”之间的矛盾——即如何在不放弃参数共享带来的高效训练优势的前提下，让不同智能体拥有差异化、异构化的策略，从而释放协作潜力。
- **整体意义**：该工作为异构多智能体策略训练提供了一种通用的解决方案，在共享与不共享之间架起一座“可调节的桥梁”，对提升 MARL 在复杂协作任务中的表现具有理论与实践价值。

## 2. 方法论

- **核心思想**：Kaleidoscope 提出一种**自适应的部分参数共享机制**。它保留一组**公共共享参数**（common parameters），同时为每个智能体分配多组**可学习的掩码**（learnable masks），由掩码来决定哪些参数共享、哪些参数独立。
- **技术细节**：
  - 通过一组或多组二值化/可微分的掩码，对共享参数进行选择性地“开启”或“关闭”，使得不同智能体的有效网络结构产生差异。
  - 方法通过**鼓励掩码之间的差异（discrepancy among masks）** 来促进策略网络的多样性，但同时又保留了大部分参数共享带来的训练效率。
  - 采用可微的掩码学习方式，使其能端到端地与策略网络一起训练，避免了离散选择的不可导问题。
  - 该机制可**动态调节**共享程度，即在训练过程中根据任务需要自动平衡“高样本效率”与“广泛的策略表达能力”。
- **扩展设计**：作者进一步将该方法扩展到 actor-critic 算法中的 **critic 集成（ensemble）**，通过多样化的 critic 集成改善价值估计的准确性。
- **算法流程（文字描述）**：初始化一组公共网络参数 → 为每个智能体初始化一组可学习的掩码 → 在前向传播中，掩码对公共参数进行调制，生成每个智能体的策略网络 → 训练时同时优化策略损失和掩码差异正则 → 随着训练进行，掩码自适应地确定每个智能体参数共享的边界。

## 3. 实验设计

- **基准环境（Benchmarks）**：
  - **Multi-Agent Particle Environment (MPE)**：经典的多智能体粒子环境，用于基础协作任务验证。
  - **Multi-Agent MuJoCo (MAMuJoCo)**：连续控制的多人协同控制任务。
  - **StarCraft Multi-Agent Challenge v2 (SMACv2)**：更具挑战性的星际争霸微操任务，是目前 MARL 领域的公认高难度基准。
- **对比方法**：与现有的参数共享方法（existing parameter sharing approaches）进行对比，包括完全参数共享基线等（具体对比算法名称在原文摘要中未逐一列出）。
- **评估维度**：主要考察策略性能（最终回报/胜率）、样本效率以及策略多样性等指标。

## 4. 资源与算力

- **未明确说明**：在提供的文本（标题、摘要、元数据）中，**未提及**实验所使用的 GPU 型号、数量、训练时长、参数量等算力资源信息。
- 需要指出：论文正文（完整 PDF）中或可查见相关实验配置，但当前获取的内容中**不包含**算力资源的具体描述。

## 5. 实验数量与充分性

- **实验数量**：从摘要可见，论文在**三个不同的基准套件**（MPE、MAMuJoCo、SMACv2）上进行了验证，覆盖了离散/连续控制、合作程度、任务复杂度等不同维度，整体覆盖面较广。
- **扩展实验**：额外包含了对 critic ensemble 的扩展验证，说明实验不仅关注策略网络本身，还考虑了价值估计环节的改进。
- **充分性评估**：
  - 从实验场景的广度和基准的代表性来看，实验设计较为**充分**，具有较强的说服力。
  - 但就当前提供的文本而言，**消融实验的细节**（例如不同掩码初始化策略、掩码数量影响、不同异构程度下性能变化）并未在摘要中体现，具体充分性需查看论文正文。
  - 公平性方面，由于未列出所有对比方法的具体名称和超参数设置，无法从当前信息完全判断对比的公平性，但选取上述三大主流基准本身即有较强公信力。

## 6. 主要结论与发现

- 完全参数共享虽然高效，但会导致策略同质化，从而限制协作性能的上限。
- Kaleidoscope 通过可学习掩码实现**自适应部分参数共享**，在提高策略网络多样性的同时保持了较高的样本效率。
- 在多个 MARL 基准环境中，Kaleidoscope 相较现有参数共享方法取得了**更优的性能**，验证了“通过可学习掩码实现异构策略”这一思路的有效性。
- 将掩码机制扩展到 critic 集成可以进一步**改善价值估计**，带来额外性能增益。

## 7. 优点

- **方法简洁通用**：通过一组可学习的掩码即可实现异构策略，与现有 actor-critic 架构易于结合，不依赖任务特定的先验知识。
- **动静结合**：既能享受参数共享的训练效率，又能通过掩码差异引入策略多样性，二者动态平衡。
- **扩展性强**：不仅适用于策略网络，还能推广到 critic 集成，体现了方法的通用性和潜力。
- **实验扎实**：在三大主流基准（MPE、MAMuJoCo、SMACv2）上验证，实验环境覆盖面和难度均较高，结果具有较强说服力。
- **开源贡献**：代码已公开在 GitHub，便于后续研究者复现和在此基础上做进一步探索，有利于学术社区的交流和验证。

## 8. 不足与局限

- **算力信息缺失**：当前材料中未报告训练资源、时间和成本，不利于评估方法的实际工程开销。
- **对比细节有限**：摘要中仅说明与“现有参数共享方法”对比，未能明确列出所有基线方法的版本、超参数调优情况，存在一定对比公平性的不确定性。
- **计算开销分析不足**：引入多组可学习掩码会带来额外的参数量和计算开销，文摘中未讨论这部分代价在资源受限场景下的影响。
- **理论分析有限**：对于“掩码差异”与“策略多样性”之间的定量关系、掩码收敛特性等，当前信息未体现深入的理论分析。
- **应用范围限制**：方法主要针对协作型 MARL 任务验证，在竞争性或混合动机场景下的泛化能力尚待检验；对大规模智能体（如数百个）的可扩展性也有待进一步讨论。

---

（完）
