---
title: "CHDP: Cooperative Hybrid Diffusion Policies for Reinforcement Learning in Parameterized Action Space"
title_zh: CHDP：参数化动作空间强化学习的协作混合扩散策略
authors: "Bingyi Liu, Jinbo He, Haiyong Shi, Enshu Wang, Weizhen Han, Jingxiang Hao, Peixi Wang, Zhuangzhuang Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39537/43498"
tags: ["query:hetero-marl"]
score: 8.0
evidence: 通过离散与连续动作智能体的协作混合扩散策略，处理多智能体强化学习中的异构动作空间。
tldr: 本文针对混合离散-连续动作空间建模难、可扩展性差的问题，提出协作混合扩散策略框架。该框架将混合动作空间问题视为完全合作博弈，利用离散扩散策略与连续扩散策略两个协作智能体进行建模，连续策略以离散动作表示为条件，显式建模二者关联。实验证明CHDP在机器人控制和游戏AI等参数化动作空间任务中显著提升策略表达力和性能，为异构动作空间下的多智能体协作提供了有效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39537/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1801, \"height\": 630, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39537/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 827, \"height\": 606, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39537/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 884, \"height\": 796, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39537/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1790, \"height\": 780, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39537/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1621, \"height\": 415, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39537/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 837, \"height\": 338, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39537/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 833, \"height\": 359, \"label\": \"Table\"}]"
motivation: 混合动作空间在机器人控制和游戏AI中普遍存在，但策略表达力有限且高维扩展性差。
method: 提出CHDP框架，将混合动作空间建模为完全合作博弈，用离散和连续扩散策略两个协作智能体联合决策。
result: 在机器人控制、游戏AI等任务中，CHDP显著提升了策略表达力与性能。
conclusion: 协作式混合扩散建模能有效处理参数化动作空间，支持异构动作智能体的协同学习。
---

## Abstract
Hybrid action space, which combines discrete choices and continuous parameters, is prevalent in domains such as robot control and game AI. However, efficiently modeling and optimizing hybrid discrete-continuous action space remains a fundamental challenge, mainly due to limited policy expressiveness and poor scalability in high-dimensional settings. 
To address this challenge, we view the hybrid action space problem as a fully cooperative game and propose a Cooperative Hybrid Diffusion Policies (CHDP) framework to solve it.
CHDP employs two cooperative agents that leverage a discrete and a continuous diffusion policy, respectively.
The continuous policy is conditioned on the discrete action's representation, explicitly modeling the dependency between them.
This cooperative design allows the diffusion policies to leverage their expressiveness to capture complex distributions in their respective action spaces.
To mitigate the update conflicts arising from simultaneous policy updates in this cooperative setting, we employ a sequential update scheme that fosters co-adaptation.
Moreover, to improve scalability when learning in high-dimensional discrete action space, we construct a codebook that embeds the action space into a low-dimensional latent space. 
This mapping enables the discrete policy to learn in a compact, structured space. 
Finally, we design a Q-function-based guidance mechanism to align the codebook's embeddings with the discrete policy's representation during training.
On challenging hybrid action benchmarks, CHDP outperforms state-of-the-art method by up to 19.3% in success rate.

---

## 论文详细总结（自动生成）

# CHDP: 协作混合扩散策略——论文总结

## 1. 核心问题与研究动机

- **问题背景**：混合动作空间（Hybrid Action Space）在机器人控制、游戏 AI 等领域中普遍存在，其动作由离散选择（如选择工具、移动模式）和连续参数（如力度、速度）共同组成。现有 DRL 方法在该领域面临两大核心挑战：
  - **策略表达力不足**：混合动作任务常具有多模态特性（如足球射门可左脚或右脚），而传统方法基于高斯分布或确定性策略，本质上是单模态的，无法同时建模互斥且等价的多个策略，导致平均化折中或坍塌到单一模式。
  - **高维可扩展性差**：高维混合动作空间存在组合爆炸问题（如非抓取操作中大量离散接触点选择），现有方法缺乏有效策略解决该问题，导致探索困难、样本效率低。
- **研究意义**：解决混合动作空间中的策略表达力与可扩展性双重挑战，是释放 DRL 在复杂真实场景应用潜力的关键。

## 2. 方法论与技术细节

### 核心思想
将混合动作空间问题建模为一个**完全合作博弈**，提出 **CHDP（Cooperative Hybrid Diffusion Policies）** 混合扩散策略框架。CHDP 包含两个协作智能体，分别使用离散扩散策略和连续扩散策略，连续策略以离散策略的输出为条件，显式建模二者间的依赖关系。

### 关键技术一：双层扩散策略协作
- **离散策略（πθd）**：输入状态 s，通过迭代去噪过程生成潜在表示 e，再经向量量化（VQ）映射到码本中最近的码字，得到离散动作索引及对应嵌入向量 ek。
- **连续策略（πθc）**：以状态 s 和离散动作嵌入 ek 为条件，通过扩散去噪过程生成连续动作 ac。
- 二者共享同一 Q 函数进行训练，实现协同优化。

### 关键技术二：顺序更新机制
- 采用启发自异构智能体强化学习（HARL）的**顺序更新方案**，而非同时更新：
  - **第 1 步**：更新离散策略，为其策略改进目标提供稳定的连续动作目标（从经验池采样并固定）；
  - **第 2 步**：基于更新后的离散策略输出，联合优化连续策略与码本参数；
  - 对离散策略输出使用 stop-gradient 操作，防止梯度回传造成更新冲突。
- 该机制避免了两个策略同时优化时相互干扰的问题（MADDPG 式并发更新的缺陷），促进协同适应。

### 关键技术三：Q 引导码本
- 借鉴 VQ-VAE 思想，构造可学习码本 Eζ ∈ R^(K×de)，将高维离散动作空间嵌入低维潜在空间，K 为离散动作数，de 为嵌入维度。
- 码本不以重建（reconstruction）为目标，而是通过**下游 Q 值引导**衡量码字效用：Q 值梯度流过连续动作传播回所选码字，引导其朝支持高值动作的方向移动；同时，离散策略也用同一 Q 函数优化，使码本嵌入与离散策略的潜在表达在其享隐空间中隐式对齐。

### 核心损失函数
- 离散策略目标：L(θd) = Ld(θd) − α·E[Qϕ(s, e, ac)]（ac 来自经验池，固定不更新）；
- 连续策略与码本联合目标：L(θc, ζ) = Ld(θc) + α·Lq(θc, ζ)；
- 评论家更新采用双 Q 学习范式与 MSBE 损失。

## 3. 实验设计

- **基准任务**：8 个标准 PAMDP 基准环境，包括 Platform、Goal、Catch Point、Hard Goal，以及 4 种不同复杂度（n=4/6/8/10）的 Hard Move 任务（离散动作空间大小分别为 2⁴=16 到 2¹⁰=1024），与 HyAR 评估环境保持一致。
- **对比基线**：
  - HyAR-TD3（当前 SOTA）；
  - PDQN-TD3、PA-TD3、HHQN-TD3（均为 TD3 架构扩展的经典方法）；
  - HPPO（PPO 类基线）；
- **实验类别**：
  1. **主实验**：8 个环境上的成功率对比与学习曲线分析；
  2. **消融实验**：在 Hard Goal 和 Hard Move (n=6) 上评估 4 个变体（无扩散策略、无码本、无顺序更新、二者皆无）；
  3. **定性分析**：在 Hard Move (n=6) 中设计单步可解任务，统计 100 次试验中动作分布，对比 CHDP 与 HyAR 的多模态策略发现能力。

## 4. 资源与算力

- **论文未明确报告**训练所需的 GPU 型号、数量、训练时长等具体算力信息。
- 仅提及扩散步数 N=15、η=5、码本嵌入维度 de=8 等超参数配置，但未给出运行时间和硬件资源消耗的详细说明。

## 5. 实验数量与充分性评估

- **实验总量**：
  - 5 个以上对比算法 × 8 个环境的主实验；
  - 2 个环境上的 5 组消融实验；
  - 1 组针对多模态表达力的定性分析。
- **统计可靠性**：所有主要结果基于 5 次独立运行的平均值与标准差（最终 5 次评估的平均作为单次运行得分），保证了一定的统计可信度。
- **公平性**：采用与 HyAR 完全相同的基准、评估协议和 TD3 类基线，确保了对比的公平性。
- **充分性评估**：实验规模较为系统，覆盖了表达力（多模态）、可扩展性（组合空间）和采样效率等多个维度；但仍主要局限于模拟环境，缺乏真实机器人平台验证，且部分消融实验（表 2）采用与表 1 不同的默认参数，需注意上下文。

## 6. 主要结论与发现

- CHDP 在所有 8 个基准任务上取得 SOTA 性能，最高相对优先方法 HyAR 提升 **19.3%** 成功率（Hard Move n=10 上从 69.0% 提升至 79.8%）。
- **表达力优势**：在 Hard Goal 任务上比 HyAR 高 19.3 个百分点（79.5% vs 60.2%），证明扩散策略能有效捕获多模态分布。
- **可扩展性优势**：在 Hard Move n=8（256 个离散动作）上仍保持 90.6% 成功率，而基线方法急剧退化（HyAR 88.3%，其余低于 18.8%）。
- **消融证明**：扩散策略、码本、顺序更新三核心组件均不可或缺——去除任一组件均导致显著性能下降，尤其在相应关键任务上（如无码本时 Hard Move 从 93.9% 崩溃至 11.1%）。
- **定性实验验证**：CHDP 学会至少 3 种不同成功策略（如主策略利用西南向基底向量加负连续参数反转力向量，同时也能发现东北向的正参数直接策略），而确定性基线 HyAR 完全坍塌为单一固定策略，直观展示了混合扩散策略表达力的优势。

## 7. 方法亮点与贡献

- **创新性框架**：首次将扩散策略引入混合动作空间，并以双智能体协作博弈视角建模离散—连续动作依赖关系，方法上具有原创性。
- **表达力与可扩展性兼得**：不同于现有方法在二者之间权衡（如 HyAR 可扩展但表达力受限，HyDo 表达力强但扩展性差），CHDP 同时解决两大挑战。
- **码本设计的巧妙性**：以 Q 值引导而非重建训练码本，避免预训练阶段、实现任务感知的语义嵌入，并通过 VQ 操作将离散策略学习约束在紧凑结构化空间。
- **实证的严谨性**：消融实验和定性分析均有针对性设计（如单步可解任务用于隔离多步轨迹干扰），能有效支撑核心结论。
- **实用的训练机制**：顺序更新方案借鉴 HARL 思想，缓解多智能体同时更新的非平稳性问题，工程实现上简洁且有效。

## 8. 不足与局限性

- **实验覆盖范围**：全部实验在模拟环境（PAMDP 基准）中完成，未在真实机器人、自动驾驶等物理系统上验证，实际部署的可行性未知。
- **公平性局限**：当前较好基线均为基于 TD3 架构的模型，未与其他近期基于能量模型、高斯混合模型或不同扩散架构的新方法（如 HyDo、LOM、RPG）进行直接对比。
- **超参数敏感性**：论文没有报告扩散步数 N、η、码本大小 K 和嵌入维度 de 等关键超参数的敏感性分析，实际应用时调参成本可能较高。
- **部分实验细节不清**：计算资源、训练时间、码本初始化方式、量化梯度的具体处理（如是否有直通估计）等细节未充分披露。
- **可扩展性验证上限有限**：Hard Move 最大离散动作数为 1024（n=10），但对更大规模的现实离散动作空间（数万级）是否依然有效仍需进一步验证。
- **多步长期收益**：定性实验中构造的是单步任务，对多步长期规划和信用分配问题中的多模态策略学习效果，论文的深入分析有限。

**（完）**
