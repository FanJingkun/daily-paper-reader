---
title: Conditional Diffusion Model for Multi-Agent Dynamic Task Decomposition
title_zh: 条件扩散模型用于多智能体动态任务分解
authors: "Yanda Zhu, Yuanyang Zhu, Daoyi Dong, Caihua Chen, Chunlin Chen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40158/44119"
tags: ["query:mcd"]
score: 9.0
evidence: 面向长视界协作任务分解的双层分层多智能体强化学习框架
tldr: 针对从零学习动态任务分解需要大量样本的问题，本文提出CD3T，一个基于条件扩散模型的两层分层MARL框架。高层策略学习子任务表征并生成子任务选择策略，低层策略据此协同决策。在部分可观测环境下，该方法自动推断子任务与协调模式，提升样本效率与长视界任务性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 动态不确定环境中，从零学习长视界协作任务分解需要大量训练样本，且联合动作空间巨大，本文拟解决该样本效率问题。
method: 提出条件扩散模型CD3T，构建高层子任务表征学习与低层策略协作的两层框架，自动推断子任务与协调模式。
result: 在复杂协作MARL任务中验证了CD3T能高效学习动态任务分解，提升长视界任务表现。
conclusion: CD3T为长视界部分可观测环境下的合作任务提供了一种层级式自动分解框架，有效改善学习效率与协同能力。
---

## Abstract
Task decomposition has shown promise in complex cooperative multi-agent reinforcement learning (MARL) tasks, which enables efficient hierarchical learning for long-horizon tasks in dynamic and uncertain environments. However, learning dynamic task decomposition from scratch generally requires a large number of training samples, especially exploring the large joint action space under partial observability. In this paper, we present the Conditional Diffusion Model for Dynamic Task Decomposition (CD3T), a novel two-level hierarchical MARL framework designed to automatically infer subtask and coordination patterns. The high-level policy learns subtask representation to generate a subtask selection strategy based on subtask effects. To capture the effects of subtasks on the environment, CD3T predicts the next observation and reward using a conditional diffusion model. At the low level, agents collaboratively learn and share specialized skills within their assigned subtasks. Moreover, the learned subtask representation is also used as additional semantic information in a multi-head attention mixing network to enhance value decomposition and provide an efficient reasoning bridge between individual and joint value functions. Experimental results on various benchmarks demonstrate that CD3T achieves better performance than existing baselines.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究动机

- 论文聚焦于**多智能体强化学习（MARL）中的复杂协作任务**，尤其是在部分可观测、长视界、动态不确定环境下，从零学习有效的协调策略非常困难。
- 现有CTDE（集中训练、分布执行）框架虽然缓解了部分可观测问题，但随着智能体数量增加，联合动作–观测空间呈指数级增长，导致有效状态探索稀疏、协调难度大。
- **任务分解**（将复杂任务拆分为子任务）被认为是一种有前景的手段，但以往方法往往依赖于简单的网络结构提取动作/角色表征，难以捕捉智能体之间以及智能体与环境之间的动态交互，表示能力不足，且从零学习分解需要大量训练样本。
- 本文提出利用**条件扩散模型**（Conditional Diffusion Model）的强表示能力与随机过程建模能力，解决动态任务分解中的表征学习、样本效率和子任务多样性问题。

## 2. 方法论：CD³T 框架

### 2.1 核心思想
- 提出一个**两层级分层MARL框架**，名为 **CD³T（Conditional Diffusion Model for Dynamic Task Decomposition）**。
- 高层策略：学习子任务表征，通过子任务选择器为每个智能体动态分配子任务；
- 低层策略：各智能体在分配到的子任务空间内协作学习并共享专用技能；
- 子任务表征同时被用于**多头注意力混合网络**，增强价值分解与信用分配。

### 2.2 关键技术细节

**（1）基于扩散模型的智能体动作语义表征**
- 将智能体 \(i\) 的 one-hot 动作 \(a_i\) 映射为 \(d\) 维表征 \(z_{a_i}\)，作为扩散模型的未加噪样本 \(z_0\)。
- 采用带交叉注意力的 UNet 主干作为去噪网络 \(\epsilon_{\theta_d}(z_k, k, o_i, a_{-i})\)，条件信息为智能体自身的局部观测 \(o_i\) 和其他智能体的联合动作 \(a_{-i}\)。
- 扩散损失（简化版 DDPM 目标）：
  \[
  L_d(\theta_d)=\mathbb{E}_{\epsilon\sim\mathcal{N}(0,I),(o,a)\sim D}\left[\left\|\epsilon-\epsilon_{\theta_d}(z_k,k,o_i,a_{-i})\right\|^2\right]
  \]
- 进一步利用动作表征预测下一观测和全局奖励，增强对“动作–环境效应”的建模：
  \[
  L_p(\theta)=\mathbb{E}_{(o,a,r,o')\sim D}\left[\sum_i\|f_{do}(z_{a_i},o_i,a_{-i})-o'_i\|_2^2 + \lambda_{dr}\left(f_{dr}(z_{a_i},o_i,a_{-i})-r\right)^2\right]
  \]
- 总损失为：\(L(\theta)=L_p(\theta)+\eta_d L_d(\theta_d)\)。

**（2）动态子任务分解**
- 在训练初期（前 50K 步）收集动作表征后，通过 **k-means 聚类**将动作空间划分为有限个子任务。
- 每个子任务 \(\phi_j\) 的表征 \(z_{\phi_j}\) 为该子任务动作空间内所有动作表征的平均值。
- 每隔 \(\Delta T\) 个时间步，子任务选择器根据智能体历史信息 \(h_\tau\) 与各子任务表征的内积计算 Q 值：
  \[
  Q_{\phi_i}(\tau_i,\phi_j)=z_{\tau_i}^\top z_{\phi_j}
  \]
  将 Q 值最大的子任务分配给该智能体。

**（3）信用分配与价值分解**
- 借鉴 IGM 原则，引入**基于干预的注意力调整函数**缓解全局状态与联合价值之间的虚假相关。
- 子任务选择器的信用权重：
  \[
  \lambda_{h,i}^{\phi}=\frac{\exp((w_{z_\phi}z_\phi)^\top \text{ReLU}(w_s s))}{\sum_{i=1}^N\exp((w_{z_\phi}z_\phi)^\top\text{ReLU}(w_s s))}
  \]
- 联合价值：
  \[
  Q_{\text{tot}}^{\Phi}=c^{\phi}(s)+\sum_{h=1}^{H}w_h^{\phi}\sum_{i=1}^{N}\lambda_{h,i}^{\phi}Q_{\phi_j^i}(\tau_i,\phi_j^i)
  \]
- 子任务策略层采用类似机制，但使用动作表征 \(z_a\) 而非子任务表征 \(z_\phi\)，得到 \(Q_{tot}\)，并分别用 TD 损失优化。

**（4）算法流程（文字描述）**
1. 预训练条件扩散模型，学习动作语义表征；
2. 在前 50K 时间步后，对动作表征聚类得到子任务集合；
3. 每 \(\Delta T\) 时间步，高层选择器为每个智能体分配子任务；
4. 低层子任务策略在受限动作空间内更新；
5. 训练过程中，混合网络结合全局状态、子任务/动作表征进行信用分配；
6. 执行时遵循 CTDE，只使用局部信息。

## 3. 实验设计

### 3.1 基准与数据集
- **Level-based Foraging (LBF)**：两个自定义场景（4-agents & 2-food，3-agents & 3-food）。
- **StarCraft Multi-Agent Challenge (SMAC)**：8个地图，覆盖 easy、hard、super hard（如 8m、2s3z、3s5z、5m_vs_6m、2c_vs_64zg、3s5z_vs_3s6z、6h_vs_8z、corridor）。
- **SMACv2**：额外补充实验（见附录）。

### 3.2 对比方法
- 经典价值分解方法：**VDN、QMIX、QTRAN、QPLEX**。
- 角色/子任务相关方法：**RODE、CDS**。
- 分组/角色最新方法：**GoMARL、ACORM**（实验中 ACORM 改为仅一次聚类，记为 ACORM_oc）、**DT2GS**。
- 消融变体：**CD³T w/o Diffusion**（替换为MLP）、**CD³T w/o Subtask-based Attention**（替换为QMIX方式）、**CD³T (subtask=3/4/5)**。

## 4. 资源与算力

- 文中**未明确说明**使用的 GPU 型号、数量、显存规模或具体训练时长，仅在算法流程中提到前 50K 时间步用于训练扩散模型和聚类。
- 实验采用 5 个随机种子报告均值±标准差，可推断有一定计算开销，但具体算力细节缺失。

## 5. 实验数量与充分性评估

- **实验组数较多**：
  - LBF：2个场景；
  - SMAC：8个地图；
  - SMACv2：附录中补充；
  - 消融实验：3种变体 + 不同子任务数（3/4/5）对比；
  - 可视化分析：子任务表征分布、子任务选择频率、动作空间缩减比较。
- **充分性总体较好**：覆盖了不同难度、不同任务类型，对比了多种主流与最新方法，并进行了组件消融。
- **存在一定局限性**：
  - SMAC 场景本身存在固有随机性较小的特点，结论能否推广到更开放的环境仍需验证；
  - 仅报告 win rate 或 return，缺少方差之外的统计显著性检验；
  - ACORM 被修改为一次聚类后对比（ACORM_oc），可能导致其性能下降，对 ACORM 不够公平；
  - 未提供算力消耗、训练时间等资源指标。

## 6. 主要结论与发现

- CD³T 在绝大多数场景下优于所有基线，尤其在 SMAC Hard 和 Super Hard 地图上优势明显。
- 扩散模型生成的动作表征能够**自然形成语义清晰的子任务簇**（如corridor场景中移动方向与攻击动作的区分），无需额外正则化即可实现行为多样性。
- 动态子任务选择模式合理，例如在走廊地图中，CD³T 会让个别智能体引诱敌人，其他智能体集火或风筝，展现出有意义的协调行为。
- 子任务表征在混合网络中的注意力机制有效缓解了全局状态与联合价值的虚假相关，改善了信用分配。
- 子任务数量增加（在有限动作空间内）通常提升性能，但需权衡计算开销。

## 7. 优点

- **方法创新性强**：将扩散模型引入分层 MARL 的子任务分解，利用其表示能力和随机过程建模优势，而非简单 MLP 或常规正则化。
- **表征可解释**：通过 PCA 可视化证明动作表征聚类结果符合任务结构，子任务分解具有语义意义。
- **架构完整**：高层选择器、低层策略、混合网络之间通过子任务/动作表征衔接，设计统一、逻辑清晰。
- **实验覆盖面广**：同时包含简单/困难/超困难地图，并加入了 SMACv2 验证泛化性。
- **可视化深入**：不仅展示学习曲线，还提供子任务动态选择、动作空间缩减对比等分析，增强了结论的可信度。

## 8. 不足与局限

- **算力开销未披露**：扩散模型训练需要较长时间，但论文没有给出 GPU 时间或内存消耗，复现成本评估困难。
- **部分基线处理可能不公**：ACORM 的对比版本是“仅一次聚类”，这可能大幅削弱其性能，不能完全代表原方法水平。
- **统计检验缺失**：仅用均值±标准差曲线，未进行显著性检验（如 t-test），某些差距是否显著不明。
- **子任务数量依赖超参**：聚类个数 \(g\) 需要人工设定，尽管做了敏感性实验，但自适应性有限。
- **理论上限未深究**：对于子任务分解的收敛性、最优性、与 IGM 的一致性虽有提及，但缺乏理论分析。
- **环境局限**：主要基于 SMAC 和 LBF，属于较为结构化的模拟环境，对更开放、真实世界多智能体任务的适用性仍需探索。

（完）
