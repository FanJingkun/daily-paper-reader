---
title: Causality-Aware Efficient Exploration for Cooperative Multi-Agent Reinforcement Learning
title_zh: 面向协作多智能体强化学习的因果感知高效探索
authors: "Hongye Cao, Tianpei Yang, Fan Feng, Hammadi Rafik Ouariachi, Yali Du, Meng Fang, Jing Huo, Yang Gao"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39071/43033"
tags: ["query:mcd"]
score: 8.0
evidence: 面向协作MARL的因果感知探索
tldr: 协作多智能体强化学习中的探索常受无关因素干扰，导致样本效率低下。论文提出因果感知高效探索（CEE），推断智能体、全局状态与奖励之间的因果关系，并据此引导探索。框架包含两个组件：先识别全局状态与奖励的因果关联，再利用因果信息指导状态空间的探索。实验表明CEE有效减少无关干扰，显著提升协作任务中的样本效率。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有内在激励探索忽略智能体、状态与奖励间的因果关系，受无关因素干扰。
method: 通过推断因果关系识别与奖励相关的全局状态，实现因果引导探索。
result: 在协作MARL基准上提升了样本效率和探索有效性。
conclusion: 将因果推断引入探索机制，提升了协作MARL的样本效率。
---

## Abstract
Exploration is critical for cooperative multi agent reinforcement learning (MARL) to improve sample efficiency. However, existing intrinsic motivation based exploration strategies in MARL overlook the causal relationships among agents, global states, and rewards, suffering from interference by irrelevant factors and resulting in sample inefficiency. To address this issue, we propose Causality aware Efficient Exploration (CEE), a novel framework that enhances sample efficiency by inferring causal relationships between agents, global states with respect to rewards, thereby enabling causality guided exploration. Specifically, CEE operates through two components. First, CEE identifies causal relationships between global states and rewards, filtering out causally irrelevant state features that do not have a high impact on rewards to keep decision critical state information. Second, CEE discovers causal relationships between agents' behaviors and rewards to quantify each agent's contribution to collective performance. To achieve this, we introduce a causal entropy objective that promotes exploration aligned with decision critical aspects of the underlying causal structure. We provide comprehensive validation through experiments on 21 challenging tasks spanning SMAC, SMAC v2, and Google Research Football (GRF) environments. Our results demonstrate that CEE achieves superior performance in terms of sample efficiency and asymptotic performance compared to existing MARL methods.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：协作多智能体强化学习（Cooperative MARL）在自动驾驶、多机器人控制、传感器网络等场景中取得显著进展，但普遍面临样本效率低下的问题。
- **核心问题**：现有基于内在激励（Intrinsic Motivation）的探索策略往往忽略智能体、全局状态与奖励之间的因果结构，导致探索过程受到与任务无关的噪声因素干扰，浪费大量交互样本，难以高效协作。
- **论文动机**：探索应关注真正影响奖励的关键因素。例如在协作足球任务中，只有靠近球门的球员和关键状态维度才对得分有决定性影响；如果不加区分地探索无关区域或不重要的智能体，学习效率会显著下降。
- **整体含义**：论文提出将因果关系发现引入探索机制，通过识别“状态-奖励”和“智能体行为-奖励”的因果关联，过滤无关信息、突出关键智能体，从而提升协作 MARL 的样本效率和最终性能。

## 2. 方法论

### 核心思想
- 提出 **因果感知高效探索（Causality-aware Efficient Exploration, CEE）** 框架，建立在值分解（Value Decomposition）架构（如 QMIX）之上。
- 通过两个因果发现组件引导探索：
  1. **因果状态过滤（Causal State Filtering）**：识别全局状态中与奖励因果相关的维度，屏蔽无关状态特征。
  2. **因果动作探索（Causal Action Exploration）**：识别各智能体行为对奖励的因果贡献，据此重新加权智能体的探索优先级。

### 关键技术细节
- **状态-奖励因果发现**：
  - 收集交互轨迹 `{(s_t, a_t, r_t, s_{t+1})}`。
  - 使用 DirectLiNGAM 算法学习结构因果模型 `q_s`，得到状态-奖励因果矩阵 `M^{s→r}`。
  - 构造二值掩码 `M_s`，屏蔽因果值最低的 `k` 个状态维度，使混合网络输入变为 `\tilde{s} = s ⊙ M_s`。
  - 混合网络计算：`Q_tot(s,a) = Mixer(Q_1, ..., Q_n, \tilde{s})`。

- **智能体-奖励因果发现**：
  - 使用智能体动作与全局奖励数据训练因果模型 `q_a`，得到智能体-奖励因果矩阵 `M^{a→r}`。
  - 利用该矩阵对联合策略中不同智能体的动作进行加权 `{ω^1 a^1_t, ..., ω^n a^n_t}`。

- **因果熵目标（Causal Entropy Objective）**：
  - 定义因果熵：`H_c(π_c(a_t|s_t; M^{a→r})) = -Σ_{i=1}^N M^{a→r} ⊙ π_c(a^i_t|s_t) log π_c(a^i_t|s_t)`。
  - 采用因果熵替代标准策略熵，使探索优先集中在因果影响大的关键智能体上。
  - 最优目标为 `J(π_c) = E[Σ γ^t (r(s_t,a_t) + α H_c(π_c(·|s_t)))]`。

- **优化方式**：
  - 使用修改后的 Bellman 算子：`T^{π_c} Q_tot = r(s,a) + γ E_{s'~P}[V_tot(s')]`，其中 `V_tot(s) = E_{a~π_c}[Q_tot(s,a) + αH_c(π_c(a|s))]`。
  - 混合网络用 TD(λ) 更新，损失函数为 `L_Q(θ) = 1/2 (Q^θ_tot(s_t,a_t) - T^π_c_λ Q_tot(s_t,a_t))²`。
  - 策略更新通过保持 IGM 原则的保序变换函数 `f` 进行：`L_{π_c}(φ) = 1/2 (Σ f^i_φ(Q_i(o_i,a_i), \tilde{s}_t) - Q_tot(s_t,a_t))²`。

- **算法流程（Algorithm 1）**：
  - 使用策略 `π_c` 与环境交互并存入回放缓冲区；
  - 每隔 `T` 步采样数据学习两个因果模型，更新因果掩码和加权矩阵；
  - 每步梯度更新混合网络和策略网络参数。

## 3. 实验设计

### Benchmark 与场景
- **SMAC**：4 个任务，包含 hard（8m_vs_9m, 2c_vs_64zg）和 super hard（3s5z_vs_3s6z, corridor）。
- **SMAC-v2**：15 个任务，覆盖 protoss、terran、zerg 三种种族，以及 5v5、10v10、30° 视野、90° 视野等设置。
- **GRF（Google Research Football）**：2 个任务（Academy Counterattack Hard 和 Easy）。
- 总共 **21 个任务**，规模最高支持 14 个协作智能体。

### 对比方法
- 传统 CTDE 方法：VDN、QMIX、QPLEX、HPN-QMIX。
- 因果 MARL 方法：LAIES。
- 探索增强方法：Soft-QMIX。
- 重建引导方法：RGP。

### 实验设置
- 基于 PyMARL2 代码库，所有实验使用 **5 个随机种子**。
- 评估指标为各任务的胜率（win rate）。
- 额外进行属性分析：
  - 因果权重热力图可视化（zerg 5v5）；
  - 消融实验（3 个 SMAC-v2 任务，分别去掉因果状态过滤和因果动作探索）；
  - 计算开销分析和温度参数 α 的敏感性分析。

## 4. 资源与算力

- **文中未明确说明**具体的 GPU 型号、GPU 数量、训练总时长等详细算力资源信息。
- 仅在计算负担分析中给出了 3 个 SMAC-v2 任务的训练时间对比（以小时为单位），显示 CEE 相比 QMIX、QPLEX、Qatten 存在一定额外开销，但额外时间控制在 1 小时以内，且部分任务（如 zerg）计算时间低于 Qatten。
- 因此，论文在算力资源透明度方面存在不足，读者无法准确复现其训练成本。

## 5. 实验数量与充分性

- **实验量较大且较为充分**：覆盖 21 个任务，包含多种环境、难度、视野条件和智能体规模，能够较好地验证方法的泛化性。
- **消融实验**：在 3 个 SMAC-v2 任务上验证了两个组件（因果状态过滤、因果动作探索）的贡献。
- **属性分析**：包括因果权重可视化、计算时间对比、超参数（α）敏感性分析。
- **公平性考量**：
  - 与基线方法共享 PyMARL2 代码库和超参数设置，使用 5 个随机种子，结果包含均值和标准差，整体较公平。
  - 但实验主要集中在 **基于值分解的方法**，未与 MAPPO、MADDPG 等策略梯度方法对比；也缺少对 DirectLiNGAM 因果发现假设（线性、非高斯）在非线性环境中的鲁棒性分析。

## 6. 主要结论与发现

- CEE 在 SMAC、SMAC-v2 和 GRF 共 21 个任务中，普遍取得优于现有方法的胜率和样本效率。
- 在 SMAC-v2 全部 12 个评估场景中，CEE 均取得最佳结果，平均胜率达 55.9%，显著高于 Soft-QMIX（50.9%）、QPLEX（45.6%）、HPN-QMIX（48.3%）等基线。
- 在部分可观测（30° 视野）条件下，CEE 提升最为明显，说明因果过滤和因果探索在信息受限情况下更具价值。
- 在 GRF 任务上，CEE 优于专门处理因果关系的 LAIES 方法，表明奖励引导的因果探索比仅分析智能体-状态因果更有效。
- 消融实验表明，因果动作探索模块贡献更大，但两个组件单独使用都能超越 QMIX，缺一不可的组合效果最佳。

## 7. 优点

- **新颖性**：首次在协作 MARL 中系统性地发现并利用“奖励引导的因果关系”（状态-奖励、智能体行为-奖励），区别于已有主要关注动作-状态因果的工作。
- **方法设计清晰**：两个组件分工明确——状态过滤减少噪声，动作探索突出关键智能体，并与 QMIX 的值分解架构自然结合。
- **因果熵目标合理**：将因果加权引入最大熵框架，实现“因-果引导的探索”，技术上具有简洁性和可扩展性。
- **实验覆盖面广**：21 个任务包括多种难度、不同视野和最多 14 个智能体的场景，验证了方法在不同条件下的有效性。
- **分析维度丰富**：包含消融、因果权重可视化、计算开销、超参数敏感性等，增强了结论的可信度。

## 8. 不足与局限

- **未建模智能体-状态因果依赖**：论文在 Limitation 中承认，CEE 没有显式建模智能体与全局状态之间的因果结构，可能限制其对复杂因果关系的刻画能力。
- **因果发现假设较强**：采用 DirectLiNGAM，依赖线性非高斯、Markov 条件和 faithfulness 假设；在真实高度非线性、非平稳环境中，因果矩阵估计可能不稳定。
- **实验方法覆盖有限**：仅与值分解类方法对比，未与主流策略梯度 MARL（如 MAPPO）比较，也缺少在连续动作空间或真实机器人任务上的验证。
- **计算开销未充分量化**：未给出 GPU 明细和总训练成本，实际部署时因果模型的周期更新会引入额外开销。
- **超参数敏感风险**：因果掩码规模 `k`、因果更新间隔 `T`、温度系数 `α` 等需要额外调节，缺乏更系统的敏感性分析。
- **仅评估胜率单一指标**：未报告奖励曲线、样本效率的统计显著性检验等更细粒度指标，可能不足以全面反映性能差异。

（完）
