---
title: "Unified Mirror Descent: Towards a Big Unification of Decision Making"
title_zh: 统一镜像下降：迈向决策制定的统一框架
authors: "Pengdeng Li, Shuxin Li, Chang Yang, Xinrun Wang, Shuyue Hu, Xiao Huang, Hau Chan, Bo An"
date: 2023-09-22
pdf: "https://openreview.net/pdf?id=Ggu3cWldTy"
tags: ["query:mcd"]
score: 8.0
evidence: 面向合作、竞争与混合决策的统一算法
tldr: 该文提出统一镜像下降（UMD）算法，尝试用单一算法解决单智能体、合作多智能体、竞争多智能体及混合合作-竞争决策问题。UMD算法将多种基础策略的镜像下降更新协同整合，并针对不同博弈场景自适应调整更新规则。在包括MARL常见测试环境在内的多组任务上，UMD展示了可与专门算法媲美的性能。这项工作为统一多智能体决策算法提供了理论框架和实证基础。
source: ICLR-2024-Rejected-Public
selection_source: conference_retrieval
motivation: 单智能体、合作、竞争及混合决策问题长期独立发展，缺乏统一算法框架。
method: 提出统一镜像下降（UMD），将多种基础算法协同整合为单一决策算法。
result: 算法在多种决策场景中均有效，展示了统一框架的潜力。
conclusion: 开启通用决策算法方向，有望统一多类决策问题的求解范式。
---

## Abstract
Decision-making problems, encompassing single-agent, cooperative multi-agent, competitive multi-agent, and mixed cooperative-competitive cases, are ubiquitous in real-world applications. In the past several decades, substantial strides in theoretical and algorithmic advancements have been achieved within these fields. Nevertheless, these fields have been predominantly evolving independently, giving rise to a fundamental question: Can we develop a single algorithm to effectively tackle all these scenarios? In this work, we embark upon an exploration of this question by introducing a unified approach to address all types of decision-making scenarios. First, we propose a unified mirror descent (UMD) algorithm which synergistically integrates multiple base policy update rules. Specifically, at each iteration, the new policy of an agent is computed by weighting the base policies obtained through different policy update rules. One of the advantages of UMD is that only minimal modifications are required when integrating new policy update rules. Second, as the evaluation metric of the resulting policy is non-differentiable with respect to the weights of the base policies, we propose a simple yet effective zero-order method to optimize these weights. Finally, we conduct extensive experiments on 24 benchmark environments, which shows that in over 87\% (21/24) games UMD performs better than or on-par with the base policies, demonstrating its potential to serve as a unified approach for various decision-making problems. To our knowledge, this is the first attempt to comprehensively study all types of decision-making problems under a single algorithmic framework.

---

## 论文详细总结（自动生成）

基于提供的论文内容，以下是对该工作的结构化中文总结。

## 1. 核心问题与整体含义（研究动机与背景）
- 决策问题广泛存在于现实应用中，涵盖**单智能体、合作多智能体、竞争多智能体以及混合合作-竞争**四类场景。
- 过去几十年，这些子领域在理论和算法上均取得了显著进展，但**长期独立发展**，缺乏统一的算法框架。
- 核心研究问题：**能否设计一个单一算法，有效应对所有类型的决策场景？**
- 该论文首次尝试在统一算法框架下系统研究所有决策问题，具有开创性意义。

## 2. 方法论：核心思想、关键技术细节与算法流程
- 提出**统一镜像下降（Unified Mirror Descent, UMD）**算法。
- 核心思想：**协同整合多种基础策略更新规则**，在每次迭代中，将不同策略更新规则得到的基策略进行加权融合，形成智能体的新策略。
- 关键技术细节：
  - 使用镜像下降作为基础优化框架，结合多种更新规则（如策略梯度、自然策略梯度等）生成候选基策略。
  - 通过加权组合基策略来更新当前策略，权重反映不同更新规则对当前场景的适应性。
  - 由于最终策略的评估度量对基策略权重**不可微**，提出一种**简单有效的零阶方法（zero-order method）**来优化权重。
- 优点：集成新策略更新规则时**只需极小的修改**，具有良好的可扩展性。
- 算法流程（文字描述）：
  1. 初始化策略与权重；
  2. 对每个智能体，分别应用多种基础策略更新规则，得到若干基策略；
  3. 利用零阶优化方法评估各基策略在当前环境下的表现，更新权重；
  4. 将基策略按权重加权，得到新的联合策略；
  5. 重复迭代直至收敛。

## 3. 实验设计
- 使用了**24个基准环境（benchmark environments）**。
- 场景覆盖广泛：包括**单智能体、合作多智能体、竞争多智能体、混合合作-竞争**等决策类型。
- 对比方法：以各基础策略更新规则对应的专用算法作为基线（即 UMD 的基策略），比较 UMD 与这些基策略的性能。
- 评估指标：最终策略在相应环境中的表现（例如奖励、胜率等场景相关指标）。

## 4. 资源与算力
- 论文提供的内容中**未明确说明**所使用的 GPU 型号、数量、训练时长或具体算力资源。
- 因此无法从该材料中获取训练成本信息。

## 5. 实验数量与充分性
- 覆盖 24 个环境，数量较多，且横跨所有目标决策类型，具备一定的广度。
- 结果显示：在**超过 87%（21/24）的游戏**中，UMD 优于或与基策略持平。
- 但论文摘要未提供详细的消融实验或权重敏感性分析，也没有报告每个环境的实验重复次数、方差或统计显著性检验。
- 总体而言，实验设计覆盖了主要场景，但**充分性有限**，尤其是缺乏对零阶优化方法收敛性、权重解释性等内部机制的深入实验验证。

## 6. 主要结论与发现
- UMD 能够通过单一算法在多数决策场景中达到与专门算法相当或更优的性能，验证了统一决策算法的可行性。
- 该工作为未来开发通用决策算法提供了理论框架和实证基础，具有启发意义。
- 这是首次在单一算法框架下综合研究所有类型决策问题的尝试。

## 7. 优点
- **统一性**：首次尝试用同一算法覆盖单智能体、合作、竞争和混合决策，跨领域贡献明显。
- **可扩展性**：算法设计允许轻松集成新的策略更新规则，具有模块化优势。
- **权重优化方案**：针对不可微评估度量，提出简洁的零阶优化方法，降低了实现复杂度。
- **实验规模较大**：在24个不同性质的环境上进行验证，增强了结论的可信度。

## 8. 不足与局限
- 论文提供的文本信息有限，缺少对算法收敛性、理论保证形式的严格讨论。
- 实验细节不完整：未报告具体环境名称、各环境上的数值结果、训练成本与超参数设置。
- 评估指标单一，主要依赖总体性能水平，未充分分析不同场景下权重变化规律、鲁棒性和泛化性。
- 基策略的选择可能影响整体性能，但文中未深入探讨基策略集合的构建原则。
- 应用限制：零阶方法在高维策略空间可能效率较低，实际部署时需考虑计算开销。

（完）
