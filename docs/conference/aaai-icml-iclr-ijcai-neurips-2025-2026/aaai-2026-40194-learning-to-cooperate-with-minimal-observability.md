---
title: Learning to Cooperate with Minimal Observability
title_zh: 在最小可观测性下学习合作
authors: "Chin-wing Leung, Paolo Turrini, Fernando P. Santos, Mirco Musolesi"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40194/44155"
tags: ["query:mcd"]
score: 7.0
evidence: 最少可观测条件下的多智能体合作
tldr: 在独立学习智能体的合作研究中，已有方法依赖观察他人策略或行为来选择合作伙伴。本文放宽该约束，提出Observer Model，使智能体仅凭直接经验和少量间接第三方观测即可形成合作认知。大规模社会实验发现，纯直接经验不足以维持合作，而少量间接观测能有效促进亲社会行为，为弱观测条件下的多智能体合作提供了可行的机制。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 713, \"height\": 173, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 503, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 568, \"height\": 458, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1351, \"height\": 569, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1657, \"height\": 555, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1826, \"height\": 529, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 867, \"height\": 420, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40194/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1785, \"height\": 515, \"label\": \"Figure\"}]"
motivation: 现有合作学习依赖观察他人行为或策略，本文研究RL智能体在仅有最少行为信息时如何维持合作。
method: 提出Observer Model，利用直接经验与有限的第三方间接观测估计他人行为，辅助合作决策。
result: 实验显示，仅靠直接经验在大规模社会中难以维持合作，而少量间接观测可显著促进利他合作。
conclusion: 证明了最小观测条件下合作仍可能涌现，为轻量观测的多智能体社会学习提供理论依据。
---

## Abstract
Cooperation among independent learning agents is desirable as it enables reaching collectively rewarding states. Recent work has shown that artificial agents can learn to act pro-socially without the need for predefined cooperative preferences or behavioural heuristics, provided that they can observe others' actions or policies and select them as partners accordingly. This paper relaxes this constraint, studying reinforcement learning (RL) agents operating with only minimal information about others' behaviour. We propose a novel `Observer Model', where agents gain insights from direct experience and limited, indirect observations. We show that direct experience alone cannot sustain cooperation, particularly in large societies. However, even minimal observations of third-party interactions, allowing as few as one observer per gameplay, lead to significant improvements, enabling the population to achieve and sustain robust cooperation across varying population sizes. Through numerical analysis, we show the co-evolution of strategy and interaction structure and disentangle how learning happens under various settings. Analysing the partner selection graph, we identify the reasons for cooperation to emerge, and we explore how different learning and exploration rates affect the outcome of social dilemmas played among RL agents.

---

## 论文详细总结（自动生成）

# 论文总结：Learning to Cooperate with Minimal Observability

## 1. 论文的核心问题与整体含义

- 论文关注的是**多智能体强化学习中的合作涌现问题**。已有研究表明，独立学习的强化学习智能体只要能够观察其他智能体的行为或策略，并据此进行合作伙伴选择，就能在类似囚徒困境（Prisoner's Dilemma）等社会困境中自发形成合作。
- 然而，现实场景中观察他人行为往往受到限制：交互可能是私密的、个体不愿共享经验、直接可观测范围有限。因此本文提出一个关键问题：**当智能体只具备最少量的行为信息时，合作还能否涌现并维持？**
- 论文的核心目标是放松“全局可观测性”假设，研究在仅有直接经验和极少量间接观察条件下，独立 Q-learning 智能体能否通过伙伴选择机制实现稳定合作。
- 论文整体含义是：**合作并不需要完全的信息公开，只要存在极轻微的“旁观者”信息渠道（每局游戏仅需一名观察者），即可显著促进合作，这为弱观测条件下多智能体系统的合作机制设计提供了理论依据。**

## 2. 论文提出的方法论

- 核心框架：**Observer Model（观察者模型）**，在原有“伙伴选择 + 社会困境博弈”框架上，引入“第三方观察者”机制。
- 智能体模型：每个智能体使用 **epsilon-greedy Q-learning**；伙伴选择（Partner Selection）策略采用 **Deep Q-Network（DQN）**，博弈阶段策略采用标准 **Q-learning**。
- 状态与记忆：
  - 每个智能体只能记忆其他智能体**最近一次与自己博弈时的动作**（直接经验）。
  - 观察者模型额外允许每局博弈中随机选择一个第三方智能体作为“观察者”，观察这对玩家的动作并更新自己的记忆。
- 算法流程（每轮）：
  1. 每个智能体根据记忆中其他智能体的最后动作，选择本轮要博弈的伙伴（伙伴选择阶段）。
  2. 被选择的两个智能体各自根据记忆中对方的最后动作，选择合作（C）或背叛（D），进行一轮囚徒困境博弈并获得收益。
  3. 随机选一个旁观者，记录这两个智能体本局的动作。
  4. 两个玩家和旁观者都更新各自的记忆；两个玩家根据收益更新 Q 值。
- 关键改进：论文还引入了**伙伴选择策略的“互更新”（mutual update）**，即被选中的智能体也会根据“被选择”的经验更新其伙伴选择 Q 值（而不仅仅是主动选择方更新）。这样可加快学习速度，但单靠它不足以维持合作。
- 公式与训练：
  - 标准 Q-learning 更新：  
    \( Q_i(s_t,a_t) \leftarrow Q_i(s_t,a_t) + \alpha[G_t - Q_i(s_t,a_t)] \)，其中 \( G_t = r_t + \gamma \max_{a'} Q_i(s_{t+1},a') \)。
  - DQN 更新采用目标网络与经验回放，参数更新用：  
    \( \theta_i \leftarrow \theta_i + \alpha[Y_t - Q(s_t,a_t;\theta_i)] \nabla_\theta Q(s_t,a_t;\theta_i) \)。
- 该模型的关键设计是：**仅需每局一个旁观者，不需要全局信息，不需要声誉机制或共享规范**，智能体只基于最近观察到的最少信息做决策。

## 3. 实验设计

- 场景设定：
  - 使用 **2 人对称囚徒困境（PD）**作为核心社会困境。收益矩阵为：T=4, R=3, P=1, S=0（满足 T > R > P > S 且 2R > T+S）。
  - 多智能体群体中，每个智能体每轮选择一个伙伴，无法拒绝被选择；每个智能体至少参与一局，可被多次选择而参与多局。
- Benchmark：
  - 全局信息模型（Global Information Model）：来自 Anastassacos et al. AAAI 2020，智能体可观察到所有其他人最近一次的动作，作为合作效果的上界/基准。
  - 本地信息模型（Local Information Model）：智能体只能基于自己的直接经验（与对方博弈时对方的最近一次动作）进行决策。
- 对比实验：
  - 混合不同概率 \( p_G \) 下使用全局信息，观察合作率随全局信息比例的变化。
  - 对比有无“伙伴选择互更新”的影响。
  - 对比 Observer Model 与 Local Information Model 在合作率、策略类型分布、伙伴选择图结构上的差异。
  - 分析不同学习阶段（4 个阶段）中策略类型的演化。
  - 不同群体规模：20、30、50 个智能体，以及观察者数量由 1 增加到 2 的效果。
  - 不同学习率（α = 0.001、0.01）和探索率（ε = 0.005、0.05）的敏感性分析。

## 4. 资源与算力

- 论文**未明确说明**使用了多少 GPU、GPU 型号、并行计算规模或具体训练时长。
- 仅提到 DQN 采用单隐层 256 个神经元、ReLU 激活，经验回放大小为 160、batch size 32、目标网络更新间隔 100，并通过网格搜索优化超参数（α、ε 范围 0.001 到 0.05）。
- 默认参数：α = 0.005，ε = 0.01，γ = 1，种群规模 30，结果在 20 次模拟上取平均。
- 就论文提供的信息而言，无法给出精确的算力清单；但鉴于实验场景为小规模 Q-learning/DQN 群体（20–50 智能体），推测训练规模属于中小规模，对算力需求不高。

## 5. 实验数量与充分性

- 实验类型较丰富：
  - 全局信息比例 \( p_G \) 的渐变实验（图 2）；
  - 互更新策略的对照实验（图 3）；
  - Observer Model 与 Local Information Model 的长时间演化比较（图 4）；
  - 策略类型分布统计（图 5）；
  - 伙伴选择图的入度/出度分析（图 6）；
  - 群体规模（20–50）与观察者数量（1 或 2）的鲁棒性实验（图 7）；
  - 学习率、探索率的敏感性实验（图 8 及附录）。
- 充分性评估：
  - 优点：覆盖面较广，既有主结果、消融实验、机制分析、鲁棒性分析，又包含超参数敏感性，对合作涌现的“为什么”有较深入剖析。
  - 不足：结果全部基于同一种博弈（囚徒困境），没有扩展到公共品博弈、公共资源困境等多体问题；也没有改变收益矩阵（如不同的 T/R/P/S 取值）进行系统性验证。因此结论的外部效度仍有局限。
  - 公平性：与既有全局信息模型的对比合理，且混合概率实验能说明信息量与合作率的单调关系；但未与其他现有合作机制（如声誉机制、社会规范等）作直接比较，公平性分析仍是部分充分的。

## 6. 论文的主要结论与发现

1. **直接经验不足以维持合作**：当智能体只能依赖自己的直接经验和伙伴选择时，合作率很低，尤其在较大群体中。
2. **全局信息并非必需**：合作率随全局信息比例上升；当 \( p_G = 0.3 \) 时，合作率已可稳定在 85% 左右，说明部分信息已有较大效果。
3. **Observer Model 有效**：仅每局增加一名旁观者，就能使群体在 30000 集内达到并维持超过 90% 的互惠合作（C,C），而 Local Information Model 下背叛/剥削（D,C）很快占据主导。
4. 合作涌现的过程可分为四个阶段：
   - 阶段 1（0–1000 集）：剥削行为流行，策略类型分布均匀；
   - 阶段 2（1000–12500 集）：互惠背叛大幅下降，互惠合作与剥削接近均衡，伙伴选择逐步有效；
   - 阶段 3（12500–17000 集）：互惠合作快速增长，TFT 策略开始增多；
   - 阶段 4（17000 集以后）：合作稳定维持，TFT 与 ALL-C 成为主要策略。
5. **策略演化机制**：在有限观测下，TFT（以牙还牙）策略逐渐占据主导，加上 ALL-C 智能体，共同惩罚并改造 ALL-D 智能体，从而使合作得以稳定。
6. **伙伴选择图更分散**：Observer Model 下智能体平均主动选择约 4.26 个不同伙伴，而 Local Information Model 下只有约 1.5 个；观察者机制使伙伴选择更多样化，避免局部锁定，从而降低被背叛的风险。
7. **鲁棒性**：群体规模增加到 50 时合作率约降到 85%；若将每局观察者数量增加到 2，合作率可回升至 90% 以上。
8. **超参数影响**：较低探索率会减慢收敛；较高学习率/探索率会促使更多 ALL-C 策略，导致合作率长期低于 80%，削弱群体抗背叛能力。

## 7. 优点

- 问题设定新颖且贴近现实：切中“最小可观测性”这一重要但研究较少的条件，比“全局信息”假设更符合实际多人交互场景。
- 机制设计简洁明了：仅需每局一个旁观者，不需要共享声誉、社会规范或显式合作偏好，具备较强的可解释性和可迁移性。
- 分析深入：不仅报告合作率，还通过策略分类（ALL-C、TFT、R-TFT、ALL-D）、伙伴选择图的入度/出度分析、学习阶段划分，揭示了“合作如何涌现”的内在机制。
- 鲁棒性验证较全面：群体规模、观察者数量、学习率、探索率均有实验，结论可信度较高。
- 对比实验设计合理：从全局信息到本地信息再到最小观察者信息的渐进式比较，能够清晰展示信息量与合作率的因果关系。

## 8. 不足与局限

- 博弈类型单一：仅在 2 人囚徒困境上验证，未涉及多人社会困境（如公共品博弈、公共资源困境），实际多智能体合作的普适性仍不确定。
- 收益矩阵固定：未就不同 T/R/P/S 取值（不同困境强度）进行敏感性分析，可能限制结论在更广泛博弈条件下的适用性。
- 对真实世界复杂性的简化：观察者随机选择、被选中者不能拒绝博弈、记忆仅保留最近一次动作等假设都较理想化；现实中存在拒绝交互、记忆较长、信息噪音等复杂因素。
- 算力信息缺失：未报告 GPU 型号、训练时长、能耗等，不利于复现或评估实际计算成本。
- 样本量有限：每次实验只取 20 次模拟的平均；策略类型箱线图来自单次代表性模拟，统计显著性检验（如置信区间、方差分析）未充分展开。
- 与替代方法的横向比较不足：没有与基于声誉、间接互惠、社会规范等已有合作机制在同一条件下直接对比，难以判断 Observer Model 在性能上的相对优劣。
- 缺乏理论保证：论文以数值实验为主，没有给出关于“最小观察者数量临界值”或“收敛性”的理论分析，未来工作需要理论支撑。

（完）
