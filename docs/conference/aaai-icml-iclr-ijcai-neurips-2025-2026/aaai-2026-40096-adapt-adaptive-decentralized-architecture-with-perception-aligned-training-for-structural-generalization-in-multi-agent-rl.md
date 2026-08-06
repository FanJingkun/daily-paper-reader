---
title: "ADAPT: Adaptive Decentralized Architecture with Perception-Aligned Training for Structural Generalization in Multi-Agent RL"
title_zh: ADAPT：用于多智能体RL结构泛化的自适应去中心化架构与感知对齐训练
authors: "Zhixiang Zhang, Shuo Chen, Yexin Li, Feng Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40096/44057"
tags: ["query:mcd"]
score: 6.0
evidence: 去中心化训练与执行，结构泛化以应对可变智能体/对象数量
tldr: 现有多智能体强化学习架构通常固定输入输出尺寸，结构泛化方法依赖集中训练。ADAPT首次在去中心化训练与执行框架下支持结构泛化，采用以对象为中心的集合表示，使智能体可处理可变数量的可感知或可控制对象，从而提升可扩展性与隐私性。实验验证了其在多智能体协作任务中的有效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40096/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1823, \"height\": 885, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40096/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1850, \"height\": 696, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 818, \"height\": 445, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1773, \"height\": 260, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40096/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1865, \"height\": 658, \"label\": \"Table\"}]"
motivation: 传统MARL架构难以应对可变输入输出规模，且集中训练存在扩展性和隐私问题。
method: 提出ADAPT框架，采用目标中心集合表示与感知对齐训练，支持DTDE下的结构泛化。
result: 实验验证ADAPT在可变智能体数量下保持良好的泛化性能。
conclusion: 实现了无需集中训练的结构泛化，兼顾可扩展性与隐私保护。
---

## Abstract
Multi-agent reinforcement learning (MARL) excels in cooperative and competitive tasks, but most architectures are tied to fixed input-output sizes and require retraining when the number of perceptible or controllable objects changes. While structural generalization techniques mitigate this, they rely on centralized training, raising concerns about scalability and privacy. We propose ADAPT, the first framework to support structural generalization under a decentralized training and decentralized execution (DTDE) paradigm. Every agent adopts an object-centric view, encoding each observed object into a feature vector and aggregating them into a variable-length set representation. To enable each agent to infer task-level contexts from this dynamic input independently, we propose a dynamic-consistency loss that enforces spatio-temporal alignment between context representations and observed environmental dynamics. Agents then condition their policies on the inferred contexts to make locally aligned decisions. For zero-shot transfer, we propose FINE (Foresight INdex for multi-agEnt), a metric that considers Q-value overestimation and enables cross-policy comparison of long-term impact, facilitating effective policy transfer. Experiments show that ADAPT surpasses existing DTDE methods and outperforms CTDE baselines in zero-shot generalization.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题背景**：多智能体强化学习（MARL）在合作与竞争任务中表现出色，但主流架构通常将网络输入输出维度与环境中可感知/可控制对象的数量绑定，导致当对象数量变化时，模型无法直接迁移，必须重新设计并训练。
- **现有结构泛化方法的局限**：已有工作（如 OPT、UPDeT、FlickerFusion、REFIL、MATTAR）虽尝试支持结构泛化，但均依赖**集中式训练（CTDE）**，存在通信开销大、隐私风险高、可扩展性差等问题。
- **本文目标**：提出首个在**去中心化训练与去中心化执行（DTDE）** 范式下支持结构泛化的 MARL 框架——ADAPT，使每个智能体能够独立处理可变数量的对象，并实现零样本迁移，兼顾可扩展性与隐私保护。

## 2. 方法论

- **核心思想**：采用**以对象为中心（object-centric）** 的视角，将所有环境实体（队友、敌人、资源等）统一建模为对象，编码为变长集合表示，使网络参数与对象数量解耦。
- **关键架构（ADAPT）**：
  - 每个智能体维护独立完整的 Q 网络（DTDE）。
  - 观测通过三层线性层 + GRU（时间聚合）+ 多头自注意力（空间聚合）处理，得到注意力增强表示。
  - 该表示分为两个分支：
    - 一个分支平均后输出“作用于自身”的动作分数（如移动）；
    - 另一个分支按对象切分，输出“作用于对象”的动作分数（如攻击目标）。
  - 最终拼接形成完整 Q 值向量，动作空间大小仅取决于单对象动作集，与对象总数无关。
- **动态一致性损失（DC Loss）**：
  - 使用 Huber 损失约束相邻时间步之间三类变化的对齐：原始观测变化 \(\Delta O^k\)、GRU 隐藏状态变化 \(\Delta H^k\)、注意力输出变化 \(\Delta A^k\)。
  - 通过掩码处理序列 padding，并沿特征维计算 L2 范数后取均值。
  - 作用：强制内部表示与环境动力学在时空上保持一致，提升表示质量与泛化能力。
- **零样本动作选择机制：FINE（Foresight INdex for multi-agEnt）**：
  - 定义 \(F^k_t = \frac{\max_{u} Q^k_t(O^k_t, u)}{\max_{u_0} Q^k_0(O^k_0, u_0)}\)，即当前 Q 最大值与初始 Q 最大值的比值。
  - 通过 Bellman 方程展开，FINE 等价于衡量未来尚未实现回报的比例；值越高表示策略更具远见。
  - 在零样本迁移时，多个预训练策略各自提出动作，选择 FINE 值最高的动作执行，避免多数投票可能忽略少数正确策略的问题。
- **ADAPT-share 变体**：
  - 允许训练阶段在智能体间通信梯度（而非观测），更新共享参数；执行时仍完全去中心化，进一步简化策略分配，并降低通信量与隐私风险。

## 3. 实验设计

- **Benchmark 场景**：
  - **SMAC（StarCraft Multi-Agent Challenge）**：用于复杂合作战斗任务。
  - **MPE（Multi Particle Environments）** 的 simple-tag 环境：捕食者-猎物任务，涉及 prey、predators、blocks 三类对象。
- **训练-测试设置**：
  - 从零训练比较：在 SMAC 的 2s3z、5m、5m vs 6m、3s vs 4z 地图上与 I2Q 对比。
  - 零样本迁移：
    - SMAC 上：3m → 5m，5m → 7m。
    - MPE 上：3prey-3predator 和 2prey-4predator 作为源地图，分别迁移到 6 种未见过的目标地图（如 4prey-8predator、10prey-20predator、8prey-8predator、20prey-20predator、6prey-4predator、12prey-10predator）。
- **对比方法**：
  - DTDE 基线：I2Q。
  - CTDE 结构泛化基线：UPDeT、OPT-CTDE。
  - 额外对比：OPT-DTDE（将 OPT 改为 DTDE 的版本）、ADAPT+vote（多数投票策略）、ADAPT-share。

## 4. 资源与算力

- **文中未明确说明**：论文正文没有报告 GPU 型号、数量、训练时长等具体算力资源信息，仅提及在 MPE 上训练最多 3.5M 时间步，SMAC 各地图训练步数不同（如 4M、10M、20M 步）。因此无法从文中获得详细的硬件配置。

## 5. 实验数量与充分性

- **实验组数**：
  - SMAC 从零训练：4 张地图 × 5 个随机种子，报告均值±标准差。
  - SMAC 零样本迁移：2 种迁移设置（3m→5m、5m→7m），5 种子。
  - MPE 零样本迁移：2 个源地图 × 6 个目标地图 = 12 组迁移任务，5 种子。
  - 消融/变体分析：ADAPT+FINE vs ADAPT+vote vs ADAPT-share，可视为对选择策略和梯度共享的消融。
- **充分性评价**：
  - 实验覆盖了不同对象数量规模、不同任务复杂度，基准选择较全面（包括 DTDE 和 CTDE 代表性方法）。
  - 多数结果报告了均值和标准差，并多次重复试验，统计合理性较好。
  - 但**缺少针对 DC Loss 的消融实验**（如不加入 DC Loss 的 ADAPT 版本），也没有对 FINE 与投票法在更多复杂场景下的深入分析；实验局限于 SMAC 和 MPE，未涉及更复杂的真实世界任务。
  - 部分 MPE 结果（如 2prey-4predator 源地图）中 ADAPT-share 表现波动较大（标准差较高），其优势不如 FINE 稳定，说明实验结论在个别设定下不够稳健。

## 6. 主要结论与发现

- ADAPT 在 DTDE 范式下成功实现结构泛化，且从零训练时性能优于 I2Q——在 SMAC 中不仅胜率更高（例如 5m vs 6m 地图胜率 86.25% vs 40.00%），且盟友死亡数明显更少。
- ADAPT+FINE 在零样本迁移中优于 ADAPT+vote，证明 FINE 能更有效地筛选具有长远眼光的策略。
- ADAPT 系列在大多数迁移任务上超越 UPDeT、OPT-CTDE 和 OPT-DTDE，展示了良好的可扩展性和鲁棒性。
- ADAPT-share 在部分场景（如 MPE 的 6prey-4predator 源设置）表现突出，但整体稳定性不如 FINE 选择机制。
- 总体说明：去中心化训练结合目标中心表示与感知对齐约束，能够达到甚至超过集中式结构泛化方法的效果。

## 7. 优点

- **范式创新**：第一个在 DTDE 下支持结构泛化的 MARL 框架，摆脱对集中训练和全局信息的依赖。
- **架构简洁可扩展**：对象中心表示 + GRU + 自注意力，使网络可处理任意数量对象，动作输出维度不随对象数变化。
- **DC Loss 设计巧妙**：通过无监督的时序一致性约束，增强内部表示与环境动力学的对齐，不依赖额外监督信号。
- **FINE 度量新颖**：基于 Q 值之比衡量策略“远见”，提供了一种轻量级、可解释的跨策略动作选择方法，且适用于零样本迁移。
- **实验对比充分**：覆盖两类基准环境、多种对象规模，并与多种 CTDE/DTDE 方法比较，报告多次随机种子的统计结果。
- **ADAPT-share 提供梯度共享选项**：在通信受限场景中能进一步简化部署，同时减少通信量和隐私泄露。

## 8. 不足与局限

- **缺少消融实验**：没有单独验证 DC Loss 的贡献，也没有分析各组件（GRU、注意力、线性层分支）对泛化的具体影响。
- **基准环境有限**：SMAC 和 MPE 相对简单，且均为合成环境，未在更真实、更复杂的大规模任务（如交通控制、无人机集群）中验证。
- **FINE 的适用条件**：FINE 依赖 Q 值的绝对量级可比性，若不同策略的 Q 值尺度差异较大，可能产生偏差；论文未深入分析 Q 值高估对 FINE 的影响（虽然提到考虑过估计，但缺乏形式化保证）。
- **ADAPT-share 稳定性不足**：在部分 MPE 任务中方差很大，何时使用梯度共享优于 FINE 选择机制缺乏明确准则。
- **与 CTDE 基线的公平性**：UPDeT 结果直接引用原论文数据，可能因训练步数、超参数设置不同而存在不完全公平的比较。
- **理论分析较浅**：对 DC Loss 为何能提升结构泛化缺乏理论解释，FINE 的动机也主要基于 Bellman 展开的直观理解，缺少收敛性或最优性保证。
- **算力信息缺失**：未报告训练所需的计算资源，不利于复现和估计应用成本。

（完）
