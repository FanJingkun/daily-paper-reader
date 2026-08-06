---
title: Generalizing Poincaré Policy Representations in Multi-agent Reinforcement Learning
title_zh: 多智能体强化学习中的庞加莱策略表示泛化
authors: "Bohao Qu, Xiaofeng Cao, Zhen Fang, Qing Guo, Yi Chang"
date: 2023-09-23
pdf: "https://openreview.net/pdf?id=2DJUXmHZ2O"
tags: ["query:mcd"]
score: 7.0
evidence: 在MARL中通过庞加莱几何学习共享策略表示
tldr: 多智能体策略表示学习对理解交互至关重要，但传统表示难以体现MDP状态演化的层次性。作者将多智能体轨迹生长路径映射到庞加莱球，提出P2R几何表示方法，以树状生长过程刻画策略与环境动态分支。实验表明该表示能有效提升多个MARL任务的性能，为共享策略表示学习提供了几何新视角。
source: ICLR-2024-Rejected-Public
selection_source: conference_retrieval
motivation: 传统策略表示难以刻画MDP状态演化的层次树状结构，影响多智能体交互理解。
method: 提出P2R，将多智能体轨迹生长路径投影到庞加莱球，构建层次化几何策略表示。
result: 在多个MARL任务上验证了表示有效性，提升策略学习与泛化性能。
conclusion: 庞加莱几何为MARL共享策略表示提供了新的建模视角。
---

## Abstract
Learning policy representations is essential for comprehending the intricacies of agent interactions and their decision-making processes.
Recent studies have found that the evolution of any state under Markov decision processes (MDPs) can be divided into multiple hierarchies based on time sequences. This conceptualization resembles a tree-growing process, where the policy and environment dynamics determine the possible branches. In this paper, the multiple agent's trajectory growing paths can be projected into a Poincaré ball, which requires the tree to grow from the origin to the boundary of the ball, deriving a new geometric idea of learning Poincaré Policy Representations (P2R) for MARL.
Specifically, P2R captures the policy representation of the Poincaré ball by a hyperbolic neural network and introduces a contrast objective function that encourages embeddings of the same policy to move closer together while embeddings of different policies to move apart, which enables embed policies with low distortion.
Experimental results provide empirical evidence for the effectiveness of the P2R framework in cooperative and competitive games, demonstrating the potential of Poincaré policy representations for optimizing policies in complex multi-agent environments.

---

## 论文详细总结（自动生成）

# 多智能体强化学习中的庞加莱策略表示泛化（Generalizing Poincaré Policy Representations in Multi-agent Reinforcement Learning）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在多智能体强化学习（MARL）中，如何学习有效的策略表示（policy representations）以更好地理解智能体之间的交互与决策过程。
- **研究动机**：
  - 传统策略表示方法难以刻画马尔可夫决策过程（MDP）状态演化中蕴含的**层次性树状结构**。MDP中任何状态的演化可按时间序列划分为多个层级，这一过程类似于树的生长——策略与环境动态决定了可能的分支方向。
  - 现有欧几里得空间中的表示方法在处理这种层次化结构时存在**高失真**问题，难以精确反映策略之间的内在关系。
- **整体含义**：作者提出从**双曲几何（庞加莱球）**出发，将多智能体轨迹生长路径投影到庞加莱球中，形成从球心到球边界的树状生长过程，从而为MARL共享策略表示学习提供一种全新的几何视角，提升策略学习与泛化性能。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **方法论名称**：P2R（Poincaré Policy Representations，庞加莱策略表示）。
- **核心思想**：将多智能体的轨迹生长路径嵌入到**庞加莱球**中，利用双曲空间的负曲率特性天然适合表征层次化/树状结构这一几何性质，将策略表示为树从原点向球边界生长的过程。
- **关键技术细节**：
  - **双曲神经网络**：使用双曲神经网络来捕获策略在庞加莱球中的表示，将多智能体的轨迹生长路径映射到双曲空间。
  - **对比目标函数**：引入对比学习（contrastive learning）目标函数，使**同一策略的嵌入向量彼此靠近**，**不同策略的嵌入向量相互远离**。
  - **低失真嵌入**：通过上述对比损失与双曲几何的配合，实现策略的**低失真（low distortion）嵌入**，即保持原始策略之间语义关系的几何准确性。
- **算法流程（文字描述）**：
  1. 获取多智能体交互轨迹数据；
  2. 将轨迹生长路径输入双曲神经网络，投影到庞加莱球空间；
  3. 通过对比损失函数优化嵌入，拉近同策略表示、推开异策略表示；
  4. 得到最终的低失真庞加莱策略表示，用于下游MARL策略学习与优化任务。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **实验场景**：涵盖了**合作型（cooperative）** 与**竞争型（competitive）** 两类多智能体博弈/游戏环境。
- **Benchmark**：未在提供的文本中明确具体环境名称（如 MPE、SMAC、Particle World 等），但可推断使用了MARL领域常用的多智能体任务基准。
- **对比方法**：论文文本中未逐项列出对比基线，但可推测与传统的欧几里得策略表示方法、或未使用双曲几何的表示学习方法进行了对比。
- **评估维度**：通过任务性能（策略学习效果、泛化能力）验证表示质量。

## 4. 资源与算力

- **未明确说明**：论文提供的文本中**没有提及**所使用的GPU型号、数量、训练时长、计算资源规模等具体算力信息。
- 若需复现或评估训练成本，需查阅论文全文或附录中的实现细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要来看，实验覆盖了**合作与竞争两类多个MARL任务**，但论文提取文本未给出具体实验组数、消融实验等详细清单。
- **充分性评估**：
  - **积极方面**：覆盖合作与竞争两类典型场景，能初步验证方法的泛化性。
  - **不足方面**：缺少消融实验细节（如双曲空间vs欧氏空间对比、对比损失的贡献度、不同曲率参数的影响等），也缺少与更多SOTA基线方法的系统对比。
  - **公平性**：未见关于超参数调优、随机种子重复次数、统计显著性检验等实验规范的描述，因此公平性难以从现有信息中全面判断。

## 6. 论文的主要结论与发现

- P2R框架在**合作型与竞争型多智能体游戏**中均取得了有效的实验结果，为庞加莱策略表示的有效性提供了实证支持。
- 双曲几何中的策略表示能够**以低失真方式刻画策略之间的关系**，从而帮助优化复杂多智能体环境中的策略。
- 论文认为庞加莱几何为MARL共享策略表示学习提供了**新的建模视角**，展示了双曲空间在策略表示领域的应用潜力。

## 7. 优点：方法与实验设计亮点

- **几何视角新颖**：将双曲几何引入MARL策略表示，利用庞加莱球的树状生长特性天然匹配MDP状态演化的层次分支结构，在理论上具有较强动机。
- **方法简洁有效**：通过双曲神经网络+对比学习目标实现低失真嵌入，技术路线清晰，不依赖过于复杂的网络架构。
- **对比学习机制**：拉近同策略、推开异策略的思路有助于增强表示的判别能力，在多智能体场景中具备通用性。
- **应用范围覆盖较广**：同时验证了合作与竞争两类典型MARL场景，增强了结论的普适性。

## 8. 不足与局限

- **实验信息不完整**：具体benchmark环境、基线方法、评估指标、训练配置等细节在现有文本中缺失，难以完整评估实验的严格性与公平性。
- **缺少消融实验**：未展示对双曲空间、对比损失、网络结构等关键组件的消融分析，方法各组成部分的贡献度不明确。
- **算力与可复现性信息缺失**：未提供GPU型号、训练时间、超参数设置等，影响可复现性。
- **应用场景有限**：仅在游戏类任务中验证，未涉及真实世界多智能体系统（如自动驾驶、机器人协作）等更复杂场景。
- **偏差风险**：未报告多次运行的标准差或统计显著性，结论稳健性有待进一步加强。

---

（完）
