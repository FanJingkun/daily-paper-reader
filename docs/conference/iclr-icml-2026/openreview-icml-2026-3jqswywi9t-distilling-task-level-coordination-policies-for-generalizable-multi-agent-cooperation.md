---
title: Distilling Task-Level Coordination Policies for Generalizable Multi-Agent Cooperation
title_zh: 蒸馏任务级协作策略以实现可泛化的多智能体合作
authors: "Zimo Zhai, Manjie Xu, Wei Liang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a2824b9ddbecf9a18ac11a681b21eb47373a72fb.pdf"
tags: ["query:mcd"]
score: 7.0
evidence: 将 LLM 的任务级协调策略蒸馏为轻量多智能体策略，实现泛化协作
tldr: LLM 虽具备强大推理能力，但直接作为多智能体协调器推理成本高，且低层控制策略难以涌现。SynCoord 提出自监督蒸馏流程，通过任务级工具接口约束 LLM 交互并收集数据，再将高层协调决策蒸馏为轻量智能体策略。实验表明蒸馏后的策略在未见过的任务上泛化良好，同时大幅降低推理开销，为多智能体协作提供了一条可扩展的实现路径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 直接部署 LLM 做多智能体协调成本高，且难以形成稳定的低层控制策略。
method: 定义任务级工具接口，使用 LLM 生成协调数据，并自监督蒸馏为轻量智能体策略。
result: 蒸馏策略在未知任务上表现良好，推理成本显著降低。
conclusion: 任务级协调蒸馏可高效构建可泛化、可扩展的多智能体协作策略。
---

## Abstract
Large language models have shown strong reasoning abilities and are increasingly explored as high-level coordinators for multi-agent systems. However, directly deploying LLMs for coordination remains challenging, as effective policies often fail to reliably emerge at the low-level control stage, and inference costs limit scalability. 
We propose SynCoord (Synthetic Coordination Distillation), a self-supervised pipeline that distills task-level decision-making for cooperation from high-capacity reasoning models into lightweight agent policies. Our approach does not rely on explicit supervision or handcrafted coordination rules. Instead, we define a set of task-level tool interfaces that constrain LLM interaction and enable the collection of interaction trajectories, which are then used to train compact coordinated policies. This distillation process transfers coordination behaviors that are difficult to elicit through prompting alone, while substantially reducing inference overhead at execution time.  
We evaluate our method on cooperative multi-agent benchmarks including Overcooked-AI and Level-Based Foraging (LBF), under varying team sizes and environment scales. Experimental results show that the distilled policies achieve success rates and execution efficiency comparable to reinforcement learning–based methods, while exhibiting fewer erroneous or redundant actions. Moreover, the learned task-level coordination policy generalizes effectively to unseen team compositions and larger layouts without retraining.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的详细中文总结。

## 论文总结：蒸馏任务级协作策略以实现可泛化的多智能体合作

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：如何高效地将大语言模型（LLM）的强推理能力转化为**多智能体系统**中可部署、可泛化的低层控制策略。
- **研究动机**：
  - **直接部署LLM作为协调器的三大挑战**：① 有效的协作策略难以在低层控制阶段稳定涌现；② 推理成本高昂，限制扩展性；③ 依赖显式监督或手工设计的协调规则。
  - 传统强化学习方法虽然能训练底层策略，但在任务级协调决策上往往缺乏推理能力，且难以泛化到新场景。
- **整体含义**：提出一种自监督的蒸馏管道——**SynCoord（Synthetic Coordination Distillation）**，将高容量推理模型的“任务级决策”蒸馏为轻量级智能体策略，在保持协作性能的同时大幅降低推理开销，并实现对新任务/新组合的泛化。

### 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：把“协调”从高层推理中剥离，通过定义**任务级工具接口（task-level tool interfaces）**来约束LLM交互，使其生成“可执行”的协调轨迹，再将这些轨迹作为监督信号，**自监督蒸馏**成紧凑的轻量策略。
- **关键技术细节**：
  - **任务级工具接口**：不要求LLM直接控制低层动作，而是定义一组高层工具/操作（如目标分配、区域指派、子任务选择等），让LLM在受限空间中输出协调决策。这既降低了LLM交互复杂度，又使生成数据对低层策略训练具有直接可用性。
  - **自监督蒸馏**：无需人工标注或预定义规则。LLM通过工具接口与环境交互，产生“协调轨迹”数据集，再训练轻量策略模仿这些高层协调决策。
  - **轻量策略**：蒸馏得到的策略不依赖LLM推理，执行时仅需本地前向计算，从而显著降低推理开销。
- **算法流程（文字描述）**：
  1. 定义任务级工具接口，屏蔽低层动作空间。
  2. 使用LLM（高容量推理模型）在受约束接口下与环境交互，收集协调轨迹。
  3. 将收集的轨迹作为训练数据，通过自监督方式蒸馏为轻量级智能体策略（如Transformer/MLP策略）。
  4. 执行时由轻量策略直接输出协调决策，LLM不再参与推理。

### 3. 实验设计：数据集 / 场景、Benchmark、对比方法

- **Benchmark与场景**：
  - **Overcooked-AI**：经典合作烹饪游戏，考验时序协作与分工（如传递食材、共享锅具）。
  - **Level-Based Foraging（LBF）**：基于等级的食物采集环境，要求智能体依据等级与目标位置进行任务分配。
  - 实验覆盖**不同团队规模**与**环境规模**，用于考察方法的稳定性与扩展性。
- **对比方法**：论文未明确列出对比基线的具体名称（从摘要看，主要与**基于强化学习（RL）的方法**进行对比），但重点比较了：
  - 与传统RL训练策略的**成功率**和**执行效率**；
  - 在“未见过的团队组成”和“更大布局”下的**泛化能力**；
  - 对**错误/冗余动作**数量的行为质量对比。
- **泛化测试**：直接测试蒸馏策略在未参与训练的团队规模和更大布局上的表现，无需重新训练。

### 4. 资源与算力

- 论文正文（Abstract）**没有明确提及**使用的GPU型号、数量、训练时长等算力信息。
- 由于本文来自OpenReview的PDF元数据（未提供完整正文），无法从可用材料中提取训练资源细节。需要说明：该论文在资源开销方面只报告了**推理阶段的成本优势**（显著低于直接使用LLM），但训练成本未披露。

### 5. 实验数量与充分性

- **实验数量**：两组Benchmark（Overcooked-AI与LBF），各含不同团队规模/环境规模的变体。总体实验数量有限，未在可用材料中看到消融实验（如去掉工具接口、不同蒸馏模型规模对比等）。
- **充分性与客观性评估**：
  - **优势**：泛化测试设计合理（未见团队/更大布局），对比RL方法具有明确说服力。
  - **不足**：缺乏消融实验、缺乏与直接LLM协调器（如ReAct + LLM）的推理成本量化对比，以及蒸馏策略在不同LLM（如GPT-4 vs. 开源模型）间的稳定性验证。
  - 总体而言，初步验证了方法有效性，但实验覆盖范围有限，尚不足以完全确立普适性。

### 6. 主要结论与发现

- **性能相当**：蒸馏策略在成功率与执行效率上接近基于RL的方法。
- **行为更优**：相比RL方法，蒸馏策略表现出**更少的错误或冗余动作**，行为质量更高。
- **泛化能力强**：任务级协调策略无需重新训练即可泛化到**未见过的团队组成**和**更大的布局**。
- **成本降低**：执行时不再需要LLM在线推理，推理开销大幅下降，使方法具备可扩展性。

### 7. 优点（方法与实验亮点）

- **自监督流程**：不依赖显式监督或手工协调规则，数据完全由LLM与受限接口交互产生，可扩展性好。
- **任务级接口设计**：巧妙地将“抽象决策”与“低层控制”解耦，既发挥了LLM推理能力，又避免了低层策略难涌现的难题。
- **推理成本大幅下降**：蒸馏后仅需轻量前向计算，使多智能体系统可以大规模部署。
- **泛化验证充分**：泛化实验针对团队规模和布局大小，贴近实际应用需求。
- **行为质量维度**：不仅报告成功率，还关注“错误/冗余动作”，评估更全面。

### 8. 不足与局限

- **实验范围有限**：仅两个Benchmark（Overcooked-AI与LBF），场景相对简单，缺乏更复杂（如部分可观测、对抗性、异构动作空间）环境验证。
- **算力与开销数据缺失**：未提供训练阶段算力投入，也未量化对比直接使用LLM的具体推理成本差异。
- **消融实验不足**：未报告工具接口设计、蒸馏策略容量、LLM型号选择等因素对性能的影响，无法判断各组件贡献。
- **对比基线单一**：仅与RL方法对比，未与直接LLM协调器、分层RL或模仿学习方法进行系统比较。
- **应用限制**：任务级工具接口的设定需要为每类任务特殊设计，跨任务类型的通用性尚未验证；蒸馏策略的泛化边界（到何种复杂程度失效）也未探索。
- **可信度提示**：该论文标注为“ICML-2026-Accepted”，但以当前实际年份来看（若无特别语境）这一信息在时间上显得异常，需谨慎对待其真实发表状态。

（完）
