---
title: Effective Policy Learning for Multi-Agent Online Coordination Beyond Submodular Objectives
title_zh: 超越子模目标的多智能体在线协调有效策略学习
authors: "Qixin Zhang, Yan Sun, Can Jin, Xikun ZHANG, Yao Shu, Puning Zhao, Li Shen, Dacheng Tao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=isAYKdLwtB"
tags: ["query:mcd"]
score: 6.0
evidence: 面向多智能体在线协调，处理子模与弱子模目标并给出逼近保证
tldr: "多智能体在线协调(MA-OC)中，策略学习需要在线应对子模类目标。该文提出MA-SPL算法，不仅对一般子模目标实现最优(1-c/e)逼近，还扩展到α-弱DR-子模和(γ,β)-弱子模等未探索场景；为减少对未知参数的依赖，进一步设计第二个在线学习算法。该工作丰富了MA-OC问题的在线策略学习理论，使其适用范围更广。"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多智能体在线协调目标常具有子模或弱子模结构，已有算法覆盖场景有限。
method: 提出MA-SPL及另一在线算法，处理子模和弱子模目标并自适应参数。
result: 获得最优(1-c/e)逼近，并扩展到弱DR-子模等新场景。
conclusion: 扩展了多智能体在线协调策略学习的理论边界与适用性。
---

## Abstract
In this paper, we present two effective  policy learning algorithms for multi-agent online coordination(MA-OC) problem. The first one, **MA-SPL**,  not only can achieve the optimal $(1-\frac{c}{e})$-approximation guarantee for the MA-OC problem with submodular objectives but also can handle the unexplored  $\alpha$-weakly DR-submodular and $(\gamma,\beta)$-weakly submodular scenarios, where $c$ is the curvature of the investigated submodular functions, $\alpha$ denotes the diminishing-return(DR) ratio and the tuple$(\gamma,\beta)$ represents the submodularity ratios.  Subsequently, in order to reduce the reliance on the unknown parameters $\alpha,\gamma,\beta$ inherent in the **MA-SPL** algorithm, we then introduce the second online algorithm named **MA-MPL**. This **MA-MPL** algorithm is entirely *parameter-free* and simultaneously can maintain the same approximation ratio as the first  **MA-SPL** algorithm. The core of our **MA-SPL** and **MA-MPL** algorithms is a novel continuous-relaxation technique term as policy-based continuous extension. Compared with the well-established multi-linear extension, a notable advantage of this new policy-based continuous extension is its ability to provide a lossless rounding scheme for any set function, thereby enabling us to tackle the challenging  weakly submodular objective functions. Finally, extensive simulations are conducted to demonstrate the effectiveness of our proposed algorithms.

---

## 论文详细总结（自动生成）

## 论文中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究问题**：多智能体在线协调（Multi-Agent Online Coordination, MA-OC）问题中，如何设计高效策略学习算法，并给出逼近性能保证。
- **背景与动机**：在 MA-OC 场景中，目标函数通常具有子模（submodular）或弱子模结构，但已有算法往往只针对标准子模目标，且依赖较强的结构假设，覆盖场景有限。本文旨在突破这一局限，为更一般的目标函数类别提供在线学习策略。
- **整体含义**：论文将在线策略学习的理论边界从标准子模目标扩展到弱 DR-子模（α-weakly DR-submodular）和 (γ, β)-弱子模目标，并设计参数无关的算法，从而提升多智能体协调策略的普适性和实用性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（文字说明）
- **核心思想**：提出两种在线策略学习算法，分别命名为 **MA-SPL** 和 **MA-MPL**，用于解决带子模/弱子模目标的多智能体在线协调问题。
- **算法 1：MA-SPL**
  - 对于具有子模目标的 MA-OC 问题，能获得 **最优的 (1 - c/e) 逼近保证**，其中 c 为子模函数的曲率（curvature）。
  - 还能处理此前未被探索的 **α-弱 DR-子模目标**（α 为 DR 比率）和 **(γ, β)-弱子模目标**（γ、β 为子模比率）。
- **算法 2：MA-MPL**
  - 为了解决 MA-SPL 对未知参数 α、γ、β 的依赖问题，进一步提出 **完全参数无关（parameter-free）** 的在线算法 MA-MPL。
  - 在无需知道 α、γ、β 具体值的情况下，MA-MPL 仍能保持与 MA-SPL 相同的逼近比率。
- **关键技术：基于策略的连续扩展（policy-based continuous extension）**
  - 这是两种算法共同的核心组件，不同于经典的多线性扩展（multi-linear extension）。
  - 该新扩展的关键优势在于：**对任意集合函数都能提供无损舍入（lossless rounding）**，因此可以应对具有挑战性的弱子模目标函数。
- **流程概要**（文字说明）：
  1. 将离散的多智能体策略选择问题松弛为连续域上的优化问题，利用策略连续扩展构造目标函数。
  2. 在连续域上执行在线梯度/镜像类更新，获得分数策略。
  3. 利用无损舍入机制将连续策略映射回离散决策，保证逼近损失可控，从而获得最终近似保证。

### 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法
- **实验场景**：摘要中仅提及“进行了大量模拟实验（extensive simulations）”以验证算法有效性。
- **数据集 / 基准**：论文文本未具体说明使用了哪些真实数据集或标准 benchmark，也未列出对比算法的名称。
- **对比方法**：未在摘要中明确给出，推测会与已有的 MA-OC 在线学习算法进行对比，但具体细节无法从提供文本中确认。
- **评估指标**：未明确给出，通常为累积遗憾（regret）或近似逼近比等指标，但需要查看全文确认。

### 4. 资源与算力
- **未明确说明**：摘要及元数据中**没有提及**任何算力信息，包括 GPU 型号、数量、训练时长、计算集群等。
- 可能的解释：该工作侧重于理论贡献（算法设计与逼近保证），实验可能为小规模仿真，未涉及大规模深度学习训练，因此未占用大量算力资源。但这一推断需要以论文全文为准。

### 5. 实验数量与充分性
- **实验数量**：摘要仅笼统地说“extensive simulations”，未给出具体实验组数、消融实验数量或案例数量。
- **充分性评估**：
  - 由于提供文本缺乏实验细则，无法判断实验的充分性和公平性。
  - 从摘要表述看，作者通过多组模拟验证了 MA-SPL 和 MA-MPL 的有效性，但**没有展示真实应用场景**，也未提供与理论界匹配的数值验证细节。
  - 因此，实验充分性需要依据全文评估；仅从摘要看，模拟实验可能不足以覆盖所有声称的理论场景（如弱子模目标）。

### 6. 论文的主要结论与发现
- **理论突破**：MA-SPL 在子模目标下达到最优的 (1 - c/e) 逼近，同时首次覆盖 α-弱 DR-子模和 (γ, β)-弱子模目标。
- **参数无关性**：MA-MPL 可以在不依赖未知参数的情况下获得与 MA-SPL 相同的逼近保证，提升了算法的实际可用性。
- **方法论贡献**：提出的基于策略的连续扩展技术优于传统多线性扩展，能实现无损舍入，为处理弱子模目标提供了新工具。
- **实验验证**：仿真结果证明了所提算法的有效性，但具体量化结果未在摘要中呈现。

### 7. 优点
- **理论深度**：将逼近保证从标准子模推广到弱子模/弱 DR 子模，显著扩展了 MA-OC 的适用范围。
- **算法设计的创新性**：提出“policy-based continuous extension”，解决了弱子模函数下多线性扩展无法无损舍入的核心困难。
- **参数无关的实用价值**：MA-MPL 消除对 α、γ、β 等未知参数的依赖，降低了实际部署的调参成本。
- **同时具备最优性**：在标准子模情形下，能达到理论上最优的 (1 - c/e) 逼近比，说明扩展没有牺牲标准情形下的性能。

### 8. 不足与局限
- **实验信息缺失**：摘要未提供任何具体实验结果、数据规模、基准方法，导致读者无法评估实验的充分性与公平性。
- **算力信息缺失**：未说明任何计算资源，不利于复现和性能可扩展性评估。
- **应用场景有限**：仅提到模拟实验，缺乏真实世界多智能体任务（如机器人协调、传感器网络等）的验证，实用性证据不足。
- **潜在偏差风险**：对弱子模目标的实验覆盖是否涵盖不同 α、γ、β 取值组合尚不明确，若仅测试部分参数范围，理论的泛化性可能得不到充分支撑。
- **表述简洁性**：摘要中许多关键细节（如遗憾界的具体形式、运行时间复杂度）未展示，限制了对算法效率的全面判断。

（完）
