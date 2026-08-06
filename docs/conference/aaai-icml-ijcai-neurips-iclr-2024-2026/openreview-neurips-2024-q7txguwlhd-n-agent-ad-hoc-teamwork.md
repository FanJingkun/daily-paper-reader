---
title: N-agent Ad Hoc Teamwork
title_zh: N智能体临时团队协作
authors: "Caroline Wang, Arrasy Rahman, Ishan Durugkar, Elad Liebman, Peter Stone"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=q7TxGUWlhD"
tags: ["query:mcd"]
score: 8.0
evidence: 面向多自主体临时团队协作的多智能体协作学习方法
tldr: 现有协作MARL要么控制全部智能体，要么仅控制单个智能体，难以适应真实世界中与未知队友协作的场景。本文提出N智能体临时团队协作（NAHT）问题设定，算法控制部分智能体并需与其他自主智能体协作。以自动驾驶为例说明其现实意义，为合作学习方法拓展了更广泛的问题类别。该设定为后续研究提供了新的基准与方向。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
motivation: 真实协作场景中学习算法往往只控制部分智能体，现有全控制或单智能体设定难以覆盖此类情况。
method: 定义N智能体临时团队协作问题，允许算法控制多个智能体并与外部智能体进行协同以获得全局目标。
result: 该设定扩展了合作学习可解决的场景范围，并可能催生新的算法与基准。
conclusion: 通过放宽控制范围假设，NAHT使协作MARL更贴近真实世界应用，如自动驾驶与多机构合作。
---

## Abstract
Current approaches to learning cooperative multi-agent behaviors assume relatively restrictive settings. In standard fully cooperative multi-agent reinforcement learning, the learning algorithm controls *all* agents in the scenario, while in ad hoc teamwork, the learning algorithm usually assumes control over only a *single* agent in the scenario. However, many cooperative settings in the real world are much less restrictive. For example, in an autonomous driving scenario,  a company might train its cars with the same learning algorithm, yet once on the road, these cars must cooperate with cars from another company. Towards expanding the class of scenarios that cooperative learning methods may optimally address, we introduce $N$*-agent ad hoc teamwork* (NAHT), where a set of autonomous agents must interact and cooperate with dynamically varying numbers and types of teammates. This paper formalizes the problem, and proposes the *Policy Optimization with Agent Modelling* (POAM) algorithm. POAM is a policy gradient, multi-agent reinforcement learning approach to the NAHT problem, that enables adaptation to diverse teammate behaviors by learning representations of teammate behaviors. Empirical evaluation on tasks from the multi-agent particle environment and StarCraft II shows that POAM improves cooperative task returns compared to baseline approaches, and enables out-of-distribution generalization to unseen teammates.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- 现有合作性多智能体强化学习（MARL）的设定较为局限：标准全合作 MARL 中学习算法控制**所有**智能体；而临时团队协作（ad hoc teamwork）中通常只控制**单个**智能体。
- 现实世界中的很多协作场景往往介于两者之间。例如自动驾驶中，一家公司用同一算法训练自己的车队，但上路后必须与另一家公司的车辆协作。
- 为此论文提出 **N 智能体临时团队协作（N-agent Ad Hoc Teamwork, NAHT）**：一组自主智能体需要与数量和行为均动态变化的队友进行交互和合作。该设定扩展了合作学习方法能够解决的真实场景类别。

## 2. 方法论
- **核心思想**：将学习算法可以控制多个智能体、同时与未知的、动态变化的外部队友协作这一问题形式化为 NAHT，并在此基础上设计可在该设定下学习的算法。
- **算法：POAM（Policy Optimization with Agent Modelling）**
  - 属于策略梯度类的多智能体强化学习方法。
  - 通过学习队友行为的表征（representation of teammate behaviors）来适应多样化的队友策略。
  - 在训练过程中不断调整自身策略以与所观察到的队友行为模式协同，从而实现更高的团队回报。
- 论文正式化了 NAHT 问题，但摘要中未给出具体的公式、网络结构和训练流程细节。

## 3. 实验设计
- **实验场景 / Benchmark**：
  - Multi-agent Particle Environment (MPE)
  - StarCraft II（星际争霸 II）
- **任务**：上述环境中的多智能体合作任务。
- **对比方法**：与基线方法（baseline approaches）对比，摘要未列出具体基线名称。
- **评估内容**：
  - 合作任务的累计回报（cooperative task returns）
  - 对未见过的队友的分布外泛化能力（out-of-distribution generalization）
- 实验结果表明 POAM 在任务回报上优于基线，并具有良好的泛化能力。

## 4. 资源与算力
- 提供的摘要和元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 因此无法从现有资料总结训练成本或资源需求；如需了解需查看论文正文或附录。

## 5. 实验数量与充分性
- 摘要中仅提及在 MPE 和 StarCraft II 上进行实验，并与基线方法对比，以及进行分布外泛化测试。
- 未明确给出实验的具体次数、任务数量、消融实验设计以及统计显著性分析。
- 从现有信息看，实验覆盖了两个不同的 benchmark，能初步验证方法有效性和泛化性；但**充分性有限**，尤其缺乏对算法各组件（如队友建模方式）的消融分析，以及对不同队友数量、异构性、可观测性等维度的系统性评测。

## 6. 论文的主要结论与发现
- POAM 在 NAHT 问题设定下能够有效提升合作任务的回报，并能泛化到训练时未出现过的队友。
- NAHT 问题的提出放宽了“算法控制全部智能体”或“仅控制单个智能体”的假设，使协作 MARL 更贴近真实世界应用，如自动驾驶、多机构协同等。

## 7. 优点
- **问题设定新颖且实用**：NAHT 填补了全控制与单智能体控制之间的空白，具有更强的现实意义。
- **方法设计有针对性**：通过队友行为建模实现动态适应，为处理未知、变化队友提供了可学习的思路。
- **实验验证较全面**：在两类经典 multi-agent benchmark 上验证，并专门考虑了分布外泛化，增加了结论的可信度。
- **开放性问题**：该设定为后续研究提供了新基准与方向，可能催生更多 NAHT 相关算法。

## 8. 不足与局限
- **信息不足**：摘要中缺乏算法细节、公式、超参数和具体实验配置，难以全面复现或评估公平性。
- **实验覆盖有限**：仅两个环境中的特定任务，未验证在真实复杂场景（如真实自动驾驶、高维部分可观测环境）中的表现。
- **队友建模的假设**：POAM 依赖队友行为表征，在通信受限、高度非平稳或队友数量极大时，其有效性和可扩展性未知。
- **算力与复现信息缺失**：未报告训练资源，影响可复现性和实际部署参考。
- **潜在偏差**：仅与基线比较，缺少与更先进方法的对比；分布外泛化测试的范围和难度也未在摘要中说明。

（完）
