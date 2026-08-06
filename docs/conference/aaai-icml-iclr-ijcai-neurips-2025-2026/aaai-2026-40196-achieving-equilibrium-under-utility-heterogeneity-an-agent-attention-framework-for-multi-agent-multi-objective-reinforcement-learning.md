---
title: "Achieving Equilibrium Under Utility Heterogeneity: An Agent-Attention Framework for Multi-Agent Multi-Objective Reinforcement Learning"
title_zh: 在效用异质性下实现均衡：面向多智能体多目标强化学习的智能体注意力框架
authors: "Zhuhui Li, Chunbo Luo, Liming Huang, Luyu Qi, Geyong Min"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40196/44157"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 处理多智能体多目标强化学习中的异构目标与效用函数，实现均衡。
tldr: 针对多智能体多目标系统中效用函数异构导致训练非平稳的问题，提出智能体注意力框架。该框架为每个智能体学习协调多目标冲突的注意力机制，在异构目标与效用函数下稳定优化。理论与实验表明所提方法能够收敛到均衡解，支撑机器人与传感器网络等复杂决策场景。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 637, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1558, \"height\": 808, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40196/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1822, \"height\": 330, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40196/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1828, \"height\": 1108, \"label\": \"Table\"}]"
motivation: 多智能体多目标系统中效用函数异构导致训练非平稳，现有方法难以处理。
method: 提出智能体注意力框架，为每个智能体学习协调多目标冲突的注意力机制。
result: 在异构效用函数设置下，该框架能够收敛到均衡解并提升稳定性。
conclusion: 智能体注意力机制可有效解决多目标异构带来的训练非平稳问题。
---

## Abstract
Multi-agent multi-objective systems (MAMOS) have emerged as powerful frameworks for modelling complex decision-making problems across various real-world domains, such as robotic exploration, autonomous traffic management, and sensor network optimisation. MAMOS enhances scalability and robustness through decentralised control and more accurately captures inherent trade-offs between conflicting objectives. In MAMOS, each agent uses utility functions that map return vectors to scalar values. Existing MAMOS optimisation methods face significant challenges in handling heterogeneous objective and utility function settings, where training non-stationarity is intensified due to private utility functions and the associated policies. In this paper, we first theoretically prove that direct access to, or structured modeling of, global utility functions is necessary to achieve the Bayesian Nash Equilibrium under decentralised execution constraints. To access the global utility functions while preserving the decentralised execution, we propose an Agent-Attention Multi-Agent Multi-Objective Reinforcement Learning (AA-MAMORL) framework. Our approach implicitly learns a joint belief over other agents’ utility functions and their associated policies during centralised training, effectively mapping global states and utilities to each agent's policy. During execution, each agent independently selects actions based on local observations and its private utility function to approximate a BNE, without relying on inter-agent communication. We evaluate our framework through extensive experiments in a custom-designed MAMO Particle environment and the standard MOMALand benchmark. The results demonstrate that accessibility to global preferences and our proposed AA-MAMORL significantly improves performance and consistently outperforms state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究背景**：多智能体多目标系统（MAMOS）在机器人探索、交通管理、传感器网络等真实场景中广泛应用。其核心挑战在于：各智能体拥有**异构的目标偏好（utility function）**和个性化奖励，需要在这些冲突目标间做出权衡。
- **核心问题**：现有方法在**异构目标与效用函数**设置下难以有效优化——由于每个智能体的效用函数是私有的，训练过程呈现强烈的**非平稳性（non-stationarity）**，智能体之间难以形成对彼此策略的稳定信念，导致无法收敛到贝叶斯纳什均衡（BNE）。
- **整体含义**：论文回答了"在完全去中心化执行约束下，如何访问全局效用函数以达成均衡"这一根本问题，为 MAMOS 在异构偏好场景下的优化提供了理论基础和可行框架。

## 2. 方法论

### 核心思想
- 论文首先从理论上证明：在去中心化执行约束下，**直接访问或结构化建模全局效用函数，是实现 BNE 的必要条件**。
- 根据偏好类型将问题分为两种情形：
  - **Case I**：偏好是无结构的随机变量（如均匀分布采样）——需要显式使用全局偏好。
  - **Case II**：偏好是观测的确定性函数（wi = g(oi)）——可通过集中式训练学习智能体间的关系，执行时依然保持完全去中心化。

### 关键技术与公式

1. **全局偏好 MAMORL（Case I）**：
   - 策略输入为局部观测 oi 和全局偏好 W。
   - 定义向量化状态值函数 vπi(s[t], W) 和动作值函数 qπi(s, A, W)。
   - 策略梯度：$\nabla_{\theta_{\pi_i}} J = \mathbb{E}[\nabla \log \pi_i(a_i|o_i,W) \cdot w_i^\top Q_i^{\pi_i}(s,a_1,...,a_N,W)]$
   - 引入 **GPI（Generalised Policy Improvement）** 加速偏好空间探索。

2. **Agent-Attention MAMORL（AA-MAMORL，Case II）**：
   - 策略仅以局部观测 oi 为输入：πi(ai|oi)。
   - 集中式训练中使用**智能体级注意力机制（Agent-Attention）**：
     - 每个智能体构建特征嵌入 hSi = [oi; ai; wi]。
     - 通过共享投影矩阵生成 Query/Key/Value：qi = xiWQ, K = XWK, V = XWV。
     - 计算注意力权重：$\alpha_i = \text{softmax}(q_i K^\top / \sqrt{d_h})$，反映其他智能体的效用和策略对自身奖励的影响程度。
     - 多头注意力 + 残差连接 + LayerNorm + FFN，输出各智能体的 Q 值 Qatt_i。
   - 注意力机制使每个智能体在训练时能动态建模与其他智能体的偏好-策略依赖关系，执行时无需通信。

## 3. 实验设计

- **数据集/场景**：共 9 个 MAMO 环境，分为两类：
  - **MOMA Particle 环境**（扩展自粒子环境）：Push、Adversary、Reference、Spread、Tag。
  - **MOMALand 基准**（基于 PettingZoo）：Mountain Walker、Escort、Catch、Surround。
- **对比方法**：
  - **MOMIX**（团队效用场景下的多目标方法）
  - **GPI-PD**（单智能体 MO 方法的多智能体适配版）
  - **IP（Individual Preference）**：仅基于局部观测和私有偏好的消融基线
  - **MADDPG**：标准单目标多智能体基线
  - **AA（Agent-Attention MAMORL）** 和 **GP（Global-preference MAMORL）** 为主方法。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长或其他算力资源。仅在实验设置中提到超参数设置见补充材料（Li et al. 2025），但正文未提供具体硬件信息。

## 5. 实验数量与充分性

- **实验总量**：在 9 个环境上对比 6 种方法（主方法 2 种 + 基线 4 种），每个环境报告 10 个随机种子的均值和标准差。
- **评估指标**：Global Utility（GU）和 Hypervolume（HV），覆盖偏好空间中的多样偏好设置，取 128 个初始状态的平均值。
- **消融实验**：通过 AA 与 GP、MADDPG、IP 的对比实现消融分析，验证了三个维度的贡献：
  1. 向量化奖励/动作值/偏好的必要性（对比 MADDPG）；
  2. 全局偏好建模的必要性（对比 IP，验证定理 1）；
  3. 注意力机制对异构偏好关系建模带来的额外增益（AA vs GP）。
- **充分性评价**：实验覆盖多种环境类型（合作、对抗、混合）、多种偏好设置和多种基线，规模较充分。但**缺少对偏好函数 g(oi) 不同复杂度/不同形式的敏感性分析**，也未验证方法在真实机器人系统等复杂动态环境中的表现。

## 6. 主要结论与发现

- 理论上证明了**全局效用函数的可访问性或结构化建模是实现 BNE 的必要条件**——当偏好是纯随机变量且不可观测时，BNE 不可达（定理 1）；当偏好可观测或为观测的函数时，BNE 存在（定理 2、3）。
- AA-MAMORL 在多数环境中**稳定地优于所有基线**，获得最高或接近最高的 GU 和 HV。
- GP（全局偏好）在部分环境中与 AA 性能接近，但在偏好冲突明显（如 Catch）时鲁棒性不足。
- MADDPG 和 IP 的表现证明：标量化奖励无法适应动态偏好变化；缺乏全局偏好建模使智能体无法形成对其他策略的准确信念，学习不稳定。
- **注意力机制**能够有效建模异构偏好下的智能体间关系，帮助策略在动态环境中适应其他智能体的行为，促进收敛到 BNE。

## 7. 优点

- **理论贡献扎实**：将 POMOMDP 与贝叶斯博弈桥接，严格证明了 BNE 可达性对全局偏好建模的依赖条件，为后续研究奠定了理论基础。
- **方法设计巧妙**：在保持 CTDE 框架完整性的前提下，通过注意力机制在集中训练中隐式学习联合信念，执行时无需通信，兼顾了理论必要性和实际可行性。
- **框架通用性好**：同时覆盖了偏好为随机变量和偏好为观测函数两种场景，可适配多种真实世界设置。
- **实验设计较全面**：跨 9 个环境、6 种方法、10 个种子，覆盖合作、对抗、混合性质的多种任务；同时提供学习曲线（含 95% 置信区间）和消融分析，证据链完整。

## 8. 不足与局限

- **算力资源未披露**：无法评估方法在计算成本上的实际开销，影响可复现性和实际部署评估。
- **偏好建模假设受限**：Case II 假设偏好是观测的确定性函数（wi = g(oi)），但未讨论更复杂偏好（如非平稳、记忆依赖、或策略相关的偏好）下的扩展性。
- **实验环境偏抽象**：均为粒子/网格类模拟环境（Particle、MOMALand），缺乏高保真仿真或真实硬件实验，对真实机器人、交通系统的适用性仍需验证。
- **注意力机制解释性有限**：论文未深入分析注意力权重 αij 的实际语义（如是否对应真实的智能体间影响强度），缺乏定性分析。
- **对 GPI 的作用分析不足**：GPI 被集成到框架中，但未单独消融其贡献，无法判断性能提升中注意力与 GPI 各自的贡献边界。
- **公平性考虑**：仅报告了均值和标准差，未进行显著性检验（如 t 检验），部分环境（如 Ref、Spread）方差很大，统计显著性存疑。

（完）
