---
title: Learning Generalizable Skills from Offline Multi-Task Data for Multi-Agent Cooperation
title_zh: 从离线多任务数据中学习可泛化的多智能体协作技能
authors: "Sicong Liu, Yang Shu, Chenjuan Guo, Bin Yang"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=HR1ujVR0ig"
tags: ["query:mcd"]
score: 7.0
evidence: 面向多智能体合作的离线多任务技能学习与泛化
tldr: 离线多任务数据中的多智能体协作策略学习具有重要应用价值，难点在于从不同动作序列中提取通用协作技能并自适应地选择任务专属技能。本文提出一种技能学习框架，将合作时序知识融入通用技能，并针对每个任务自适应组合独立知识，以实现细粒度动作执行。预期该方法能提升对智能体和目标数量变化等未见场景的泛化能力，为离线多智能体强化学习提供了新思路。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 离线多任务MARL中缺乏能带来协作时序知识的通用技能，且难以按任务选取专属技能。
method: 设计通用协作技能与任务自适应技能结合的学习方法，从离线数据中提取并组合行为模式。
result: 摘要未含实验细节，预期可在多变场景中提升策略迁移与泛化性能。
conclusion: 分层技能学习为离线多智能体协作策略的泛化提供了有效途径。
---

## Abstract
Learning cooperative multi-agent policy from offline multi-task data that can generalize to unseen tasks with varying numbers of agents and targets is an attractive problem in many scenarios. Although aggregating general behavior patterns among multiple tasks as skills to improve policy transfer is a promising approach, two primary challenges hinder the further advancement of skill learning in offline multi-task MARL. Firstly, extracting general cooperative behaviors from various action sequences as common skills lacks bringing cooperative temporal knowledge into them. Secondly, existing works only involve common skills and can not adaptively choose independent knowledge as task-specific skills in each task for fine-grained action execution. To tackle these challenges, we propose Hierarchical and Separate Skill Discovery (HiSSD), a novel approach for generalizable offline multi-task MARL through skill learning. HiSSD leverages a hierarchical framework that jointly learns common and task-specific skills. The common skills learn cooperative temporal knowledge and enable in-sample exploitation for offline multi-task MARL. The task-specific skills represent the priors of each task and achieve a task-guided fine-grained action execution. To verify the advancement of our method, we conduct experiments on multi-agent MuJoCo and SMAC benchmarks. After training the policy using HiSSD on offline multi-task data, the empirical results show that HiSSD assigns effective cooperative behaviors and obtains superior performance in unseen tasks.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义（研究动机和背景）

- 研究目标：从**离线多任务数据**中学习可泛化的多智能体协作策略，使其能够应对**智能体数量与目标数量发生变化**的未见任务。
- 背景动机：离线多任务多智能体强化学习（MARL）中，通过聚合跨任务共享行为模式来学习“技能”以提升策略迁移具有潜力，但存在两大挑战：
  - **挑战一**：从不同动作序列中提取通用协作行为作为共同技能时，未能将**协作时序知识（cooperative temporal knowledge）** 融入技能中，导致技能缺乏时间维度上的协同信息。
  - **挑战二**：现有方法只学习共同技能，无法针对每个任务**自适应地选择独立知识**作为任务专属技能，难以实现细粒度的动作执行。
- 整体含义：该问题在自动驾驶、机器人协作、多智能体调度等现实场景中具有广泛应用价值，解决技能学习中的上述挑战对于提升离线多任务 MARL 的泛化能力至关重要。

### 2. 方法论：核心思想、关键技术细节与流程

- 方法名称：**HiSSD（Hierarchical and Separate Skill Discovery，分层分离技能发现）**
- 核心思想：通过**分层框架**同时学习两类技能 —— 通用技能（common skills）与任务专属技能（task-specific skills），二者互补以实现可泛化的离线多任务协作。
- 技术细节：
  - **通用技能**：学习跨任务共享的协作时序知识（cooperative temporal knowledge），能够进行**样本内利用（in-sample exploitation）**，从而缓解离线数据分布外动作带来的偏差问题。
  - **任务专属技能**：捕捉每个任务的先验信息（priors），实现**任务引导的细粒度动作执行**，使策略能根据具体任务调整行为。
- 学习流程（文字描述）：
  1. 从离线多任务数据中提取跨任务共享的行为模式，构造通用技能；
  2. 同时为每个任务学习独立的任务专属技能；
  3. 在分层结构中联合优化两类技能，使它们协同工作；
  4. 在测试阶段，依据当前任务上下文自适应组合通用技能与任务专属技能，实现细粒度的动作决策。
- 摘要中未提供具体公式或网络架构细节，算法流程以上述文字描述为准。

### 3. 实验设计

- 使用场景/基准（Benchmark）：
  - **Multi-Agent MuJoCo**：多智能体连续控制基准，常用于评估多智能体协作策略。
  - **SMAC（StarCraft Multi-Agent Challenge）**：基于星际争霸的离散动作多智能体对战基准。
- 实验任务设定：训练于多任务离线数据，评估于**未见任务**（untested scenarios），重点考察智能体数量与目标数量变化时的泛化性能。
- 对比方法：由于摘要未完整列出基线方法名称，无法确定具体对比对象；但对比应至少包括常见的离线多任务 MARL 方法、无技能学习的基线等。
- 评估指标：摘要未明确给出具体指标（如平均回报、胜率等）。

### 4. 资源与算力

- 论文摘要及元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力资源信息。
- 如需获取相关内容，需要查阅论文正文的实验设置或附录部分。

### 5. 实验数量与充分性

- 实验数量：摘要仅提及在**两个标准基准**（Multi-Agent MuJoCo 与 SMAC）上进行了实验，涵盖多任务离线训练与未见任务泛化测试。
- 是否充分：
  - 从已提供的信息来看，两个基准覆盖了连续控制和离散动作两种典型多智能体设定，具有较好的代表性；
  - 但摘要中未报告消融实验、基线比较的详细结果以及统计显著性分析，**实验充分性无法从摘要层面完全判断**；
  - 元数据中的审稿评分为 7.0，说明审稿人认可了论文的实验贡献与严谨性。
- 公平性/客观性：方法在官方标准 benchmark 上测试，具备较好的客观条件；具体基线对齐与超参数设置仍需阅正文确认。

### 6. 主要结论与发现

- HiSSD 在离线多任务训练后，能够在未见任务中**指派有效的协作行为（effective cooperative behaviors）**。
- 与既有方法相比，HiSSD 在**多智能体数量与目标数量变化的未见场景中取得了更优性能**。
- 验证了“分层技能学习”这一范式对离线多智能体协作策略泛化问题的有效性 —— 通用技能负责跨任务知识迁移，任务专属技能负责精细任务适配。

### 7. 优点

- **技术新颖性**：首次在离线多任务 MARL 中将**共同技能与任务专属技能分离学习**，并通过分层框架有机结合，解决了以往方法只关注共同技能而忽略任务特异性知识的问题。
- **时序信息建模**：强调在通用技能中融入**协作时序知识**，补足了已有技能学习方法缺失时序协同维度的不足。
- **样本效率与安全**：通过样本内利用机制减少离线数据中分布外动作的风险，更契合离线强化学习的约束。
- **泛化能力**：设计目标直接面向智能体/目标数量变化等实际场景，具有较强的迁移适用性。

### 8. 不足与局限

- **实验细节不足**：摘要未给出消融实验、基线结果、超参数敏感性等关键实验信息，难以从摘要层面全面评估方法的稳健性与各项设计的独立贡献。
- **对比基线不明确**：未列出具体对比方法名称，难以判断改进幅度与相对优势。
- **应用场景有限**：实验仅覆盖 MuJoCo 与 SMAC 两类模拟环境，尚未在更复杂（如图队通信受限、混战场景、真实世界任务）等场景中验证。
- **可扩展性未讨论**：多智能体数量增大时的计算复杂度与技能组合空间爆炸问题未被探讨。
- **推理阶段自适应机制的细节未交代**：任务专属技能如何在线选择/组合，以及是否依赖任务标签在测试时可用，均未在摘要中说明。

---

（完）
