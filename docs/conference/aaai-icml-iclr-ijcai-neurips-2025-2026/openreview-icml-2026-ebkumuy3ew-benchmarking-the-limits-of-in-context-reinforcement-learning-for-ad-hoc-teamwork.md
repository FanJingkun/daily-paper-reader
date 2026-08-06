---
title: Benchmarking the Limits of In-Context Reinforcement Learning for Ad-Hoc Teamwork
title_zh: 针对临时团队协作的上下文强化学习能力基准测评
authors: "Yuheng Jing, Kai Li, Jiajun Zhang, Zeyao Ma, Jiaxi Yang, Lei Zhang, Zhe Wu, Jinmin He, Junliang Xing, Jian Cheng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/07673d682b86b5258be9823a6cf2f578ef59653d.pdf"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 面向临时团队协作的大规模基准，包含多种强化学习与启发式队友，处理与未知伙伴的协调问题。
tldr: 为评测上下文强化学习在临时团队协作中的能力，构建大规模基准ICRL4AHT。基于JAX实现的高吞吐Overcooked-V2，包含覆盖强化学习与启发式策略的多样化队友集合，支持可控的训练-测试分布偏移，并提供从队友生成到在线评估的完整流水线。实验系统评估了多种历史条件上下文强化学习算法，揭示了其在未知队友协调中的能力边界与不足之处。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 上下文强化学习在临时团队协作中的效果尚未被系统评估，缺乏大规模基准。
method: 构建ICRL4AHT基准，基于JAX Overcooked-V2提供多样化队友与可复现的端到端评测流程。
result: 评测了多种历史条件上下文强化学习算法，揭示其在未知队友协调中的能力边界。
conclusion: 该基准为研究临时团队协作下的上下文强化学习提供了标准化测试平台。
---

## Abstract
In-Context Reinforcement Learning (ICRL) has enabled foundation agents to adapt instantaneously to novel tasks, yet its efficacy in Ad-Hoc Teamwork (AHT)—where coordination with unknown partners is required—remains unexplored. To rigorously evaluate this, we introduce a large-scale benchmark **ICRL4AHT**, built upon a high-throughput JAX implementation of Overcooked-V2. Our benchmark includes a large, diverse teammate suite spanning both RL and heuristic policies, enabling controlled train-test shifts, and provides a reproducible end-to-end pipeline for teammate generation, learning-history collection, dataset construction, and online multi-episode evaluation. We evaluate representative history-conditioned ICRL algorithms, including Algorithm Distillation (AD) and Decision-Pretrained Transformer (DPT), across millions of transitions. Results reveal notable limitations: contrary to their success in single-agent domains, these baselines fail to exhibit robust test-time adaptation in multi-agent settings. Specifically, these methods frequently underperform random baselines across both unseen teammate and unseen layout tracks, with no clear in-context improvement over long horizons. These findings highlight the challenges of strategic inference under partial observability within the OvercookedV2 AHT protocol, establishing our benchmark as a critical testbed for next-generation coordination algorithms.

---

## 论文详细总结（自动生成）

# 论文总结：《针对临时团队协作的上下文强化学习能力基准测评》

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：上下文强化学习（In-Context Reinforcement Learning, ICRL）使基础智能体能够即时适应新任务，在单智能体领域已展示出显著能力。然而，其在**临时团队协作（Ad-Hoc Teamwork, AHT）** 中的效力——即需要与未知伙伴实时协调的场景——此前缺乏系统研究。
- **核心问题**：现有 ICRL 方法在单智能体任务中的成功，能否迁移到多智能体协作环境？这些方法在面对从未见过的队友时，是否具备真正的在线适应能力？
- **研究意义**：AHT 中与未知伙伴的协调是现实世界智能体部署的关键挑战（如人机协作、异构系统协同），系统评估 ICRL 在该场景下的能力边界，对下一代协调算法的发展具有重要的指导意义。

## 2. 方法论

- **总体思路**：构建一个大规模、标准化、可复现的基准平台 **ICRL4AHT**，系统评估历史条件型 ICRL 算法在 AHT 环境下的表现，揭示其能力边界与不足。
- **核心组件**：
  - **环境实现**：基于高吞吐量（high-throughput）的 **JAX 重写版 Overcooked-V2**，支持大规模并行环境模拟，大幅提升了数据采集和训练效率。
  - **多样化队友套件**：涵盖**强化学习策略**与**启发式策略**的广泛队友集合，使智能体在训练和测试阶段能与多种行为模式的伙伴交互。
  - **受控训练-测试分布偏移**：支持系统性地构建训练与测试阶段队友行为的分布差异，从而测量算法的泛化与适应能力。
  - **端到端流水线**：提供从队友生成、学习历史收集、数据集构建到在线多回合评估的完整、可复现流程。
- **评估算法**：评测了两类代表性历史条件型 ICRL 算法：
  - **Algorithm Distillation (AD)**：将一系列强化学习训练轨迹作为上下文，通过监督学习蒸馏出可即时改进的策略。
  - **Decision-Pretrained Transformer (DPT)**：基于决策预训练的 Transformer，从历史交互序列中提取信息以指导当前决策。
- **训练规模**：算法在**数百万次转移（millions of transitions）** 上进行训练与评估。

## 3. 实验设计

- **场景基准**：以 Overcooked-V2 为任务环境，构建 AHT 协议下的协作场景（合作烹饪任务），要求智能体与未知队友在同一厨房空间中实时协调。
- **评估维度**：
  - **未见队友（unseen teammate）**：测试时队友是训练中未出现的策略。
  - **未见布局（unseen layout）**：测试时厨房布局是训练中未见的。
- **对比基线**：
  - **随机基线（random baseline）**：作为下界参照。
  - 两种 ICRL 算法（AD 与 DPT）。
  - 在不同队友策略和布局条件下进行系统对照。
- **评估方式**：采用多回合在线评估（online multi-episode evaluation），检验算法在测试阶段随回合增长是否展现出上下文改进（in-context improvement）。

## 4. 资源与算力

- 原文**未明确说明**具体使用的 GPU 型号、数量或训练时长。
- 仅提及基于 **JAX 的高吞吐实现**处理了**数百万次转移**的训练数据，暗示其具备大规模并行计算能力，但具体的计算资源配置在文中未披露。

## 5. 实验数量与充分性

- **实验覆盖**：评估涵盖了两种主要 ICRL 算法、多种 RL 与启发式队友策略、以及“未见队友”和“未见布局”两条主要测试轨道，实验场景设置较为系统。
- **充分性评估**：
  - **优点**：在统一的基准协议下进行了大规模、端到端的评估，结果具备较高的可信度与可复现性。
  - **不足**：ICRL 算法种类有限（仅 AD 和 DPT），缺少更多近期 ICRL 变体的对比；未提供针对模型规模、上下文长度等关键因素的系统消融实验，使得结论的精细程度受限。
  - **总体判断**：作为基准建立工作，实验覆盖足以支撑“现有 ICRL 方法在 AHT 中表现不佳”的结论，但作为方法论层面的深入分析，仍有进一步扩展的空间。

## 6. 主要结论与发现

- **核心发现**：现有 ICRL 算法在 AHT 环境下**未能表现出鲁棒的测试时适应能力**。
  - AD 和 DPT 在未见队友与未见布局两条测试轨道上**频繁不如随机基线**。
  - 在长交互视野上**未观察到清晰的上下文改进**（即随交互回合增加，表现没有持续提升）。
- **深层含义**：与单智能体领域的成功形成鲜明对比，多智能体场景下部分可观测性（partial observability）带来的**战略推断（strategic inference）挑战**是阻碍 ICRL 适应的关键因素——智能体不仅需要理解任务，还需要实时推断并响应未知队友的行为模式。
- **基准价值**：ICRL4AHT 为下一代协调算法研究提供了一个关键测试平台（critical testbed），有助于推动面向 AHT 场景的 ICRL 方法创新。

## 7. 优点

- **填补空白**：首次系统评估 ICRL 在 AHT 中的能力，填补了该交叉领域的研究空白。
- **大规模与多样化**：提供涵盖 RL 与启发式策略的广泛队友套件，且基于 JAX 的高吞吐实现支持大规模训练。
- **受控分布偏移**：支持系统性的训练-测试偏移设置，有助于精确诊断泛化与适应能力。
- **端到端可复现**：完整流水线设计（队友生成→历史收集→数据集构建→在线评估）保证了实验的可复现性与标准化。
- **负结果价值**：明确揭示现有方法的失败模式，为领域前进提供了清晰的问题导向。

## 8. 不足与局限

- **算法覆盖有限**：仅评估 AD 和 DPT 两种历史条件型 ICRL 算法，未能覆盖更多元的 ICRL 变体或结合记忆、显式建模等机制的方法。
- **环境单一性**：所有实验基于 Overcooked-V2 单一环境，其结论向其他 AHT 场景（如通信受限、非对称信息、更复杂任务）的推广性有待验证。
- **队友多样性边界**：虽涵盖 RL 与启发式策略，但未知队友的类型空间仍有限，可能低估或高估真实世界队友行为的多样性。
- **缺少机理分析**：对失败原因的探索停留在现象层面（“不如随机基线”），未深入分析是上下文利用失效、策略推断失败，还是长期信用分配问题所致。
- **计算资源不透明**：未披露训练具体算力消耗，不利于其他研究者在同等条件下复现或对比。
- **偏见风险**：基于负结果给出的结论较鲜明，但在算法种类不足、消融不充分的情况下，存在“当前 ICRL 方法不够好，而非 ICRL 范式本身失效”的偏差风险。

（完）
