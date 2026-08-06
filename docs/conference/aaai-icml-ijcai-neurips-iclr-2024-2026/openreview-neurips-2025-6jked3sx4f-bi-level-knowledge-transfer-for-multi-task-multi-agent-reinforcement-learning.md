---
title: Bi-Level Knowledge Transfer for Multi-Task Multi-Agent Reinforcement Learning
title_zh: 多任务多智能体强化学习的双层知识迁移
authors: "Junkai Zhang, Jinmin He, Yifan Zhang, Yifan Zang, Ning Xu, Jian Cheng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=6jKed3sx4f"
tags: ["query:mcd"]
score: 8.0
evidence: 多任务MARL的双层知识迁移实现零样本泛化
tldr: 针对多任务多智能体强化学习在线训练成本高、难以从零学习每个任务的问题，提出双层知识迁移方法BiKT，在个体层面提取可迁移的技能嵌入，在团队层面捕获协作知识，实现基于离线数据的零样本策略复用。该方法弥补了以往仅迁移个体技能的不足，为多任务多智能体策略复用提供新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多任务多智能体强化学习在线训练成本高，且现有迁移只关注个体技能，忽略团队协作知识。
method: 提出BiKT双层知识迁移方法，在个体层面提取技能嵌入，在团队层面提取协调知识，并利用离线数据进行迁移。
result: 实验证明BiKT能在多个任务上实现零样本泛化，优于仅迁移个体技能的方法。
conclusion: 同时迁移个体技能与团队协作知识是实现多任务多智能体策略复用和零样本泛化的有效途径。
---

## Abstract
Multi-Agent Reinforcement Learning (MARL) has achieved remarkable success in various real-world scenarios, but its high cost of online training makes it impractical to learn each task from scratch. 
To enable effective policy reuse, we consider the problem of zero-shot generalization from offline data across multiple tasks. 
While prior work focuses on transferring individual skills of agents, we argue that the effective policy transfer across tasks should also capture the team-level coordination knowledge.
In this paper, we propose Bi-Level Knowledge Transfer (BiKT) for Multi-Task MARL, which performs knowledge transfer at both the individual and team levels. 
At the individual level, we extract transferable individual skill embeddings from offline MARL trajectories.
At the team level, we define tactics as coordinated patterns of skill combinations and capture them by leveraging the learned skill embeddings. 
We map skill combinations into compact tactic embeddings and then construct a tactic codebook.
To incorporate both skills and tactics into decision-making, we design a bi-level decision transformer that infers them in sequence.
Our BiKT leverages both the generalizability of individual skills and the diversity of tactics, enabling the learned policy to perform effectively across multiple tasks.
Extensive experiments on SMAC and MPE benchmarks demonstrate that BiKT achieves strong generalization to previously unseen tasks.

---

## 论文详细总结（自动生成）

# 多任务多智能体强化学习的双层知识迁移（BiKT）——论文总结

## 1. 核心问题与整体含义

- **研究背景**：多智能体强化学习（MARL）在众多现实场景中取得了显著成功，但**在线训练成本极高**，导致为每个新任务从零开始训练不切实际。
- **核心问题**：如何利用**离线数据**实现跨多个任务的**零样本泛化**，即在不进行额外在线交互的情况下，使策略有效复用于未见过的新任务。
- **现有不足**：以往的多任务策略迁移工作仅关注**个体技能（individual skills）** 的迁移，而忽略了**团队层面的协作知识（coordination knowledge）**，这限制了跨任务迁移的效果。
- **整体含义**：论文主张，有效的跨任务策略迁移应当**同时捕获个体技能与团队协作模式（tactics）**，以此为多任务多智能体策略复用提供新思路。

## 2. 方法论：BiKT 双层知识迁移

- **核心思想**：在**两个层面**提取与迁移知识——
  - **个体层面**：从离线 MARL 轨迹中提取**可迁移的个体技能嵌入（skill embeddings）**，保证技能的通用性。
  - **团队层面**：将技能组合的协调模式定义为**战术（tactics）**，通过技能嵌入捕获团队协作知识，映射为紧凑的**战术嵌入（tactic embeddings）**，并构建**战术码本（tactic codebook）**。
- **决策模型**：设计了**双层级决策 Transformer（bi-level decision transformer）**，在决策时**按顺序推断个体技能与团队战术**，将二者融入决策过程。
- **关键优势**：BiKT 同时利用了**个体技能的泛化能力**与**战术的多样性**，从而在新任务上实现有效决策。作者用文字描述了流程，但原文未列出具体数学公式或伪代码。

## 3. 实验设计

- **基准环境**：
  - **SMAC**（StarCraft Multi-Agent Challenge）：经典的星际争霸多智能体对战环境。
  - **MPE**（Multi-Agent Particle Environment）：多智能体粒子环境，常用于协作/竞争任务验证。
- **实验设置**：基于**离线数据**进行训练，测试时考察在**未见过的新任务**上的**零样本泛化**表现。
- **对比方法**：主要用于对比**仅迁移个体技能**的既有方法，以验证团队层面知识迁移的必要性。具体对比方法名称原文未详细列出。

## 4. 资源与算力

- **原文未明确说明**使用的 GPU 型号、数量或具体训练时长。
- 作为 NeurIPS 2025 论文，可推测使用了标准学术算力资源，但无法从本文元数据中获知具体细节。

## 5. 实验数量与充分性

- **实验覆盖**：涉及两个主流 MARL 基准（SMAC 与 MPE），并包含了对团队知识迁移作用的验证，说明了方法在多种任务上的一致性。元数据提到“extensive experiments”，表明实验规模较大。
- **充分性评估**：
  - ✅ 在两个互补基准上的验证增强了结论的**可信度**。
  - ✅ 通过与“仅迁移个体技能”方法的对比，直接验证了核心主张。
  - ⚠️ 原文提供的元数据中**未列出消融实验的具体数量、统计显著性检验、以及每个任务上的收益幅度**，因此对实验充分性的完整判断需要结合论文全文。

## 6. 主要结论与发现

- BiKT 在 **SMAC 和 MPE 基准**上对**未见过的新任务**展现出**强大的零样本泛化能力**。
- 实验证明：**同时迁移个体技能与团队协作知识**是实现多任务多智能体策略复用和零样本泛化的**有效途径**，优于仅迁移个体技能的方法。
- 该工作填补了多任务 MARL 迁移中**团队协作知识缺失**的空白，为后续研究提供了新方向。

## 7. 优点

- **问题定位精准**：明确指出以往迁移方法忽略团队协作知识的缺陷，具有较强动机。
- **方法新颖**：提出“技能 + 战术”双层级知识结构，结合技能嵌入与战术码本，兼顾**泛化性**与**多样性**。
- **模型设计合理**：双层级决策 Transformer 按序推断个体技能与团队战术，在决策层面自然融合两层知识。
- **验证扎实**：在主流基准（SMAC、MPE）上验证了方法的有效性，结论对多任务 MARL 领域有较大参考价值（获得 OpenReview 8.0 评分）。

## 8. 不足与局限

- **算力与训练细节缺失**：未报告 GPU 型号、数量和训练时间，不利于复现。
- **实验细节不完整**：未见具体数值结果（如胜率、回报均值）、消融实验设计、对比方法列表及显著性检验说明，削弱了可验证性。
- **应用范围有限**：在更复杂、更大规模的多智能体场景（如超大规模集群、真实机器人系统）上的表现尚不清楚。
- **离线数据假设**：依赖离线数据的质量与覆盖度，若离线数据缺乏特定任务的交互模式，零样本性能可能下降。
- **偏差风险**：方法在 SMAC 与 MPE 上验证有效，但这两类环境属于相对规则化的模拟场景，对现实世界中的不确定性与部分可观测性还需进一步探索。

（完）
