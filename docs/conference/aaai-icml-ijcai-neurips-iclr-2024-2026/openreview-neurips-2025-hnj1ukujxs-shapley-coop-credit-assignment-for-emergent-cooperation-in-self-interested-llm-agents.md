---
title: "Shapley-Coop: Credit Assignment for Emergent Cooperation in Self-Interested LLM Agents"
title_zh: Shapley-Coop：自利LLM智能体涌现合作中的信用分配
authors: "Yun Hua, Haosheng Chen, Shiqin Wang, Wenhao Li, Xiangfeng Wang, Jun Luo"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=HnJ1UkuJXS"
tags: ["query:mcd"]
score: 9.0
evidence: 面向自利大语言模型智能体涌现合作中的信用分配方法
tldr: 针对自利LLM智能体在开放环境中缺乏协调规则、易陷入社会困境的问题，提出Shapley-Coop协作流程。该方法借鉴人类临时合作的模式，通过Shapley值进行信用分配，激励智能体选择集体最优行为。实验表明该方法能促进自利智能体间的有效合作，提升整体任务表现。这为基于大规模语言模型的多智能体协作中的信用分配提供了可行方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM智能体在开放环境中因激励错配常陷入社会困境，缺少显式协调准则，集体效率低下。
method: 提出Shapley-Coop协作流程，借鉴人类临时合作方式，利用Shapley值对自利智能体进行信用分配以促进合作涌现。
result: 在需涌现合作的任务中，该方法缓解了社会困境，提升了集体产出。
conclusion: 信用分配机制可有效协调自利智能体，为开放式多智能体系统提供新思路。
---

## Abstract
Large Language Models (LLMs) are increasingly deployed as autonomous agents in multi-agent systems, and promising coordination has been demonstrated in handling complex tasks under predefined roles and scripted workflows.
However, significant challenges remain in open-ended environments, where agents are inherently self-interested and explicit coordination guidelines are absent. 
In such scenarios, misaligned incentives frequently lead to social dilemmas and inefficient collective outcomes.
Inspired by how human societies tackle similar coordination challenges—through temporary collaborations like employment or subcontracting—a cooperative workflow \textbf{Shapley-Coop} is proposed. 
This workflow enables self-interested Large Language Model (LLM) agents to engage in emergent collaboration by using a fair credit allocation mechanism to ensure each agent’s contributions are appropriately recognized and rewarded.
Shapley-Coop introduces structured negotiation protocols and Shapley-inspired reasoning to estimate agents’ marginal contributions, thereby enabling effective task-time coordination and equitable post-task outcome redistribution. 
This results in effective coordination that fosters collaboration while preserving agent autonomy, through a rational pricing mechanism that encourages cooperative behavior.
Evaluated in two multi-agent games and a software engineering simulation, Shapley-Coop consistently enhances LLM agent collaboration and facilitates equitable outcome redistribution, accurately reflecting individual contributions during the task execution process.

---

## 论文详细总结（自动生成）

## Shapley-Coop：自利LLM智能体涌现合作中的信用分配

### 1. 核心问题与研究动机

- **背景**：大型语言模型（LLM）越来越多地被部署为多智能体系统中的自主智能体。在预定义角色和脚本化工作流下，LLM智能体已经展现出良好的协调能力。
- **核心问题**：在开放环境中，智能体天然是自利的（self-interested），且缺乏显式协调准则。此时，激励错配（misaligned incentives）经常导致**社会困境（social dilemmas）**和低效的集体产出。
- **研究意义**：该问题触及多智能体系统（MAS）领域的基础性挑战——如何让理性自利的个体自愿选择合作，而非依赖外部强制或预设脚本。这对于将LLM智能体从封闭脚本场景推广到真实开放世界应用至关重要。

### 2. 方法论

**核心思想**：借鉴人类社会应对协调挑战的方式——如雇佣、分包等**临时合作（temporary collaborations）**模式，为自利LLM智能体设计一套协作流程。

**方法细节**：

- **Shapley-Coop整体流程**：提出一种合作工作流，使自利LLM智能体在协作过程中自发、涌现地进行合作，而非靠系统预设合作规则。
- **公平信用分配机制**：利用**Shapley值**（合作博弈论中的经典解概念，用于按边际贡献分配总收益）判断每个智能体的贡献，并对其进行合理回报，从而激励合作。
- **结构化协商协议**：引入任务执行过程中的协商机制，使智能体在任务期间能有效协调。
- **Shapley启发的推理过程**：通过估算各智能体的边际贡献，实现两类时间尺度上的协调——任务执行期间的实时协调（task-time coordination）与任务结束后的成果再分配（post-task outcome redistribution）。
- **理性定价机制**：通过合理的经济激励使得合作行为成为理性自利智能体的最优策略，从而在保持智能体自主性的前提下促进协作。

### 3. 实验设计

- **测试场景**：包括两个多智能体游戏和一个软件工程仿真环境。论文中提到具体场景包括需要涌现合作的任务。
- **评估方式**：对比Shapley-Coop与其他方法（基线方法）在任务表现上的差异，重点考察合作涌现情况和集体产出。
- **评测指标**：协作水平的提升程度、成果再分配的公平性（是否准确反映任务执行期间各智能体的实际贡献）。
- **整体结果**：在所有评估场景中，Shapley-Coop一致地提升了LLM智能体的协作表现，并实现了公平的成果分配。

> 注：提取到的文本中仅给出了场景类型概述（游戏、仿真），没有具体列出数据集名称和基线方法的具体名称（如占优策略均衡/惩罚机制等具体对照）。

### 4. 资源与算力

- 提取的文本中**未明确说明**实验所用GPU型号、数量和训练/推理时长。
- 若需详细了解算力配置，需查阅论文正文或附录中的实验设置部分。

### 5. 实验数量与充分性

- **实验数量**：从摘要与元数据来看，使用了**三类场景**（两个多智能体游戏＋一个软件工程仿真）进行验证，但提取文本中没有提到消融实验的具体情况。
- **充分性评估**：
  - 三类场景覆盖了博弈论与真实世界模拟，有一定场景多样性，能初步验证方法的普适性。
  - 但未提及消融实验（如移除Shapley分配、替换为其他信用分配方法等），因此**实验的系统性与可控性尚不够充分**。
  - 文中未说明基线方法的具体选择和数量，公平性细节需要进一步考证。

### 6. 主要结论与发现

- Shapley-Coop能有效**缓解自利智能体间的社会困境**，提升集体产出。
- 信用分配机制（以Shapley值为核心）能够**准确反映各智能体的贡献**，实现公平的成果再分配。
- 该方法在**保持智能体自主性**的前提下，通过经济激励驱动合作涌现，为开放式多智能体系统提供了一条可行的路径。

### 7. 优点

- **理论根基扎实**：Shapley值是合作博弈论中公认的公平分配解概念，理论上能唯一满足公平性公理（对称性、效率、可加性、虚拟性），使信用分配具有公理化保证。
- **思想启发性强**：将人类社会的临时合作制度（雇佣、分包）映射到LLM智能体协作场景，跨学科视角新颖。
- **契合自利智能体假设**：方法不强迫智能体合作，而是通过理性定价机制使合作成为自利个体的最优选择，契合开放环境中智能体的真实行为假设。
- **时序覆盖完整**：同时处理任务执行中的实时协调和任务完成后的成果再分配，覆盖了合作的全生命周期。
- **被NeurIPS 2025接收且评分较高（9.0分）**，表明评审专家对其创新性和价值持肯定态度。

### 8. 不足与局限

- **实验覆盖有限**：仅三个场景，缺少更大规模、更多样化的验证（如更复杂的社会困境、多轮谈判场景），说明文本中的benchmark信息尚不够充实。另外，推理过程中具体使用哪个模型也需查看正文，当前提取内容未阐明。
- **消融/敏感性分析不足**：提取的信息中未提及关于参数、谈判轮数、智能体数量等消融实验，对机制设计和各模块的独立贡献缺乏偏倚控制。
- **商业现实：尚未看到算力开销等成本分析**，这使得方法能否规模化和落入真实成本预算存疑。
- **实际应用挑战**：基于Shapley的边际贡献计算通常需要多名代理参与多次联合推演，在多代理系统规模较大时，计算成本可能很高，需做近似或采样处理。
- **摘要信息有限**：由于受PDF验证页面限制，正文细节（公式、算法伪代码、具体baseline名称与数值结果）均无法获取，以上局限评估基于摘要层面。

### 9. 总体评价

Shapley-Coop提出了一种将公平信用分配引入自利LLM智能体协作的创新思路。其价值在于不依赖人为脚本化的协作规则，而是通过经济学方式让合作"自然涌现"，为LLM多智能体系统的开放场景应用提供了重要参考。但方法的普适性、消融验证和计算效率仍有待更全面的实验证据。

（完）
