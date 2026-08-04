---
title: Heterogeneous Agent Q-weighted Policy Optimization
title_zh: 异构智能体Q加权策略优化
authors: "Bor-Jiun Lin, Chun-Yi Lee"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=kPrvXjUZPG"
tags: ["query:mcd"]
score: 8.0
evidence: 面向异构智能体的Q加权策略优化方法，属于多智能体强化学习核心方法
tldr: 针对多智能体强化学习中稳定性和表达力难以兼得的问题，提出异构智能体Q加权策略优化（HAQO）框架，统一顺序优势更新、Q加权变分替代和熵正则化。该框架在理论上保证单调改进，并能够建模多模态异构协调策略。实验表明HAQO在多种MARL基准上优于现有方法，为异构多智能体协同提供了有保证的优化框架。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多智能体方法在稳定性和表达能力之间取舍，难以同时保证不偏离学习和多模态策略建模。
method: 提出HAQO，结合顺序优势更新、Q加权变分替代与熵正则化，统一优化框架并给出单调改进保证。
result: 在异构多智能体任务上，HAQO较现有方法取得更好性能，并保持训练稳定性。
conclusion: HAQO有效平衡MARL中的稳定性和表达力，适用于异构协同决策问题。
---

## Abstract
Multi-agent reinforcement learning (MARL) confronts a fundamental tension between stability and expressiveness. Stability requires avoiding divergence under non-stationary updates, while expressiveness demands capturing multimodal strategies for heterogeneous coordination. Existing methods sacrifice one for the other: value-decomposition and trust-region approaches ensure stability but assume restrictive unimodal policies, while expressive generative models lack optimization guarantees. To address this challenge, we introduce **H**eterogeneous **A**gent **Q**-weighted Policy **O**ptimization (**HAQO**), a framework unifying sequential advantage-aware updates, Q-weighted variational surrogates, and entropy regularization. Our analysis establishes monotone improvement guarantees under bounded critic bias, extending trust-region theory to diffusion-based policies with intractable log-likelihoods. HAQO achieves superior returns and reduced variance compared to policy-gradient baselines across diverse benchmarks. The ablation studies confirm sequential updates ensure stability, expressive policies enable multimodality, and entropy regularization prevents collapse. HAQO reconciles stability and expressiveness in MARL with theoretical rigor and practical effectiveness.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

多智能体强化学习（MARL）面临一个根本性矛盾：**稳定性**与**表达力**难以兼得。

- **稳定性**要求算法在非平稳更新下避免发散，常见做法是价值分解（value-decomposition）或信任域（trust-region）方法，但这类方法通常假设单模态（unimodal）策略，表达力受限。
- **表达力**要求策略能够捕获异构智能体协调中的多模态行为，生成式模型（如扩散模型）虽能建模复杂策略，但缺乏优化保证，训练易不稳定。

现有方法往往牺牲一方来满足另一方，缺少一个既能提供理论保证、又能建模多模态异构策略的统一框架。论文正是针对这一缺口提出解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

论文提出 **HAQO（Heterogeneous Agent Q-weighted Policy Optimization，异构智能体Q加权策略优化）** 框架，统一了三个关键组件：

- **顺序优势感知更新（sequential advantage-aware updates）**：让异构智能体依次更新策略，利用优势函数引导更新方向，增强稳定性。
- **Q加权变分替代（Q-weighted variational surrogates）**：在难以直接计算扩散策略对数似然的情况下，构造可优化的Q加权替代目标，将信任域理论扩展到基于扩散的策略。
- **熵正则化（entropy regularization）**：防止策略坍缩，保持多模态表达能力。

**理论保证**：论文证明在评论家（critic）偏差有界的情况下，HAQO 能保证策略的单调改进，这扩展了信任域策略优化理论，使其适用于对数似然不可解的扩散策略。

**算法流程（文字说明）**：
1. 初始化异构智能体策略与评论家网络；
2. 在每个迭代中，按一定顺序逐个更新智能体的策略，更新时使用Q加权变分替代目标，并加入熵正则项；
3. 使用收集的经验数据更新评论家；
4. 重复迭代直至收敛，保证每一轮更新都不会导致性能下降。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **场景/数据集**：论文未详细列出具体环境名称，但提到使用了多种 MARL 基准（diverse benchmarks），重点覆盖异构多智能体任务。
- **Benchmark**：标准的多智能体强化学习基准环境（具体环境未在摘要中说明，需要查看论文正文）。
- **对比方法**：主要对比了策略梯度类基线（policy-gradient baselines），包括现有基于价值分解、信任域以及生成式策略的方法。HAQO 在回报（returns）和方差（variance）上均优于这些基线。

## 4. 资源与算力

- 论文摘要和提供的元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- 可推测算法涉及扩散策略与多智能体训练，计算成本会高于简单策略梯度方法，但具体资源消耗需查阅论文正文或附录。

## 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平

- 摘要中提到的实验包括：
  - 在多种基准上与策略梯度基线进行性能对比；
  - 消融实验（ablation studies），验证三个组件的必要性：
    - 顺序更新保证稳定性；
    - 表达性策略支持多模态；
    - 熵正则化防止策略坍缩。
- **数量与充分性**：摘要未给出具体实验数量，但提及“diverse benchmarks”和明确的消融研究，说明实验设计比较全面。不过，由于缺乏具体环境和基线细节，无法完全判断实验的广泛性与公平性。若正文包含多个任务、多种随机种子和与其他最新方法的对比，可认为充分。

## 6. 论文的主要结论与发现

- HAQO 在多种异构多智能体基准上取得了**更高的回报**和**更低的方差**，优于策略梯度基线。
- 消融实验证实：
  - 顺序优势更新是稳定性的关键；
  - 表达性策略能有效建模多模态行为；
  - 熵正则化避免策略坍缩。
- 论文实现了 MARL 中**稳定性与表达力的调和**，提供了理论严谨且实践有效的优化框架。

## 7. 优点：方法或实验设计上有哪些亮点

- **理论贡献**：将信任域理论扩展到对数似然不可解的扩散策略，并给出单调改进保证，在生成式策略 MARL 中较为罕见。
- **统一框架**：将顺序更新、Q加权替代和熵正则化整合为单一目标，兼顾稳定性与表达力。
- **针对性强**：解决了现有方法在稳定性和表达力之间的取舍问题，直接回应了 MARL 中的核心矛盾。
- **消融验证**：通过消融实验清晰展示各组件的作用，增强了结论的可信度。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验细节不足**：摘要中未列出具体环境、任务难度、智能体数量、基线版本等信息，难以评估实验的全面性和公平性。
- **算力资源未披露**：没有讨论训练成本，不利于读者评估方法的实用性和可复现性。
- **理论假设的约束**：单调改进保证依赖于“评论家偏差有界”的假设，实际中该条件可能难以严格满足。
- **扩散策略的推断速度**：这类方法通常比简单高斯策略更耗计算资源，论文未提及推理效率问题。
- **应用范围**：方法针对异构智能体，但异构程度如何界定、是否适用于大规模智能体场景，摘要未给出说明。
- **可能存在的偏差风险**：对比基线是否包含最新的强 baseline，如 MAPPO、QMIX 以外的先进方法，需要查看正文确认，否则结论可能有选择性偏差。

（完）
