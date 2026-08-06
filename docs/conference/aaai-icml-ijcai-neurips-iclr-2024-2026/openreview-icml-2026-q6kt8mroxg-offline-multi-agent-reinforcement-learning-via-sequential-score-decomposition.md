---
title: Offline Multi-Agent Reinforcement Learning via Sequential Score Decomposition
title_zh: 通过顺序得分分解的离线多智能体强化学习
authors: "Dan Qiao, Wenhao Li, Shanchao Yang, Hongyuan Zha, Baoxiang Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/42646707bb9bbc1bf3382011364ece1fae959b04.pdf"
tags: ["query:mcd"]
score: 8.0
evidence: 通过顺序得分分解处理多模态联合行为，提供协同正则化，解决离线合作MARL的分布偏移
tldr: 针对离线合作多智能体强化学习因多模态联合行为导致的分布偏移问题，提出OMSD方法。它将联合行为策略顺序分解为各个条件分布，并利用扩散生成模型为每个智能体提供模态协调正则化，从而更准确地约束联合策略。该方法缓解了独立策略正则化带来的对齐问题，增强了离线协调性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 离线合作MARL数据常是多种协作行为的混合，导致联合行为分布多模态，独立策略正则化会造成分布偏移。
method: 提出OMSD，将联合行为策略顺序分解为单个条件分布，并用扩散生成模型进行模态协调的正则化。
result: 通过模态协调正则化有效降低了分布偏移，提升了离线合作MARL的性能（实验未在摘要中详述）。
conclusion: 顺序得分分解为解决离线多智能体策略对齐和分布偏移提供了新方法。
---

## Abstract
Offline cooperative multi-agent reinforcement learning (MARL) faces unique challenges due to the distribution shift between online and offline data collection. While online MARL typically converges to a single coordinated joint policy, offline datasets are often mixtures of diverse cooperative behaviors, resulting in highly multimodal joint behavior distributions. In such settings, independent policy regularization often misaligns joint policy constraints and leads to severe distribution shift. To address this, we propose OMSD, which sequentially decomposes the joint behavior policy into individual conditional distributions and leverages diffusion-based generative models to provide modality-coordinated regularization for each agent. Combined with centralized critic guidance, OMSD achieves coordinated exploration within high-value, in-distribution regions, and avoids out-of-distribution joint actions. Experiments across multiple datasets on various continuous control tasks demonstrate that OMSD consistently achieves state-of-the-art performance, especially in challenging multimodal scenarios. Our results highlight the necessity of modality-aware coordination for robust offline MARL.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机：** 离线合作多智能体强化学习（MARL）中，由于离线数据收集过程与在线交互过程存在天然差异，导致严重的分布偏移问题。在线 MARL 通常收敛到单一且协调的联合策略，而离线数据集往往是多种不同协作行为模式的混合，使得联合行为分布呈现高度多模态。
- **核心挑战：** 在这种多模态联合行为分布下，如果对每个智能体独立施加策略正则化，容易导致联合策略约束的错位，进而引发严重的分布偏移，使智能体在评估或部署时产生越界（out-of-distribution）的联合动作。
- **整体含义：** 论文指出，离线稳健 MARL 需要“模态感知的协调”（modality-aware coordination），单纯依赖独立正则化是不够的，必须从联合行为分布的结构出发，设计能够协调多个智能体模态信息的策略约束方法。

## 2. 论文提出的方法论：核心思想、关键技术细节与流程

- **方法名称：** OMSD（Offline Multi-agent reinforcement learning via Sequential score Decomposition，即“通过顺序得分分解的离线多智能体强化学习”）。
- **核心思想：** 将联合行为策略进行“顺序分解”，转化为一系列条件分布，从而更准确地刻画多模态联合行为；同时利用基于扩散的生成模型，为每个智能体提供“模态协调正则化”。
  - 通过分解，可以避免独立策略正则化导致的联合约束错位。
  - 扩散生成模型用于捕捉各条件分布的高维、多模态特性。
- **关键技术细节：**
  - 顺序分解：将联合策略 \( \pi(a_1, a_2, \dots, a_n | s) \) 分解为 \( \pi(a_1|s) \pi(a_2|s, a_1) \cdots \pi(a_n|s, a_1, \dots, a_{n-1}) \) 的形式，从而保留智能体之间的依赖关系。
  - 模态协调正则化：每个条件分布都通过扩散模型进行学习，并通过某种得分（score）匹配机制，使各智能体的生成过程在共享的数据模态保持一致。
  - 集中式评论家（centralized critic）引导：在训练或策略生成过程中，使用集中式价值函数作为引导信号，使智能体在高价值且处于数据分布内的区域进行“协调探索”（coordinated exploration），避免选择超出分布范围的联合动作。
- **流程概述（文字说明）：**
  1. 从离线数据集中估计联合行为策略的顺序分解形式。
  2. 对每个条件分布训练扩散生成模型，学习其得分函数。
  3. 在策略优化时，使用集中式评论家提供价值信号，结合扩散模型的正则化，约束策略保持在分布内并偏向高价值区域。
  4. 通过协调探索生成动作，最终得到稳健的离线合作策略。

注：论文摘要未给出具体公式编号，上述说明基于抽象描述进行概括。

## 3. 实验设计

- **数据集 / 场景：** 多个连续控制任务上的多种数据集（具体数据集名称未在摘要或元数据中列出）。
- **Benchmark：** 需要提及的是，论文标题和摘要指向离线合作 MARL，实验中大概率采用类似 D4RL 中的多智能体连续控制扩展（如 Multi-Agent MuJoCo）或其他标准离线 MARL 基准，但当前提供的信息不明确。
- **对比方法：** 摘要仅说明“始终达到最先进性能”，未具体列出对比基线。通常离线 MARL 会有如 ICQ、CQL、IQL 或 BCQ 等方法的变体，但本文信息不完整。

## 4. 资源与算力

- **论文当前提供的信息中未明确提及：** 未给出 GPU 型号、数量、训练时长、预算等算力信息。
- 因此，无法从现有文本中总结具体算力投入，只能指出该信息缺失。

## 5. 实验数量与充分性

- **实验数量：** 摘要提到“在多种连续控制任务上的多个数据集”进行实验，并特别强调“在具有挑战性的多模态场景中”效果最佳，但没有给出具体实验组数、任务个数、数据集个数或消融实验细节。
- **充分性评估：**
  - 从摘要看，实验覆盖了多个数据集和多种任务，具有一定广度。
  - 但缺少消融实验、基线对比的详细列表以及统计显著性说明，因此无法完全判断实验的充分性和公平性。
  - 因为只提供摘要和元数据，没有正文的完整图表，无法客观评估所有实验细节。

## 6. 论文的主要结论与发现

- OMSD 通过顺序得分分解，有效缓解了离线合作 MARL 在多模态联合行为下的分布偏移问题。
- 结合集中式评论家引导，OMSD 能够实现在高价值、分布内区域的协调探索，避免越界联合动作。
- 在多个连续控制任务和数据集上，OMSD 一致达到当前最优（state-of-the-art）性能，尤其是在多模态场景下。
- 研究强调了“模态感知协调”对稳健离线 MARL 的必要性。

## 7. 优点：方法或实验设计上的亮点

- **方法新颖性：** 将联合策略拆分为顺序条件分布，直击多模态联合行为带来的对齐问题，比独立正则化更符合离线数据的实际产生过程。
- **扩散模型的应用：** 利用扩散生成模型对复杂条件分布建模，具备较强的表达能力和多模态捕获能力。
- **集中式评论家协同：** 将集中式价值信息融入生成过程，形成“生成+引导”的框架，兼顾分布约束和任务目标。
- **问题定位精准：** 明确指出独立策略正则化在多模态场景下的失败机理，并提出针对性解决方案。
- **实验覆盖挑战性场景：** 特别在多模态环境中验证，展示了方法的针对性和优势。

## 8. 不足与局限

- **信息不足：** 当前提供的文本仅包含摘要和元数据，缺少方法具体实现、公式细节和完整实验图表，难以客观评价全部技术贡献。
- **实验细节不透明：** 数据集名称、任务列表、基线方法、超参数设置、评估标准等均未公开，无法判断实验的公平性和全面性。
- **未报告算力：** 没有提供计算资源信息，不利于可重复性和成本估算。
- **应用限制：** 方法依赖于顺序分解，智能体数量较多时可能导致条件分布链过长、误差累积或扩散模型训练复杂度增加；同时，对集中式评论家的质量也有较高要求。
- **泛化性未知：** 仅涉及连续控制任务，未讨论离散动作空间、部分可观测场景或非合作/竞争环境下的表现。

（完）
