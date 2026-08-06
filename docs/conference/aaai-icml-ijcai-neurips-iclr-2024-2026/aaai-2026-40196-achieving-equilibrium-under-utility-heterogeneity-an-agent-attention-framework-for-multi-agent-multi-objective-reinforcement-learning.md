---
title: "Achieving Equilibrium Under Utility Heterogeneity: An Agent-Attention Framework for Multi-Agent Multi-Objective Reinforcement Learning"
title_zh: 效用异构下实现均衡：多智能体多目标强化学习的智能体注意力框架
authors: "Zhuhui Li, Chunbo Luo, Liming Huang, Luyu Qi, Geyong Min"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40196/44157"
tags: ["query:hetero-marl"]
score: 7.0
evidence: 多智能体多目标强化学习，智能体注意力框架处理异质效用函数
tldr: 该工作针对多智能体多目标系统中效用函数异构导致的训练不平稳问题，提出一种智能体注意力框架。该框架将每个智能体的目标抽象为个体效用，并通过注意力机制协调不同效用之间的冲突，从而在异构目标下达到更稳定的均衡。实验验证了该方法在机器人探索、交通管理等复杂场景中的有效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 637}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1558, \"height\": 808}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1822, \"height\": 330}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40196/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1828, \"height\": 1108}]"
motivation: 多智能体多目标系统中效用函数异构会加剧训练非平稳性，已有优化方法难以处理。
method: 提出智能体注意力框架，通过注意力机制建模不同效用函数间的冲突与协调，学习均衡策略。
result: 在异构效用设置下的多个决策任务中提升了均衡稳定性与整体性能。
conclusion: 为异构效用多智能体多目标强化学习提供了一种有效的注意力协调方法。
---

## Abstract
Multi-agent multi-objective systems (MAMOS) have emerged as powerful frameworks for modelling complex decision-making problems across various real-world domains, such as robotic exploration, autonomous traffic management, and sensor network optimisation. MAMOS enhances scalability and robustness through decentralised control and more accurately captures inherent trade-offs between conflicting objectives. In MAMOS, each agent uses utility functions that map return vectors to scalar values. Existing MAMOS optimisation methods face significant challenges in handling heterogeneous objective and utility function settings, where training non-stationarity is intensified due to private utility functions and the associated policies. In this paper, we first theoretically prove that direct access to, or structured modeling of, global utility functions is necessary to achieve the Bayesian Nash Equilibrium under decentralised execution constraints. To access the global utility functions while preserving the decentralised execution, we propose an Agent-Attention Multi-Agent Multi-Objective Reinforcement Learning (AA-MAMORL) framework. Our approach implicitly learns a joint belief over other agents’ utility functions and their associated policies during centralised training, effectively mapping global states and utilities to each agent's policy. During execution, each agent independently selects actions based on local observations and its private utility function to approximate a BNE, without relying on inter-agent communication. We evaluate our framework through extensive experiments in a custom-designed MAMO Particle environment and the standard MOMALand benchmark. The results demonstrate that accessibility to global preferences and our proposed AA-MAMORL significantly improves performance and consistently outperforms state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

### 研究动机与背景
- **研究对象**：多智能体多目标系统（MAMOS），广泛存在于机器人探索、自主交通管理、传感器网络优化等现实场景。此类系统通过去中心化控制提升可扩展性与鲁棒性，同时需要权衡多个冲突目标。
- **核心挑战**：在MAMOS中，每个智能体拥有**异构的效用函数**（将回报向量映射为标量值的偏好函数）。这些效用函数是私有的、动态变化的且彼此可能冲突，导致训练过程高度非平稳，已有方法（如团队效用假设下的MO-MIX）无法处理最一般的个体效用（Individual Utility）设置。
- **核心问题**：如何将全局状态、效用函数与联合策略映射到每个智能体的个体策略上，使各智能体在**没有通信、完全去中心化执行**的条件下，仍能学会近似达到**贝叶斯纳什均衡（BNE）** 的最优去中心化策略。

### 整体含义
- 该论文首次从理论上证明了在去中心化执行约束下，**直接访问或结构化建模全局效用函数是达到BNE的必要条件**；并提出了一种在不依赖智能体间通信的前提下协调异构效用、逼近BNE的通用框架。

---

## 2. 论文提出的方法论

### 核心思想
- 将MAMOS优化问题形式化为一般性的**部分可观测多目标马尔可夫决策过程（POMOMDP）**，并与贝叶斯博弈建立理论联系。
- 根据效用函数的性质将问题分为两类，分别设计两种MAMORL框架：
  - **案例I**：效用函数为非结构化随机变量（`wi ~ Unif(Δk)`），需显式使用全局偏好进行训练。
  - **案例II**：效用函数是观测的确定性函数（`wi = g(oi)`），则可用**智能体注意力机制**隐式建模全局信息，实现真正的CTDE。

### 理论贡献（三个定理）
| 定理 | 内容 |
|------|------|
| **定理1** | 若智能体只知道其他智能体偏好服从均匀分布而无法观测具体取值，则经典BNE概念不再适用。 |
| **定理2** | 若每个智能体在执行前能观测到其他智能体的偏好，则行为策略层面的BNE存在。 |
| **定理3** | 若每个智能体的偏好是其私有观测的连续确定性函数（`wi = g(oi)`），则在标准紧致性和连续性假设下，混合策略BNE存在。 |

### 框架一：Global-Preference-Based MAMORL（案例I）
- 策略输入为**局部观测 + 全局偏好**：`πi(ai|oi, W)`。
- 使用**向量化动作价值函数** `Qπi(s, a1,...,aN, W)` 近似多目标期望回报。
- 策略梯度：`∇θπi J = E[∇ log πi(ai|oi,W) · wi⊤ Qπi(...)]`，即用偏好加权后的标量Q值引导策略更新。
- 引入**广义策略改进（GPI）**：从随机偏好采样的多个候选策略中选择期望回报最大的动作，加速偏好空间探索。
- 适用场景：需要全局协调且通信可用的场景（如无人机编队）。

### 框架二：Agent-Attention MAMORL（AA-MAMORL，案例II）
- **去中心化执行**：每个智能体仅基于局部观测 `oi` 和私有偏好 `wi` 决策，策略为 `πi(ai|oi)`。
- **集中式注意力Critic**（核心创新）：
  1. **特征嵌入层**：每个智能体的 `[oi; ai; wi]` 经线性编码映射到共同嵌入空间得到 `xi`。
  2. **多头注意力层**：通过查询、键、值变换计算智能体间的注意力权重 `αij`，动态量化智能体j的效用与策略对智能体i的影响。
  3. **输出层**：注意力输出经残差连接、层归一化和前馈网络处理后，切片生成各智能体的向量化Q值 `Qatt_i`。
- **损失函数**：基于注意力机制的MO时间差分误差 `Latt(θQi) = E[(Qatt_i - yatt_i)²]`，通过最小化该误差联合更新嵌入层、注意力模块和输出层。
- 优势：无需全局偏好作为输入，保持CTDE完整性，无通信开销，同时对智能体间偏好依赖关系进行显式关系推理。

---

## 3. 实验设计

### 测试场景与Benchmark（共9个环境）
| Benchmark类别 | 环境名称 |
|---|---|
| **MOMA Particle**（自扩展，基于Lowe et al. 2017） | Push、Adversary、Reference、Spread、Tag |
| **MOMALand**（Felten et al. 2024，基于PettingZoo） | Mountain Walker、Escort、Catch、Surround |

### 偏好设置
- 每个智能体的偏好被建模为观测的**线性函数**（逐智能体特定但跨轮次与基线保持一致，确保公平性）。

### 对比方法
| 方法 | 说明 |
|---|---|
| **AA-MAMORL（AA）** | 本文提出的智能体注意力框架（案例II） |
| **GP-MAMORL（GP）** | 本文提出的全局偏好框架（案例I） |
| **MOMIX** | 团队效用+个体奖励的MO方法，离散动作空间，不适用于个体偏好 |
| **GPI-PD** | 单智能体MO算法（GPI+Dyna式优先级更新）修改为多智能体版本 |
| **IP（Individual Preference）** | 仅用局部观测和私有偏好学习的个体方法（相当于消融） |
| **MADDPG** | 用当前偏好标量化多目标的单目标基线（相当于消融） |

### 评估指标
- **全局效用（GU）**：各智能体偏好加权效用之和的平均，在128个初始状态下对多样偏好设置取平均，用于近似偏好空间覆盖。
- **超体积（HV）**：目标空间中参考点与非支配解围成的体积，衡量Pareto前沿质量。

---

## 4. 资源与算力

- **论文中未明确说明**使用的GPU型号、数量、训练时长、参数量等算力相关信息。
- 全文仅给出训练回合数（约2×10⁵ episodes，参见学习曲线横轴）和10个随机种子的重复实验设置，未披露具体硬件配置与训练时间开销。

---

## 5. 实验数量与充分性

### 实验数量
- **9个多智能体多目标环境**，覆盖粒子物理（5个）和MOMALand（4个）两大体系。
- **10个训练种子**，报告GU和HV的均值与标准差。
- **4个学习曲线可视化**（Mountain Walker、Surround、Tag、Push），展示训练动态。
- **消融研究**：通过AA vs GP vs MADDPG vs IP的对比，分别验证向量化动作价值、全局偏好建模和注意力机制的必要性。

### 充分性与客观性评估
- **优点**：
  - 环境覆盖面广，同时涵盖协作与对抗场景（如Adversary、Tag），能较全面评估方法在不同博弈类型下的表现。
  - 所有方法在相同偏好函数和种子条件下比较，控制变量较好。
  - 消融设计合理——MADDPG验证向量化表示的必要性，IP验证全局偏好建模的必要性。
- **不足**：
  - 缺少对注意力权重本身的定量分析（如可视化或统计注意力模式是否合理）。
  - 缺少与更多近期多智能体MO SOTA方法的对比。
  - 未报告训练时间成本与计算资源消耗，无法评估方法的实际工程可用性。
  - 部分环境（如Catch的GP方法、Push的HV指标）结果方差很大，说明某些设置下稳定性仍有隐患。

---

## 6. 论文的主要结论与发现

1. **理论层面**：证明了在去中心化执行约束下，**全局效用的可观测性或可建模性是达到BNE的必要条件**；当偏好是观测的确定性函数时，BNE的理论可实现性成立。
2. **方法层面**：提出的AA-MAMORL通过集中式注意力Critic隐式学习其他智能体的效用-策略联合信念，在保持完全去中心化、无通信执行的同时，有效逼近BNE。
3. **实验层面**：
   - AA在**大多数环境**中取得最优或接近最优的GU和HV结果，学习稳定性和收敛速度优于所有基线。
   - 在最具挑战性的**Multi-Walker**和**Push**等环境中，只有AA和GP能学到有意义策略，其他基线完全无法收敛。
   - IP的糟糕表现实证支持定理1——没有全局偏好信息的智能体无法形成对其他策略的准确信念，BNE不可达。
   - MADDPG的失败说明**标量化奖励**在偏好动态变化场景下泛化能力严重不足，向量化表示是MAMO学习的基本前提。

---

## 7. 优点

1. **理论严谨性**：给出了BNE可达性的三个定理及证明，对"为什么个体偏好设置下已有方法失效"提供了理论层面的根本解释，而不只是经验性观察。
2. **问题建模清晰**：将MAMOS问题划分为五种奖励-效用组合，并明确定位本文解决的是最困难的"个体奖励+个体效用"一般化设置。
3. **CTDE范式创新**：用注意力机制在集中式训练阶段隐式建模他者效用-策略的联合信念，使执行阶段完全去中心化且无需通信，兼顾了协调性与可扩展性。
4. **消融设计巧妙**：GP与AA的对比天然形成了"全局偏好显式输入 vs 隐式建模"的对照实验，实证分析了两种路径的适用边界。
5. **实验覆盖面广**：9个跨两套benchmark的环境、对抗与协作并存、离散与连续动作、以及多种偏好与奖励组合，验证了方法的通用性。

---

## 8. 不足与局限

1. **偏好函数假设受限**：案例II要求偏好必须是观测的确定性函数（`wi = g(oi)`），且实验仅验证了线性映射形式。现实中偏好可能受历史状态、内部状态或随机因素影响，非线性、非平稳偏好的泛化性未得到验证。
2. **理论证明不完整**：三个定理的具体证明细节被引用至arXiv版本（Li et al. 2025），公开正文中未展示核心推导过程，削弱了论文自包含性。
3. **算力信息缺失**：未报告GPU型号、训练时长、内存等资源信息，无法评估框架的工程成本与可复现难度。
4. **注意力机制缺乏解释性分析**：未展示注意力权重αij的学习结果或语义分析，"智能体如何协调异构偏好"的内部机制仍是一个黑盒。
5. **对比方法范围的局限**：GPI-PD本质是单智能体算法，MOMIX限制于团队效用，与AA不在同一问题设置下比较，削弱了"超越SOTA"这一结论的说服力。缺少与其他多智能体MO方法（如Pareto-DQN类、多智能体MO进化算法）的直接竞争比较。
6. **方差问题**：某些环境下AA的HV标准差较大（如Sur的318.4±72.0、Push的2183.1±469.9），说明在部分复杂环境中结果稳定性仍不理想。
7. **应用边界**：论文实验均为模拟环境，未验证在真实机器人或交通系统中的部署可行性；注意力机制的参数量随智能体数量增长时是否保持可扩展性也未探讨。

---

（完）
