---
title: "From Solo to Symphony: Orchestrating Multi-Agent Collaboration with Single-Agent Demos"
title_zh: 从独奏到交响：利用单智能体演示编排多智能体协作
authors: "Xun Wang, Zhuoran Li, Lin Yanshan, Hai Zhong, Longbo Huang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=VIVDdYtes3"
tags: ["query:mcd"]
score: 8.0
evidence: 利用单智能体演示迁移到多智能体协作学习
tldr: 多智能体强化学习从头训练效率极低，且现有离线或迁移方法仍依赖昂贵的多智能体数据。本文提出SoCo框架，先用单智能体演示预训练共享策略，再适配到协作任务，从而利用更容易获取的单智能体经验。实验表明该方法显著降低多智能体数据需求，提升协作学习效率，为搜索救援、家居协作等场景提供可行方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多智能体从零训练效率低下，多智能体数据昂贵，单智能体演示更易获取。
method: 提出SoCo框架，先预训练共享单智能体策略，再迁移适配到协作多智能体学习。
result: 在多个协作任务上验证了利用单智能体演示可有效降低多智能体数据依赖并提升学习效率。
conclusion: 单智能体演示是多智能体协作学习的重要资源，SoCo为低数据成本协作训练提供了新思路。
---

## Abstract
Training a team of agents from scratch in multi-agent reinforcement learning (MARL) is highly inefficient, much like asking beginners to play a symphony together without first practicing solo. Existing methods, such as offline or transferable MARL, can ease this burden, but they still rely on costly multi-agent data, which often becomes the bottleneck. In contrast, solo experiences are far easier to obtain in many important scenarios, e.g., collaborative coding, household cooperation, and search-and-rescue. To unlock their potential, we propose Solo-to-Collaborative RL (SoCo), a framework that transfers solo knowledge into cooperative learning. SoCo first pretrains a shared solo policy from solo demonstrations, then adapts it for cooperation during multi-agent training through a policy fusion mechanism that combines an MoE-like gating selector and an action editor. Experiments across diverse cooperative tasks show that SoCo significantly boosts the training efficiency and performance of backbone algorithms. These results demonstrate that solo demonstrations provide a scalable and effective complement to multi-agent data, making cooperative learning more practical and broadly applicable.

---

## 论文详细总结（自动生成）

# 论文总结：From Solo to Symphony（从独奏到交响）

## 1. 核心问题与整体含义
- **研究背景**：多智能体强化学习（MARL）中，让一组智能体从零开始协作训练效率极低，如同让初学者不经过独奏练习直接合奏交响乐。现有的离线 MARL 或迁移学习方法虽能缓解训练负担，但仍依赖昂贵的多智能体数据，这成为实际应用的瓶颈。
- **研究动机**：在许多重要场景（如协作编程、家务协作、搜索救援）中，单智能体经验比多智能体数据更容易获得。如何利用这些“独奏”经验来促进多智能体“合奏”学习，是本文的核心问题。
- **整体含义**：本文提出 SoCo 框架，将单智能体演示知识迁移到多智能体协作训练中，以降低对多智能体数据的依赖，使协作学习更高效、更实用。

## 2. 提出的方法论
- **核心思想**：先让智能体通过单智能体演示学习基础技能（预训练），再在多智能体训练过程中逐步适配协作行为。这类似先练好独奏，再学习合奏配合。
- **框架名称**：Solo-to-Collaborative RL（SoCo）。
- **关键技术细节**：
  - **阶段一：单智能体预训练**。从单智能体演示数据中学习一个共享的 solo 策略（shared solo policy），作为初始知识。
  - **阶段二：多智能体适配**。在多智能体训练过程中，通过一种**策略融合机制**（policy fusion mechanism）将预训练的 solo 策略与在线学习策略结合。
  - **策略融合机制包含两个组件**：
    - **类 MoE 门控选择器（MoE-like gating selector）**：动态决定在某个状态下应更多依赖预训练知识还是在线探索知识。
    - **动作编辑器（action editor）**：对所选动作进行修正或调整，使其更适应协作环境。
- **算法流程（文字描述）**：
  1. 收集单智能体演示数据，预训练共享策略。
  2. 初始化多智能体训练，每个智能体同时拥有预训练策略和可学习策略。
  3. 在每个决策时刻，门控选择器根据局部观测或全局状态分配两者权重，动作编辑器对最终动作进行微调。
  4. 在线训练过程中，策略与门控/编辑器联合优化，逐步提升协作能力。

## 3. 实验设计
- **实验场景**：摘要中提及协作编码（collaborative coding）、家务协作（household cooperation）、搜索救援（search-and-rescue）等典型多智能体协作任务。
- **Benchmark**：具体环境名称未在元数据中提供。根据场景推测可能使用了类似 Multi-Agent Particle Environment、SMAC、Overcooked 或定制环境，但无法确认。
- **对比方法**：摘要未列出具体基线名称，仅说明 SoCo 能显著提升“骨干算法”（backbone algorithms）的性能。可能的对比对象包括：从零训练的 MARL 基线、离线 MARL 方法、迁移学习 MARL 方法等。
- **消融实验**：从方法设计看，推测会对“策略融合机制”中的门控选择器、动作编辑器进行消融，以验证各自贡献，但元数据中未明确。

## 4. 资源与算力
- **未明确说明**。论文元数据和摘要中未提及 GPU 型号、数量、训练时长等算力信息。因此无法评估训练所需计算资源，也无法判断方法在资源受限场景下的可行性。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提到“across diverse cooperative tasks”，即覆盖了不同协作任务，但具体任务数量、每组实验的重复次数、随机种子设置等均未给出。
- **充分性评估**：
  - 由于缺少实验细节（如环境配置、基线实现、超参数、统计显著性），无法严格判断实验的完备性和公平性。
  - 方法的核心创新点（预训练+策略融合）理论上需要消融和对比实验来支撑，但目前只能确认有初步验证，缺少公开的可复现细节。
  - 总体而言，摘要中的结果提示积极信号，但学术意义上的充分性需依赖全文数据。

## 6. 主要结论与发现
- SoCo 框架能够显著提升多智能体协作学习的训练效率和最终性能。
- 单智能体演示可作为多智能体数据的有效补充，降低协作训练的数据成本。
- 该框架为搜索救援、协作编程、家务协作等真实应用场景提供了可行的低数据成本训练方案。

## 7. 优点
- **新颖性**：首次系统性地将单智能体演示用于多智能体协作学习，利用易获取的数据源，切中 MARL 的数据瓶颈痛点。
- **实用性**：单智能体数据采集成本低，方法具有较高的落地潜力。
- **设计巧妙**：类 MoE 门控和动作编辑器的策略融合机制，能动态平衡预训练知识与在线探索，避免灾难性遗忘或过度依赖先验。
- **泛化潜力**：框架与具体骨干算法解耦，可适配不同 MARL 算法。

## 8. 不足与局限
- **信息透明度不足**：本次总结仅基于论文元数据和摘要，缺少全文实验细节，无法对方法实现和结果进行深入验证。
- **实验覆盖未知**：未明确 benchmark 名称、任务数量、对比基线和消融设置，难以判断结论的普适性。
- **迁移偏差风险**：单智能体策略可能包含仅适用于单智能体环境的动作模式，直接迁移到协作场景可能产生偏差，需要动作编辑器和门控进行复杂修正，其鲁棒性有待验证。
- **扩展性问题**：当智能体数量增加或环境异构性增强时，共享 solo 策略的适用性可能下降，论文未提供相关分析。
- **应用限制**：某些协作任务很难从单智能体演示中提炼有效知识（如需要强通信和严格分工的任务），SoCo 的适用范围需要进一步界定。

（完）
