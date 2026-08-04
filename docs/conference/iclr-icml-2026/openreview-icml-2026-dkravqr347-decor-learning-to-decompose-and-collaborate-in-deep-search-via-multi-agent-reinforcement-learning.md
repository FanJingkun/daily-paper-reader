---
title: "DECOR: Learning to Decompose and Collaborate in Deep Search via Multi-Agent Reinforcement Learning"
title_zh: DECOR：通过多智能体强化学习学习深度搜索中的分解与协作
authors: "Ruiqing Chen, Zekun Zhang, Gong-Duo Zhang, Lihong Gu, Lin Zhou"
date: 2026-04-30
pdf: "https://openreview.net/pdf/13a4f4f8b9c2fb1c20f65672134c7740690d311c.pdf"
tags: ["query:mcd"]
score: 6.0
evidence: 通过角色分解与多智能体强化学习协作，可迁移到协作决策任务
tldr: 本文针对深度搜索中单一模型认知负荷过重以及现有多智能体方法不能从协作失败中学习的问题，提出DECOR框架。该框架将深度搜索分解为规划者、过滤者和回答者三种角色，通过多智能体强化学习联合优化这些角色，并采用混合奖励策略协调整体目标。实验表明DECOR能有效提升检索与回答质量，证明可学习的角色分工优于冻结模型编排。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多智能体深度搜索依赖冻结模型，无法从协作失败中学习，且单一模型易认知过载。
method: 将深度搜索建模为MARL问题，用规划者、过滤者、回答者角色分解任务，并通过混合奖励进行联合优化。
result: 实验显示DECOR显著提升深度搜索的检索与最终回答质量，优于无训练的编排方法。
conclusion: 可学习的角色分解与协作是多智能体深度搜索的有效范式。
---

## Abstract
Monolithic agents in deep search often suffer from "cognitive overload," while existing multi-agent approaches mostly rely on frozen models that cannot learn from collaboration failures. To bridge this gap, we propose $\textbf{DECOR}$ ($\textbf{DE}$compose and $\textbf{CO}$llaborate via $\textbf{R}$ole-specialized agents),  a framework formulating deep search as a Multi-Agent Reinforcement Learning (MARL) problem. DECOR functionally decomposes the task into three specialized roles: a $\textit{Planner}$ to navigate, a $\textit{Filter}$ to curate a noise-reduced memory, and an $\textit{Answerer}$ for synthesis. Unlike training-free orchestration, we jointly optimize these agents using a hybrid reward strategy that harmonizes role-specific intrinsic feedback with team-level outcome signals. Experiments on seven benchmarks show that DECOR significantly outperforms strong monolithic baselines, demonstrating the necessity of learning-based functional decomposition in handling cognitive overload.

---

## 论文详细总结（自动生成）

根据您提供的论文元数据与摘要，以下是对论文《DECOR: Learning to Decompose and Collaborate in Deep Search via Multi-Agent Reinforcement Learning》的中文结构化总结。

---

## 一、论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：深度搜索（Deep Search）任务中，单一的“整体式（Monolithic）”智能体在同时承担导航、信息筛选和答案合成等多重认知任务时，容易发生“认知过载（Cognitive Overload）”，导致检索与回答质量下降。
- **现有方法的不足**：已有的多智能体深度搜索方法大多依赖**冻结的（frozen）预训练模型**进行编排，即只做推理、不做学习更新，因此**无法从协作失败中积累经验**，合作的鲁棒性和任务适应性有限。
- **研究意义**：论文提出通过**强化学习**让多个角色化智能体在协作中共同优化，将“不可学习的编排”升级为“可学习的协作”，从而解决认知过载问题，并验证**基于学习的角色分解**是否优于静态的、无训练的模型编排。

---

## 二、论文提出的方法论

- **总体框架：** 将深度搜索任务形式化为**多智能体强化学习（MARL）问题**，提出 **DECOR** 框架，全称为 **DE**compose and **CO**llaborate via **R**ole-specialized agents（通过角色特化智能体进行分解与协作）。
- **核心思想：** 将深度搜索这一复杂任务按功能解耦为三个专门角色，每个智能体只关注自己的子任务，降低单一模型的认知负荷，并通过联合训练学习协同策略。
- **关键技术细节：**
  - **角色分解（Decomposition）**：将任务拆分为三种角色：
    - **规划者（Planner）**：负责导航与搜索路径规划，决定下一步检索行为；
    - **过滤者（Filter）**：负责筛选和整理检索记忆，剔除噪声信息，维护精炼的上下文；
    - **回答者（Answerer）**：基于精炼记忆进行最终答案的合成与生成。
  - **多智能体强化学习（MARL）联合优化**：三个角色并非冻结模型，而是通过强化学习共同训练，学习在协作中动态适配彼此的行为，以最大化整体任务绩效。
  - **混合奖励策略（Hybrid Reward Strategy）**：结合两类奖励信号：
    - **角色内在反馈（Role-specific intrinsic feedback）**：衡量每个智能体在各自子任务（如检索相关性、过滤准确率）上的表现；
    - **团队级外部结果信号（Team-level outcome signals）**：衡量最终回答的全局质量。
  - 这种混合奖励设计旨在平衡个体分工优化与整体协作目标，避免各自为政带来的协作失调。
- **算法流程（文字描述）**：
  1. 输入查询后，规划者根据当前状态决定检索动作（如生成查询词、选择检索工具）；
  2. 检索结果进入过滤者的记忆更新环节，滤除冗余；
  3. 回答者结合过滤后的压缩记忆生成答案；
  4. 训练时，收集各角色奖励（内在+团队）并利用 MARL 算法进行梯度更新；
  5. 通过与环境（搜索环境）反复交互，逐步提升整体协同策略。

---

## 三、实验设计

- **基准（Benchmarks）**：论文在**七个基准（seven benchmarks）**上开展实验，覆盖深度搜索中的检索与回答质量评估。但受限于摘要，未逐一列出具体数据集名称。
- **对比方法**：
  - 以**强单调（monolithic）基线**为主要对照，即使用单一模型完成全部搜索与回答工作的传统方法；
  - 以及现有**无训练编排（training-free orchestration）**的多智能体基线。
- **评估维度**：重点评估检索质量（如导航与过滤效果）和最终答案生成质量。

---

## 四、资源与算力

- 论文摘要与元数据中**未明确提及**使用的 GPU 型号、数量、训练时长等算力信息。
- 因此，关于具体的硬件资源与训练成本，本文无法给出量化结论，需要参考论文正文中的实验设置部分。

---

## 五、实验数量与充分性

- **实验规模**：覆盖 7 个 benchmark，对比了单调基线与无训练编排方法，并进行了整体性能比较。
- **充分性与客观性分析**：
  - 从数量上看，7 个基准提供了**跨场景的多样性**，可初步支撑“DECOR 优于单调基线与冻结编排”的结论；
  - 但摘要中未披露每种基准上的**详细数值、方差或显著性检验**，无法完全判断实验的统计稳健性；
  - 也未明确是否进行**消融实验**（例如去掉混合奖励中的内在反馈、去掉过滤者等），因此对框架各组件的贡献归属尚不明确；
  - 总体而言，实验设计方向合理，但**报告透明度有限**，需要看全文以核实公平性。

---

## 六、论文的主要结论与发现

- DECOR 在七个基准上一致显著优于强单调基线和现有无训练多智能体编排方法，表明**可学习的角色分工与协作是缓解认知过载的有效途径**。
- 混合奖励策略能有效协调角色内在目标与团队整体输出目标，是实现协作优化的关键机制。
- 研究证实：**基于学习的角色分解（learned functional decomposition）**优于冻结模型的静态编排，是多智能体深度搜索的一种更有前景的范式。

---

## 七、优点

- **问题定位清晰**：直击单一模型认知过载和现有多智能体方法不可学习的痛点。
- **框架设计具有新意**：用 MARL 统一规划、过滤、回答三阶段，将深度搜索从“流水线编排”升级为“协作式学习系统”。
- **角色特化思想合理**：通过功能解耦减少每个模型的认知负担，具有较好的直觉性和可解释性。
- **奖励设计兼顾个体与全局**：混合奖励（内在+团队）在机制上能缓解多智能体信用分配（credit assignment）的部分困难。
- **结论具备泛化潜力**：元数据提示该方法可迁移至协作决策任务，说明框架的价值可能超越深度搜索本身。

---

## 八、不足与局限

- **信息受限**：由于当前只有摘要，实验细节（数据集名称、评价指标、数值结果）不足，难以独立复现或验证。
- **消融不足**：没有明确展示各角色的单独贡献和混合奖励的每个组件的影响，组件设计的最优性存疑。
- **算力与效率未报告**：缺少训练成本信息，而 MARL 方法通常面临训练开销大、样本效率低的问题，这是实际落地的重要考量。
- **可能存在的偏差风险**：
  - 7 个基准可能偏向特定任务类型（如问答型搜索），对开放式探索型搜索的覆盖未知；
  - 对比基线是否包含同等量级的 RL 训练模型、是否对齐训练成本等公平性问题在摘要中无法确认。
- **应用限制**：
  - 需要为每个角色维护独立的 RL 策略，模型规模与推理开销可能显著高于单一智能体；
  - 角色划分的合理性（固定三种角色是否最优）可能因任务而异，需要自适应或扩展能力；
  - 与真实搜索引擎和在线反馈环境的对接尚未在摘要中展开说明，离实际部署有一定距离。

---

（完）
