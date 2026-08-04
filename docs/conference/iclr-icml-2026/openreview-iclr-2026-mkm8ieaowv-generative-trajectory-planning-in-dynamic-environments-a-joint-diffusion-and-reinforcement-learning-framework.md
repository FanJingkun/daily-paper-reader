---
title: "Generative Trajectory Planning in Dynamic Environments: A Joint Diffusion and Reinforcement Learning Framework"
title_zh: 动态环境下的生成式轨迹规划：扩散模型与强化学习联合框架
authors: "Minjoo Kim, Soohyun Park"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=MKM8iEaowV"
tags: ["query:maspd"]
score: 4.0
evidence: 结合扩散模型和强化学习的动态环境轨迹规划，属于运动规划方向
tldr: 针对动态环境中实时轨迹规划需要同时保证安全性和能效的问题，提出将扩散模型与深度强化学习结合的通用框架。扩散模型通过建模短视子路径生成多样化候选轨迹，强化学习在滚动时域内优化轨迹并学习局部避碰和平滑性。实验表明该框架在动态障碍环境下相比基线方法具有更好的安全性和能效表现，为机器人运动规划提供了一种可迁移的生成式方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 动态环境中的实时轨迹规划需要兼顾安全与能效，传统方法难以高效生成多样化可行轨迹。
method: 提出扩散模型与强化学习联合框架，分解轨迹为子路径，学习局部避碰和平滑性。
result: 在含静态和动态障碍物的环境中，验证了该方法在安全性和能量效率上的优势。
conclusion: 该生成式轨迹规划框架可迁移到机器人运动规划任务，提升实时性能。
---

## Abstract
Real-time trajectory optimization requires planners that can simultaneously ensure safety and energy efficiency in environments containing both static and dynamic obstacles. This paper introduces a generalized framework that combines diffusion-based trajectory generation with deep reinforcement learning (DRL). The diffusion component generates diverse candidate trajectories by modeling feasible sub-paths, where a sub-path denotes a short-horizon segment aligned with receding-horizon execution. In this formulation, the entire trajectory is decomposed into consecutive sub-paths, enabling the diffusion model to learn local collision avoidance and smoothness while maintaining consistency across the fully identified path (e.g., global path and whole trajectory). The DRL component then evaluates these candidates online, selecting actions that improve safety while adapting to dynamic obstacles and maintaining energy-efficient behavior. The joint design leverages the generative diversity of diffusion and the adaptive decision-making of DRL, producing a planner that is both responsive and reliable. To assess effectiveness, the method is evaluated in unmanned aerial vehicle (UAV) path optimization scenarios with dynamic obstacles. The results demonstrate that sub-path training enhances the generalization of diffusion-based planners by linking local feasibility to global performance, and that the approach offers a practical solution for real-time UAV trajectory optimization with improved safety and efficiency.

---

## 论文详细总结（自动生成）

# 中文论文总结

## 1. 核心问题与研究动机

- **核心问题**：在包含静态与动态障碍物的真实环境中，实时轨迹规划需要同时满足安全性（避碰）与能量效率两大目标，而传统优化方法难以在在线约束下高效生成多样化且可行的候选轨迹。
- **研究背景与动机**：
  - 无人机等自主系统的路径优化在高动态环境下对实时性要求高，规划器必须快速响应环境变化。
  - 单一方法（如纯优化或纯学习）难以兼顾生成多样性、局部避碰、全局一致性和能效。
  - 作者提出将扩散模型的生成能力与深度强化学习（DRL）的自适应决策能力结合，形成通用且可迁移的生成式轨迹规划框架。

## 2. 方法论

- **核心思想**：提出“扩散模型 + 深度强化学习”联合框架——扩散模型负责离线/在线生成多样化候选轨迹（以子路径为单位），强化学习负责在线评估与动作选择，二者通过滚动时域执行机制衔接。
- **关键技术细节**：
  1. **子路径（Sub-path）分解**：将完整轨迹分解为一系列短时域子路径段，与滚动时域（receding-horizon）执行对齐；扩散模型对子路径建模，学习局部避碰与平滑性。
  2. **全局一致性约束**：在子路径生成中保持与全局路径/完整轨迹的一致性，使得局部可行性能够映射为全局性能。
  3. **DRL 在线评估与选择**：DRL 对扩散模型生成的候选子路径进行实时评价，选择在动态障碍环境下更安全的动作，同时保持能量高效。
- **公式与算法流程**：论文正文未提供明确的数学公式或伪代码，但算法流程可概括为：
  - 输入全局参考路径 + 当前环境感知信息；
  - 扩散模型生成多个候选子路径；
  - DRL 评估每个候选的安全性/能效，选择最优子路径执行；
  - 随滚动时域推进，循环执行上述步骤。

## 3. 实验设计

- **应用场景**：无人机（UAV）路径优化任务，环境包含静态障碍物与动态障碍物。
- **数据集/Benchmark**：未明确说明使用特定公开数据集或标准 benchmark；属于自建仿真环境。
- **对比方法**：提到了“基线方法”（baselines），但摘要中未列出具体对比算法名称（如 RRT*、MPC、纯 DRL 等无法确认）。

## 4. 资源与算力

- **文中未明确说明**：未提及 GPU 型号与数量、训练时长、参数量等算力资源信息。
- 需要指出：论文文本中缺少计算资源相关披露，因此无法评估训练成本与复现门槛。

## 5. 实验数量与充分性

- **实验数量**：从论文摘要看，实验仅在无人机动态避障场景下进行，未报告多场景、多机器人的广泛验证；未明确提到进行了消融实验（虽然元数据出现“消融”标签，但摘要正文未描述具体消融设计）。
- **充分性与客观性评估**：
  - **不足**：缺少消融实验细节、基线方法说明不完整、未提供量化指标（如碰撞率、能耗数值、成功率等）的具体数值，实验规模与对比广度有限。
  - **公平性**：由于未给出基线明细与实验设置细节，难以判断对比是否完全公平。

## 6. 主要结论与发现

- 子路径训练方式能够增强扩散模型规划器的泛化能力：通过学习局部可行性，可提升整体轨迹性能。
- 扩散模型与 DRL 的联合框架在动态障碍环境中，相比基线方法在安全性和能量效率上均有改善。
- 该框架具备实时性与可迁移性，为机器人运动规划提供了一套实用的生成式方案。

## 7. 优点

- **方法新颖**：将扩散模型的生成多样性与 DRL 的自适应决策有机结合，形成统一框架。
- **局部与全局兼顾**：通过子路径分解与全局一致性约束，巧妙衔接局部避碰与全局路径优化。
- **实用性导向**：面向实时在线规划场景，支持滚动时域执行，符合实际机器人部署需求。
- **可迁移性强**：框架设计具有一定通用性，不依赖特定机器人平台。

## 8. 不足与局限

- **实验结果不透明**：未提供具体量化结果与对比数据，削弱了说服力。
- **场景覆盖有限**：仅在无人机仿真场景验证，缺乏真实世界实验和多类机器人平台的验证。
- **基线对比不详**：未列出具体对比方法，读者难以评估相对优势的真实来源。
- **缺少消融分析**：未说明扩散模型与 DRL 各自贡献度，联合框架的有效性分析不够深入。
- **算力信息缺失**：没有训练成本信息，影响复现与实际部署评估。
- **公式细节缺失**：未给出数学模型或算法伪代码，方法复现门槛较高。

（完）
