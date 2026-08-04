---
title: "Mosaic: Runtime-Efficient Multi-Agent Embodied Planning"
title_zh: Mosaic：运行时高效的多智能体具身规划
authors: "Kunjal Panchal, Saayan Mitra, Sunav Choudhary, Victor Bursztyn, Somdeb Sarkhel, Hui Guan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f7fafa43658ea324e0d63f842d3dc878e83cbfd3.pdf"
tags: ["query:maspd"]
score: 8.0
evidence: 运行时高效的多智能体具身规划，结合空间语义记忆与整数规划动作分配
tldr: 基于 LLM 的多智能体具身规划常因状态追踪不准确和协作冗余产生大量失败动作，导致延迟过高。Mosaic 通过智能体中心的语义记忆以相对坐标存储物体，支持几何变换与协同，并利用整数线性规划在每步分配动作以保证物理可行性和执行效率。实验显示 Mosaic 显著降低了运行延迟并提升了协作任务完成率，使多智能体具身规划更加实用。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM 多智能体具身规划延迟高，主要来自部分可观测下的状态追踪不准与协作冗余。
method: 用相对坐标构建智能体中心语义记忆，并通过整数线性规划分配动作，约束物理可行性。
result: 在具身规划基准上大幅降低延迟，提升协作完成率。
conclusion: 语义记忆与整数规划结合可显著提升多智能体具身规划的效率与可扩展性。
---

## Abstract
LLM-based multi-agent embodied planning remains impractical due to prohibitively high execution latency. We identify failed actions as the dominant bottleneck, stemming from two core challenges: inaccurate state tracking under partial observability and inefficient coordination that produces redundant or conflicting actions. We introduce Mosaic, a runtime-efficient multi-agent planning framework that addresses both challenges. Mosaic maintains accurate yet lightweight state tracking through agent-centric semantic memory that stores objects in relative coordinates, enabling geometric transformations and coordination. It ensures efficient coordination through Integer Linear Programming that allocates actions at every planning step, enforcing physical feasibility and inter-agent coordination constraints. Across AI2-THOR and search-and-rescue benchmarks, Mosaic achieves 27–32% faster execution, 30–33% fewer LLM calls, 25–31% fewer steps, and 4–10% points higher success rates. These results demonstrate that efficient memory and constraint-guided coordination are critical for scalable, low-latency multi-agent planning.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：基于 LLM 的多智能体具身规划（multi-agent embodied planning）在真实场景中应用受限，主要瓶颈是**执行延迟过高**，导致系统不实用。
- **核心问题**：论文识别出**失败动作（failed actions）**是延迟的主要来源。失败动作由两大挑战引发：
  - **部分可观测性下的状态追踪不准确**：智能体难以准确感知和维护环境状态，导致决策错误。
  - **协作低效**：多个智能体之间容易产生**冗余或冲突动作**，浪费执行时间与 LLM 调用。
- **整体含义**：该论文旨在通过高效的记忆机制和约束引导的协调方法，显著降低多智能体具身规划的运行时延迟，使其更接近实际可用。

### 2. 论文提出的方法论

- **核心思想**：将**智能体中心语义记忆**与**整数线性规划（Integer Linear Programming, ILP）**相结合，分别解决状态追踪不准确和协作低效问题。
- **关键技术与流程**：
  - **智能体中心语义记忆（Agent-Centric Semantic Memory）**：
    - 以**相对坐标**存储物体位置，而非全局绝对坐标。
    - 这种表示允许智能体进行**几何变换**，并在多智能体之间共享和协调空间信息。
    - 优点在于**轻量**且能够保持对环境的准确追踪，同时适配部分可观测场景。
  - **整数线性规划动作分配**：
    - 在每个规划步骤中，使用 ILP 为所有智能体分配动作。
    - 在分配时**显式施动物理可行性约束**（如避免碰撞）和**智能体间协调约束**（如避免冗余或冲突）。
    - 通过约束求解，保证每步动作不仅可行，而且整体协作效率最优，从而减少失败动作和多余步骤。

### 3. 实验设计

- **数据集/场景**：
  - **AI2-THOR**：经典的具身智能交互模拟环境，用于验证日常室内任务的执行。
  - **搜索与救援（Search-and-Rescue）** 基准：多智能体协作搜索任务，强调稀疏信息与协作。
- **Benchmark**：两个具身多智能体规划基准。
- **对比方法**：论文未在摘要中明确列出具体基线方法名称，但通过与默认 LLM 多智能体规划方案对比，展示了在延迟、LLM 调用次数、步数和成功率等方面的相对提升。

### 4. 资源与算力

- 论文摘要中**未明确说明**所使用的 GPU 型号、数量、训练或推理时长等算力信息。
- 需指出：由于该论文面向推理阶段的运行时效率，可能更侧重推理开销，而非训练开销，但元数据与摘要均未提供具体硬件配置。

### 5. 实验数量与充分性

- 从摘要可见的实验指标包括：
  - **执行速度**：提升 27–32%。
  - **LLM 调用次数**：减少 30–33%。
  - **执行步数**：减少 25–31%。
  - **任务成功率**：提升 4–10 个百分点。
- 覆盖了两个不同的多智能体场景基准（AI2-THOR 和搜索救援），但摘要中**未提及消融实验**（例如单独验证语义记忆或 ILP 的贡献）或更细致的多任务、多场景分析。
- 从所见信息来看，实验**能初步证明方法有效性**，但**充分性有限**——缺少消融、基线细节、统计显著性讨论等，因此在完整论文中需进一步核实。

### 6. 论文的主要结论与发现

- Mosaic 在多个指标上显著优于现有方案：
  - **执行延迟降低** 27–32%。
  - **LLM 调用次数减少** 30–33%。
  - **执行步数减少** 25–31%。
  - **成功率提升** 4–10 个百分点。
- 结论：**高效记忆机制**与**约束引导的协调策略**是构建可扩展、低延迟多智能体具身规划系统的关键。

### 7. 优点

- **针对性明确**：精准定位失败动作这一延迟主因，并分别从记忆与协作两个角度提出对策。
- **设计思路清晰**：用相对坐标语义记忆解决部分可观测问题，用 ILP 保证物理可行性与协作无冲突，理论动机合理。
- **运行时高效**：不仅减少步数，还大幅降低 LLM 调用次数，这对实际部署非常重要。
- **通用性较强**：在模拟具身环境（AI2-THOR）和任务型搜索场景上都取得了提升，说明方法对场景类型有一定泛化能力。

### 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量、运行时间等资源信息，难以评估实际部署成本。
- **实验细节不充分**：摘要中未给出消融实验、基线方法列表、任务难度分布、统计方差等信息，无法完整判断实验公平性与稳健性。
- **应用限制**：论文主要在模拟环境中验证，未涉及真实机器人系统的噪声、延迟和不确定性。
- **潜在偏差风险**：相对坐标记忆可能存在累积误差；ILP 求解在大规模智能体数量下可能成为新的计算瓶颈；对于动态环境或完全未知环境，其适应性尚不明确。

（完）
