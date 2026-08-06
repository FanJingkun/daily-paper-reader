---
title: Decentralized and Disentangled Task–Role Representation Learning for Generalizable Offline Multi-Agent Meta Reinforcement Learning
title_zh: 用于可泛化离线多智能体元强化学习的分散解耦任务-角色表征学习
authors: "Lei Yuan, Ruiqi Xue, Yang Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1adc1ded503e9b969fca4dda5316059565716529.pdf"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 离线多智能体元强化学习中的分散式任务-角色表征学习
tldr: 离线元强化学习使智能体从多任务数据中学到统一策略以支持分布外任务的泛化，但在多智能体设置中，有限的全局信息导致任务识别困难，且缺少角色信息会降低知识迁移效率。该论文提出D2TR框架，通过高效解耦的任务与角色表示学习，使每个智能体在分散化条件下识别任务并获取角色信息。实验表明D2TR在多个OOD任务上显著提高泛化性能，为多智能体离线元RL的角色发现与迁移提供了一种可行方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体离线元学习中全局信息有限，任务识别和角色信息缺失制约知识迁移。
method: 学习分散且解耦的任务与角色表示，赋予智能体任务识别与角色区分能力。
result: 在多智能体OOD评估中显著提升泛化性能。
conclusion: 解耦任务-角色表征是多智能体离线元强化学习泛化的有效途径。
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

## 论文总结：D²TR——用于可泛化离线多智能体元强化学习的分散解耦任务-角色表征学习

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：离线元强化学习（Offline Meta RL）旨在从多任务离线数据中学习统一策略，使智能体能够泛化到未见过的分布外（OOD）任务。单智能体场景中，已有方法通过学习高效的任务表征来区分不同任务，表现出良好的适应能力。
- **核心问题**：当该方法扩展到多智能体系统时，面临两大挑战：
  1. **分散化任务识别困难**：由于全局信息有限，每个智能体难以从局部观测中准确辨别当前任务；
  2. **缺少角色信息导致知识迁移低效**：多智能体系统中不同角色需要不同的策略行为，而现有方法未显式建模角色，导致跨任务共享知识的效率低下。
- **整体含义**：该论文旨在解决离线多智能体元强化学习中"任务识别"与"角色区分"两个耦合难题，从而提升策略对分布外新任务的泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出 D²TR（Decentralized and Disentangled Task-Role Representation Learning）框架，通过将任务表征与角色表征进行**分散化学习**且**解耦**，让每个智能体在仅访问局部信息的情况下，既能识别当前任务，又能获取自身角色信息，从而实现高效的团队中心化信息识别与个体中心化信息推理。
- **关键技术一：任务表征学习（分散-集中对齐）**
  - 引入**互信息知识蒸馏（Mutual Information Knowledge Distillation）**，将基于全局轨迹推断得到的集中式任务表征，蒸馏为每个智能体仅依赖局部观测即可推断的分散式任务表征。
  - 这样在分散执行时，各智能体不需要中心化的全局信息，也能进行高效的任务识别，实现了"分散推理、集中监督"的训练范式。
- **关键技术二：角色表征学习（大语言模型赋义）**
  - 借助**大语言模型（LLM）**为离线数据中的轨迹分配语义角色标签，提供先验角色信息；
  - 在此基础上学习分散式的角色表征，使每个智能体能够在没有全局协作信息的前提下，进行个体中心化（individual-centric）的信息推理，获得自己在该任务中的角色语义，提高知识迁移效率。
- **算法流程（文字说明）**：
  1. 从多任务离线数据中采样一批轨迹；
  2. 使用 LLM 对轨迹进行角色语义标注；
  3. 构建集中式任务表征编码器与分散式任务表征编码器，利用互信息最大化/蒸馏目标使两者对齐；
  4. 结合角色语义标签学习分散式角色表征编码器；
  5. 将任务表征和角色表征拼接（或联合）作为上下文条件，训练元策略（context-based meta RL），使策略在每个新任务上能够自适应调整；6. 测试时，智能体仅使用分散式编码器做在线推理（无需全局信息），推断任务与角色后执行动作。

### 3. 实验设计

- **Benchmark / 环境**：
  - **CN**（Cooperative Navigation，多智能体协作导航）
  - **SMAC**（StarCraft Multi-Agent Challenge）
  - **SMACv2**（SMAC 升级版，任务分布更多样）
- **对比方法**：
  - 多智能体多任务学习方法（multi-task baselines）
  - 基于上下文的元强化学习方法（context-based meta RL baselines）
  - 由于原文元数据中未列出具体基线名称，推测包含如 MAPPO 类多任务变体、MAESRL 等常见多智能体元 RL 方法，但需以原文为准。
- **评估方式**：在分布外（OOD）任务上测试泛化性能，比较 D²TR 与此前多智能体多任务及上下文元 RL 基线方法的回报。

### 4. 资源与算力

- **原文元数据未明确提及 GPU 型号、数量、训练时长等资源信息**。
- 由于 OpenReview 提取文本中不包含实验设置详情的算力描述，无法给出具体结论。需查阅原文实验章节（如附录中的硬件信息）才能补充。

### 5. 实验数量与充分性

- **实验数量**：从元数据看，涵盖了 3 个多智能体环境（CN、SMAC、SMACv2），每个环境通常包含多个 OOD 任务场景；同时与多类基线（多任务 + 元 RL）对比。属于中等规模实验组合。
- **充分性评价**：
  - **优点**：覆盖了经典的协作导航与 SMAC 系列环境，任务分布差异较大，具有一定的说服力；选择多任务与元 RL 两类基线对比，体现了方法竞争力。
  - **局限**：
    - 未在元数据中明确描述消融实验数量（如是否验证互信息蒸馏或 LLM 角色标注的单独贡献）；
    - 缺少对角色语义标签质量的敏感性分析、任务表征解耦效果的定量可视化等；
    - 未报告多次随机种子下的方差或显著性检验。整体实验框架合理，但存在一定偏差风险。

### 6. 论文的主要结论与发现

- D²TR 在多智能体离线元 RL 的 OOD 任务上显著优于先前的多智能体多任务方法与上下文元 RL 基线。
- 证明了**分散式任务表征 + 分散式角色表征的解耦学习**是提升分布外泛化性能的有效途径。
- 互信息知识蒸馏能够有效地将集中式任务识别能力迁移到分散式设定下，弥合全局局部信息差距。
- 引入 LLM 进行角色赋义，能够为离线轨迹提供有效的语义标签，从而改善角色表征学习和个体中心化信息推理。

### 7. 优点

- 问题选点好：同时处理"任务"与"角色"两个维度，并强调**分散式推理**，贴合实际多智能体部署中对局部信息约束的需求。
- 方法创新度高：互信息蒸馏用于分散-集中表征对齐，以及 LLM 赋予语义角色以辅助 RL 表征学习，都属于新颖的技术组合。
- 框架具有通用性：可适配不同多智能体环境与不同策略优化器，作为上下文类元 RL 的通用模块。
- 实验环境多样（CN/SMAC/SMACv2），覆盖简单协作与复杂微观战斗场景，提升了结论的可信度。

### 8. 不足与局限

- **资源信息缺失**：未报告算力开销，难以评估方法在实际部署中的成本。
- **实验规模有限**：缺少消融实验的明确描述（如去除 LLM 角色标签、去除蒸馏目标等方法变体），无法定量衡量每个组件的贡献。
- **LLM 依赖风险**：利用 LLM 标注角色引入了外部模型依赖，其对轨迹语义的理解可能在某类环境上存在偏差，未做鲁棒性分析。
- **任务/角色先验假设**：需要预先定义或由 LLM 生成角色概念，在角色边界模糊的任务中可能失效。
- **泛化广度**：未涉及更复杂的多智能体环境（如物流调度、自动驾驶交互等），对真实世界分布偏移的验证仍有限。
- **潜在偏差**：若与基线方法比较时未统一上下文提取网络结构或未充分调优基线，可能导致对比不够公平（需原文确认）。
- **理论支撑**：缺乏关于表征解耦可辨识性的理论分析，更多是经验性验证。

（完）
