---
title: "CAMAR: Continuous Actions Multi-Agent Routing"
title_zh: CAMAR：连续动作多智能体路由
authors: "Artem Pshenitsyn, Aleksandr Panov, Alexey Skrynnik"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40209/44170"
tags: ["query:maspd"]
score: 8.0
evidence: 连续动作多智能体路由基准、路径规划、合作与竞争任务
tldr: "该文提出连续动作多智能体路由基准CAMAR，专门面向连续状态与动作空间的多智能体路径规划，支持合作与竞争交互，并以每秒10万步速度高效运行。同时设计三级评估协议，便于分析算法性能，并允许将RRT/RRT*等经典规划方法集成到MARL流程。该基准填补了连续空间协调规划基准的空白，为多智能体导航与路由研究提供标准化平台。"
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 867, \"height\": 612}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 755, \"height\": 405}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1809, \"height\": 370}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 787, \"height\": 491}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 284}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 843, \"height\": 396}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 837, \"height\": 395}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40209/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1768, \"height\": 793}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40209/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1835, \"height\": 691}]"
motivation: 现有MARL基准较少同时覆盖连续状态/动作与复杂协调规划，缺乏连续动作路径规划标准平台。
method: 构建连续动作多智能体路径规划基准，支持合作/竞争任务，集成经典方法并给出三级评估协议。
result: 基准运行效率高，评估协议便于追踪进展，为算法比较与集成提供支持。
conclusion: 为连续空间多智能体路由与路径规划研究提供了新的基础平台与评测标准。
---

## Abstract
Multi-agent reinforcement learning (MARL) is a powerful paradigm for solving cooperative and competitive decision-making problems. While many MARL benchmarks have been proposed, few combine continuous state and action spaces with challenging coordination and planning tasks. We introduce CAMAR, a new MARL benchmark designed explicitly for multi-agent pathfinding in environments with continuous actions. CAMAR supports cooperative and competitive interactions between agents and runs efficiently at up to 100,000 environment steps per second. We also propose a three-tier evaluation protocol to better track algorithmic progress and enable deeper analysis of performance. In addition, CAMAR allows the integration of classical planning methods such as RRT and RRT* into MARL pipelines. We use them as standalone baselines and combine RRT* with popular MARL algorithms to create hybrid approaches. We provide a suite of test scenarios and benchmarking tools to ensure reproducibility and fair comparison. Experiments show that CAMAR presents a challenging and realistic testbed for the MARL community.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- 多智能体强化学习（MARL）在合作与竞争决策任务中表现优异，但现有 MARL 基准大多存在以下三个缺口：
  - 许多环境使用离散动作空间，无法表示机器人真实的平滑运动；
  - 部分环境虽支持连续状态/动作，但无法扩展到大量智能体和障碍物；
  - 另一些可扩展的环境任务过于简单，缺乏强协调性与真实导航能力。
- 经典 MAPF 通常限于离散网格，而真实机器人运动在连续空间中，需要考虑动力学与光滑路径。因此，连续空间下的多智能体路径规划（MAPF）对仓储物流、无人机集群等应用至关重要。
- 高保真机器人模拟器（Gazebo、Isaac Sim、AirSim 等）适合机器人控制与感知，但速度慢、难以同时模拟数百个智能体，不适合大规模 MARL 研究。
- 针对这一空白，论文提出 **CAMAR（Continuous Actions Multi-Agent Routing）** 基准，面向连续状态与动作空间的多智能体路径规划，兼顾现实性、可扩展性与高效训练需求。

## 2. 方法论：核心思想、关键技术细节

### 2.1 环境设计
- **连续二维空间**：无预定义网格，智能体通过施加力控制运动，采用高效动态模型。
- **碰撞模型**：基于平滑接触模型，智能体受到附近智能体与障碍物的排斥力；当距离小于最小距离 `d_min` 时，排斥力平滑增加，避免突变：
  - 公式：`f_collision_ij(t) = f0 · (Δx_ij/‖Δx_ij‖) · k · log(1 + e^-(‖Δx_ij‖ - d_min)/k)`，否则为 0。
- **两种内置动态模型**：
  - **HolonomicDynamic**：全向移动，智能体状态为位置+速度，动作为 2D 力，使用半隐式 Euler 更新，含阻尼、质量、最大速度约束。
  - **DiffDriveDynamic**：差速驱动模型，状态为位置+航向角，动作为线速度与角速度，符合常见轮式机器人运动约束。

### 2.2 观测设计
- 采用 **LIDAR-inspired 向量观测**：不使用射线追踪，而是计算智能体到附近物体（智能体或障碍物）的归一化渗透向量，对距离不连续性强且易于 GPU 向量化。
- 每个智能体只能看到自身 sensing window 内的物体，保留最近 `max_obs` 个物体；观测还包含指向目标的 ego-centric 归一化向量。

### 2.3 圆形碰撞检测与地图生成
- 所有物体用**圆**表示，距离计算简单高效，便于 GPU 并行。
- 复杂地图可通过多个小圆组合模拟墙壁、隧道、迷宫等。
- 内置地图生成器包括：
  - `random grid`：随机网格障碍物、智能体与目标；
  - `labmaze grid`：基于 Lab-Maze 生成的连通房间迷宫；
  - `movingai`：来自 MovingAI 基准的二维网格地图；
  - `caves cont`：基于 Perlin 噪声生成的连续洞穴地形；
  - `string grid` / `batched string grid`：基于文本布局定义障碍物，支持并行多种地图变体。

### 2.4 奖励函数
- 每步奖励为四项之和：
  - `r_all_g`：所有智能体到达目标时额外 +0.5（合作奖励）；
  - `r_on_g_i`：自身到达目标 +0.5；
  - `r_collision_i`：发生碰撞 -1；
  - `r_g_dist_i`：距离缩短的 shaping 奖励。

### 2.5 异构智能体支持
- CAMAR 支持不同尺寸和不同动力学模型的异构智能体，例如全向模型与差速模型混合共存，为异构 MARL 研究提供平台。

### 2.6 评估协议
- 提出三级评估协议：
  - **Easy**：相同地图类型与智能体数量，测试未见过的起点/目标；
  - **Medium**：结构相似但智能体数量和障碍物参数不同的地图；
  - **Hard**：完全未见过的地图类型（如 MovingAI 街道集合），智能体数可能不同。
- 指标：Success Rate（SR）、Flowtime（FT）、Makespan（MS）、Coordination（CO）；使用 IQM 与 95% 置信区间聚合结果。

## 3. 实验设计：场景、基准与对比方法

### 3.1 Benchmark 场景
- 主要实验结果在两种程序化地图上进行：
  - `random grid`
  - `labmaze grid`
- 每种地图有 6 个版本，变化障碍物密度与智能体数量（8 或 32 个）；labmaze 还变化连接概率（0.4~1.0）。
- 另设一个异构协作场景 `hetero give way`：两个智能体需通过窄通道，其中只有较小的红色智能体能进入中心房间，大智能体必须等待。

### 3.2 对比方法
- **6 个 MARL 算法**：IPPO、MAPPO、IDDPG、MADDPG、ISAC、MASAC。
- **2 个经典非学习方法**：RRT+PD、RRT*+PD。
- **6 个混合方法**：RRT*+IPPO、RRT*+MAPPO、RRT*+IDDPG、RRT*+MADDPG、RRT*+ISAC、RRT*+MASAC。混合方法在每回合开始时由 RRT* 生成样本路径供策略参考。
- 性能对比：CAMAR 与 VMAS 进行模拟速度对比，在相同地图尺寸、智能体数量和单张 H100 GPU 条件下测量 steps per second（SPS）。

## 4. 资源与算力

- **GPU 型号**：单块 NVIDIA H100 GPU。
- **总训练时长**：约 1000 小时。
- **训练规模**：共训练 **532 个模型**。
- **评估规模**：在 **5184 个任务**上评估，每个任务 1000 个 episode。
- **训练步数**：IPPO、MAPPO 训练 20M 步；IDDPG、MADDPG、ISAC、MASAC 训练 2M 步（每个场景独立训练）。
- 说明：论文明确提供了上述算力信息，实验规模较大。

## 5. 实验数量与充分性

- **实验充分性**：
  - 覆盖了 6 种 MARL 算法、2 种经典规划方法、6 种混合方法，共 14 种方法；
  - 在两种地图类型、12 个地图变体上评估；
  - 另有异构智能体场景实验；
  - 完成了大规模可扩展性测试（最多 800 个智能体、9920 个障碍物）。
- **客观性与公平性**：
  - 使用固定随机种子、IQM + 95% CI 统计，确保可复现性；
  - 非学习方法（RRT/RRT*+PD）作为强基线参与对比；
  - 混合方法融合经典规划与 RL，形成更全面的比较体系。
- **潜在不平衡**：训练步数设定不统一（20M vs 2M），可能对 off-policy 算法不完全公平；实验主要集中在 random grid 和 labmaze grid，尚未展示 movingai、caves 等地图在完整评测协议下的结果。

## 6. 主要结论与发现

- **MAPPO 表现最优**：在 random grid 地图上取得最高成功率（0.830），且协调性满分；在 labmaze grid 上仍是 MARL 中最优（0.568）。
- **混合方法效果不一**：RRT*+MAPPO 能进一步降低 Flowtime（971 vs 984），RRT*+IPPO 相对 IPPO 也有提升；但对 MADDPG、MASAC 反而有害，原因可能是集中式 critic 处理含 RRT* 特征的长输入向量时训练不稳定。
- **经典规划方法有竞争力**：RRT*+PD 无需学习即可在 labmaze grid 达到最高成功率（0.692），说明在奖励稀疏、窄通道环境中完整路径规划仍具优势。
- **环境难度设计有效**：labmaze grid 因窄走廊和稀疏奖励显著降低所有 MARL 基线性能，证明该基准对算法挑战性强。
- **异构智能体实验**：HetIPPO（异构版本）优于共享策略 IPPO，但 HetMAPPO 失败，可能因为集中式 critic 无法处理更复杂的异构输入。
- **性能优势显著**：CAMAR 在多种条件下远超 VMAS，支持 100,000+ SPS，最多可在 800 智能体时保持约 1400 SPS，在 32 智能体 + 960 障碍物时达到 161K SPS。

## 7. 优点

- **性能极高**：利用 JAX GPU 加速，模拟速度超过 100,000 SPS，远优于 VMAS 等现有环境，适合大规模训练。
- **连续动作空间**：支持真实机器人动力学（全向与差速模型），比离散网格更贴近实际应用。
- **三级评估协议**：Easy/Medium/Hard 分层评估泛化能力，比单一测试集更科学。
- **经典与学习方法的融合**：不仅提供 RL 基线，还集成了 RRT/RRT* 并构建混合方法，拓宽了基准适用性。
- **多样化地图与异构智能体**：支持多种地图生成器以及异构尺寸/动力学智能体，场景丰富。
- **可复现性**：提供固定随机种子、完整训练/评估脚本和公开代码库，便于社区公平比较。

## 8. 不足与局限

- **实验覆盖有限**：论文声明支持多样化地图，但详细的基准结果仅展示 random grid 与 labmaze grid；movingai、caves 等地图未纳入主实验，三级协议中的 Medium/Hard 评估也尚未完整呈现。
- **训练步数不一致**：on-policy 算法训练 20M 步，off-policy 算法仅 2M 步，可能导致 off-policy 算法（MADDPG、MASAC 等）表现被低估，影响公平性。
- **部分算法失败缺乏深入分析**：MASAC 和 MADDPG 在两个地图上成功率很低，论文仅提及长输入和训练不稳定，未提供更细致的诊断或消融。
- **RRT* 集成方式较简单**：混合方法仅在回合开始时生成路径并作为观测特征，未利用其后续重规划功能，可能低估了混合方法的潜力。
- **物理逼真度有限**：虽然采用连续动力学，但简化为圆形物体和力模型，未包含真实传感器噪声、电机延迟、复杂几何形状等，仍与真实机器人系统存在差距。
- **规模化上限仍有限**：800 智能体时速度降至约 1400 SPS，对于数千个智能体的超大规模场景仍有提升空间。

（完）
