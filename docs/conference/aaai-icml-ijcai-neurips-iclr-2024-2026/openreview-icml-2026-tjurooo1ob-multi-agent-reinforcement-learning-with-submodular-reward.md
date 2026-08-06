---
title: Multi-Agent Reinforcement Learning with Submodular Reward
title_zh: 基于子模奖励的多智能体强化学习
authors: "Wenjing Chen, Chengyuan Qian, Shuo Xing, Yi Zhou, Victoria G. Crawford"
date: 2026-04-30
pdf: "https://openreview.net/pdf/52c8952ec563ab82ff66954193671cbf67c864cb.pdf"
tags: ["query:mcd"]
score: 8.0
evidence: 围绕子模奖励的合作MARL，直接涉及奖励设计与边际收益递减现象
tldr: 合作多智能体强化学习中，联合奖励常呈子模性（边际收益递减），而现有方法多假设奖励可加。论文首次为该类问题建立形式化框架，提出贪心策略优化算法，利用子模结构避免联合策略空间指数爆炸。在已知动态下，算法达到1/2近似保证且复杂度随智能体数量多项式增长。该工作为多无人机监控、协作探索等重叠贡献场景提供了可扩展的理论支撑。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 标准MARL假设奖励可加，但现实中多智能体贡献重叠导致联合奖励呈边际收益递减，现有方法难以处理。
method: 形式化子模奖励下的合作MARL问题，提出贪心策略优化算法，利用子模性降低联合策略优化复杂度。
result: 在已知动态下实现1/2近似比，样本复杂度和regret界有理论保证，复杂度关于智能体数量为多项式。
conclusion: 首次为子模奖励MARL建立理论框架，克服联合策略优化维数灾难，适用于重叠贡献场景。
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

## 1. 核心问题与整体含义

- **背景**：标准多智能体强化学习（MARL）通常假设联合奖励是**可加（additive）**的，即总奖励为各智能体奖励之和。然而，现实中的许多协作任务（如多无人机监控、协作探索）中，多个智能体的贡献会**重叠**，导致联合奖励呈**边际收益递减**（diminishing marginal returns）现象，即新增一个智能体对团队总收益的提升是递减的。
- **核心问题**：这类具有**子模性（submodularity）**的联合奖励无法被现有可加性假设框架处理，且联合策略优化面临**维度灾难**——联合策略空间随智能体数量 K 呈指数增长。
- **研究意义**：该工作是**首次为该类子模奖励 MARL 问题建立正式理论框架**，为多智能体系统在“贡献重叠”场景下的学习与优化提供了可证明的算法保障，填补了可加奖励假设之外的重要理论空白。

---

## 2. 方法论

- **核心思想**：利用联合奖励的**子模结构**（即边际收益递减性），将联合策略优化问题转化为具有子模属性的组合优化问题，从而避免对指数级联合策略空间的直接穷举搜索。
- **已知动态（Known Dynamics）下的贪心策略优化**：
  - 提出一个**贪心策略优化算法**，在每一步迭代中选择当前对团队边际贡献最大的策略/动作，构造联合策略。
  - 利用子模函数贪心选择的理论性质，证明该算法达到了 **1/2 近似比**（approximation ratio）。
  - **复杂度优势**：算法复杂度关于智能体数量 K 为**多项式级**，克服了联合策略优化的指数爆炸问题。
- **未知动态（Unknown Dynamics）下的 UCB 学习算法**：
  - 在环境转移概率未知的情况下，算法引入 **UCB（Upper Confidence Bound）** 类型的置信上界来平衡**探索与利用**。
  - 理论保证：在 T 个回合内达到 **1/2-遗憾**，遗憾界为 \(O(H^2 K S \sqrt{A T})\)，其中 H 为回合长度，S 为状态数，A 为动作数，T 为总回合数。
- **核心公式（文字说明）**：遗憾界表达式 \(O(H^2KS\sqrt{AT})\) 体现了对状态规模、动作规模、智能体数量与回合数的多项式依赖，而非指数依赖，验证了算法的可扩展性。

---

## 3. 实验设计

- **场景**：文中提到的动机场景为**多无人机监控**和**协作探索**，但所提供文本（元数据与摘要）中**未包含具体实验细节**。
- **数据集 / Benchmark**：未提及任何使用的数据集名称、仿真环境或基准测试平台（如 SMAC、MAMuJoCo 等）。
- **对比方法**：未提及与哪些现有 MARL 方法进行了对比。由于该框架是首次正式针对子模奖励设定，可能缺乏直接可比的 baseline。

> 需要说明：以上实验相关的空白是**因提供的文本中未给出实验章节**，而非作者明确省略。

---

## 4. 资源与算力

- 所提供文本（元数据、摘要、TLDR）中**未提到任何算力信息**。
- **未涉及内容**：未说明所使用 GPU 型号、数量、训练时长、显存占用或计算集群配置。
- 该文从内容看偏重**理论贡献**（近似保证、后悔界），可能实验部分（如有）规模有限，但无法从当前提供的资料中确认。

---

## 5. 实验数量与充分性

- **实验数量**：在提供的文本中**无法得知具体实验组数**（如不同任务、不同智能体数量、消融实验等）。
- **充分性评估**：
  - 从理论角度，文中给出了**明确的数学定理与复杂度分析**（1/2 近似比、\(O(H^2KS\sqrt{AT})\) 遗憾界），在理论上是自洽的。
  - 但**缺乏实验验证**意味着：无法评估算法在真实或模拟环境中的实际效果、收敛速度以及与基线方法的相对性能。
  - 关于实验是否客观、公平，因缺乏实验描述，**目前无法做出判断**。

---

## 6. 主要结论与发现

- 首次为**子模奖励的合作 MARL** 问题建立了形式化的理论框架。
- 在**已知动态**下，贪心策略优化算法可实现 **1/2 近似保证**，且计算复杂度关于智能体数量 K 为**多项式级**，突破了联合策略优化的指数维度灾难。
- 在**未知动态**下，基于 UCB 的学习算法可达到 \(O(H^2KS\sqrt{AT})\) 的 1/2-遗憾，同时保证样本效率。
- 该框架适用于**多智能体贡献重叠**的现实场景，为多无人机监控、协作探索等任务提供了**可扩展的理论支撑**。

---

## 7. 优点

- **理论创新性强**：首次针对子模奖励（而非可加奖励）下的合作 MARL 建立正式理论框架，填补了重要空白。
- **利用问题结构**：成功利用子模函数的数学性质（边际收益递减），设计出具有多项式复杂度的优化算法，规避了联合策略空间的维数灾难。
- **理论保证完整**：同时覆盖已知动态（近似比）与未知动态（遗憾界）两种设置，遗憾界对状态、动作、智能体数量均呈多项式依赖。
- **实用动机明确**：以多无人机监控、协作探索等真实场景为背景，应用价值清晰。

---

## 8. 不足与局限

- **实验验证缺失**：从提供的文本看，**未包含任何实验部分**（数据集、仿真环境、基线对比、消融研究均未提及），因此算法的**实际效果未知**，理论与实践之间尚存鸿沟。
- **近似保证可能次优**：1/2 近似比是否已达到该问题下理论上限（即是否为最优常数）在文本中未见讨论。
- **动态/奖励结构假设较强**：算法依赖于“奖励具有子模性”这一结构性假设，若真实任务的奖励不具备该性质或其子模性难以验证，方法将不适用。
- **覆盖场景有限**：目前仅讨论合作 MARL 下的同质贡献重叠场景，未涉及异构智能体、非平稳环境、部分可观测等更复杂的现实条件。
- **算力与可复现性信息不足**：未报告训练资源与实现细节，第三方难以复现或扩展其成果。
- **审稿评分中等**：该论文在评审中获得 8 分（outperforming），说明被认可，但也提示对于实验完备性等方面存在进一步提升空间。

---

（完）
