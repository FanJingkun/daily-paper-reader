---
title: Conditional Diffusion Model for Multi-Agent Dynamic Task Decomposition
title_zh: 基于条件扩散模型的多智能体动态任务分解
authors: "Yanda Zhu, Yuanyang Zhu, Daoyi Dong, Caihua Chen, Chunlin Chen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40158/44119"
tags: ["query:mcd"]
score: 8.0
evidence: 面向长期任务的合作MARL层次化任务分解
tldr: 复杂合作多智能体强化学习常面临长时域任务样本效率低的问题，动态任务分解可帮助层次化学习。本文提出CD3T，一种基于条件扩散模型的两层层次化MARL框架，高层策略学习子任务表示并由子任务效应生成选择策略，从而自动推断子任务与协调模式。实验表明CD3T在部分可观测动态环境中显著提升学习效率与长期协同性能，为扩散模型在MARL任务分解中的应用提供了新方向。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1825, \"height\": 722, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 856, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1837, \"height\": 738, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1843, \"height\": 1065, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1664, \"height\": 456, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1778, \"height\": 516, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 875, \"height\": 580, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40158/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1830, \"height\": 569, \"label\": \"Figure\"}]"
motivation: 复杂合作MARL任务中动态任务分解从零学习样本效率低，长时域任务在部分可观测下探索困难。
method: 提出CD3T，两层层次化MARL框架，以条件扩散模型作为高层策略生成子任务选择策略，捕捉子任务效应与协调模式。
result: 在动态、部分可观测环境中验证了CD3T，显著提升长期协同任务的学习效率和性能。
conclusion: 研究表明扩散模型可有效用于MARL动态任务分解，为长时域层级协同决策提供新途径。
---

## Abstract
Task decomposition has shown promise in complex cooperative multi-agent reinforcement learning (MARL) tasks, which enables efficient hierarchical learning for long-horizon tasks in dynamic and uncertain environments. However, learning dynamic task decomposition from scratch generally requires a large number of training samples, especially exploring the large joint action space under partial observability. In this paper, we present the Conditional Diffusion Model for Dynamic Task Decomposition (CD3T), a novel two-level hierarchical MARL framework designed to automatically infer subtask and coordination patterns. The high-level policy learns subtask representation to generate a subtask selection strategy based on subtask effects. To capture the effects of subtasks on the environment, CD3T predicts the next observation and reward using a conditional diffusion model. At the low level, agents collaboratively learn and share specialized skills within their assigned subtasks. Moreover, the learned subtask representation is also used as additional semantic information in a multi-head attention mixing network to enhance value decomposition and provide an efficient reasoning bridge between individual and joint value functions. Experimental results on various benchmarks demonstrate that CD3T achieves better performance than existing baselines.

---

## 论文详细总结（自动生成）

## 一、核心问题与整体含义（研究动机与背景）

- 复杂合作多智能体强化学习（MARL）在传感器网络、机器人集群、自动驾驶等真实场景中具有广阔应用前景，但仍面临两大挑战：
  - **部分可观测性**：每个智能体只能获取局部观测，难以获得全局协调信息；
  - **联合动作-观测空间随智能体数量指数增长**：有价值的状态探索极为稀少，协调困难。
- 传统CTDE框架（如VDN、QMIX等）通过集中训练缓解部分可观测性，但参数共享易导致智能体行为趋同，缺乏多样性，阻碍有效合作。
- **任务分解**被视为自然解决方案：将复杂任务拆分为子任务，让不同智能体专注于特定子任务，可有效降低动作-观测空间复杂度、提升学习效率。
- 然而，已有角色/技能/分组方法多依赖简单网络结构提取动作表示，表示能力有限，难以捕捉智能体与环境间的动态交互。
- 本文提出 **CD³T（Conditional Diffusion Model for Dynamic Task Decomposition）**，利用扩散模型强大的表示能力与随机过程建模能力，实现动态、可泛化的子任务分解与协调，是首个（据作者所述）将条件扩散模型系统性用于MARL动态任务分解的工作。

## 二、论文提出的方法论

### 核心思想
- 采用**两层层次化MARL框架**：
  - **高层**：子任务选择器（subtask selector）通过子任务表示学习，每隔 ∆T 个时间步为每个智能体分配合适的子任务；
  - **低层**：各子任务对应的策略网络在受限子任务动作空间中进行学习和执行。
- 利用**条件扩散模型**建模动作语义表示，捕捉子任务对环境的影响（即预测下一观测和奖励），并通过聚类自动发现子任务结构。

### 关键技术细节

1. **动作表示学习（Action Representation Learning via Diffusion）**
   - 将智能体 i 的 one-hot 动作 ai 映射为 d 维表示 zai，作为扩散过程的原始样本 z0；
   - 采用带交叉注意力的 UNet 主干作为去噪网络，条件为局部观测 oi 与其他智能体动作 a−i；
   - 扩散训练损失（简化变分下界）：
     ```
     Ld(θd) = E[ || ε − εθd(zk, k, oi, a−i) ||² ]
     ```
   - 另设预测器 fdo 与 fdr，利用动作表示预测下一观测 o′ 和全局奖励 r，构成预测损失 Lp；
   - 总动作表示学习损失：L(θ) = Lp(θ) + ηd × Ld(θd)。

2. **子任务动态分解（Subtask Dynamic Decomposition）**
   - 在训练前 50K 步收集样本并训练扩散模型；
   - 使用 **k-means 聚类**对动作表示进行聚类，将任务分解为有限子任务集合 Φ = {ϕ1, ..., ϕg}；
   - 子任务表示由聚类内动作表示取平均得到：zϕj = (1/|Aj|) Σ zam；
   - 子任务选择器以 GRU 编码历史信息 hτ，通过 MLP 编码器 fϕ 得到 zτ，用点积 Qϕi(τi, ϕj) = zτᵢᵀzϕj 评估各子任务价值并选择最优；
   - 子任务选择每 ∆T 步执行一次，期间智能体在受限子任务空间中行动。

3. **信用分配与值分解（Learning Decomposition with Credit Assignment）**
   - 引入基于干预的注意力调整函数（intervention-based adjustment），满足 **IGM**（Individual-Global-Max）原则；
   - 对子任务选择器：用点积注意力计算信用权重 λϕh,i，联合值函数：
     ```
     QΦ_tot = cϕ(s) + Σh wϕh Σi λϕh,i Qϕj_i(τi, ϕj_i)
     ```
     对应的 TD loss 以 ∆T 步累积回报为目标；
   - 对子任务策略层：类似地用动作表示 za 计算信用权重 λh,i，联合值函数 Qtot = c(s) + Σh wh Σi λh,i Qi(τi, ami)，用标准 TD loss 优化；
   - 低层策略仅在所属子任务的动作空间中选择原始动作。

4. **训练与执行**
   - CTDE范式下，集中训练、分散执行；
   - 执行时仅需局部观测和子任务分配信息，每个智能体在对应子任务的受限动作空间中决策，显著缩小决策空间。

## 三、实验设计

### Benchmark 与场景
- **LBF（Level-Based Foraging）**：两个定制场景——4智能体2食物、3智能体3食物；
- **SMAC（StarCraft Multi-Agent Challenge）**：8张地图，覆盖 easy / hard / super hard 难度：
  - easy: 8m, 2s3z, 3s5z
  - hard: 5m_vs_6m, 2c_vs_64zg
  - super hard: 3s5z_vs_3s6z, 6h_vs_8z, corridor
- **SMACv2**：在附录中补充验证（额外实验）。

### 对比基线
- 经典值分解方法：VDN、QMIX、QTRAN、QPLEX；
- 角色/技能/分组方法：CDS、RODE、GoMARL、ACORM（使用一次聚类变体ACORM_oc）、DT2GS；
- 消融变体：CD³T w/o Diffusion（用MLP代替扩散模型）、CD³T w/o Subtask-based Attention（用QMIX式混合替代注意力混合）、CD³T (subtask=3/4/5)（不同聚类数）。

### 评价指标
- LBF：平均测试回报（Average Test Return）；
- SMAC：测试胜率（Test Win Rate）；
- 所有曲线为5个随机种子下的均值±标准差。

## 四、资源与算力

- **论文未明确说明**所用的GPU型号、数量、训练时长、参数量等算力信息；
- 仅在方法中提及“初始化扩散模型训练 50K timesteps”、聚类一次在 50K 处进行，以及为公平比较将ACORM改为仅一次聚类（ACORM_oc），但未给出具体硬件配置。

## 五、实验数量与充分性

### 实验数量
- LBF 2个场景 + SMAC 8个场景 + SMACv2（附录）+ 3组消融实验 + 子任务数量敏感性实验 + 可视化分析（子任务生成过程、动态子任务选择、动作空间缩减），实验总量比较充分。

### 充分性与客观性评价
- **优点**：覆盖了多个难度层次与多种任务类型，基线选择全面（含经典与最新方法），消融实验逐一验证每个核心组件；可视化分析（PCA聚类、子任务切换时序、动作空间维度对比）为理解方法提供了直观证据。
- **潜在不足**：
  - 将ACORM由逐时间步聚类改为仅一次聚类（ACORM_oc），虽出于效率与一致性考量，但可能弱化了ACORM本身的优势，存在对比公平性疑问；
  - 未报告推理/训练的计算成本对比，扩散模型的计算开销是否划算有待验证；
  - 未提供统计显著性检验，部分场景中与GoMARL等差距较小。

## 六、主要结论与发现

- CD³T在几乎所有测试场景上取得最优或最优级性能，尤其在SMAC的Hard和Super Hard地图上优势明显；
- 扩散模型生成的子任务表示明显优于MLP结构：在LBF上，CD³T优于基于角色（RODE）和分组（GoMARL）的方法；在SMAC上，替换MLP的消融变体性能大幅下降；
- 子任务注意力混合网络对信用分配至关重要，去除后性能下降；
- 子任务数量增加（3→5）有助于性能提升，5个子任务即可满足需求；
- 扩散模型能够捕捉动作语义中的结构规律（如corridor地图中同朝向移动动作的相似效应），自动形成有意义的子任务簇；
- CD³T能有效缩减智能体决策的动作空间（尤其在复杂场景中），提升决策效率和协调结构；
- 可视化显示CD³T可以在一个episode内动态切换子任务，实现“诱敌”“风筝”“集火”等协调策略。

## 七、优点

- **方法创新性**：将扩散模型引入MARL子任务分解，利用其多模态分布建模能力和条件生成能力获取更丰富、更可分的动作语义表示；
- **层次化设计完整**：高层（子任务选择）与低层（子任务策略）协同，兼顾任务分解的长期稳定性和短时灵活性；
- **信用分配设计合理**：多注意力头 + 子任务/动作语义表示 + 全局状态干预调整，较好的平衡了个体价值与联合价值的推理桥梁；
- **实验验证较扎实**：多基准、多难度场景、多基线对比、多角度消融、可视化分析齐全；
- **动态性与自适应性**：子任务分配随环境变化实时调整，不依赖专家知识；
- **动作空间缩减**：实际减少了平均可用动作数（如图7所示），利于扩展到大规模场景。

## 八、不足与局限

- **计算开销较大**：扩散模型的训练和采样需要额外计算资源，论文未报告总训练时间或显存开销，限制了实际部署的参考价值；
- **子任务数量需要预先指定**：g 作为超参数（3~5），论文虽给出了经验值，但缺少自适应确定子任务数量的机制；
- **基线公平性风险**：修改了ACORM（改为一次性聚类）以契合自身设置，可能影响对比结论的客观性；
- **性能优势的普适性有限**：在LBF上优势微小，在简单SMAC地图上与其他强基线（如GoMARL）差距不大，优势主要集中在复杂/超难场景；
- **理论基础薄弱**：对子任务的定义和分解的最优性未提供理论保证，聚类得到的子任务是否全局最优未做探讨；
- **应用范围限制**：实验集中于StarCraft和LBF等规范benchmark，未涉及真实机器人、自动驾驶等物理世界场景；
- **未报告鲁棒性分析**：对随机种子、聚类初始化、扩散迭代步数等的敏感性分析不足。

（完）
