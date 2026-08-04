---
title: "LLM-Navi: Navigating Motion Agents in Cluttered and Dynamic Environments via LLM reasoning"
title_zh: LLM-Navi：通过大语言模型推理在杂乱动态环境中导航运动智能体
authors: "Yubo Zhao, Qi Wu, Yifan Wang, Yu-Wing Tai, Chi-Keung Tang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=lA7V3EqWmA"
tags: ["query:maspd"]
score: 9.0
evidence: LLM-Navi支持多智能体导航、动态避障与闭环重规划
tldr: 现有大语言模型导航方法受限于简单静态环境，难以处理现实多智能体动态场景。LLM-Navi将真实地图、动态智能体及其轨迹统一编码为token，激发LLM内生的零样本空间推理能力，无需训练即可完成多智能体协同、动态避障与闭环重规划。实验表明其在不同智能体与任务间具有良好的泛化性，为基于LLM的自主导航提供了新思路。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 大语言模型在现实多智能体动态环境中的空间推理能力尚未被充分利用。
method: 将环境地图、动态智能体与轨迹编码为token，统一输入LLM进行零样本导航决策。
result: 支持多智能体协调、动态避障和闭环重规划，跨智能体与任务泛化良好。
conclusion: LLM具备的多智能体空间推理能力可被直接用于复杂动态导航场景。
---

## Abstract
We introduce **LLM-Navi**, a novel  large language model-based (LLMs) framework for autonomous navigation in dynamic and cluttered environments. Unlike prior studies constraining LLMs to simplistic, static settings with limited movement options, LLM-Navi enables robust spatial reasoning in realistic, multi-agent scenarios, achieved by uniformly encoding the environments (e.g., real-world floorplans), dynamic agents, and their trajectories as *tokens*. In doing so, we unlock the zero-shot spatial reasoning capabilities inherent in LLMs without requiring retraining or fine-tuning. LLM-Navi supports multi-agent coordination, dynamic obstacle avoidance, and closed-loop replanning, demonstrating generalization across diverse agents, tasks, and environments through text-based interactions. Our experiments show that LLMs can autonomously generate collision-free trajectories, adapt to dynamic changes, and resolve multi-agent conflicts in real time. We extend this framework to humanoid motion generation, showcasing its potential for real-world applications in robotics and human-robot interaction.  This work thus establishes a first foundation for integrating LLMs into embodied spatial reasoning tasks, offering a scalable and semantically grounded alternative to traditional methods.

---

## 论文详细总结（自动生成）

以下是基于论文元数据与摘要内容生成的中文详细总结。

> 说明：由于原始论文 PDF 正文无法直接获取（仅提供标题、摘要和元数据），以下总结主要基于摘要与元数据信息撰写；其中涉及实验细节、算力等无法确认的内容，均已明确指出为“未提供”或“无法评估”。

---

# LLM-Navi：通过大语言模型推理在杂乱动态环境中导航运动智能体 — 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：近年来，大语言模型（LLMs）在自然语言处理领域取得了显著突破，其内生的常识推理与语义理解能力也为机器人导航等具身智能任务带来了新可能。然而，已有将 LLM 用于导航的研究大多局限于**简单、静态的环境**，且运动选项有限（如只能在几个预设方向中选择），与真实世界复杂动态场景差距较大。
- **核心问题**：如何在**现实多智能体、动态且杂乱**的环境中，充分利用 LLM 的空间推理能力，实现无需训练即可自主导航？具体挑战包括：多智能体协同、动态障碍物规避、以及闭环重规划等。
- **整体意义**：该工作首次系统性地探索了 LLM 在**具身空间推理任务**中的应用，提出了一种基于统一 token 编码的通用框架，为将 LLM 集成到实际机器人导航与人机交互中奠定基础，是一种可扩展且具有语义依据的替代传统方法的方案。

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

- **核心思想**：将真实世界环境（如楼层平面图）、动态智能体及其轨迹**统一编码为 token**，输入到预训练 LLM 中，利用 LLM 内生的**零样本空间推理能力**进行导航决策，**无需微调或重新训练**。
- **关键技术细节**：
  - **统一 token 编码**：将环境地图、动态智能体状态、运动轨迹等异构信息映射到统一的 token 表示空间，使 LLM 能够直接理解并处理。
  - **零样本推理**：不更新模型参数，完全依靠预训练 LLM 的推理能力生成导航指令。
  - **动态障碍物感知**：通过编码实时感知到的动态智能体信息，使 LLM 可以预测并避开移动障碍物。
  - **多智能体协调**：多个智能体的状态与行为同时编码为 token，LLM 能识别潜在冲突并生成协调策略。
  - **闭环重规划**：在执行过程中不断接收环境反馈，若发生冲突或环境变化，则基于新的 token 序列重新生成轨迹。
- **算法流程（文字描述）**：
  1. **环境感知与状态采集**：获取环境地图、自身体位、动态智能体的当前状态与历史轨迹。
  2. **统一编码**：将上述信息映射为 token 序列。
  3. **LLM 推理**：将 token 序列输入预训练 LLM，生成当前时刻的导航决策（如目标方向、速度或下一步动作）。
  4. **执行与反馈**：执行决策，并实时监测环境变化；若检测到动态障碍物接近或与其他智能体产生冲突，则返回步骤 2 进行**闭环重规划**，直至到达目标。

## 3. 实验设计：数据集、场景、Benchmark 与对比方法

根据摘要与元数据，可以确认的实验设计信息如下：

- **数据集/场景**：
  - 使用**真实世界楼层平面图**（floorplans）构建导航环境。
  - 涉及**多智能体动态环境**，包含动态障碍物与其他运动智能体。
  - 扩展实验应用于**人形运动生成（humanoid motion generation）** 任务，展示在机器人领域的泛化潜力。
- **Benchmark**：论文中**未明确提及具体公开基准数据集名称**；tag 中的 “query:maspd” 可能指向某个多智能体/动态规划相关的评测任务，但缺乏更多信息确认。
- **对比方法**：摘要中**未列出具体对比方法**，但从研究背景推测，可能对比了传统路径规划算法（如 A*、DWA 等）以及已有基于 LLM 的静态导航方法。具体对比基线无法从现有材料确认。

## 4. 资源与算力

- **未提供具体信息**：在可获得的摘要与元数据中，**没有说明 GPU 型号、数量、训练时长或推理资源消耗**。
- 需要补充说明的是：由于该方法是零样本推理（无需训练），其算力需求主要集中在大规模预训练模型（如 GPT 系列）的**推理阶段**，实际部署成本取决于所选 LLM 的规模与推理优化程度。具体数值有待论文正文补充。

## 5. 实验数量与充分性

- **实验维度（从摘要可以看出）**：
  - 多智能体协调（解决多智能体冲突）
  - 动态障碍物规避
  - 闭环重规划能力
  - 跨智能体与跨任务泛化性
  - 人形运动生成等扩展应用
- **充分性评估**：
  - 实验设计覆盖了多个关键维度，**维度较全面**，初步证明了方法的有效性和泛化性。
  - 然而，由于缺少具体的实验数量（如场景个数、运行回合数、成功率等量化指标），**无法对实验充分性做出完整判断**。
  - 从可得信息看，实验基本相符“验证框架可行性”的目标，但**定量对比与消融实验细节缺失**，作为学术论文可能需要在正文中提供更严格的数据支撑。

## 6. 论文的主要结论与发现

- LLM 能够在**无需训练和微调**的情况下，通过统一 token 编码，完成杂乱动态环境下的自主导航任务。
- LLM 可以**自主生成无碰撞轨迹**，并**实时适应环境动态变化**（如新出现的障碍物）。
- LLM 具备**多智能体冲突解决能力**，能够生成协调策略以避免智能体之间的碰撞。
- LLM 的空间推理能力可以**跨智能体、跨任务、跨环境泛化**，不局限于特定场景。
- 该框架能够进一步扩展到**人形运动生成**等真实机器人应用，表明其在实际机器人与人类交互中的潜在价值。

## 7. 优点

- **零样本、无需训练**：直接利用预训练 LLM 的推理能力，极大降低了计算资源与数据标注成本，便于快速部署。
- **统一编码框架**：将环境、智能体、轨迹统一为 token，设计简洁且可扩展，适用于多种类型的场景和任务。
- **支持动态多智能体环境**：突破了以往 LLM 导航仅限静态简单环境的局限，更贴近真实应用需求。
- **闭环重规划能力**：使系统能够适应实时变化，增强了鲁棒性。
- **跨任务泛化**：从 2D 导航扩展到人形运动生成，展示了方法的通用性，为 LLM 在具身智能中的广泛应用提供了新思路。
- **语义可解释性**：基于文本交互，导航决策具有一定的语义可解释性，便于人类理解和干预。

## 8. 不足与局限

- **实验细节不充分**：摘要中未提供量化指标、具体数据集规模、成功率等关键数据，难以全面评估与复现。
- **算力信息缺失**：未说明 LLM 推理的延迟和资源开销，而实时性对导航应用至关重要。
- **实时性可能的瓶颈**：大规模 LLM 推理速度较慢，在要求毫秒级响应的动态环境中可能难以满足实时性需求。
- **安全性与可靠性问题**：LLM 基于统计推理，缺乏严格的安全保证机制；在无安全约束的情况下直接用于真实机器人导航可能存在碰撞风险。
- **感知依赖**：将真实世界地图和动态物体转换为 token 的过程需要有效的外部感知模块支撑，感知噪声或丢失会影响整体性能。
- **Token 长度扩展问题**：随着智能体数量和环境复杂度增加，token 序列可能急剧增长，超出 LLM 的上下文窗口限制，影响可扩展性。
- **对比与消融不足**：从现有材料看，未展示与其他方法的系统性对比和消融实验，削弱了说服力。

---

**（完）**
