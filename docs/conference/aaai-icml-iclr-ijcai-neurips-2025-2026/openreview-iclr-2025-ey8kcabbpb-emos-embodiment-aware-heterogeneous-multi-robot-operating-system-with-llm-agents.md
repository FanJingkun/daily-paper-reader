---
title: "EMOS: Embodiment-aware Heterogeneous Multi-robot Operating System with LLM Agents"
title_zh: EMOS：感知具身差异的异构多机器人操作系统与LLM智能体
authors: "Junting Chen, Checheng Yu, Xunzhe Zhou, Tianqi Xu, Yao Mu, Mengkang Hu, Wenqi Shao, Yikai Wang, Guohao Li, Lin Shao"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=Ey8KcabBpB"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 面向异构机器人的具身感知LLM智能体协调合作
tldr: 异构多机器人系统需要根据每个机器人的物理组成而非预定义角色来分配职责。本文提出EMOS，一个感知具身差异的LLM多智能体框架，将智能体的能力与其物理形态绑定，并引入了Habitat基准。该框架旨在改进异构机器人间的有效协作，为语言模型驱动的多机器人协调提供了具身感知的新思路。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: LLM多智能体系统在机器人控制中未充分绑定智能体能力与物理形态，难以支持异构多机器人协作。
method: 提出EMOS框架，让每个智能体的能力来源于其具体机械构成，并构建Habitat基准测试验证协作效果。
result: 在异构机器人协作任务中展示了系统有效性，但抽象未给出具体指标。
conclusion: 强调具身感知对异构多机器人大规模协作的积极作用，为具身LLM多智能体系统奠定基础。
---

## Abstract
Heterogeneous multi-robot systems (HMRS) have emerged as a powerful ap-
proach for tackling complex tasks that single robots cannot manage alone. Current
large-language-model-based multi-agent systems (LLM-based MAS) have shown
success in areas like software development and operating systems, but applying
these systems to robot control presents unique challenges. In particular, the ca-
pabilities of each agent in a multi-robot system are inherently tied to the physical
composition of the robots, rather than predefined roles. To address this issue,
we introduce a novel multi-agent framework designed to enable effective collab-
oration among heterogeneous robots with varying embodiments and capabilities,
along with a new benchmark named Habitat-MAS. One of our key designs is
Robot Resume: Instead of adopting human-designed role play, we propose a self-
prompted approach, where agents comprehend robot URDF files and call robot
kinematics tools to generate descriptions of their physics capabilities to guide
their behavior in task planning and action execution. The Habitat-MAS bench-
mark is designed to assess how a multi-agent framework handles tasks that require
embodiment-aware reasoning, which includes 1) manipulation, 2) perception, 3)
navigation, and 4) comprehensive multi-floor object rearrangement. The experi-
mental results indicate that the robot’s resume and the hierarchical design of our
multi-agent system are essential for the effective operation of the heterogeneous
multi-robot system within this intricate problem context.

---

## 论文详细总结（自动生成）

# 论文总结：EMOS：感知具身差异的异构多机器人操作系统与LLM智能体

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：异构多机器人系统（HMRS）能够应对单个机器人无法完成的复杂任务，是机器人领域的重要方向。当前基于大语言模型的多智能体系统（LLM-based MAS）在软件开发和操作系统等领域已取得显著成功，但直接应用于机器人控制面临独特挑战。
- **核心问题**：在多机器人系统中，每个智能体的能力本质上由其物理组成（即“具身”）决定，而不是由预定义的角色决定。现有 LLM 多智能体系统往往将智能体与角色绑定，忽略了机器人物理形态对能力边界的影响，导致异构机器人难以高效协作。
- **整体含义**：需要一种“具身感知”（embodiment-aware）的智能体框架，使 LLM 智能体能够理解自身机器人的物理结构，并据此进行任务规划和动作执行，从而实现异构机器人之间的有效协作。

## 2. 提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：提出 EMOS 框架，将每个智能体的能力与其具体机器人的机械构成（embodiment）绑定，替代传统的人工设计角色扮演（human-designed role play）。
- **关键技术细节 — Robot Resume（机器人简历）**：
  - 采用**自提示（self-prompted）**方法，让智能体直接读取机器人的 **URDF 文件**（统一机器人描述格式）。
  - 调用**机器人运动学工具**，生成对自身物理能力的描述（如自由度、关节范围、末端执行器能力等）。
  - 这些描述作为“简历”引导智能体在任务规划和动作执行中的行为决策。
- **层次化多智能体系统设计**：采用分层架构（hierarchical design），将任务规划与执行分解到不同层级的智能体，以适配异构机器人的能力差异。
- **算法流程（文字说明）**：
  1. 输入任务目标与环境信息；
  2. 每个机器人智能体解析自身 URDF 文件，调用运动学工具计算可达空间、运动学约束等，生成“Robot Resume”；
  3. 基于 Resume 和任务上下文，高层级智能体进行任务分解与分配；
  4. 低层级智能体根据分配的原子任务，结合自身能力约束生成具体动作序列并执行；
  5. 多智能体协作完成整体任务。

## 3. 实验设计：数据集 / 场景 / Benchmark / 对比方法
- **Benchmark：Habitat-MAS**，用于评估多智能体框架处理“具身感知推理”任务的能力，包含四类任务：
  1. **操作（manipulation）**：如抓取、放置等需要机械臂能力的任务；
  2. **感知（perception）**：依赖传感器能力的目标识别或状态估计任务；
  3. **导航（navigation）**：依赖移动底盘能力的路径规划与到达任务；
  4. **综合多楼层物体重排（comprehensive multi-floor object rearrangement）**：需要多机器人跨楼层协作的复杂综合任务。
- **对比方法**：摘要中未明确列出具体基线方法，但实验结果验证了“Robot Resume”和层次化设计对系统有效性的必要性（可能包含消融实验，如去掉 Resume 或非层次化结构）。

## 4. 资源与算力
- **论文摘要未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 仅可推测使用了 Habitat 仿真环境，但具体计算资源、训练轮次、推理成本等均未在提供内容中提及。

## 5. 实验数量与充分性
- **实验数量**：摘要中仅笼统描述实验结果表明 Robot Resume 和层次化设计“至关重要”，没有给出具体实验组数、数值指标或对比表格。
- **充分性评估**：
  - 从任务类型看，覆盖操作、感知、导航和综合任务，场景较全面，初步验证了框架的泛化性。
  - 但缺少定量结果（如成功率、任务完成时间、资源开销等），也没有明确说明消融实验的严格程度和基线对比的公平性。
  - 因此，实验设计的**框架性较好，但报告不完整**，难以据此判断方法的性能优势和稳定性。

## 6. 主要结论与发现
- **Robot Resume 是关键**：让智能体基于 URDF 文件和运动学工具生成能力描述，能有效提升异构多机器人协作的任务规划与执行效果，优于人工设定角色。
- **层次化设计重要**：分层多智能体架构能够更好协调不同具身能力的机器人，在处理复杂多楼层重排等任务时表现更优。
- **具身感知是核心方向**：将 LLM 智能体的能力与物理形态绑定，是语言模型驱动多机器人系统迈向大规模实用的重要基础。

## 7. 优点（方法与实验设计亮点）
- **创新性**：首次（或相对少见地）将“机器人简历”作为智能体自提示机制，让 LLM 从 URDF 中主动学习自身物理能力，替代固定角色扮演，思路新颖且具有通用性。
- **层次化设计**：兼顾任务分解的全局性和动作执行的局部性，适合异构机器人协同。
- **系统化 Benchmark**：Habitat-MAS 涵盖感知、操作、导航和综合任务，为后续研究提供了统一的评估平台。
- **实际可行性**：基于标准 URDF 文件和运动学工具，易于迁移到真实机器人系统。

## 8. 不足与局限
- **实验报告不完整**：摘要中缺乏具体量化指标、误差棒、统计显著性检验，无法独立评估方法效果。
- **Benchmark 范围有限**：目前只提到 Habitat 仿真环境，未提及真实机器人实验，仿真到现实的迁移能力未验证。
- **对比基线缺失**：未说明与哪些现有 LLM 多智能体系统或传统 HMRS 方法进行对比，公平性难以确认。
- **可扩展性存疑**：当机器人数量增多或形态差异极大时，Robot Resume 的生成成本和层次化调度的复杂度可能成为瓶颈。
- **算力信息缺失**：未报告训练/推理资源，不利于复现和成本评估。

（完）
