---
title: "GRDC: A Unified Graph-Driven Framework for Role Discovery and Communication in Multi-Agent Reinforcement Learning"
title_zh: GRDC：用于多智能体强化学习中角色发现与通信的统一图驱动框架
authors: "Zihong Gao, Hongjian Liang, Yuanhui Hao, Lei Hao, Liangjun Ke"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40185/44146"
tags: ["query:hetero-marl"]
score: 9.0
evidence: 基于图驱动的角色发现与通信统一框架，解决部分可观测下的MARL协作问题
tldr: 该工作针对部分可观测条件下多智能体强化学习中的沟通与角色分配问题，提出统一的图驱动框架GRDC。GRDC利用任务引发的协作依赖关系，联合进行角色发现与通信学习，避免因误判协作对象导致的沟通错误或角色错配。实验结果显示GRDC在多个协作任务上有效提升了协调性能，为MARL中的角色与通信联合建模提供了新的方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 441}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 879, \"height\": 460}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 859, \"height\": 475}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 880, \"height\": 422}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 884, \"height\": 856}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 884, \"height\": 458}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 877, \"height\": 982}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40185/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 732, \"height\": 534}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40185/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1826, \"height\": 403}]"
motivation: 现有通信方法和基于角色的方法忽视任务引发的协作依赖，导致部分可观测下误通信或角色错配。
method: 提出图驱动框架GRDC，联合建模任务依赖的协作关系、角色发现与通信学习。
result: 在多个协作环境下显著减少了误通信和角色错配，提升了MARL协调性能。
conclusion: 统一的图驱动角色发现与通信为部分可观测MARL提供了更可靠的协作方法。
---

## Abstract
Effective coordination in Multi-Agent Reinforcement Learning (MARL) is particularly challenging under partial observability, where agents must reason about potential collaborators using only local information. Existing methods fall into two categories: communication-based approaches that enable message exchange but often fix or misidentify who the collaborators are, and role-based approaches that encourage specialization based on behavioral similarity. However, both lines of work overlook the task‑induced cooperative dependencies that decide which agents should collaborate, leading to miscommunication or role misassignment under partial observability. We introduce GRDC (Graph‑driven Role Discovery and Communication), a unified framework that approximates these dependencies by dynamically constructing local interaction graphs from trajectory embeddings, then uses these graphs to infer roles via prototype matching and to restrict communication to intra‑role agents with attention-based aggregation. Beyond role inference and communication, GRDC maximizes role entropy, decorrelates prototypes, and dynamically prunes redundant ones to obtain structured yet compact role specialization. Experimental results on Predator Prey, Cooperative Navigation, and SMACv2 demonstrate that GRDC consistently outperforms state-of-the-art communication- and role-based baselines, improving coordination efficiency and training stability across tasks.

---

## 论文详细总结（自动生成）

# GRDC：用于多智能体强化学习中角色发现与通信的统一图驱动框架——论文总结

## 一、核心问题与研究动机

- **背景**：多智能体强化学习（MARL）在部分可观测条件下，各智能体仅能依赖本地局部观测进行决策，难以准确识别"谁才是当前任务中真正需要协作的对象"，这严重制约了协同效率。
- **现有方法的两大范式及其缺陷**：
  - **基于通信的方法**（如 CommNet、DIAL、ATOC、TGCNet 等）：允许智能体间交换信息，但常常**预先固定通信对象**或**错判协作对象**，忽略了协作关系本身是动态、异构、随任务状态变化的。文中用图 1a 为例：Zealot 3 同时与多个智能体通信并在实际协作，而 Zealot 1 只与 Zealot 3 通信并期待支援，最终因通信对象错配导致行动失败。
  - **基于角色的方法**（如 ROMA、RODE 等）：依据**行为相似性**聚类角色，但文中指出这是**因果倒置**——协作*导致*行为相似，而非行为相似*意味着*协作。图 1b 展示了行为相似但不协作的智能体被错误分配到同一角色的情况。
- **核心洞察**：两类方法共同的根本缺陷在于**忽略了任务本身引发的、动态变化的协作依赖关系**——正是这种依赖关系决定了哪些智能体应当协作。缺乏这一统一视角，导致误通信（miscommunication）与角色误配（role misassignment）。
- **研究目标**：设计一个统一框架，显式建模任务驱动的协作依赖，并基于该依赖结构同时驱动角色发现与通信行为。

## 二、方法：GRDC 框架

GRDC 是一个**图驱动的统一框架**，将局部交互图构建、基于原型匹配的离散角色推断和角色内通信整合为一体，以 MAPPO 作为学习后端（CTDE 范式）。核心流程如下：

1. **基于图的交互建模**（Graph-based Interaction Modeling）
   - 每个智能体动态构建**可见性约束的局部交互图**：若两个智能体彼此在视野范围内（满足部分可观测约束），则互为邻居。
   - 轨迹编码器 `f_en` 将当前观测与历史轨迹映射为嵌入 `hτi`。
   - 利用 soft-attention 计算邻居权重 `ωgij`（公式 1），聚合邻居特征得到局部关系上下文 `xi`，并与自身轨迹嵌入拼接为域表示 `zi = [hτi ∥ xi]`。该表示融合了个体历史行为与局部交互结构，为角色推断奠定了关系语义基础。

2. **基于原型匹配的角色发现**（Role Discovery via Prototype Matching）
   - 设共 K 个可学习的角色原型 `ρ = {ρ1, ..., ρK}`，每个智能体将 `zi` 与各原型进行点积相似度匹配，经 **Gumbel-Softmax** 采样得到离散且可微的角色分配（公式 2），支持端到端优化。
   - 相比 RODE 基于行动空间相似性聚类角色，GRDC 从个体轨迹 + 局部图结构推断角色，从根本上纠正了因果方向——角色由协作关系推导，而非由行为相似性推导。

3. **角色内通信**（Intra-role Communication）
   - 消息交换**仅限于被分配到同一角色**的智能体之间，杜绝跨角色干扰。
   - 通过注意力机制对同角色成员的轨迹嵌入进行加权聚合（公式 3），生成消息 `βiρk` 并输入策略网络 `πi(hτi, βiρk)`。
   - 这一设计将通信流量与功能角色对齐，缓解了因通信对象错配造成的协作冲突。

4. **角色表示的结构化正则化**（Structural Regularization）
   - **角色熵最大化**（公式 4）：最大化经验边缘角色分布的熵，防止角色坍缩（role collapse），鼓励所有角色被均衡使用。
   - **原型去相关**（公式 5）：对归一化原型矩阵计算余弦相似度矩阵，并用 Frobenius 范数惩罚其偏离单位阵，迫使原型之间相互正交、语义离散。
   - **动态原型剪枝**（公式 6）：统计各原型在当前批次中的使用频次 `uk`，利用敏感度阈值 `ηmin = κ·(B·N)` 裁剪利用不足的原型，提升角色空间紧凑性。

5. **优化与复杂度**
   - 使用 MAPPO（PPO-clip 目标，GAE 优势估计），actor 损失 = Lclip + λre·Lre + λpd·Lpd（公式 8），critic 通过 TD 误差更新（公式 9）。
   - 时间复杂度为 **O(bg·n + K·n + bρ·n)**，其中 n 为智能体数、bg 为最大邻居数、bρ 为同角色最大成员数，均为远小于 n 的常数，因此整体复杂度随智能体数量**线性扩展**。相比之下 SR-MARL、MASIA、T2MAC、TGCNet 等为 O(n²)。

## 三、实验设计：场景、基准与对比方法

- **测试基准**（三个部分可观测协同任务）：
  - **Cooperative Navigation (CN)**：进行了部分可观测性改造——每个智能体视野半径限制为 0.3，仅感知视野内实体的相对位置与速度。
  - **Predator Prey (PP)**：同样限制视野半径为 0.3。
  - **SMACv2**：九个星际争霸微型战斗场景，涵盖 **Protoss / Terran / Zerg** 三个种族 × **5v5 / 10v10 / 20v23** 三种规模。
- **对比基线**（6 个）：
  - 通信类：**TGCNet、T2MAC、MASIA**
  - 角色类：**SR-MARL、RODE**
  - 通用 MARL 基线：**MAPPO**
- **公平性保证**：所有方法共享相同的超参数设置（详见附录 A）；每个算法使用 4 个随机种子，每个种子运行 32 个评估 episode；学习曲线报告均值和 95% 置信区间。
- **消融实验设计**：
  1. 移除**局部交互图**（角色仅由自身轨迹推断）——用于验证图结构的必要性。
  2. 移除**角色内通信**（智能体仅基于轨迹嵌入 + 角色原型决策）——用于验证通信模块的增益。
  3. 分别移除**角色熵最大化 / 原型去相关 / 动态剪枝 / 三者全部移除**——用于验证结构化正则化的贡献。
- **可解释性分析**：
  - 对 CN、PP 中学到的角色原型做 **3D PCA 投影**，结合角色动态图分析各角色的激活/剪枝情况。
  - 对 protoss 10v10 场景做**动作空间相似性分析**：统计不同角色智能体选择攻击目标的分布。

## 四、资源与算力

- **论文未报告任何算力信息**——没有提及 GPU 型号、GPU 数量、训练时长、内存消耗或总计算预算。这是论文在可复现性和资源配置说明上的一个明确缺失。限于论文提供的内容，无法给出算力层面的量化分析。

## 五、实验数量与充分性评估

- **实验规模**：共覆盖 **11 个任务场景**（CN、PP + SMACv2 九个地图）的主性能对比，加上 **7 个消融变体**（移除局部图、移除通信、移除三种正则化各一种及全部移除）以及 2 个可解释性分析实验（PCA 角色可视化、动作分布分析），实验总量较为充足。
- **统计严谨性**：4 个随机种子、95% 置信区间、每个种子 32 个评估 episode，属于 MARL 论文中的常见配置；文中还强调 GRDC 的置信区间在多数场景中与最弱基线不重叠，说明了差异的统计显著性。
- **评价**：实验设计整体**覆盖面广、比较充分**。同时需注意一些问题：
  - **对比选择**：基线均为较早期的知名方法（RODE、TGCNet、MASIA、T2MAC、SR-MARL），缺少与 2024-2025 年最新 SOTA 角色的比较（如基于大模型或 transformer 的新方法），存在一定时滞。
  - **优势程度不一**：SMACv2 的 protoss 20v23 上 GRDC 胜率仅 22.81%，且标准差高达 18.13——说明该场景下性能波动巨大；terran 10v10 上 GRDC 与 MASIA 的差距在 MASIA 标准差以内，作者也承认无显著差异。
  - **消融实验仅在 PP 上进行**，缺少在 SMACv2 大规模场景下验证各模块贡献的消融。

## 六、主要结论与发现

1. **主实验结果**：GRDC 在全部 11 个任务中的 **10 个上取得最佳性能**。CN 平均奖励 −2.29、PP 平均奖励 −1.33，分别比次优方法（MASIA）提升 8.4% 和 10.1%，比最弱基线（T2MAC/RODE）最高提升 47.6%。SMACv2 九个场景平均胜率超全部基线 **5.95%**，且样本效率显著更高（学习曲线收敛更快）。
2. **图结构是有效的**：移除局部交互图后，角色推断质量大幅下降，甚至不如 MAPPO——证明轨迹嵌入在部分可观测下信息不足，局部图结构提供了必要的关系语义。
3. **角色内通信是关键增益**：移除通信模块后性能下降 29.3%，说明角色原型虽提供了语义分组，但仅靠原型无法实现动态行为对齐，注意力式角色内通信能有效增强组内一致性。
4. **三个正则化缺一不可**：分别移除熵最大化、去相关、剪枝后性能分别下降 30.1%、34.58%、38.35%；三正则全部移除则性能暴跌 68.42% 且训练不稳定。其中**动态剪枝影响最大**。
5. **角色结构具有语义解释性**：PCA 可视化显示，CN 中活跃角色 {0,1,3,4,9} 对应空间相邻的占位智能体，PP 中活跃角色 {1,3,4,5,8,9} 对应追捕同一猎物的捕食者，而剪枝角色聚成一簇、语义冗余。
6. **因果方向验证正确**：protoss 10v10 中，同角色智能体（如角色 2 的智能体 {7,8,9}）呈现高度一致的目标选择分布，而不同角色有显著不同的行动模式——验证了"协作先于行为相似性"的因果路径。

## 七、方法优点

- **统一视角**：首次将通信与角色发现纳入同一图驱动框架，令二者互为条件、协同演进，避免了此前两类方法各自为政的割裂。
- **因果建模正确**：基于任务驱动的协作依赖（交互图）推断角色，而非基于行为相似性，纠正了 RODE 等方法的逻辑倒置。
- **通信结构化**：角色内通信将消息路由与功能语义绑定，在保留通信信息量的同时抑制跨角色干扰，且注意力聚合实现端到端可微。
- **可扩展性好**：时间复杂度 O(n) 级别，相比 O(n²) 的稠密通信方法更适合大规模智能体场景，在 SMACv2 20v23 等场景验证了这一优势。
- **正则化体系完整**：熵最大化（防坍缩）+ 原型去相关（防冗余）+ 动态剪枝（促紧凑）三管齐下，兼顾角色空间的多样性与紧凑性，且对训练稳定性有显著改善。
- **可解释性**：通过 PCA 可视化和动作分布分析验证了学到角色的语义一致性，增加了模型透明度。
- **消融设计清晰**：逐步移除各模块的实验能够精确归因各组件贡献。

## 八、不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量、训练时间等关键资源信息，不利于复现和资源配置参考。
- **基线时效性不足**：对比方法集中于较早期的代表性工作（多数为 2018-2024 年），缺少与 2025 年最新文献的直接比较。
- **统计强度有限**：4 个随机种子 + 32 个评估 episode 属于最低可接受范围；部分场景（如 protoss 20v23）胜率标准差高达 18.13%，置信区间宽泛，统计可靠性存疑。
- **消融覆盖不全**：结构正则化消融仅在 PP 上执行，未在更复杂的 SMACv2 场景验证其普适性；角色内通信的消融也仅报告单环境结果。
- **应用范围限制**：实验全部为仿真环境（导航类 + 游戏类），没有涉及机器人控制、交通信号、自动驾驶等更接近实际应用的场景；对受限带宽、通信延迟、部分智能体故障等真实部署约束未作考虑。
- **角色机制假设**：角色内通信假设同角色成员的信息高度相关，但某些任务可能恰好需要**跨角色**的信息交换（如"指挥官-执行者"分工结构），此时 GRDC 的通信隔离可能成为限制。
- **超参数敏感性**：原型数量 K、剪枝阈值 κ、正则系数 λre/λpd 等关键超参数的敏感性分析未在论文中充分讨论，可能影响方法在实际场景中的可调性。

（完）
