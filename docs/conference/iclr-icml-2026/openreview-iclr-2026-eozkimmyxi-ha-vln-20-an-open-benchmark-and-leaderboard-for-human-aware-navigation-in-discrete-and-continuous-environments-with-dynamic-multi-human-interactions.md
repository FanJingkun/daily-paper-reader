---
title: "HA-VLN 2.0: An Open Benchmark and Leaderboard for Human-Aware Navigation in Discrete and Continuous Environments with Dynamic Multi-Human Interactions"
title_zh: HA-VLN 2.0：离散与连续环境中与动态多人类交互的人类感知导航开放基准与排行榜
authors: "Yifei Dong, Fengyi Wu, Qi He, Zhi-Qi Cheng, Heng Li, Minghan Li, Zebang Cheng, Yuxuan Zhou, Jingdong Sun, Qi Dai, Alexander G Hauptmann"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=EoZKimMYXi"
tags: ["query:maspd"]
score: 4.0
evidence: 离散与连续环境中的动态多人类交互导航基准，涉及碰撞规避与动态交互
tldr: 本文提出HA-VLN 2.0基准与排行榜，首次系统地在离散和连续环境中引入社会感知约束，模拟动态多人类交互与部分可观测场景，并评估导航智能体在目标准确性和个人空间保持上的表现。实验显示现有先进智能体在动态人类参与下性能显著下降，并提供了真实机器人部署验证。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLN研究忽略动态拥挤环境，缺乏对社会感知约束的标准化评测。
method: 构建HAPS 2.0数据集与模拟器，建模多人类交互和户外场景，提出统一任务指标与排行榜。
result: 基于16844条社会语言指令的评测显示主流智能体在动态人类和部分可观测下性能急剧下降。
conclusion: 为人类感知导航提供标准化基准，推动动态环境下的机器人导航研究。
---

## Abstract
Vision-and-Language Navigation (VLN) has been studied mainly in either discrete or continuous settings, with little attention to dynamic, crowded environments. We present HA-VLN 2.0, a unified benchmark introducing explicit social-awareness constraints. Our contributions are: (i) a standardized task and metrics capturing both goal accuracy and personal-space adherence; (ii) HAPS 2.0 dataset and simulators modeling multi-human interactions, outdoor contexts, and finer language–motion alignment; (iii) benchmarks on 16,844 socially grounded instructions, revealing sharp performance drops of leading agents under human dynamics and partial observability; and (iv) real-world robot experiments validating sim-to-real transfer, with an open leaderboard enabling transparent comparison. Results show that explicit social modeling improves navigation robustness and reduces collisions, underscoring the necessity of human-centric approaches. By releasing datasets, simulators, baselines, and protocols, HA-VLN 2.0 provides a strong foundation for safe, socially responsible navigation research.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 视觉语言导航（Vision-and-Language Navigation, VLN）长期以来主要在**离散或连续环境**中研究，忽略了真实世界常见的**动态、拥挤场景**。
- 现有 VLN 智能体缺乏对社会交互（如避让行人、保持个人空间、遵循社会规范）的显式建模，导致在动态多人类环境中导航不安全、不自然。
- 论文旨在填补这一空白，提出一个**统一的、带有社会感知约束的基准**，推动 VLN 研究从“静态场景”走向“动态人类共存场景”，为安全、负责任的机器人导航奠定基础。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（用文字说明即可）

- 核心思想：在 VLN 任务中显式引入**社会感知约束**，同时评估“目标准确性”和“个人空间保持”两个维度。
- 关键技术细节：
  - 构建 **HAPS 2.0 数据集**与模拟器，支持**多人类交互**、**户外场景**以及**更细粒度的语言-运动对齐**。
  - 设计标准化的**任务定义和评价指标**，统一衡量导航成功率与社交合规性。
  - 提供**排行榜（Leaderboard）** 和标准化协议，便于公平对比不同方法。
  - 强调**部分可观测性**，模拟真实机器人只能观察有限范围的场景。
- 算法流程（文字说明）：智能体接收语言指令和传感器输入 → 在动态环境中规划路径 → 在导航过程中实时避让动态人类目标 → 根据统一指标（目标达成 + 个人空间保持）进行评分。

## 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法

- 数据集：**HAPS 2.0 数据集**，包含 **16,844 条社会语言指令**，覆盖多人类交互和户外场景。
- Benchmark：**HA-VLN 2.0** 统一基准，同时支持**离散和连续环境**，以及真实机器人部署验证。
- 对比方法：对**现有主流 VLN 智能体**进行基准测试，评估其在动态人类和部分可观测条件下的性能下降程度。
- 额外实验：进行了**真实世界机器人实验**，验证 sim-to-real 迁移效果。

## 4. 资源与算力

- 论文提供的摘要和元数据**未明确说明**使用的 GPU 型号、数量、训练时长或算力消耗。
- 仅提到开源了数据集、模拟器、基线和协议，但未披露训练硬件的具体信息。

## 5. 实验数量与充分性

- 已知实验包括：
  - 在 16,844 条社会指令上的基准测试；
  - 对比主流智能体在动态人类下的性能；
  - 显式社会建模与无社会建模的对比（消融性质）；
  - 真实机器人部署实验。
- 充分性评价：从摘要看，实验覆盖了合成环境与真实环境，具备基本对比和消融，但由于**缺少详细实验表格、统计显著性检验和具体性能数字**，无法全面评估其客观性和公平性。若论文正文包含更详细的多次重复实验和误差分析，则充分性更强；但当前信息有限。

## 6. 论文的主要结论与发现

- 现有先进 VLN 智能体在动态人类参与和部分可观测条件下，**性能显著下降**。
- 显式社会建模能够**显著提升导航鲁棒性**并**减少碰撞**。
- 证明了以人为中心的导航方法在动态环境中的必要性。
- HA-VLN 2.0 作为开放基准，有效促进透明比较和后续研究。

## 7. 优点

- **标准化程度高**：统一任务定义、指标和排行榜，便于社区公平比较。
- **覆盖真实需求**：首次系统考虑动态多人类交互和户外场景，贴近实际部署。
- **细粒度语言-运动对齐**：数据集设计更精细，有助于跨模态理解。
- **验证充分**：不仅做模拟评测，还进行真实机器人实验，验证 sim-to-real 可用性。
- **开源开放**：释放数据集、模拟器、基线和协议，推动领域发展。

## 8. 不足与局限

- **算力信息缺失**：未报告训练资源和能耗，影响可复现性评估。
- **实验细节不透明**：摘要未提供具体性能数据、绝对指标和误差范围，难以独立判断效果规模。
- **社会建模范围有限**：主要关注个人空间保持和碰撞规避，对更复杂的社会规则（如群体行为、文化差异）可能未覆盖。
- **真实环境规模未知**：真实机器人实验的规模、场景数量和多样性未说明，迁移结论的普适性存疑。
- **基准的生态依赖**：若模拟器与真实世界的动态人类行为仍有差距，排行榜优势不一定能完全转化为实际部署价值。

（完）
