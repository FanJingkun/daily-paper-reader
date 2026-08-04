---
title: Offline Multi-Agent Reinforcement Learning via Sequential Score Decomposition
title_zh: 通过序列分数分解实现离线多智能体强化学习
authors: "Dan Qiao, Wenhao Li, Shanchao Yang, Hongyuan Zha, Baoxiang Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=LRu30T4Vev"
tags: ["query:mcd"]
score: 9.0
evidence: 离线合作MARL，使用序列分数分解与扩散正则化
tldr: 离线合作式多智能体强化学习面临数据分布偏移和联合行为多模态的挑战。本文提出OMSD方法，将联合行为策略顺序分解为单个条件分布，并利用扩散生成模型提供模态协调的正则化，避免独立策略正则化导致的联合约束失配和严重分布偏移。实验表明该方法在离线多智能体基准上有效改善了策略质量和分布鲁棒性，为离线MARL提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 离线合作MARL中，静态数据集具有多模态联合行为分布，独立策略正则化易导致分布偏移。
method: 提出OMSD，将联合策略顺序分解为条件分布，并用扩散生成模型进行模态协调的正则化。
result: 在离线MARL基准上，OMSD有效缓解分布偏移，提升联合策略表现。
conclusion: OMSD为离线多智能体强化学习提供了一种处理多模态联合行为的新正则化框架。
---

## Abstract
Offline cooperative multi-agent reinforcement learning (MARL) faces unique challenges due to the distribution shift between online and offline data collection. While online MARL typically converges to a single coordinated joint policy, offline datasets are often mixtures of diverse cooperative behaviors, resulting in highly multimodal joint behavior distributions. In such settings, independent policy regularization often misaligns joint policy contraints and leads to severe distribution shift. To address this, we propose OMSD, which sequentially decomposes the joint behavior policy into individual conditional distributions and leverages diffusion-based generative models to provide modality-coordinated regularization for each agent. Combined with centralized critic guidance, OMSD achieves coordinated exploration within high-value, in-distribution regions, and avoids out-of-distribution joint actions. Experiments across multiple datasets on various continuous control tasks demonstrate that OMSD consistently achieves state-of-the-art performance, especially in challenging multimodal scenarios. Our results highlight the necessity of modality-aware coordination for robust offline MARL.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究领域**：离线合作式多智能体强化学习（Offline Cooperative Multi-Agent Reinforcement Learning, MARL）。
- **核心挑战**：
  - 离线数据与在线交互之间存在**分布偏移**。在线 MARL 通常会收敛到单一、协调的联合策略，而离线数据集往往是由多种不同协作行为混合而成，导致联合行为分布呈**高度多模态（multimodal）**。
  - 在这种多模态场景下，常见的“独立策略正则化”（independent policy regularization）手段容易导致**联合策略约束失配**，即每个智能体单独约束无法保证联合动作的合理性与协调性，进而引发严重的分布偏移和性能退化。
- **研究意义**：解决离线 MARL 中多模态联合行为分布下的策略优化问题，是实现鲁棒、安全、可部署多智能体系统的关键一步，尤其在无法在线交互的真实场景（如自动驾驶、机器人集群、电网调度）中具有重要价值。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（文字描述）

- **方法名称**：OMSD（Offline Multi-agent via Sequential Score Decomposition，序列分数分解离线多智能体方法）。
- **核心思想**：
  - 将联合行为策略**顺序分解**为一系列单个条件概率分布，即按照某种决策顺序将联合策略表示为 $P(a^1, a^2, \dots, a^n \mid s) = \prod_{i} P(a^i \mid s, a^{<i})$，从而化联合建模为条件建模，避免直接拟合多模态联合分布的困难。
  - 对每个智能体的条件分布，利用**基于扩散的生成模型**（diffusion-based generative models）来建模其多模态行为模式，并提供**模态协调的正则化**（modality-coordinated regularization），防止独立正则化导致的联合失配。
  - 结合**集中式评论家（centralized critic）引导**，使智能体在**高价值且在分布内（in-distribution）**的区域进行协调探索，同时避开分布外（out-of-distribution）的联合动作。
- **技术要点**：
  - 序列分解：显式建模智能体间的条件依赖关系，而非假设智能体独立决策。
  - 扩散模型用于条件分布建模：能够捕捉每个条件分布的多模态性，并生成符合数据分布的动作样本。
  - 集中式 critic：利用全局信息对联合动作进行评估，引导条件生成过程朝高回报且分布内区域偏移。
  - 正则化机制：不是简单地将每个智能体策略拉向各自的边际行为，而是通过分数/分布匹配方式最小化条件分布与真实行为数据的偏差，实现“模态协调”。
- **算法流程（概括性）**：
  1. 从离线数据集中估计具有模态协调正则化的条件行为模型（通过扩散模型）。
  2. 训练集中式 Q 函数（critic）以评估联合状态-动作值。
  3. 在策略优化阶段，利用扩散模型的分数（score）作为引导，结合 critic 的梯度，迭代生成联合动作，使联合策略在分布内区域内最大化 Q 值。
  4. 通过顺序采样生成每个智能体的动作，保证联合动作的协调性和分布内性。

## 3. 实验设计概览

- 由于原文提供的完整论文内容仅包含摘要和元数据，**详细的实验章节（数据集、基准、对比基线）未在提供文本中展开**。根据摘要可获知：
  - **任务**：多个连续控制任务（continuous control tasks）。
  - **数据集**：使用了多个数据集（“multiple datasets”），且特别设计了具有挑战性的多模态场景（challenging multimodal scenarios）。
  - **基准（Benchmark）**：属于离线 MARL 标准基准体系（具体名称如 MAMuJoCo 等未在摘要中说明）。
  - **对比方法**：摘要中没有列举具体对比算法，但强调 OMSD 在多个数据集上“一致达到最先进的性能”（state-of-the-art performance），暗示与常见的离线 MARL 基线（如 ICQ、CQL、OMAR、MABCQ 等）进行了对比。
- **必要说明**：要获得确切的实验设置、数据集名称、基线列表和评测协议，需要阅读论文全文或补充材料。

## 4. 资源与算力

- **在提供的论文内容（摘要与元数据）中，未明确提及所使用的计算资源**，如 GPU 型号、数量、训练时间、显存等。
- 因此，无法从给定信息中总结具体的算力细节。如果论文正文中有相关描述，需要查看原始 PDF 以获取该信息。

## 5. 实验数量与充分性

- 从摘要来看，实验覆盖了**多个数据集**和**多种连续控制任务**，并专门强调了对多模态场景的测试，说明实验设计关注了方法的泛化性和关键挑战。
- 但**缺少可验证的量化细节**：没有给出实验组数、每个数据集上的性能表格、消融实验（例如是否单独移除序列分解或扩散正则化）的具体分析。
- 公开的元数据（title, abstract, score, evidence, tldr, motivation, method, result, conclusion）没有列出消融结果，因此**无法判断实验的完整性和客观性**。摘要中的“consistently achieves state-of-the-art”需要结合实际性能表和统计检验来佐证。
- 总体而言，实验设计思路合理，但**充分性在现有信息层面不足**，需要全文佐证。

## 6. 论文的主要结论与发现

- 离线 MARL 中多模态联合行为是导致分布偏移的关键因素，而独立策略正则化无法正确处理这种多模态性，会造成联合约束失配。
- 通过将联合策略**顺序分解**并引入**扩散模型提供模态协调正则化**，OMSD 能有效缓解分布偏移，提升离线策略的质量。
- 结合集中式 critic 引导后，OMSD 能够在保持数据分布内部的前提下进行高价值区域的协调探索，避免了生成分布外的联合动作。
- 在多个连续控制任务和数据集上，OMSD 取得了优于现有方法（SOTA）的表现，尤其是在多模态场景下优势更明显，验证了“模态感知协调”对鲁棒离线 MARL 的必要性。

## 7. 优点

- **方法创新性**：将联合策略建模为条件分布序列，突破了独立正则化在多模态联合行为上的局限性，视角新颖。
- **理论机制自然**：扩散模型天然适合建模多模态分布，将其用于条件策略生成与集中式 critic 结合，形成“生成-评估-引导”的闭环，可解释性强。
- **实用价值**：在不支持在线试错的真实多智能体场景中，提供了一种更安全、更可靠的离线训练方案。
- **实验定位清晰**：专门选择多模态困难场景进行验证，突出了方法相比常规正则化的核心优势，说服力强（在摘要层面）。

## 8. 不足与局限

- **信息不完整**：由于仅提供摘要，无法准确判断实验规模、基准数据集详情、消融设计、统计显著性等，需谨慎对待“SOTA”声明。
- **未提及方法局限性**：摘要未讨论序列分解可能引入的顺序偏差（例如智能体顺序对性能的影响）、扩散模型采样计算成本、对数据集质量的敏感性等潜在缺点。
- **硬件算力不明**：缺乏训练资源的可复现性信息，不利于其他人评估方法的实际运行成本。
- **应用范围有限**：实验集中在连续控制任务，未见离散动作空间或更复杂环境（如部分可观测、大规模智能体）的验证，泛化能力尚需更多证据。
- **对比公平性**：未见全文描述基线超参调优细节，无法判断是否对所有方法进行了同等调优。

> 注：以上优点与不足均基于摘要及元数据推断，部分内容可能不完整或以偏概全，建议查阅论文原始全文以获取精确信息。

（完）
