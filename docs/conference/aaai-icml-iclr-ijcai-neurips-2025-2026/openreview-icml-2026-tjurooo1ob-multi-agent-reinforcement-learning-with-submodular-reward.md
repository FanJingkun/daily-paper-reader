---
title: Multi-Agent Reinforcement Learning with Submodular Reward
title_zh: 具有子模奖励的多智能体强化学习
authors: "Wenjing Chen, Chengyuan Qian, Shuo Xing, Yi Zhou, Victoria G. Crawford"
date: 2026-04-30
pdf: "https://openreview.net/pdf/52c8952ec563ab82ff66954193671cbf67c864cb.pdf"
tags: ["query:mcd"]
score: 7.0
evidence: 研究子模奖励下的协作MARL，提出贪心策略优化并给出近似保证
tldr: 在协作多智能体强化学习中，许多真实场景的联合奖励满足子模性，即新增智能体的边际收益递减。该文首次为这类子模奖励建立正式框架，并在已知动力学下提出贪心策略优化算法，以多项式复杂度取得1/2近似最优，同时给出样本效率与遗憾界保证。这一工作为多无人机监控、协作探索等应用提供了有理论保障的MARL建模与优化方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 子模奖励能刻画智能体贡献重叠带来的边际收益递减，现有MARL多假设可加奖励。
method: 建立子模奖励MARL形式化框架，提出贪心策略优化并分析复杂度与遗憾界。
result: 取得了1/2近似最优保证，复杂度关于智能体数量为多项式。
conclusion: 为协作MARL中子模奖励场景提供了首个具有理论保证的求解框架。
---

## Abstract
In this paper, we study cooperative multi-agent reinforcement learning (MARL) where 
the joint reward exhibits submodularity, which is a natural property capturing 
diminishing marginal returns when adding agents to a team. Unlike standard 
MARL with additive rewards, submodular rewards model realistic scenarios 
where agent contributions overlap (e.g., multi-drone surveillance, 
collaborative exploration). We provide the first formal framework for this 
setting and develop algorithms with provable guarantees on sample efficiency and regret bound. For known dynamics, 
our greedy policy optimization achieves a $1/2$-approximation with polynomial 
complexity in the number of agents $K$, overcoming the exponential curse of 
dimensionality inherent in joint policy optimization. For unknown dynamics, 
we propose a UCB-based learning algorithm achieving a $1/2$-regret of $O(H^2KS\sqrt{AT})$ over 
$T$ episodes.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：标准协作多智能体强化学习（MARL）通常假设联合奖励具有可加性，即智能体对总奖励的贡献相互独立。然而许多真实场景中，智能体的贡献会重叠，新增智能体的边际收益递减。
- **核心问题**：本文研究联合奖励满足**子模性**（submodularity）的协作 MARL 问题。子模性自然刻画了“多智能体协作时边际收益递减”的属性，例如多无人机监控、协作探索等任务。
- **研究意义**：这是**首次**为子模奖励下的 MARL 建立正式理论框架，并给出样本效率与遗憾界的可证明保证，填补了该方向的理论空白。

## 2. 论文提出的方法论

- **核心思想**：利用子模函数特有的贪心优化性质，避免直接搜索联合策略造成的“维度灾难”，从而在智能体数量多项式复杂度内求得近似最优解。
- **形式化框架**：将带子模联合奖励的协作 MARL 建模为新的优化问题，明确其与可加奖励 MARL 的区别。
- **已知动力学下的算法**：
  - 提出贪心策略优化算法（Greedy Policy Optimization）。
  - 通过逐智能体贪心选择策略，获得 **1/2 近似最优** 保证。
  - 计算复杂度关于智能体数量 $K$ 为多项式，克服了联合策略优化固有的指数复杂度。
- **未知动力学下的算法**：
  - 提出基于 UCB（Upper Confidence Bound）的学习算法。
  - 在 $T$ 个回合内实现 **1/2 遗憾界**：$O(H^2 K S \sqrt{AT})$，其中 $H$ 为回合长度、$S$ 为状态数、$A$ 为动作数、$K$ 为智能体数。
  - 该算法在探索与利用之间权衡，保证了样本效率。

## 3. 实验设计

- 由于提供的文本仅包含摘要和元数据，**未涉及任何实验设计信息**。
- 文中未提及使用的数据集、具体场景、基准方法（baseline）或对比算法。
- 从摘要可推测，作者可能针对多无人机监控、协作探索等典型子模场景进行仿真，但具体细节需要查阅论文全文。

## 4. 资源与算力

- 提供文本中**没有说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 若论文正文有相关说明，此处无法获取。仅从摘要无法判断实验规模。

## 5. 实验数量与充分性

- 由于实验细节缺失，无法评估实验组数（如不同智能体规模、不同环境、消融实验等）。
- 摘要作为主要贡献在于理论分析，实验部分可能作为补充验证，但**充分性和公平性无法从当前文本判断**。

## 6. 论文的主要结论与发现

- 子模奖励 MARL 可以被形式化建模，且其最优策略可以通过贪心方式近似求解。
- 在已知动力学下，贪心策略优化能取得 **1/2 近似** 保证，且复杂度为多项式，突破了联合策略搜索的指数难关。
- 在未知动力学下，UCB 学习方法能够获得 **$O(H^2 K S \sqrt{AT})$** 的遗憾界，同样达到 1/2 近似水平。
- 该工作为协作 MARL 中的子模奖励场景提供了首个具有理论保证的求解框架。

## 7. 优点

- **理论创新**：首次将子模优化与 MARL 结合，建立了严格的数学框架。
- **算法保证**：同时提供近似比、样本复杂度和遗憾界的理论证明，而非仅凭经验。
- **复杂度可控**：算法相对于智能体数量是多项式复杂度，适合多智能体实际部署。
- **适用面广**：覆盖已知与未知动力学两种情形，具有较强推广性。

## 8. 不足与局限

- **实验细节缺失**：本文提供的摘要和元数据未包含实验部分，无法验证理论结果在真实环境中的表现。
- **假设限制**：奖励子模性是一个较强假设，并非所有协作任务都满足；对于奖励非子模的情况，该框架不适用。
- **近似系数**：1/2 近似虽然具有理论保证，但在实际性能上可能存在较大差距，且未知动力学下的遗憾界依赖状态数等参数，在大规模状态空间中可能仍然较高。
- **文本不完整**：基于 OpenReview 的提取文本，无法获取完整论文内容，因此关于实验设计、基线的对比、消融分析等均无法评估。

（完）
