---
title: "ADAPT: Adaptive Decentralized Architecture with Perception-Aligned Training for Structural Generalization in Multi-Agent RL"
title_zh: ADAPT：面向多智能体强化学习结构泛化的自适应分散架构与感知对齐训练
authors: "Zhixiang Zhang, Shuo Chen, Yexin Li, Feng Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40096/44057"
tags: ["query:mcd"]
score: 8.0
evidence: 在DTDE范式下实现多智能体结构泛化
tldr: 多智能体强化学习中的多数架构固定输入输出维度，在可感知或可控物体数量变化时需要重训练。ADAPT首次在分布式训练和分布式执行范式下支持结构泛化。每个智能体以对象为中心编码观测对象，聚合为可变长度的集合表示，从而适应物体数量的动态变化。实验显示ADAPT在无需集中式训练的情况下，在不同规模的任务上实现了出色泛化和性能，兼顾了可扩展性与隐私性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40096/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1823, \"height\": 885}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40096/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1850, \"height\": 696}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 818, \"height\": 445}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1773, \"height\": 260}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1865, \"height\": 658}]"
motivation: 现有结构泛化依赖集中训练，难以兼顾可扩展性与隐私保护。
method: 采用对象中心编码与集合表示，在DTDE下实现任意数量物体的感知与决策。
result: 在不同规模任务上达到结构性泛化且性能优异。
conclusion: ADAPT证明了无集中式训练的结构泛化可行性，提升实用价值。
---

## Abstract
Multi-agent reinforcement learning (MARL) excels in cooperative and competitive tasks, but most architectures are tied to fixed input-output sizes and require retraining when the number of perceptible or controllable objects changes. While structural generalization techniques mitigate this, they rely on centralized training, raising concerns about scalability and privacy. We propose ADAPT, the first framework to support structural generalization under a decentralized training and decentralized execution (DTDE) paradigm. Every agent adopts an object-centric view, encoding each observed object into a feature vector and aggregating them into a variable-length set representation. To enable each agent to infer task-level contexts from this dynamic input independently, we propose a dynamic-consistency loss that enforces spatio-temporal alignment between context representations and observed environmental dynamics. Agents then condition their policies on the inferred contexts to make locally aligned decisions. For zero-shot transfer, we propose FINE (Foresight INdex for multi-agEnt), a metric that considers Q-value overestimation and enables cross-policy comparison of long-term impact, facilitating effective policy transfer. Experiments show that ADAPT surpasses existing DTDE methods and outperforms CTDE baselines in zero-shot generalization.

---

## 论文详细总结（自动生成）

# ADAPT：面向多智能体强化学习结构泛化的自适应分散架构与感知对齐训练 —— 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：多智能体强化学习（MARL）在交通控制、无人机集群、实时策略游戏、多机器人搜救等任务中表现出色，但大多数主流架构将网络输入输出维度与环境中可感知/可控制对象的数量绑定。当对象数量变化时，网络尺寸不再匹配，必须重新设计和训练，这被称为“结构泛化”（structural generalization）问题。
- **现有方法及其局限**：已有结构泛化方法（如 UPDeT、OPT、FlickerFusion、REFIL、MATTAR）虽然能处理可变对象数量，但全部依赖集中式训练与分散式执行（CTDE）范式，存在通信开销大、隐私风险高、随智能体数量扩展性差等问题。
- **核心问题**：能否在不依赖集中训练的前提下，让每个智能体独立适应任意数量的对象并实现跨场景结构泛化？
- **论文回答**：提出 ADAPT——据作者称**首个在“分散训练+分散执行”（DTDE）范式下支持结构泛化的 MARL 框架**，兼顾可扩展性与隐私性，同时具备训练效率和零样本迁移能力。

## 2. 论文提出的方法论

### 2.1 总体思路
- 采用**对象中心（object-centric）视角**：将每个可感知实体（队友、对手、环境物体）视为独立对象，编码为特征 token。
- 每个智能体独立拥有完整的 Q 网络，在 DTDE 下完成学习和决策，不依赖其他智能体的内部状态或全局观测。

### 2.2 网络架构（ADAPT 核心）
- 输入：局部观测 \(O_k^t \in \mathbb{R}^{B \times E \times D_O}\)，E 为对象数，可变长度。
- 处理流水线：
  1. **Linear-1**：将每个对象编码为嵌入向量；
  2. **GRU**：按时间步进行时序聚合（temporal aggregation），保留历史信息；
  3. **多头自注意力**：在对象维度上进行空间聚合（spatial aggregation），为不同对象分配差异化权重；
  4. 输出 \(A_k^t\) 分为两个分支：
     - 对对象维度取平均 → Linear-2 → **对自身动作**的 Q 值（如移动）；
     - 沿对象维度拆分 → Linear-3 → **对每个对象交互动作**的 Q 值（如攻击目标）；
  5. 拼接得到完整 Q 向量，动作空间大小只取决于“每类对象的动作数”，与对象总数无关，从而实现结构可扩展。

### 2.3 动态一致性损失（Dynamic Consistency Loss, DC Loss）
- 目的：让内部表示（GRU 隐状态、注意力上下文）与观测的变化保持时空一致，使智能体仅凭局部信息也能推断任务级上下文。
- 公式：
  \[
  L_{DC} = \xi \cdot \left[ \frac{1}{\sum M_{valid}} \sum Huber(\|\Delta O^k\|, \|\Delta H^k\|) + \frac{1}{\sum M_{valid}} \sum Huber(\|\Delta O^k\|, \|\Delta A^k\|) \right]
  \]
  其中：
  \[
  \Delta x_t = x_{t+1} - x_t, \quad x \in \{O^k, H^k, A^k\}
  \]
  \[
  M_{valid,t} = M_t \cdot M_{t+1}
  \]
  \[
  \|\Delta \cdot\| \text{ 经 MaskedNormalize 处理}
  \]
- 损失使用 **Huber 损失**（δ 通常取 1），兼顾小误差的 MSE 平滑与大误差的 MAE 鲁棒性，防止异常值干扰训练。

### 2.4 训练策略（基于 I2Q 的分散学习）
- 每个智能体独立学习三个函数：
  - \(Q^k(\tau^k, u^k)\)：个体动作价值函数；
  - \(Q^k_{ss}(\tau^k, \tau'^k)\)：转移价值函数；
  - \(V^k(\tau^k)\)：状态价值函数。
- 使用三个 MSE 损失交替更新，目标网络提供稳定目标，\(\lambda\) 平衡 \(V^k\) 与 \(Q^k\) 的影响。
- 通过建模“理想转移”缓解 DTDE 下环境非平稳性问题。

### 2.5 零样本迁移的动作选择：FINE 指标
- 问题：多个独立训练的策略如何在新的未知场景中选择动作？
- **FINE（Foresight INdex for multi-agEnt）** 定义：
  \[
  F_k^t = \frac{\max_{u_k^t} Q_k^t(O_k^t, u_k^t)}{\max_{u_k^0} Q_k^0(O_k^0, u_k^0)}
  \]
- 展开 Bellman 方程后：
  \[
  F_k^t = \frac{1}{\gamma^t}\left(1 - \frac{\sum_{m=0}^{t-1}\gamma^m r_m}{\max Q_k^0}\right)
  \]
- 含义：最大化 FINE 等价于最小化“已获得收益在总收益中的占比”，即优先选择**未来回报占比更大、更具远见**的动作。
- 每次决策时，多个训练策略提出候选动作，选择 FINE 值最高的策略作为顾问。

### 2.6 ADAPT-share（变体）
- 允许训练时交换梯度信息（P2P 或经轻量服务器聚合），不交换观测数据。
- 所有智能体参数保持一致，部署时可直接用于任意新场景。
- 相比 CTDE：通信开销更小、隐私更强、扩展性更好。

## 3. 实验设计

- **基准环境 1：SMAC（StarCraft Multi-Agent Challenge）**
  - 从零训练对比：2s3z、5m、5m vs 6m、3s vs 4z 地图，对比 I2Q。
  - 零样本泛化对比：3m → 5m、5m → 7m，对比 UPDeT、OPT-CTDE、OPT-DTDE。
  - 指标：测试胜率（test win rate）、友军死亡数。
- **基准环境 2：MPE（Multi Particle Environments）simple-tag**
  - 三类对象：prey、predators、blocks。
  - 训练源地图：3prey-3predator、2prey-4predator。
  - 共 12 个零样本目标地图（如 4prey-8predator、10prey-20predator、20prey-20predator 等），分别从两个源地图迁移。
  - 对比方法：ADAPT+FINE、ADAPT+vote、ADAPT-share、UPDeT、OPT-CTDE。
  - 指标：测试胜率（捕食者团队获胜即所有 prey 至少被抓一次）。

## 4. 资源与算力

- **论文未明确说明**使用的 GPU 型号、数量、训练时长等硬件资源。
- 仅提及训练步数（SMAC 中 4M–20M 步，MPE 中最多 3.5M timesteps），但没有给出具体的计算资源描述。
- 各方法均报告为收敛后的结果，但缺乏运行时间的对比信息。

## 5. 实验数量与充分性

- **从零训练对比**：SMAC 4 张地图，对比 I2Q，每方法 5 个随机种子，报告均值±标准差。
- **零样本泛化对比**：
  - SMAC：2 组迁移设置（3m→5m、5m→7m），对比 4 种基线；
  - MPE：2 个源地图 × 6 个目标地图 = 12 组泛化任务，对比 4 种基线（含消融 ADAPT+vote 和 ADAPT-share）。
- **总体评估**：
  - 实验数量较为充分，覆盖了不同环境、不同对象比例、不同总规模；
  - 与 CTDE 强基线（UPDeT、OPT-CTDE）和 DTDE 基线（I2Q、OPT-DTDE）均有对比；
  - 包含 FINE 与投票法的消融，验证了 FINE 的有效性；
  - 不足：缺少对 DC Loss 本身的消融实验（未展示去除 DC Loss 后的性能变化），未报告不同规模场景下的推理时间、通信量等定量结果。

## 6. 论文的主要结论与发现

1. ADAPT 在 SMAC 从零训练中显著优于 I2Q：胜率更高且友军死亡数更少（如 5m vs 6m 地图胜率从 40.00% 提升至 86.25%）。
2. 在 SMAC 零样本迁移中，ADAPT+FINE 全面优于 UPDeT、OPT-CTDE、OPT-DTDE 及 ADAPT+vote。
3. 在 MPE 的 12 组跨规模泛化任务中，ADAPT+FINE 在 9/12 组任务上优于 ADAPT+vote，并在绝大多数任务上超过 UPDeT 和 OPT-CTDE。
4. ADAPT-share 在部分 MPE 场景（如 6prey-4predator、12prey-10predator）优于 ADAPT+FINE，尽管其训练性能有时落后，说明梯度共享可以提升部分场景的泛化稳定性。
5. FINE 指标能有效识别更具远见的策略，促进零样本动作选择。
6. ADAPT 证明了**在完全分散的 DTDE 范式下实现结构泛化是可行的**，无需集中式训练即可达到甚至超越 CTDE 方法的效果。

## 7. 优点

- **范式创新**：首个在 DTDE 下实现结构泛化的 MARL 框架，打破了结构泛化对集中训练的依赖，具有实际部署价值。
- **架构设计巧妙**：对象中心编码 + GRU + 自注意力的组合使输入输出维度与对象数量解耦，天然适应动态对象集合。
- **DC Loss 自监督**：无需额外标签，仅利用观测与内部表征的时间差分对齐，提升表示质量，训练稳定。
- **FINE 指标新颖**：从 Q 值比例视角量化“远见”，简单而有效地解决了多策略动作选择的难题。
- **ADAPT-share 实用**：梯度共享模式为允许有限通信的场景提供了折中方案，兼顾隐私与效果。
- **实验较扎实**：在两类主流 MARL 基准上验证，包含从零训练与零样本迁移，统计报告规范（5 seeds，均值±标准差）。

## 8. 不足与局限

- **硬件资源信息缺失**：未报告 GPU 型号、数量、训练耗时，复现成本难以估计。
- **消融不完整**：未单独验证 DC Loss 的贡献，未对比去除此损失后的性能下降幅度。
- **环境覆盖有限**：仅涉及 SMAC 和 MPE 两类模拟环境，未在更复杂的大规模或真实场景中验证。
- **FINE 依赖初始 Q 值**：其定义涉及 \(Q_k^0\)，若初始 Q 值估计有偏或在不同策略间不可比，可能影响选别准确性；论文对此讨论不够深入。
- **ADAPT-share 表现不稳定**：在部分 MPE 任务中标准差很大（如 20prey-20predator 为 ±28.15），说明梯度共享在某些场景下不够鲁棒。
- **技术细节省略**：如注意力头数、GRU 维度、超参数设置、DC Loss 的权重 ξ 取值等未在正文中给出，复现存在一定困难。

（完）
