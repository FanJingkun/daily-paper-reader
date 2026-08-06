---
title: Decentralized and Disentangled Task–Role Representation Learning for Generalizable Offline Multi-Agent Meta Reinforcement Learning
title_zh: 去中心化解耦任务-角色表示学习用于可泛化的离线多智能体元强化学习
authors: "Lei Yuan, Ruiqi Xue, Yang Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1adc1ded503e9b969fca4dda5316059565716529.pdf"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 面向离线多智能体元RL学习去中心化解耦任务-角色表示，解决角色信息缺失问题
tldr: 离线元强化学习希望从多任务数据中学到统一策略并泛化到分布外任务，但在多智能体场景中，由于全局信息受限，去中心化任务识别困难，且缺少角色信息导致知识迁移低效。该文提出D2TR框架，基于上下文元RL学习高效的去中心化解耦任务-角色表示，使每个智能体能够在有限信息下识别任务与自身角色，从而提升离线多智能体元学习的泛化与迁移能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体离线元强化学习中，去中心化任务识别困难且缺乏角色信息导致迁移低效。
method: 提出D2TR框架，学习去中心化解耦的任务-角色表示以指导元策略适应。
result: 提升了OOD任务上的泛化能力与角色感知的知识迁移效率。
conclusion: 为多智能体元学习中的任务识别与角色发现提供了表示学习新思路。
---

## Abstract
Offline meta reinforcement learning (RL) enables agents to learn a unified policy from multi-task offline data to support generalization in out-of-distribution (OOD) tasks. 
Recent approaches in single-agent RL tackle this by learning an efficient task representation to distinguish between tasks, showing promising adaptation ability.
However, when extended to multi-agent settings, these methods struggle with decentralized task identification due to limited global information, and suffer from inefficient knowledge transfer in the absence of role information.
To address this, we propose D$^2$TR, a novel context-based meta RL framework with efficient decentralized and disentangled task-role identification.
Specifically, D$^2$TR first introduces mutual information knowledge distillation to align decentralized task representations with centralized task representations inferred from global trajectories, enabling efficient decentralized team-centric information identification. Next, D$^2$TR leverages a large language model to assign semantic roles to trajectories in offline data, and achieves effective individual-centric information inference by learning decentralized role representations.
Extensive experiments conducted on commonly used multi-agent environments, including CN, SMAC, and SMACv2, demonstrate that D$^2$TR exhibits strong generalization performance to unseen tasks, outperforming prior multi-agent multi-task and context-based meta RL baselines.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：离线元强化学习（offline meta RL）旨在从多任务离线数据中学习统一策略，并支持对分布外（OOD）任务的泛化。单智能体场景中，已有方法通过学习高效任务表示来区分任务，表现出较好的适应能力。
- **核心问题**：将该思路扩展到多智能体场景时面临两大挑战：
  1. **去中心化任务识别困难**：由于各智能体只能获取有限的局部/全局信息，难以像单智能体那样准确识别当前任务。
  2. **缺乏角色信息导致迁移低效**：多智能体系统中不同智能体承担不同角色，缺少角色感知的知识迁移会降低元学习的适应效率。
- **整体含义**：论文试图解决离线多智能体元强化学习中“如何在去中心化条件下同时识别任务与自身角色，从而提升 OOD 泛化能力”的关键问题。

## 2. 论文提出的方法论

- **框架名称**：D²TR（Decentralized and Disentangled Task–Role Representation Learning，去中心化解耦任务-角色表示学习）。
- **核心思想**：基于上下文元强化学习框架，学习高效的去中心化、解耦的任务表示与角色表示，使每个智能体在有限信息下既能识别团队层面的任务，也能识别个体层面的角色。
- **关键技术细节**：
  1. **去中心化任务表示学习**：通过互信息知识蒸馏（mutual information knowledge distillation），将基于全局轨迹推理出的集中式任务表示的信息，对齐到各智能体仅凭局部观测推断出的去中心化任务表示上，从而实现高效的“以团队为中心”的信息识别。
  2. **去中心化角色表示学习**：利用大型语言模型（LLM）为离线数据中的轨迹赋予语义角色标签，并据此学习去中心化的角色表示，支持“以个体为中心”的信息推断。
- **算法流程（文字说明）**：
  - 从多任务离线数据中采样轨迹；
  - 用全局轨迹编码器得到集中式任务表示；
  - 用每个智能体的局部轨迹编码器得到去中心化任务表示；
  - 通过互信息蒸馏使去中心化表示逼近集中式表示；
  - 用 LLM 为轨迹分配角色语义标签，训练角色表示编码器；
  - 将任务表示与角色表示作为上下文输入元策略，指导策略在目标任务上的适应与决策。

## 3. 实验设计

- **环境/Benchmark**：
  - CN（Cooperative Navigation，合作导航）
  - SMAC（StarCraft Multi-Agent Challenge）
  - SMACv2（SMAC 的升级/变体版本）
- **对比方法**：
  - 先前的多智能体多任务强化学习方法；
  - 基于上下文的多智能体元强化学习基线方法。
- **评估目标**：在未见过的（OOD）任务上的泛化性能，以及角色感知的知识迁移效率。

## 4. 资源与算力

- **论文内容中未明确说明**使用的 GPU 型号、数量、训练时长、显存等算力信息。
- 因此无法从给定文本中总结具体资源消耗，只能指出该信息缺失。

## 5. 实验数量与充分性

- **实验数量**：从摘要和元数据来看，论文在 3 个多智能体环境上进行了实验，并对比了多类基线方法。但给定文本未提供具体的实验数量、消融实验设置、重复次数、方差分析等细节。
- **充分性评价**：
  - 覆盖了常见的多智能体基准（SMAC 为对抗性场景，CN 为合作导航场景，SMACv2 为更具随机性的版本），具有一定代表性。
  - 但缺少消融实验细节（如去除互信息蒸馏或 LLM 角色标注后的性能变化）、超参数敏感性分析、以及统计显著性检验的描述。
  - 因此，在现有文本下只能说实验设计覆盖了关键场景和基线，但“是否充分、公平”需要结合全文实验章节进一步判断。

## 6. 论文的主要结论与发现

- D²TR 在多个常用多智能体环境中，对未见过的任务表现出强泛化能力。
- 相比先前的多智能体多任务方法和基于上下文的元 RL 基线，D²TR 具有显著性能优势。
- 证明了去中心化解耦的任务-角色表示学习能够有效解决离线多智能体元 RL 中任务识别困难与角色信息缺失的问题。

## 7. 优点

- **问题切入精准**：同时考虑“去中心化任务识别”和“角色信息缺失”这两个多智能体离线元 RL 中的关键痛点，具有较强实际意义。
- **方法设计新颖**：将互信息知识蒸馏用于对齐集中式与去中心化任务表示，并引入 LLM 自动生成角色语义标签，避免了手工定义角色的成本。
- **解耦表示**：任务表示与角色表示相互独立，便于分别优化和跨任务迁移。
- **实验环境多样**：在三个不同难度和类型的多智能体基准上验证，增强了结论的可信度。

## 8. 不足与局限

- **细节缺失**：当前提供的文本只有摘要和元数据，缺少方法的具体公式、算法伪代码、实现细节、超参数设置等，无法完整评估技术方案。
- **算力未报告**：未提及训练所需的计算资源，不利于可复现性评估和资源成本估计。
- **实验描述不完整**：未详细列出消融实验、基线具体配置、每个环境的任务数量、OOD 任务的构建方式等，实验充分性难以从摘要独立判断。
- **潜在偏差风险**：LLM 给轨迹分配的角色标签可能受语义标注质量影响，若标注存在错误或歧义，可能降低角色表示的有效性；且 LLM 引入的随机性和成本未被讨论。
- **应用限制**：方法依赖离线数据中可区分的角色模式，若任务的角色分布极度复杂或不断变化，角色表示可能难以泛化；此外，去中心化任务表示蒸馏需要集中式表示的监督信息，在完全无中心训练范式中可能受限。

（完）
