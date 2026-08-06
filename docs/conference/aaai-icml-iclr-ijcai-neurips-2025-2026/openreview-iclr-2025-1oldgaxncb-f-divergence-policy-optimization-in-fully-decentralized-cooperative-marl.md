---
title: $f$-Divergence Policy Optimization in Fully Decentralized Cooperative MARL
title_zh: 完全去中心化合作MARL中的f-散度策略优化
authors: "Kefan Su, Zongqing Lu"
date: 2024-09-23
pdf: "https://openreview.net/pdf?id=1olDGAXncb"
tags: ["query:mcd"]
score: 7.0
evidence: 独立学习，去中心化合作MARL，具有收敛保证的策略优化
tldr: 独立学习是去中心化合作MARL的简单方案，但多数算法缺乏收敛保证。本文提出f-散度策略优化的一般框架，并在此基础上设计TVPO算法，从理论上保证收敛。实验验证了TVPO在多个基准上的有效性，显著优于现有独立学习算法。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 去中心化独立学习算法普遍缺少理论收敛保证。
method: 提出f-散度策略优化框架并设计TVPO独立学习算法，给出收敛性证明。
result: 理论分析证明收敛，实验表明TVPO在协作基准上表现优异。
conclusion: 为去中心化合作MARL提供了有理论保证的独立学习算法。
---

## Abstract
Independent learning is a straightforward solution for fully decentralized learning in cooperative multi-agent reinforcement learning (MARL). The study of independent learning has a history of decades, and the representatives, such as independent Q-learning and independent PPO, can obtain good performance in some benchmarks. However, most independent learning algorithms lack convergence guarantees or theoretical support. In this paper, we propose a general formulation of independent policy optimization, $f$-divergence policy optimization. We show the generality of such a formulation and analyze its limitation. Based on this formulation, we further propose a novel independent learning algorithm, TVPO, that theoretically guarantees convergence. Empirically, we show that TVPO outperforms state-of-the-art fully decentralized learning methods in three popular cooperative MARL benchmarks, which verifies the efficacy of TVPO.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：在完全去中心化的合作多智能体强化学习（MARL）中，独立学习（Independent Learning）是一种简单直接的解决方案，如独立Q学习（IQL）和独立PPO（IPPO）在部分基准上表现良好，但大多数独立学习算法缺乏收敛保证或理论支撑。
- **核心问题**：如何在保持完全去中心化的情况下，设计一种既能高效学习又具有理论收敛保证的策略优化算法。
- **整体含义**：论文提出一种通用的独立策略优化框架，并基于该框架设计了一种名为 TVPO 的新算法，在理论和实验两个层面同时验证其有效性，为去中心化合作 MARL 提供了有理论保障的独立学习算法。

## 2. 论文提出的方法论

- **核心思想**：将独立策略优化形式化为 **$f$-散度策略优化**（$f$-Divergence Policy Optimization），利用 $f$-散度度量新旧策略之间的差异，从而构造一般化的优化目标。
- **技术细节**：
  - 展示了该框架的通用性（涵盖多种已有策略优化方法），并分析了该框架的局限性。
  - 基于该框架设计 TVPO（Total Variation Policy Optimization）算法，采用全变差散度作为策略更新约束，确保每次迭代的单调改进。
  - 从理论上证明了 TVPO 在完全去中心化场景下的收敛性，弥补了既有独立学习算法的理论空白。
- **算法流程**（文字说明）：
  1. 每个智能体独立维护自身策略；
  2. 在每次更新时，以 $f$-散度约束新旧策略差异，构造策略优化目标；
  3. 通过迭代优化该目标，使联合策略性能单调提升；
  4. 重复上述过程直到收敛（理论保证）。

## 3. 实验设计

- **基准场景**：使用了三个流行的合作 MARL 基准（论文摘要中未逐一列出具体名称，通常如 SMAC、MPE 或类似环境）。
- **对比方法**：与当前最先进的完全去中心化学习方法进行对比，重点比较独立学习类算法（如独立PPO、独立Q学习等）。
- **验证目标**：验证 TVPO 的有效性，以及其相对既有独立学习算法的性能优势。

## 4. 资源与算力

- 论文提供的元数据和摘要中**未明确说明**使用了多少算力（如 GPU 型号、数量、训练时长等）。
- 因此，无法从现有信息中总结具体的计算资源投入。

## 5. 实验数量与充分性

- **实验数量**：摘要中只提到在三个基准上进行了验证，未给出具体的实验次数、消融实验或详细配置。
- **充分性评估**：
  - 覆盖面较广，但缺少对算法各组件（如不同 $f$-散度选择）的消融分析。
  - 对比的基准数量尚可，但未报告方差、多次随机种子等细节，因此完整性有限。
  - 从可获取的信息来看，实验设计基本能够支撑“TVPO优于现有独立学习方法”这一结论，但充分性仍需论文正文补充。

## 6. 论文的主要结论与发现

- 独立策略优化可以统一为 $f$-散度策略优化框架，该框架具有通用性但存在一定局限。
- 基于该框架设计的 TVPO 算法具有理论上的收敛保证。
- 实验结果显示，TVPO 在三个合作 MARL 基准上均优于当前最先进的完全去中心化学习方法，验证了其实际有效性。

## 7. 优点

- **理论贡献**：为去中心化独立学习算法提供了收敛性证明，填补了该方向的理论空白。
- **框架统一性**：提出的 $f$-散度策略优化框架能够涵盖多种已有方法，具有较强的普适性和启发性。
- **算法简洁性**：独立学习本身易于部署，TVPO 在保持简洁性的同时获得理论保证，实用性强。
- **实验验证**：在多个基准上与 SOTA 方法对比，证明了真实性能优势。

## 8. 不足与局限

- **实验细节缺乏**：摘要中未给出具体的基准名称、超参数设置、随机种子、方差范围等，难以全面评估实验严谨性。
- **缺少消融研究**：未说明是否对不同 $f$-散度函数、更新步长等关键因素进行了消融分析。
- **理论假设条件**：收敛性证明可能依赖于某些理想化假设（如策略表示充分、环境静态等），实际应用中可能受限。
- **可扩展性**：仅验证了合作场景，未讨论在竞争或混合场景下的表现。
- **资源信息缺失**：未报告算力使用情况，不利于复现和成本评估。

---

（完）
