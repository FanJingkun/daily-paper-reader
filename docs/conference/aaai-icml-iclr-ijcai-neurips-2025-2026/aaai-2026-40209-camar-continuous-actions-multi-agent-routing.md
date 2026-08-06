---
title: "CAMAR: Continuous Actions Multi-Agent Routing"
title_zh: CAMAR：连续动作多智能体路由
authors: "Artem Pshenitsyn, Aleksandr Panov, Alexey Skrynnik"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40209/44170"
tags: ["query:maspd"]
score: 8.0
evidence: 连续动作的多智能体路径规划，基准测试与三层评估协议
tldr: CAMAR是一个专为连续动作多智能体路径规划设计的新基准，支持合作与竞争场景，能以每秒10万步的速度高效运行，为MARL研究提供了可扩展的测试平台。它允许将RRT等经典规划方法集成到MARL流程中，并提出三层评估协议，从性能、鲁棒性和可解释性等维度全面跟踪算法进展。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 867, \"height\": 612, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 755, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1809, \"height\": 370, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 787, \"height\": 491, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 284, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 843, \"height\": 396, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40209/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 837, \"height\": 395, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40209/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1768, \"height\": 793, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40209/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1835, \"height\": 691, \"label\": \"Table\"}]"
motivation: 现有MARL基准缺少连续状态动作的路径规划与协调任务。
method: 设计CAMAR基准，支持连续动作多智能体路径规划，集成RRT规划器与三层评估。
result: CAMAR高效运行且验证了MARL算法在连续动作路径规划中的表现。
conclusion: 为连续动作多智能体路径规划提供了标准化评估平台。
---

## Abstract
Multi-agent reinforcement learning (MARL) is a powerful paradigm for solving cooperative and competitive decision-making problems. While many MARL benchmarks have been proposed, few combine continuous state and action spaces with challenging coordination and planning tasks. We introduce CAMAR, a new MARL benchmark designed explicitly for multi-agent pathfinding in environments with continuous actions. CAMAR supports cooperative and competitive interactions between agents and runs efficiently at up to 100,000 environment steps per second. We also propose a three-tier evaluation protocol to better track algorithmic progress and enable deeper analysis of performance. In addition, CAMAR allows the integration of classical planning methods such as RRT and RRT* into MARL pipelines. We use them as standalone baselines and combine RRT* with popular MARL algorithms to create hybrid approaches. We provide a suite of test scenarios and benchmarking tools to ensure reproducibility and fair comparison. Experiments show that CAMAR presents a challenging and realistic testbed for the MARL community.

---

## 论文详细总结（自动生成）

# CAMAR：连续动作多智能体路由 —— 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **核心问题**：现有 MARL（多智能体强化学习）基准环境在**连续状态与动作空间**下的多智能体路径规划（MAPF）任务上存在明显缺失，难以真实反映机器人导航与协调场景的需求。
- **现有基准的三大缺口**：
  - 许多环境使用离散动作空间，无法表征平滑连续运动（如 SMAC、POGEMA）；
  - 部分环境虽支持连续状态/动作，但无法扩展到大量智能体（如 VMAS、MPE）；
  - 另一些环境扩展性尚可但任务过于简单，缺乏对强协调与真实导航能力的要求。
- **实际需求**：仓库物流、无人机集群等场景要求机器人在连续空间中运动、遵循动力学约束并规划平滑路径，同时需要大规模、高速的仿真平台来支撑 MARL 训练与评估。
- **论文的回应**：提出 **CAMAR（Continuous Actions Multi-Agent Routing）** 基准，一个基于 JAX 的高性能、GPU 加速、支持连续动作的多智能体路径规划环境，填补了仿真速度、可扩展性与任务复杂性之间的空白。

## 2. 方法论：核心思想、关键技术细节

### 2.1 环境设计核心思想
- 仿真在**完全连续的二维空间**中进行，不使用预定义网格；智能体通过施加力来控制移动，采用计算高效的动力学模型。

### 2.2 动力学模型与动作空间
- **碰撞模型**：基于平滑接触力的排斥力模型，当物体距离小于最小阈值 `d_min` 时产生平滑增长的排斥力：
  - `f_collision = f0 · (Δx/‖Δx‖) · k · log(1 + e^(−(‖Δx‖ − d_min)/k))`
  - 使用对数-softplus 形式使力随距离平滑变化，避免突变。
- **两种内建动力学模型**：
  - **HolonomicDynamic**：全向移动，智能体施加 2D 力，通过半隐式欧拉方法更新位置和速度，包含阻尼、最大速度约束。
  - **DiffDriveDynamic**：差速驱动模型，智能体选择线速度与角速度，符合轮式机器人运动学约束。
- **异构智能体支持**：不同智能体可使用不同动力学模型和不同半径，在共享环境中协同导航。

### 2.3 观察系统
- **LIDAR 启发的向量观察**：不使用光线追踪，而是基于穿透向量表示周围物体位置；
- 每个智能体获得**以自身为中心的局部观察**，对附近每个物体输出归一化距离向量；超出感知窗口则为零向量；
- 仅保留最近的 `max_obs` 个物体以固定观察维度；
- 同时拼接**朝向目标的归一化向量**，帮助智能体感知目标方向。

### 2.4 圆形碰撞检测与地图生成
- 所有对象以**圆形**表示，碰撞检测仅需距离计算，适合 GPU 并行；
- 复杂结构可由多个小圆组合而成；
- 内置多种地图生成器：
  - `random grid`：随机网格障碍布局；
  - `labmaze grid`：基于 LabMaze 的迷宫生成；
  - `movingai`：集成 MovingAI 基准的经典二维网格地图；
  - `caves cont`：基于 Perlin 噪声的连续洞穴地图；
  - `string grid` / `batched string grid`：基于字符串布局的地图，支持并行多环境不同布局。

### 2.5 奖励函数
每个智能体每时间步的奖励由四项组成：
- **集体目标奖励** `r_all_g = +0.5`：所有智能体均到达目标时额外奖励；
- **个体目标奖励** `r_on_g = +0.5`：智能体到达目标时获得；
- **碰撞惩罚** `r_collision = −1`：发生碰撞时施加；
- **距离 shaping 奖励** `r_g_dist`：基于与目标距离的减少量，由超参数 `shaping` 控制强度。

### 2.6 评估指标
- **SR（Success Rate）**：成功率，最终位置在目标半径内的智能体比例；
- **FT（Flowtime）**：平均到达时间；
- **MS（Makespan）**：最晚到达时间；
- **CO（Coordination）**：协调度，基于碰撞次数归一化计算。

### 2.7 三层评估协议
- **Easy**：同地图类型、同智能体数量，仅改变起点和终点位置；
- **Medium**：相同结构但不同智能体数量和障碍参数；
- **Hard**：推广到完全未见过的 MovingAI 街道地图类型，且智能体数量往往不同。
- 所有结果通过**四分位均值（IQM）** 与 95% 置信区间汇总报告。

## 3. 实验设计

### 3.1 对比方法
- **6 种 MARL 算法**：IPPO、MAPPO、IDDPG、MADDPG、ISAC、MASAC；
- **2 种经典非学习基线**：RRT+PD、RRT*+PD（RRT 生成路径 + PD 控制器跟踪）；
- **6 种混合方法**：RRT*+IPPO、RRT*+MAPPO、RRT*+IDDPG、RRT*+MADDPG、RRT*+ISAC、RRT*+MASAC（RRT* 在 episode 开始时生成路径，将成本/路径信息加入观察）。

### 3.2 数据集与场景
- 两种程序化生成地图：**random grid** 和 **labmaze grid**；
- 每种地图 6 个版本，变化障碍密度与智能体数量（8 或 32）；
- labmaze 额外使用连接概率 0.4–1.0 控制迷宫复杂度；
- 异构智能体实验：`give way` 场景（小智能体可通过中央通道、大智能体需等待）。

### 3.3 扩展性对比实验
- 与 **VMAS** 对比，使用相同地图大小（20×20、障碍密度 0.3，120 个障碍）、相同智能体数量、同一块 NVIDIA H100 GPU；
- 逐步增加并行环境数（5→6000+）、智能体数（4→128）、障碍数（960→9920），以及极限智能体数量（800）。

## 4. 资源与算力

- **GPU**：单块 NVIDIA H100；
- **总训练时间**：约 **1000 小时**；
- 训练步数：IPPO/MAPPO 训练 **2000 万步**，IDDPG/MADDPG/ISAC/MASAC 训练 **200 万步**；
- 每种算法在 12 个地图变体上独立训练；
- 共训练 **532 个模型**，在 **5184 个任务**上评估，每任务 **1000 个 episode**。

## 5. 实验数量与充分性

- **实验规模较大**：12 种方法 × 12 个地图变体 + 异构智能体的专项实验 + 与 VMAS 的扩展性对比，覆盖面较广。
- **评估维度较完整**：包含成功率、流时间、makespan、协调度四个度量，并在 seen/unseen 任务上测试泛化能力。
- **存在的不足**：
  - 实验只选用了 random grid 和 labmaze grid 两种训练地图，`caves cont`、MovingAI 地图等仅用于 Hard 评估或在附录中提及，实验覆盖面有限；
  - 训练步数差异大：off-policy 算法（MADDPG、MASAC）仅训练 200 万步，可能不足以收敛，导致其表现较差，存在一定的不公平性；
  - MADDPG/MASAC 的较差表现可能主要源于超参数未充分调优，而非算法本质缺陷；
  - 未报告不同随机种子间的方差细节，表格置信区间基于 1K bootstrap 样本，但 IQM 的稳定性取决于评估次数。

## 6. 主要结论与发现

- **MAPPO 表现最强**：在 random grid 上达到最高成功率（0.830），并优于其他 MARL 基线；
- **RRT*+MAPPO 效率最高**：在保持高成功率的同时，获得更短的路程时间（FT=971），说明经典规划信息可与 RL 有效结合；
- **经典规划器不可忽视**：RRT*+PD 作为非学习方法，在 labmaze grid 上表现最佳（SR=0.692），说明窄通道、稀疏奖励场景中全路径规划的价值；
- **off-policy 方法表现参差**：RRT* 整合对 ISAC/IDDPG 有小幅提升，但对 MASAC/MADDPG 反而有害，原因是长向量输入导致训练不稳定；
- **labmaze 难度显著更高**：所有 MARL 基线成功率大幅下降，窄走廊带来更严峻的协调与探索挑战；
- **CAMAR 性能远超 VMAS**：在 6000 并行环境时保持约 5 万 SPS，而 VMAS 不足 1 万 SPS；128 智能体时 CAMAR 快约 20 倍；极限条件下支持 800 智能体（约 1400 SPS），32 智能体 + 9920 障碍仍可达约 1.5 万 SPS。

## 7. 优点

- **极高仿真速度**：基于 JAX 的 GPU 向量化实现，最高超过 10 万 SPS，显著优于现有同类环境；
- **连续动作与真实动力学**：支持全向和差速驱动两种模型，贴合实际机器人运动学；
- **灵活性与可扩展性**：圆形碰撞检测简单高效，支持从数到数百智能体；多种地图生成器涵盖网格、迷宫、连续洞穴、字符串布局等；
- **异构智能体支持**：不同尺寸和动力学模型的智能体共享环境，为异构 MARL 研究提供平台；
- **经典规划方法集成**：RRT/RRT* 可无缝接入 RL 管线，支持纯规划基线、学习基线和混合方法对比；
- **标准化评估协议**：三层难度递进的评估协议 + IQM/CI95 统计聚合，提升了可复现性和公平性；
- **透明可控的观察设计**：无光线追踪的向量观察平滑且连续，有助于智能体在不同物体尺寸间泛化。

## 8. 不足与局限

- **实验场景覆盖有限**：主实验只用了两种地图类型，Hard 评估中提及 MovingAI 但未在正文给出完整结果，对复杂地图上的泛化能力验证不足；
- **训练充分性不一致**：off-policy 算法仅训练 200 万步，相对 on-policy 算法 2000 万步差距悬殊，可能导致对比不公；
- **混合方法设计局限**：RRT* 信息只在 episode 开始时加入，未考虑动态路径重规划；路径成本与特征拼接可能导致中心化 critic 的输入维度膨胀，影响训练稳定性；
- **物理保真度有限**：圆形对象与简化力学模型相比真实机器人尚有一定距离，对高保真物理需求场景未必适用；
- **当前 Medium 协议仅含 8/32 智能体**，尚未覆盖更大规模智能体群体，计划在后续版本扩展；
- **中心化方法（MADDPG/MASAC）的失败可能源于调参不足**，而非算法固有局限，结论的推广需谨慎。

（完）
