---
title: Efficient Multi-agent Reinforcement Learning by Planning
title_zh: 通过规划实现高效多智能体强化学习
authors: "Qihan Liu, Jianing Ye, Xiaoteng Ma, Jun Yang, Bin Liang, Chongjie Zhang"
date: 2024-01-16
pdf: "https://openreview.net/pdf?id=CpnKq3UJwp"
tags: ["query:mcd"]
score: 8.0
evidence: 基于模型的多智能体强化学习结合规划提升样本效率
tldr: 针对现有MARL算法多为无模型方法、样本效率低的问题，提出将基于模型的规划方法引入多智能体强化学习，以提升样本效率。方法需要应对大规模联合动作空间带来的挑战，并利用智能体近乎独立的子问题来简化搜索。通过模型学习与规划搜索的结合，多智能体系统可以在有限数据下达到更优性能。
source: ICLR-2024-Accepted
selection_source: conference_retrieval
motivation: 现有MARL算法多为无模型方法，样本效率低，难以应对复杂大尺度决策任务。
method: 将MuZero等基于模型的规划方法扩展到多智能体系统，利用联合动作空间的近似独立性整合搜索与策略学习。
result: 在多种决策任务上验证了基于模型的规划能显著提升MARL样本效率。
conclusion: 基于模型的规划是提升多智能体强化学习样本效率的有效途径，可推广到大规模决策场景。
---

## Abstract
Multi-agent reinforcement learning (MARL) algorithms have accomplished remarkable breakthroughs in solving large-scale decision-making tasks. Nonetheless, most existing MARL algorithms are model-free, limiting sample efficiency and hindering their applicability in more challenging scenarios. In contrast, model-based reinforcement learning (MBRL), particularly algorithms integrating planning, such as MuZero, has demonstrated superhuman performance with limited data in many tasks. Hence, we aim to boost the sample efficiency of MARL by adopting model-based approaches. However, incorporating planning and search methods into multi-agent systems poses significant challenges. The expansive action space of multi-agent systems often necessitates leveraging the nearly-independent property of agents to accelerate learning. To tackle this issue, we propose the MAZero algorithm, which combines a centralized model with Monte Carlo Tree Search (MCTS) for policy search. We design an ingenious network structure to facilitate distributed execution and parameter sharing. To enhance search efficiency in deterministic environments with sizable action spaces, we introduce two novel techniques: Optimistic Search Lambda (OS($\lambda$)) and Advantage-Weighted Policy Optimization (AWPO). Extensive experiments on the SMAC benchmark demonstrate that MAZero outperforms model-free approaches in terms of sample efficiency and provides comparable or better performance than existing model-based methods in terms of both sample and computational efficiency.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义（研究动机和背景）
- 多智能体强化学习（MARL）在解决大规模决策任务上已取得显著突破。
- 然而，现有大多数 MARL 算法属于**无模型（model-free）**方法，样本效率低，难以应对更复杂、更具挑战性的实际场景。
- 相比之下，**基于模型的强化学习（MBRL）**，尤其是结合**规划（planning）**的算法（如 MuZero），在许多任务中能以有限数据达到超人类水平。
- 因此，本文的核心动机是：**将基于模型的规划方法引入多智能体系统，以大幅提升 MARL 的样本效率**。
- 但这一方向面临关键挑战：多智能体系统的联合动作空间巨大，直接应用单智能体规划方法不可行，需利用智能体之间的**近似独立性**来加速学习。

### 2. 论文提出的方法论
- 核心思想：将 MuZero 风格的集中式模型与蒙特卡洛树搜索（MCTS）结合，用于多智能体策略搜索。
- 提出算法：**MAZero**。
- 关键技术细节：
  - 设计了一种**巧妙的网络结构**，支持分布式执行（distributed execution）和**参数共享**，以应对多智能体规模扩展。
  - 使用**集中式模型（centralized model）**来预测状态转移、奖励等环境动态，辅助规划。
  - 引入两种新技巧以提升搜索效率：
    - **Optimistic Search Lambda (OS(λ))**：用于确定性环境且动作空间较大的场景，增强搜索的乐观性和效率。
    - **Advantage-Weighted Policy Optimization (AWPO)**：利用优势权重优化策略，使策略更新更高效。
- 算法流程大致为：通过集中式模型进行规划搜索（MCTS），结合策略优化和目标估计，在少量真实环境交互下获得高性能策略。

### 3. 实验设计
- 实验基准：使用 **SMAC（StarCraft Multi-Agent Challenge）** benchmark，这是多智能体强化学习领域常用的微操对战环境。
- 对比方法：
  - 与**无模型（model-free）**方法比较，验证样本效率的提升。
  - 与现有**基于模型（model-based）**方法比较，验证样本效率和计算效率的综合表现。
- 具体对比了哪些算法、使用了多少个地图、做了多少组不同配置的实验，摘要中未详细说明，仅提及“在 SMAC benchmark 上进行了大量实验（Extensive experiments）”。

### 4. 资源与算力
- 论文摘要中**未明确提及** GPU 型号、数量、训练时长、硬件配置或计算资源等信息。
- 因此，无法从当前提供的内容中获取具体的算力投入细节。如需了解，需查阅论文正文或附录。

### 5. 实验数量与充分性
- 摘要仅提到在 SMAC 上进行了**大量实验**，但没有列出具体的实验次数、地图数量或消融实验细节。
- 从摘要看，实验覆盖了：
  - MAZero 与无模型方法的样本效率对比；
  - MAZero 与基于模型方法的样本与计算效率对比；
  - 两项新技术的引入（OS(λ) 和 AWPO）可能包含消融验证，但摘要未明确说明。
- 整体而言，实验设计属于常见且合理的对比基准，但**公开信息不足以评估实验的完整性与统计显著性**。需阅读正文确认是否包含消融实验、多种随机种子、不同难度地图等。

### 6. 主要结论与发现
- MAZero 在样本效率上**显著优于无模型方法**。
- 与现有基于模型的方法相比，MAZero 在**样本效率和计算效率**上均表现更优或相当。
- 证明了**基于模型的规划（结合 MCTS）是提升多智能体强化学习样本效率的有效途径**，且有能力扩展到大规模决策场景。

### 7. 优点
- **方法新颖**：首次（或较早）将 MuZero 类规划算法系统性地扩展到多智能体场景，填补了该方向的研究空白。
- **针对性强**：明确应对多智能体联合动作空间过大的难题，利用“近似独立性”与参数共享设计网络结构，工程上可行。
- **技术创新**：提出的 OS(λ) 和 AWPO 对提高搜索与策略优化效率有直接贡献。
- **实验基准可靠**：SMAC 是 MARL 领域公认的标准测试平台，结果具有较高参考价值。

### 8. 不足与局限
- **实验范围有限**：目前仅基于 SMAC 环境，未涉及更广泛的连续控制、协作-竞争混合任务或真实世界场景。
- **信息不透明**：摘要中未提供训练资源、超参数设置、消融实验细节等，难以全面复现和评估。
- **算法复杂度**：基于模型的规划通常需要学习环境模型并执行搜索，计算开销可能仍然较高，尽管论文声称计算效率有优势，但具体开销未给出。
- **独立性假设的限制**：当智能体之间存在强耦合、强交互（非近似独立）时，方法的有效性可能下降。
- **应用风险**：基于模型的规划依赖所学模型的准确性，模型偏差可能影响最终性能。

（完）
