---
title: "CoNavBench: Collaborative Long-Horizon Vision-Language Navigation Benchmark"
title_zh: CoNavBench：协作长视野视觉-语言导航基准
authors: "Tianhang Wang, Xinhai Li, Fan Lu, Tianshi Gong, Jiankun Dong, Weiyi Xue, Sanqing Qu, Chenjia Bai, Guang Chen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=bMrH2PFMsi"
tags: ["query:maspd"]
score: 8.0
evidence: 协作长视野视觉-语言导航，处理拥塞与交接等挑战
tldr: 现有视觉语言导航以单智能体为中心，忽视了协作中的拥塞、交接错误和会合时机。本文提出CoNavBench基准，包含4048个单智能体与协作长时程导航任务，引入协作带来的额外挑战。该基准填补了多智能体协作导航评估空白，为研究并行化与角色分工提供平台。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 单智能体VLN基准忽视多智能体协作中的拥塞、交接和会合等关键问题。
method: 构建CoNavBench，包含单智能体和协作长时程导航任务，覆盖拥塞、交接、会合等挑战。
result: 基准包含4048个任务，可评估多智能体协作导航性能，揭示单智能体方法在协作设置下的不足。
conclusion: CoNavBench为协作长时程导航提供评测基准，推动多智能体VLN研究。
---

## Abstract
Vision-and-Language Navigation (VLN) primarily focuses on a single-agent-centric approach that executes human instructions step-by-step. In real environments with high demand or parallel workflows, collaboration VLN offers distinct benefits including shorter makespan and greater robustness through parallelism and role specialization. Collaboration VLN also brings new challenges including congestion, handoff errors, and rendezvous timing, which single-agent formulations overlook. Current datasets and protocols remain single-agent centered, which hides opportunities for assistance and ignores inter-robot interference. We fill this gap with Collaborative Long-Horizon VLN benchmark (\textbf{CoNavBench}), consisting of 4048 single and collaborative episodes with graph-level annotations and a collaboration type taxonomy that controls handoff styles and rendezvous patterns. To generate and evaluate at scale, we build \textbf{NavCraft}, an automated graph-grounded data generation platform. A two-stage hierarchical agent first produces a long-horizon base mission for the primary robot and then instantiates helper robots, allocates subgoals, and specifies validated handoffs and rendezvous. The agents operate with a scene graph in the loop derived from Habitat-Sim, which enables reachability checks, travel time, and interference assessment, and iterative schedule repair via an efficiency tool library. As a reference, we provide a collaborative baseline based on a finetuned Qwen2.5-VL-3B. Trained with CoNavBench, collaborative policies reduce makespan and improve reliability over strong single robot counterparts, yielding \textbf{18.11\%} step level success. Anonymous Website: https://navcraft.github.io.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- 现有Vision-and-Language Navigation (VLN) 研究以**单智能体为中心**，强调单个机器人按人类指令逐步执行。
- 在真实高需求或并行工作流场景中，**协作VLN**能带来更短的完成任务时间（makespan）和更强的鲁棒性，通过并行化和角色分工实现。
- 但协作VLN引入了**拥塞、交接错误、会合时机**等新挑战，这些是单智能体框架未考虑的。
- 当前数据集和协议仍然以单智能体为基础，既掩盖了“互助”的可能性，也忽略了机器人之间的相互干扰。
- 论文提出 **CoNavBench**，填补了多智能体协作长时程导航评估的空白。

## 2. 方法论

- **核心思想**：构建一个可规模化生成与评估协作VLN任务的基准平台，在任务生成中显式建模协作结构（交接、会合、拥塞控制）。
- 关键组成：
  - **CoNavBench基准**：包含4048个单智能体与协作长时程导航episodes，带有图级标注，并提供**协作类型分类法**来控制交接风格和会合模式。
  - **NavCraft**：自动化的图接地数据生成平台，支持大规模生成与评估。
- 生成流程（两阶段层次智能体）：
  1. 第一阶段：为主机器人生成一个长时程基础任务（mission）。
  2. 第二阶段：实例化辅助机器人、分配子目标，并指定经过验证的交接（handoffs）和会合（rendezvous）策略。
- 运行时机制：
  - 智能体在Habitat-Sim派生的**场景图**（scene graph）内操作，支持可达性检查、旅行时间计算和干扰评估。
  - 通过**效率工具库**进行迭代式调度修复（schedule repair）。
- 参考基线：基于微调的 **Qwen2.5-VL-3B** 构建协作策略。

## 3. 实验设计

- **基准/数据集**：CoNavBench，包含4048个任务（单智能体+协作），场景基于Habitat-Sim，提供图级标注。
- **对比方法**：摘要中提到“strong single robot counterparts”，即强单机器人方法作为对比，未列出具体方法名称。
- **主要评估指标**：step-level success（步骤级成功率）和makespan（任务完成时间），以及可靠性。
- **实验结果**：在CoNavBench上训练的协作策略相比强单机器人方法，**makespan更短、可靠性更高**，step-level success达到 **18.11%**。

## 4. 资源与算力

- 原文摘要**未明确说明**使用的GPU型号、数量、训练时长或整体算力消耗。
- 仅提及使用的模型为Qwen2.5-VL-3B，但未给出训练配置细节。
- 需要说明：资源与算力信息在现有文本中缺失。

## 5. 实验数量与充分性

- 实验数量有限：主要报告了CoNavBench的规模（4048个episodes）以及一个基线模型的单组训练结果。
- 未明确展示消融实验、不同协作类型（如交接/会合模式）的对比、或对NavCraft生成质量的系统性验证。
- 对比方法仅为“强单机器人”，缺乏与现有其他多智能体协作导航方法或更丰富基线的系统比较。
- 从现有信息看，实验设计在**规模上初步可行**，但在**充分性、公平性和客观性**方面尚有待补充：缺少多方法横向对比、统计显著性检验、以及跨场景泛化分析。

## 6. 主要结论与发现

- 协作VLN在缩短任务完成时间（makespan）和提升鲁棒性方面显著优于单智能体方案。
- 单智能体方法在协作环境下存在不足，无法处理拥塞、交接错误和会合时机问题。
- CoNavBench作为新的基准，能够有效评估协作长时程VLN性能，为后续研究提供平台。
- NavCraft可自动生成大规模、带图标注的协作任务，并支持调度修复，具有工程可行性。

## 7. 优点

- **填补空白**：首次专门面向协作长时程VLN的基准，覆盖拥塞、交接、会合等真实挑战。
- **自动化生成**：NavCraft平台降低人工标注成本，支持规模化扩展。
- **结构清晰的标注**：图级标注和协作类型分类法，使任务可控、可分析。
- **结合场景图**：利用Habitat-Sim场景图进行可达性、时间和干扰评估，提升了生成任务的合理性和可验证性。
- **提供参考基线**：基于多模态大模型（Qwen2.5-VL-3B）的微调策略，便于后续研究者复现和对比。

## 8. 不足与局限

- **信息不完整**：摘要未提供训练资源配置、具体消融实验、多方法对比等关键细节，难以全面评估实验充分性。
- **模拟环境局限性**：全部场景基于Habitat-Sim，存在sim-to-real差距，真实物理环境中的执行效果未知。
- **单一基线**：仅与“强单机器人”对比，未与其他多智能体协作导航方法或规则式方法比较，说服力有限。
- **成功率偏低**：step-level success仅18.11%，说明任务难度较大，也可能反映基准的挑战性过高，实际应用还需提升。
- **调度修复机制**的实现细节和效率未在摘要中展开，其开销和对整体性能的贡献不清楚。
- **协作类型学**覆盖的交接/会合模式有限，是否涵盖多种现实场景（如动态障碍、通信约束）尚未说明。

（完）
