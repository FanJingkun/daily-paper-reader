---
title: "Policy Search, Retrieval, and Composition via Task Similarity in Collaborative Agentic Systems"
title_zh: 协作型智能体系统中基于任务相似性的策略搜索、检索与组合
authors: "Saptarshi Nath, Christos Peridis, Eseoghene Benjamin, Xinran Liu, Soheil Kolouri, Peter Kinnell, Zexin Li, Cong Liu, Shirin Dora, Andrea Soltoggio"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39633/43594"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 跨智能体进行策略搜索、检索与组合，以支持异构能力下的协同决策。
tldr: 针对协作型智能体系统在面临多变任务时如何利用其他智能体知识的问题，提出基于任务相似性的策略搜索、检索与组合算法。智能体可根据任务相似性选择从何处获取知识、何时整合策略以加速自身学习。通过模块化共享与组合机制，在多个未知任务上验证了其有效性和泛化能力，为协作式策略复用提供了系统化方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1842, \"height\": 749, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 870, \"height\": 410, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 879, \"height\": 386, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 888, \"height\": 1087, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 872, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39633/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 872, \"height\": 391, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39633/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 882, \"height\": 352, \"label\": \"Table\"}]"
motivation: 协作型智能体系统在多种未知任务下如何选择、检索与组合其他智能体策略的问题尚待解决。
method: 提出基于任务相似性的策略搜索、检索与组合算法，实现模块化共享与组合。
result: 实验验证了策略复用能够加速智能体自身学习并提升泛化能力。
conclusion: 基于任务相似性的知识共享机制可促进协作型智能体系统的持续学习。
---

## Abstract
Agentic AI aims to create systems that set their own goals, adapt proactively to change, and refine behavior through continuous experience. Recent advances suggest that, when facing multiple and unforeseen tasks, agents could benefit from sharing machine-learned knowledge and reusing policies that have already been fully or partially learned by other agents. However, how to query, select, and retrieve policies from a pool of agents, and how to integrate such policies remains a largely unexplored area.  This study explores how an agent decides what knowledge to select, from whom, and when and how to integrate it in its own policy in order to accelerate its own learning. The proposed algorithm, Modular Sharing and Composition in Collective Learning (MOSAIC), improves learning in agentic collectives by combining (1) knowledge selection using performance signals and cosine similarity on Wasserstein task embeddings, (2) modular and transferable neural representations via masks, and (3)  policy integration, composition and fine-tuning. MOSAIC outperforms isolated learners and global sharing approaches in both learning speed and overall performance, and in some cases solves tasks that isolated agents cannot. The results also demonstrate that selective, goal-driven reuse leads to less susceptibility to task interference. We also observe the emergence of self-organization, where agents solving simpler tasks accelerate the learning of harder ones through shared knowledge.

---

## 论文详细总结（自动生成）

# 论文中文总结报告

## 1. 论文的核心问题与整体含义（研究动机和背景）

Agentic AI（智能体人工智能）的目标是创建能够自主设定目标、主动适应环境变化、并通过持续经验优化行为的系统。然而，当面对多种**未知的、非预见的任务**时，单个智能体在孤立学习的环境下，只能从自身有限的经历中获益，无法像人类一样通过协作与共享经验加速学习。现有方法（如分布式强化学习、联邦学习）通常假设一定程度的**中心化与任务均匀性**，这与去中心化、异步、任务高度异质的智能体系统需求不符。

因此，论文提出了一个关键研究问题：**在由独立、异步、任务各异的智能体组成的协作型集体中，智能体如何决定“选择什么知识、从谁那里获取、以及何时/如何将其整合到自身策略中”**，以实现知识的复用和学习的加速。该研究探索了策略搜索（Policy Search）、检索（Retrieval）与组合（Composition）这一尚未系统研究的领域，其核心目标是从“孤立学习”转向“选择性协作学习”，提升智能体系统的样本效率与适应性。

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

论文提出的算法为 **MOSAIC（Modular Sharing and Composition in Collective Learning，集体学习中的模块化共享与组合）**，其核心思想是：当智能体面临难以独立解决的任务时，可通过**任务相似性**度量，主动向其他智能体查询、筛选、获取并组合相关策略（以二进制掩码形式），从而加速自身学习。

### 2.1 核心架构组件

| 组件 | 功能 | 技术实现 |
|------|------|---------|
| 策略表示 | 将任务特定知识隔离为轻量模块 | 共享冻结主干网络 Φ（backbone）+ 二值化掩码 ϕτ（稀疏子网络），使用直通估计器（STE）训练 |
| 任务嵌入 | 在线度量任务间相似性 | 基于状态-动作-奖励（SAR）批次计算的 **Wasserstein 任务嵌入** vτ，与固定合成参考分布 μ₀ 对齐，形成共享隐空间 |
| 知识选择 | 决定向谁获取知识 | **双重准则过滤**：余弦相似度阈值（Criterion 1）+ 性能优越性筛选（Criterion 2） |
| 知识组合 | 整合他人策略到自身策略 | 可学习权重 β 的**线性掩码组合** + 策略微调 |
| 权重初始化 | 避免组合初期性能震荡 | **奖励引导初始化（RGI）**：低性能智能体偏向外部知识，高性能智能体偏向自身策略 |

### 2.2 算法流程

1. **任务嵌入计算**：智能体从回放缓冲中采集 N=128 个 SAR 样本，构建经验任务分布 μτ，通过求解 2-Wasserstein 距离的最优传输问题，获得嵌入向量 vτ（维度为 M×d，M=50 为参考点数量），并维护滑动平均以平滑波动；
2. **嵌入查询（TEQ）**：智能体定期向所有已知对等体广播自身任务嵌入、当前表现及地址；
3. **查询响应（QR）**：对等体回复其任务嵌入、表现及掩码 ID；
4. **策略选择**：智能体计算与各对等体的余弦相似度，并结合两个启发式准则（相似度阈值 θ=0.5；对等体表现优于自身）筛选候选策略；
5. **掩码请求与传输（MR/MTR）**：对通过筛选的智能体发送掩码请求并接收二值化掩码；
6. **策略组合**：通过等式 (7) 进行可学习的线性组合：ϕlc = g(βτϕτ + Σ βkϕk)，其中 β 通过 softmax 归一化，在训练中通过反向传播更新（固定接收的掩码）；
7. **掩码整合**：在新通信事件前，通过等式 (9) 将当前任务掩码与之前获取的掩码加权合并（consolidation），以控制内存规模。

该算法为异步、去中心化设计，通信只传输紧凑的嵌入向量和掩码分数，带宽开销极小。

## 3. 实验设计：数据集/场景、Benchmark 与对比方法

### 3.1 实验场景与 Benchmark

论文在三个**稀疏奖励**强化学习基准上评估 MOSAIC：

| Benchmark | 环境类型 | 任务设置 | 难度特征 |
|-----------|---------|---------|---------|
| CT-Graph（图像序列学习，ISL） | 树导航问题，节点为图像状态 | 28 个任务，4 个独立的图像集，每个含 7 个深度递增（2→8）的相关任务 | 奖励概率低至 ≈7.74×10⁻⁹，指数级分支 |
| MiniHack MultiRoom | 网格导航，像素观察 | 14 个任务，分两个难度簇（4×4 和 6×6 房间），每关增加一个房间 | 稀疏奖励，每局随机化布局 |
| MiniGrid Crossing | 网格世界导航，符号观察 | 14 个任务，7 个 SimpleCrossing + 7 个 LavaCrossing | 稀疏奖励，不同布局/障碍物 |

每个基准都包含**相似任务、不相似任务和干扰任务**，以验证 MOSAIC 能否识别并利用任务相似性、同时避免干扰。

### 3.2 对比方法（Baselines）

- **MOSAIC-NoComm**：无通信的孤立学习（同一算法但关闭共享）
- **MTPPO（多任务PPO）**：共享主干，无模块化或干扰缓解
- **MDQN**：共享编码器 + 任务特定 Q 头，无干扰避免机制
- **PCGrad+MoE**：共享编码器 + 专家子网络 + 梯度投影
- **MOORE（正交专家混合）**：通过正交化促进专家多样性，学习门控

### 3.3 消融实验

论文对 MOSAIC 的三个关键组件分别进行了消融：
- **¬Criterion 1**：去除余弦相似度选择；
- **¬Criterion 2**：去除性能准则选择；
- **¬RGI**：去除奖励引导初始化（改为固定 0.5/0.5 权重）。

## 4. 资源与算力

论文在正文中**未明确报告具体 GPU 型号、数量或训练时长**。仅在附录 G 中提及计算基础设施、超参数、架构和库的详细信息（对应正文引用：Tables 5、6、7、8）。值得注意的是，所有实验均在模拟环境中完成，且每个任务使用 5 个随机种子进行独立重复实验。无法从论文正文确认具体的硬件资源投入规模。

## 5. 实验数量与充分性评估

### 5.1 实验数量

- **CT-graph**：28 任务 × 5 种子 = **140 次独立训练运行**；
- **MiniHack**：14 任务 × 5 种子 = **70 次独立训练运行**；
- **MiniGrid**：14 任务 × 5 种子 = **70 次独立训练运行**；
- 另有 3 组核心消融实验（各 140 次运行）及附录中的额外消融（查询频率、嵌入样本数、参考分布大小）。

### 5.2 充分性与客观性评估

- 每个设置使用 **5 个随机种子**，并使用 95% 置信区间（CIs）报告结果，统计可靠性良好；
- 正式显著性检验在附录 A（Table 3）中报告；
- 消融实验设计完整，逐一验证了每个关键组件的贡献；
- 在三个不同环境（图像导航、像素网格、符号网格）上的验证增强了结论的泛化性；
- 对比基线覆盖了集中式多任务学习、模块化专家方法、参数隔离方法等主要技术路线，比较是全面的。

**总体评价**：实验设计较为充分、客观；然而，部分基线（如 MDQN）在实验中失败（最终表现仅为 0.31），可能反映出基线超参数未完全优化的问题，而非算法本身绝对失效。

## 6. 论文的主要结论与发现

1. **MOSAIC 显著优于孤立学习**：在 CT-graph 和 MiniHack 上相比孤立基线分别获得 **170.8%** 和 **128.2%** 的相对性能提升；在 CT-graph 上，MOSAIC 达到 50% 性能仅需 37 次迭代（18,944 步），而孤立学习者的最大总回报停滞在 9.6（MOSAIC 为 26.0）。
2. **选择性知识共享优于全局共享**：选择性、目标驱动的策略复用可避免任务干扰，而全局参数共享容易引发干扰。
3. **涌现隐含课程学习**（Implicit Curriculum）：分析表明，求解简单任务的智能体通过共享策略，逐步帮助求解复杂任务的智能体，形成从易到难的层次化技能积累，且在无集中协调的情况下自发涌现。
4. **任务嵌入有效捕捉任务关系**：Wasserstein 嵌入的余弦相似度能够准确重建任务分组结构；聚类后的 β 矩阵表明策略复用模式与任务相似性高度一致。
5. **各组件都是必要的**：消融研究证明，任一关键组件的移除都会显著降低性能——去掉相似性选择（¬C1）降低 22.7% 最终性能；去掉性能选择（¬C2）大幅减慢收敛速度；去掉 RGI（¬RGI）导致通信后性能出现锯齿状波动。

## 7. 优点

1. **开创性组合**：论文首次将模块化可迁移任务知识（掩码）与基于 Wasserstein 嵌入的相似性选择相结合，提出了系统化的策略搜索-检索-组合框架；
2. **去中心化与异步设计**：不依赖任何形式的中心化，适应真实场景中智能体分布式、异步的特性；
3. **通信高效**：仅交换紧凑嵌入（50×d）和二值掩码，带宽开销极小；
4. **理论联系实际**：Wasserstein 嵌入有扎实的最优传输理论支撑；可学习权重为策略组合提供自适应机制；
5. **可解释性**：组合策略可追溯到来源（哪个智能体的掩码），增强了系统的透明度；
6. **通用性**：策略表示（掩码）和组合机制对 RL 算法无特定依赖（PPO 仅用于实验），且可扩展到 LoRA 等参数高效微调方法，具备向大模型/LLM 迁移的潜力。

## 8. 不足与局限

1. **实验均在模拟环境中**：论文明确承认未在真实物理系统（如机器人平台）上验证。实际部署面临跨节点归一化、通信隐私/认证、带宽实时匹配等工程挑战未在实验中覆盖；
2. **奖励函数通用性受限**：选择与加权依赖原始的每迭代奖励信号，在奖励函数差异巨大的环境泛化性受限。论文建议引入归一化奖励或任务进度指标；
3. **仅支持正向组合**：策略组合仅限正权重线性组合（β≥0），未探索减法或更复杂的混合策略；同时未测试替代组合方式（如门控、注意力机制）；
4. **共享主干依赖**：所有智能体必须共享同一冻结主干网络，这限制了系统的开放性和异构性——不同架构的智能体无法直接参与知识共享；
5. **选择准则相对简单**：余弦相似度 + 静态表现比较属于启发式方法，缺乏自适应学习；未来的通信策略本身可被学习（但论文指出这会引入安全风险）；
6. **安全与对抗风险**：论文明确指出了潜在的安全威胁——恶意智能体可进行策略投毒（model poisoning）、免费搭车（不贡献只索取）、或传播有害策略，这些问题需要额外机制加以防护；
7. **资源消耗未报告**：未提供训练的具体计算资源需求（GPU 数量/时长），影响社区对该方法的复现成本评估；
8. **通信策略固定**：查询频率等超参数（如 θ=0.5）为人工设置，缺乏自适应调节，在不同任务分布下可能存在最优值漂移问题。

**（完）**
