---
title: Multi-agent In-context Coordination via Decentralized Memory Retrieval
title_zh: 基于去中心化记忆检索的多智能体上下文协调
authors: "Tao Jiang, Zichuan Lin, Lihe Li, Yi-Chen Li, Cong Guan, Lei Yuan, Zongzhang Zhang, Yang Yu, Deheng Ye"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39394/43355"
tags: ["query:mcd"]
score: 8.0
evidence: 合作多智能体RL、去中心化协调与奖励分配问题
tldr: 针对合作多智能体强化学习中去中心化策略部署导致的任务对齐与奖励分配失配问题，本文提出基于去中心化记忆检索的上下文协调方法。该方法利用大规模Transformer模型的少样本能力，通过检索记忆为各智能体提供协调上下文，使策略快速适应未见任务，并显著改善任务对齐。实验结果表明，所提方法在复杂协作场景中取得更优性能与泛化性。这一工作为多智能体上下文学习与去中心化协作提供了新思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1593, \"height\": 527}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 417}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1611, \"height\": 894}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 777, \"height\": 642}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39394/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 886, \"height\": 955}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39394/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1684, \"height\": 533}]"
motivation: 合作多智能体场景下，去中心化策略部署常因任务对齐和奖励分配不一致而限制策略适应效率。
method: 提出基于去中心化记忆检索的上下文协调方法，利用Transformer少样本能力，通过检索协调记忆指导各智能体协作策略。
result: 在复杂协作环境中取得较好的适应性能，验证了方法的有效性和泛化性。
conclusion: 该方法为多智能体上下文协调提供了新范式，有望提升去中心化多智能体系统的自适应协作能力。
---

## Abstract
Large transformer models, trained on diverse datasets, have demonstrated impressive few-shot performance on previously unseen tasks without requiring parameter updates. This capability has also been explored in Reinforcement Learning (RL), where agents interact with the environment to retrieve context and maximize cumulative rewards, showcasing strong adaptability in complex settings. However, in cooperative Multi-Agent Reinforcement Learning (MARL), where agents must coordinate toward a shared goal, decentralized policy deployment can lead to mismatches in task alignment and reward assignment, limiting the efficiency of policy adaptation. To address this challenge, we introduce Multi-agent In-context Coordination via Decentralized Memory Retrieval (MAICC), a novel approach designed to enhance coordination by fast adaptation. Our method involves training a centralized embedding model to capture fine-grained trajectory representations, followed by decentralized models that approximate the centralized one to obtain team-level task information. Based on the learned embeddings, relevant trajectories are retrieved as context, which, combined with the agents' current sub-trajectories, inform decision-making. During decentralized execution, we introduce a novel memory mechanism that effectively balances test-time online data with offline memory. Based on the constructed memory, we propose a hybrid utility score that incorporates both individual- and team-level returns, ensuring credit assignment across agents. Extensive experiments on cooperative MARL benchmarks, including Level-Based Foraging (LBF) and SMAC (v1/v2), show that MAICC enables faster adaptation to unseen tasks compared to existing methods.

---

## 论文详细总结（自动生成）

## 基于去中心化记忆检索的多智能体上下文协调（MAICC）——论文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：大规模Transformer模型在多样数据集上预训练后，展现出强大的少样本（few-shot）泛化能力，无需更新参数即可适应新任务。这一能力已被引入强化学习（RL）领域，即上下文强化学习（In-Context Reinforcement Learning, ICRL），在单智能体场景（如网格世界、游戏）中取得了显著成效。
- **核心问题**：将ICRL范式扩展到合作多智能体强化学习（MARL）面临两大挑战：
  1. **部分可观测性**：去中心化执行时，每个智能体只能访问局部观测，导致对整体任务特征的理解存在偏差或不完整；
  2. **信用分配（credit assignment）**：智能体通常只收到共享的团队回报，难以评估个体贡献，容易出现“懒惰智能体”（lazy agent）问题，即部分智能体无法学到有效策略。
- **研究意义**：现有ICRL方法在合作MARL中效果不佳，亟需一种能在去中心化多智能体场景下高效适应未见任务的上下文学习方法。本文提出的MAICC正是为了解决这一问题。

### 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

#### 总体框架
- MAICC遵循**集中式训练、去中心化执行（CTDE）**范式，整个流程分为三个阶段：训练嵌入模型、检索式上下文决策训练、去中心化在线快速适应。

#### 阶段一：多智能体轨迹嵌入模型
- **集中式嵌入模型（Centralized Embedding Model, CEM）**：
  - 输入：各智能体的局部观测 {o_j}、动作 {a_j} 和步后信息 P̂（包含全局奖励、done信号、任务完成标志）；
  - 引入**团队内可见性（intra-team visibility）**，使同一时间步内同一团队的观测/动作token相互可见；
  - 设计三个损失函数以精细建模轨迹：
    - 行为策略损失 L_μ：预测个体动作；
    - 奖励损失 L_R：预测全局奖励（隐含信用分配）；
    - 转移动态损失 L_T：预测下一时刻观测。
- **去中心化嵌入模型（Decentralized Embedding Models, DEMs）**：
  - 仅使用局部信息（自身观测、动作、步后信息）生成嵌入；
  - 通过最小化CEM与DEM嵌入之间的KL散度（L_DEM），将CEM学到的团队级知识蒸馏到DEM中，弥补数据不足导致的表征能力差距。
- **关键设计**：嵌入模型不使用RTG（Return-To-Go）token，理由是RTG可能导致检索到回报数值相似但任务不相关的轨迹，降低上下文信息质量。

#### 阶段二：基于检索的上下文决策训练
- 对查询子轨迹τ_q，使用DEM提取最终步嵌入（对观测、动作、步后信息嵌入做平均池化），得到查询嵌入 z_q。
- 使用**最大内积搜索（MIPS）**+余弦相似度，从离线数据集D中检索top-k个最相关的上下文轨迹 C(τ_q)。
- 将检索轨迹与当前子轨迹拼接（CONCAT），训练共享参数的因果Transformer决策模型 π_θ，损失为下一动作的负对数似然（L_π）。

#### 阶段三：去中心化上下文快速协调
- **选择性记忆机制（Selective Memory）**：
  - 在线测试时，维护一个在线回放缓冲区B（与当前任务分布对齐但初期数据少）和离线的多任务数据集D（覆盖广但存在分布偏移）；
  - 引入指数时间衰减系数 β_t = exp(-λ·t/T)，以概率 β_t 从D采样、以 1-β_t 从B采样构建混合记忆B′。早期更依赖离线数据以促进探索，后期更多利用高质量在线数据以增强利用。
- **混合效用分数（Hybrid Utility Score）**：
  - S_util(τ) = α·norm(R) + (1-α)·norm(R̃)，其中R为全局回报，R̃为DEM预测的个体回报，α为超参数（默认0.8）；
  - 检索时综合余弦相似度与效用分数：S(τ_c, τ_q) = cossim(z_c, z_q) + S_util(τ_c)，兼顾个体与团队层面的收益，缓解信用分配困境。
- **理论分析**：论文给出了在线累积遗憾上界 E[Reg_M] ≤ Õ(CH^{3/2}ω√AT)，与现有ICRL方法形式一致，说明MAICC具有理论保证。

### 3. 实验设计：数据集/场景、Benchmark与对比方法

- **Benchmark说明**：
  - **Level-Based Foraging (LBF)**：网格世界任务，智能体需协调同时采集食物；使用 LBF: 7x7-15s 和 LBF: 9x9-20s 两种设置；
  - **SMAC v1**：StarCraft多智能体挑战，按种族分为 Protoss、Terran、Zerg 三类任务；
  - **SMAC v2**：增加了随机性的改进版，使用单一模型处理全部三种任务类型（“all”设置）。
- **离线数据收集**：使用QMIX算法收集多任务离线数据集D。
- **对比方法**：
  - **MADT**：多智能体决策Transformer，支持单任务训练，不支持在线适应；
  - **AT (Agentic Transformer)**：ICRL方法，利用前序episode历史经验做上下文；
  - **RADT (Retrieval-Augmented Decision Transformer)**：检索增强ICRL方法，采用粗粒度嵌入；
  - **HiSSD**：从多任务离线数据学习可泛化技能，不支持在线适应；
  - **MAICC-S**：MAICC的消融版本，不使用CEM、只训练DEM。
- **公平性保障**：除HiSSD外，所有模型均为Transformer架构，使用相同规模的GPT-2模型，保证对比公平。

### 4. 资源与算力

- **论文未明确报告**使用的GPU型号、数量及具体训练时长等算力资源信息。
- 仅可推断：需要训练三个模型（CEM、DEM、决策模型），使用多任务离线数据集预训练，在在线阶段进行T个episode的交互式适应。作者在附录A/B/C中提供了部分细节（如超参数、推导），但完整的算力配置未在提取文本中呈现。

### 5. 实验数量与充分性

- **主要实验组数**：
  - 6个测试场景（LBF x2、SMAC v1 x3、SMAC v2 x1）；
  - 每个模型使用5个随机种子，每个种子在10个随机任务上评估，共50次测试运行，报告均值与95%置信区间；
  - 消融研究在SMACv2: all场景上设置了6组变体（默认、加RTG、β=0、β=1、去掉不同CEM损失、不同α取值）；
  - 嵌入可视化实验对4种嵌入配置进行比较（t-SNE投影）。
- **充分性评价**：
  - **优点**：实验覆盖多种任务类型（网格世界、RTS游戏），包含主实验、消融、可视化、理论分析四个维度，置信区间报告规范，对比方法已涵盖当时主流ICRL和多任务MARL方法；
  - **局限**：消融实验仅在SMACv2: all上进行，未在其他场景验证各组件普适性；训练数据仅由QMIX一种算法生成，未使用多种行为策略；在线适应episode数有限，对于极长时程任务的有效性未验证。

### 6. 论文的主要结论与发现

- MAICC在所有测试场景中均优于现有ICRL方法（AT、RADT）和多任务MARL方法（MADT、HiSSD），能够实现更快的在线适应，且复杂、任务多样性高的SMACv2场景中优势最大。
- MAICC-S与MAICC的差距验证了集中式CEM蒸馏团队信息、显式建模多智能体特征的嵌入模型对轨迹检索的必要性。
- 嵌入可视化表明：不使用RTG token、使用三个辅助损失（L_μ+L_R+L_T）能学到更精细、任务聚类更清晰的轨迹表征；加入RTG或损失不全会导致嵌入混叠和泛化能力下降。
- 消融研究确认：RTG token有害、混合记忆（指数衰减β）优于单一数据源、三个CEM损失缺一不可、混合效用分数（α=0.8）优于只用全局或只用个体回报。

### 7. 优点：方法或实验设计上的亮点

- **方法层面**：
  - 首次将ICRL范式系统性地引入Dec-POMDP，提出面向多智能体协作的完整框架；
  - 巧妙运用CTDE思想，通过CEM→DEM的知识蒸馏弥合集中训练与去中心执行的信息差；
  - 检索时采用嵌入相似度+混合效用分数双重标准，同时解决任务对齐和信用分配两个核心难题；
  - 记忆构建采用指数时间衰减的混合采样，具有理论动机，能自适应平衡探索与利用；
  - 提供在线累积遗憾的理论上界，使方法具备理论支撑；
  - 决策模型跨智能体共享参数，天然具备可扩展性。
- **实验设计层面**：
  - 覆盖两代SMAC基准（v1/v2）和LBF，任务类型多样、难度梯度清晰；
  - 对比方法选择合理，包含直接多任务方法（MADT、HiSSD）与ICRL方法（AT、RADT），并设置了自身消融版本MAICC-S；
  - 嵌入可视化直观揭示了设计选择的效果，使定性结论易于理解。

### 8. 不足与局限

- **方法方面**：
  - 记忆构建依赖指数时间衰减系数λ，需要任务相关先验；不同任务最优λ可能不同，缺乏自适应调节机制；
  - 混合效用分数中的α为固定常数（默认0.8），对不同任务和适应阶段可能不是最优；
  - DEM对个体回报的预测精度会直接影响检索质量，而预训练分布与测试任务分布存在偏移时预测可能失真；
  - 理论分析依赖数据覆盖假设（P(M)/P_D(M) ≤ C），在实际大规模任务中未必容易验证。
- **实验方面**：
  - 未报告具体算力消耗（GPU数量、训练时长），复现成本未知；
  - 消融实验仅在SMACv2: all上进行，结论的普适性有待他在其他场景验证；
  - 离线数据仅通过QMIX单一算法生成，未考虑行为策略多样性对结果的影响；
  - 未与最新的其他多智能体ICRL方法（若有）进行对比，对比集仍有扩展空间；
  - 在线适应能力评估的episode数有限（T），长期运行中的记忆膨胀和检索效率问题未被研究。

（完）
