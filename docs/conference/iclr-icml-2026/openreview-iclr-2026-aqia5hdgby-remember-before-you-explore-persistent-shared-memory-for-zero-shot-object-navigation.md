---
title: "Remember Before You Explore: Persistent Shared Memory for Zero-Shot Object Navigation"
title_zh: 探索之前先记忆：面向零样本物体导航的持久共享记忆
authors: "Zhonghua Yi, Xiaohan Zhang, Maochun Luo, Mingxia Chen, Qihao Peng, Kaiwei Wang, Sheng Yang"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=aQia5HdgBY"
tags: ["query:maspd"]
score: 8.0
evidence: 面向多智能体零样本物体导航的持久共享记忆机制
tldr: 传统零样本物体导航在任务间重置记忆，导致多智能体长期连续操作时重复探索。本文提出持久共享记忆机制，通过时间一致语义地图解耦场景记忆与任务特定信息，让单个或多智能体系统跨任务、跨智能体积累并复用语义知识。结果表明该方法能显著减少冗余探测，提升多智能体长期对象导航与协作探索效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 常规零样本导航重置记忆导致多智能体长期任务中重复探索。
method: 构建时间一致语义地图并引入持久共享记忆，实现跨任务跨智能体知识复用。
result: 在多智能体连续物体导航中减少冗余探索，提升效率。
conclusion: 共享持久记忆是实现长期多智能体导航的关键机制。
---

## Abstract
In practical applications like home robotics, a single agent over a long lifespan or a team of collaborating agents must perform a continuous stream of tasks in the same environment. However, conventional zero-shot object navigation (ZSON) paradigms, which reset memory after each task, are inherently non-collaborative and inefficient for such long-term operations as they lead to redundant exploration. 
To bridge this gap, we introduce a Persistent Shared Memory (PSM) mechanism that allows single or multi-agent systems to accumulate and reuse semantic knowledge across tasks and agents. Our approach builds an Temporally Consistent Semantic Map (TCSM), decoupling scene memory from task-specific information and maintaining semantic consistency via weighted confidence updates. On top of this memory, we design a beyond-line-of-sight (BLOS) navigation strategy that propagates stored semantics into nearby navigable areas and performs line-of-sight checks for waypoint selection, enabling reasoning about objects that are currently occluded or distant. Experiments on public benchmarks, including HM3D and MP3D, have shown that our framework avoids redundant scene re-exploration and achieves state-of-the-art performance. Our code will be made available upon acceptance.

---

## 论文详细总结（自动生成）

# 论文总结：探索之前先记忆——面向零样本物体导航的持久共享记忆（PSM）

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：在家庭机器人等实际应用场景中，单个机器人需要在长期运行中持续执行一系列任务，或多个协作机器人需要在同一环境中协同工作。
- **核心问题**：传统零样本物体导航（ZSON）范式在每次任务结束后会重置记忆，导致以下两大缺陷：
  - **非协作性**：多个智能体之间无法共享经验，各自重复探索。
  - **低效性**：在长期连续操作中，同一环境被反复重新探索，产生大量冗余动作。
- **研究意义**：解决该问题对于推动机器人从“单次任务执行”走向“长期自主生活/协作服务”具有重要意义。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**持久共享记忆（Persistent Shared Memory, PSM）**机制，使单智能体或多智能体系统能够跨任务、跨智能体积累并复用语义知识，从而避免重复探索。
- **关键技术细节**：
  - **时间一致语义地图（Temporally Consistent Semantic Map, TCSM）**：
    - 将**场景记忆**与**任务特定信息**解耦。
    - 通过**加权置信度更新**维护语义信息的时间一致性，使长期累积的记忆不会因任务切换而失效或产生冲突。
  - **超视距（Beyond-Line-of-Sight, BLOS）导航策略**：
    - 将存储的语义信息传播到附近可导航区域，使智能体能够“推理”当前被遮挡或距离较远的物体位置。
    - 通过**视线检测（line-of-sight checks）**进行路径点选择，确保导航路径的可行性与安全性。
  - **算法流程概要**：构建语义地图 → 跨任务持久化存储 → 基于BLOS策略进行语义引导的路径规划 → 执行导航并更新地图置信度。

## 3. 实验设计

- **数据集/Benchmark**：在公开基准上进行评估，包括：
  - **HM3D**（Habitat Matterport 3D）
  - **MP3D**（Matterport3D）
- **对比方法**：摘要未明确列出具体对比方法名称，但声称达到了**最先进水平（SOTA）**，说明与现有ZSON方法进行了比较。
- **测试设定**：涉及多智能体连续物体导航场景和单智能体长期任务场景。

## 4. 资源与算力

- **缺失信息**：论文摘要和元数据中**未提及**任何算力细节，包括：
  - GPU型号与数量
  - 训练时长
  - 参数量或显存占用
- **说明**：仅从现有材料无法获取实验的算力投入信息。

## 5. 实验数量与充分性评估

- **已知实验**：摘要仅提及在HM3D和MP3D两个数据集上的实验结果，以及多智能体连续导航场景下的表现。
- **不确定性**：由于材料仅为摘要级别，以下信息缺失：
  - 具体消融实验组数（如TCSM各组件、BLOS策略的独立贡献）
  - 单智能体 vs 多智能体的详细对比实验
  - 与各基线方法的定量对比表
- **评估**：实验覆盖了主要公开基准，方向和场景选择合理，但**仅凭摘要无法充分判断实验的完整性与公平性**，需依赖全文结果。

## 6. 主要结论与发现

- PSM机制能够有效避免冗余的场景重新探索。
- 在HM3D和MP3D基准上实现了SOTA性能。
- **关键结论**：共享的持久记忆是实现长期多智能体导航的关键机制。

## 7. 优点

- **问题切入点新颖**：从“长期连续任务”视角重新审视ZSON任务设定，贴合实际机器人部署需求。
- **方法设计有针对性**：
  - 场景记忆与任务信息解耦，解决了任务切换导致的记忆失效问题。
  - 加权置信度更新策略有助于保持长时记忆的语义一致性。
  - BLOS导航使智能体具备一定程度的“推理预判”能力，而非简单反应式探索。
- **应用场景扩展性强**：同时支持单智能体长期运行和多智能体协作，系统适用范围广。

## 8. 不足与局限

- **实验信息片面**：摘要未给出详细定量结果、消融实验和基线对比，难以深入验证各模块贡献。
- **算力缺失**：未报告训练/评估资源消耗，不利于复现对比。
- **潜在偏差风险**：
  - 仅验证了仿真环境（HM3D/MP3D），未涉及真实机器人平台。
  - 未讨论记忆长期累积可能带来的噪声累积、地图漂移或存储增长问题。
  - 未明确多智能体通信带宽和同步策略，实际部署可能受限于通信条件。
- **应用限制**：语义知识复用依赖预先定义的语义类别，对开放世界新物体的泛化能力未知。

（完）
