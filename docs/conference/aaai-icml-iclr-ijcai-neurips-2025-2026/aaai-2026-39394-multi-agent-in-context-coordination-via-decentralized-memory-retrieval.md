---
title: Multi-agent In-context Coordination via Decentralized Memory Retrieval
title_zh: 通过去中心化记忆检索实现多智能体上下文协调
authors: "Tao Jiang, Zichuan Lin, Lihe Li, Yi-Chen Li, Cong Guan, Lei Yuan, Zongzhang Zhang, Yang Yu, Deheng Ye"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39394/43355"
tags: ["query:mcd"]
score: 8.0
evidence: 基于去中心化记忆检索的上下文协调方法，面向合作式MARL
tldr: 在合作式多智能体强化学习中，去中心化策略部署常因任务对齐和奖励分配不一致而限制策略自适应效率。本文提出多智能体上下文协调方法，通过去中心化的记忆检索机制为智能体提供相关经验背景，使其无需更新参数即可快速适应新任务。该方法有望缓解分散部署带来的奖励错配问题，提升多智能体系统的少样本协调能力，并为大模型在MARL中的应用提供新路径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1593, \"height\": 527, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 417, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1611, \"height\": 894, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-39394/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 777, \"height\": 642, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39394/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 886, \"height\": 955, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-39394/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1684, \"height\": 533, \"label\": \"Table\"}]"
motivation: 合作式MARL去中心化部署会因任务对齐和奖励分配不一致而降低适应效率。
method: 引入去中心化记忆检索，利用上下文学习实现无需参数更新的快速协调。
result: 未见完整摘要，预计能提升多智能体少样本协调与奖励分配一致性。
conclusion: 记忆检索与上下文学习为可扩展的分散式多智能体协调提供了新方案。
---

## Abstract
Large transformer models, trained on diverse datasets, have demonstrated impressive few-shot performance on previously unseen tasks without requiring parameter updates. This capability has also been explored in Reinforcement Learning (RL), where agents interact with the environment to retrieve context and maximize cumulative rewards, showcasing strong adaptability in complex settings. However, in cooperative Multi-Agent Reinforcement Learning (MARL), where agents must coordinate toward a shared goal, decentralized policy deployment can lead to mismatches in task alignment and reward assignment, limiting the efficiency of policy adaptation. To address this challenge, we introduce Multi-agent In-context Coordination via Decentralized Memory Retrieval (MAICC), a novel approach designed to enhance coordination by fast adaptation. Our method involves training a centralized embedding model to capture fine-grained trajectory representations, followed by decentralized models that approximate the centralized one to obtain team-level task information. Based on the learned embeddings, relevant trajectories are retrieved as context, which, combined with the agents' current sub-trajectories, inform decision-making. During decentralized execution, we introduce a novel memory mechanism that effectively balances test-time online data with offline memory. Based on the constructed memory, we propose a hybrid utility score that incorporates both individual- and team-level returns, ensuring credit assignment across agents. Extensive experiments on cooperative MARL benchmarks, including Level-Based Foraging (LBF) and SMAC (v1/v2), show that MAICC enables faster adaptation to unseen tasks compared to existing methods.

---

## 论文详细总结（自动生成）

# 论文总结：Multi-agent In-context Coordination via Decentralized Memory Retrieval

## 1. 核心问题与整体含义

- **背景**：大型Transformer模型具备出色的少样本泛化能力，无需参数更新即可适应新任务。这一能力已被引入强化学习（RL），形成“上下文强化学习”（ICRL）范式，使智能体能够通过与环境交互并检索上下文来快速适应。
- **核心问题**：现有ICRL方法主要集中在单智能体场景，难以直接扩展到合作式多智能体强化学习（MARL）。原因是合作MARL需要多个智能体协同完成共享目标，而去中心化执行带来了两个关键挑战：
  1. **部分可观测性**：每个智能体只能看到局部观测，导致对整体任务特征的理解不完整或有偏；
  2. **信用分配困难**：智能体通常只获得团队奖励，无法准确评估自身贡献，容易引发“懒惰智能体”问题。
- **研究意义**：本文旨在探索一种能使去中心化多智能体团队无需参数更新即可快速适应未见过的合作任务的ICRL方法，填补该领域在去中心化部分可观测马尔可夫决策过程（Dec-POMDPs）中的空白。

## 2. 论文提出的方法论

- **总体框架**：提出 **MAICC**（Multi-agent In-context Coordination via Decentralized Memory Retrieval），包含三个主要部分：
  1. 多智能体轨迹嵌入模型；
  2. 基于检索的上下文决策训练；
  3. 去中心化上下文快速协调机制。

- **关键技术细节**：
  - **集中式嵌入模型（CEM）**：在训练时使用全局团队信息（观测、动作、事后信息 `P̂`）进行自回归建模，通过三个损失函数分别建模行为策略（Lμ）、奖励（L_R）和观测转移（L_T），实现细粒度的团队级轨迹表示，并隐含地进行信用分配。
  - **去中心化嵌入模型（DEMs）**：在测试时每个智能体只能使用局部信息，通过最小化与CEM输出的KL散度，将团队级知识蒸馏到DEMs中，增强其表示能力。
  - **轨迹检索**：利用DEM提取当前子轨迹的嵌入，通过最大内积搜索（MIPS）和余弦相似度从记忆库中检索最相关的top-k条历史轨迹，并与当前子轨迹拼接后输入决策模型（参数共享的因果Transformer）。
  - **记忆机制**：测试阶段构造“混合记忆”，以指数时间衰减系数 β_t = exp(−λ·t/T) 在离线多任务数据集与在线回放缓冲区间进行采样：早期偏向离线数据以促进探索，后期偏向在线数据以加强利用。
  - **混合效用分数**：综合全局回报 R 和预测个体回报 R̃，使用 S_util = α·norm(R) + (1−α)·norm(R̃)，其中个体回报由DEM从动作嵌入预测得到，以缓解信用分配问题。

- **算法流程**（文字说明）：
  1. 初始化CEM、DEM和决策模型πθ，以及离线数据集D和空在线缓冲B；
  2. 在离线数据集上联合训练CEM和DEM（Eq.1 和 Eq.5）；
  3. 固定嵌入模型，利用DEM检索上下文轨迹，训练决策模型πθ（Eq.6）；
  4. 测试阶段，对每个episode t，构造混合记忆B′，按混合效用分数+相似度检索，并进行去中心化执行，将经验存入B。

- **理论分析**：论文给出在线累积遗憾上界为 Õ(CH^(3/2)ω√AT)，与已有ICRL方法类似，并提供了理论保证。

## 3. 实验设计

- **数据集/场景**：
  - **Level-Based Foraging (LBF)**：两个配置——LBF: 7x7-15s 和 LBF: 9x9-20s；
  - **SMAC v1**：分为 Protoss、Terran、Zerg 三类任务；
  - **SMAC v2**：使用“all”设置，包含全部三类任务，随机性更强。
- **数据收集**：所有离线数据集由QMIX策略收集。
- **对比方法**：
  - **MADT**：多智能体决策Transformer，不支持在线适应；
  - **HiSSD**：从离线多任务数据学习通用技能，不支持在线适应；
  - **AT**（Agentic Transformer）：ICRL基线；
  - **RADT**：检索增强决策Transformer，单智能体ICRL基线；
  - **MAICC-S**：MAICC的消融版（不使用CEM，仅训练DEM）。
- **通用设置**：所有Transformer基线使用相同大小的GPT-2模型，保证公平性。

## 4. 资源与算力

- 论文正文与附录中**未明确说明**使用的GPU型号、数量、训练时长等计算资源信息。从可获取的材料看，作者未提供详细的硬件配置或训练成本数据。

## 5. 实验数量与充分性

- **主要实验**：在6个测试场景（LBF×2，SMACv1×3，SMACv2×1）上评估，每个模型用5个随机种子训练，每个种子测试10个随机任务，共50次测试运行，报告均值和95%置信区间。
- **消融实验**：在SMACv2: all上进行了4组变体测试：
  - 是否使用RTG token；
  - 不同β值（纯离线/纯在线 vs 指数衰减）；
  - 不同CEM损失组合（Lμ+L_R+L_T vs 子集）；
  - 不同α值（纯团队回报、纯个体回报 vs 混合效用）。
- **嵌入可视化**：对比了四种嵌入训练配置的t-SNE图，验证模型设计。
- **充分性评价**：实验覆盖了不同复杂度的场景和多种基线，消融设计较全面，统计上使用多种子和多任务测试，整体较为充分。但受限于任务类型（主要是收集类和战斗类），对更广泛真实世界场景的推广性仍需验证。

## 6. 主要结论与发现

- MAICC在全部6个测试场景中均显著优于基线方法，尤其在最复杂的SMACv2: all场景中优势最大，表明该方法在处理高任务多样性和随机性时更具潜力。
- 集中式CEM对DEM的蒸馏能有效增强去中心化执行中的轨迹表示能力；MAICC-S的性能下降证实了这一点。
- 去除RTG token、单独使用离线或在线记忆、缺失任一损失函数、仅用单一回报指标都会导致性能下降，说明各组件的必要性。
- 可视化显示，MAICC的嵌入模型能够形成清晰的任务聚类，而错误配置会导致重叠或过度紧凑的簇，影响检索效果。

## 7. 优点

- **方法创新性**：首个面向Dec-POMDPs的ICRL方法，将检索增强与上下文学习系统性地引入合作式多智能体场景。
- **设计完整性**：同时处理了部分可观测性和信用分配两大核心挑战，通过“集中式蒸馏+去中心化推理”模式符合CTDE范式，实用性强。
- **理论支持**：提供了在线遗憾界的理论分析，增强了方法的可信度。
- **实验扎实**：在多个标准基准、多种任务类型下进行广泛评估，消融实验清晰验证每个组件贡献。
- **可复现性**：提供代码仓库（github.com/LAMDA-RL/MAICC）。

## 8. 不足与局限

- **实验场景局限**：仅在LBF和SMAC系列（游戏类）上评估，缺乏更真实或更大规模的多Agent任务（如机器人、交通、物流等），泛化能力未知。
- **计算资源信息缺失**：未报告GPU型号、训练时间等，难以评估方法部署成本。
- **记忆机制简单**：使用固定的指数时间衰减系数β，不够自适应；作者也承认未来可引入不确定性度量来改进。
- **个体回报预测依赖模型**：混合效用分数中的个体回报是由DEM预测得到的，预测误差可能造成偏差，尤其在环境动态变化大时。
- **基线选择有限**：缺少与其他专门为多智能体设计的在线适应方法（如Meta-MARL）的对比。
- **理论假设较强**：如“检索充分性”假设在k较小或缓冲区信息冗余时是否成立仍需实践验证。

（完）
