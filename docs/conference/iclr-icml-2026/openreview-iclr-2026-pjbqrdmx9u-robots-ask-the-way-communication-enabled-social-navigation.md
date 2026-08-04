---
title: "Robots Ask the Way: Communication-Enabled Social Navigation"
title_zh: 机器人问路：支持通信的社交导航
authors: "Valentino Sacco, Luca Scofano, Ludovica Mazza, Indro Spinelli, Fabio Galasso"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=pjBQRDmx9u"
tags: ["query:maspd"]
score: 9.0
evidence: 支持通信的社交导航，在多人在环环境中主动询问与避障
tldr: 现有社交导航方法只进行反应式避障和轨迹调整，缺少主动获取信息的能力。本文提出 CommNav，让机器人询问居民关于目标人员的目击位置与移动模式，并将 Habitat 3.0 扩展为支持多人与结构化信息交换的 Habitat 3.0c。实验表明，通过主动通信能显著提高定位效率，为社交导航中的人机协同感知提供了新方向。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 社交导航缺乏主动通信，难以高效定位多个居民中的目标个体。
method: 提出 CommNav，机器人主动询问居民获取目标位置、目击与移动模式，并构建 Habitat 3.0c 仿真环境。
result: 在扩展仿真环境中显著提升目标定位效率与导航成功率。
conclusion: 主动通信能显著增强社交导航在多人环境中的目标搜索能力。
---

## Abstract
Assistive autonomous robots operating in multi-agent environments require efficient strategies to locate specific individuals among multiple residents. Current social navigation methods focus on reactive collision avoidance and trajectory adaptation, but lack mechanisms to proactively gather information through human-robot communication.

We introduce Communication-enabled Social Navigation (CommNav). In this novel task, robotic agents actively seek assistance from residents to locate target individuals by requesting information about recent sightings, locations, and movement patterns.
To evaluate CommNav, we extend Habitat 3.0 to create Habitat 3.0c, a communication-enabled variant supporting multi-human environments with structured information exchange protocols. Adding COMM to a state-of-the-art social navigation model yields an improvement of 7\% in finding a specific individual and 8\% in overall episode success.


Our experiments reveal that: (i) explicit human-robot communication substantially enhances multi-person navigation performance; (ii) pre-training COMM on a communication pretext task effectively addresses the challenge of occasional interaction signals.

---

## 论文详细总结（自动生成）

## 论文中文总结

### 1. 核心问题与研究动机

- **背景**：辅助自主机器人在多智能体（多人）环境中运行时，常需从多位居民中高效定位特定目标个体（如某位需要帮助的老人或工作人员）。
- **现存问题**：现有社交导航（Social Navigation）方法主要聚焦于**反应式避障**与**轨迹调整**，如绕开行人、保持社交距离等，但**缺乏主动获取信息的机制**——机器人无法向环境中的人询问目标的位置或移动模式。
- **核心研究问题**：机器人是否可以通过**主动的、结构化的语言/通信**（例如“请问您最近见过目标人物吗？”），显著提升在多居民环境中定位目标的效率？

### 2. 方法论：CommNav 与 Habitat 3.0c

- **核心思想**：将“主动通信”引入社交导航任务，使机器人从被动的感知-避障者转变为**主动的信息寻求者**。
- **CommNav 任务设计**：
  - 机器人导航时，可主动向遇到的居民（非目标个体）发起询问。
  - 询问内容限定为三类结构化信息：**最近目击位置（recent sightings）、当前位置（locations）、移动模式（movement patterns）**。
  - 居民根据信息交换协议给予回复，机器人融合这些信息进行路径规划与目标搜索。
- **仿真环境扩展——Habitat 3.0c**：
  - 在 Habitat 3.0 的基础上扩展，使其支持**多人类环境**与**结构化信息交换协议**（即模拟人类对机器人提问的响应机制）。
  - 这是首个支持通信式社交导航任务的仿真 benchmark 扩展。
- **算法流程（文字描述）**：
  - 输入：目标人物身份/描述、初始位置、环境地图。
  - 循环：执行导航 → 遇到居民 → 判定是否为目标，若是则终止；若否，则选择是否发起询问 → 接收结构化信息 → 更新目标可能位置的概率分布 → 重新规划路径。
  - 方法无具体数学公式披露，本质上是将通信模块作为附加动作空间（ask）叠加到现有社交导航策略之上（文中表述为“Adding COMM to a state-of-the-art social navigation model”）。

### 3. 实验设计

- **仿真环境**：基于 **Habitat 3.0c**（由 Habitat 3.0 扩展而来）。
- **Benchmark**：CommNav 任务本身即作为新提出的评估基准，涵盖多人类场景下的目标搜索与导航任务。
- **对比方法**：
  - 基线：未加入通信模块的先进社交导航模型（state-of-the-art social navigation model）。
  - 增强版：在该模型上叠加通信模块（即 COMM 模块）。
- **具体指标**：
  - 目标个体发现率：提升 **7%**。
  - 完整任务（episode）成功率：提升 **8%**。
- **预训练策略**：使用**通信前置任务（communication pretext task）**对 COMM 模块进行预训练，以缓解交互信号稀疏（偶尔才能遇到居民提问）的问题。

### 4. 资源与算力

- **论文中未明确说明**使用的 GPU 型号、数量、训练时长或硬件资源规模。
- 从该工作基于 Habitat 3.0 仿真平台推断，实验可复现性较高（仿真环境不依赖真实机器人硬件），但具体算力需求无法从公开信息中获知。

### 5. 实验数量与充分性

- **实验组数有限**：论文仅报告了**一组核心对比实验**（基线 vs. 基线+COMM），以及一个**预训练策略的有效性检验**（针对交互信号稀疏的消融式探究）。
- **充分性判断**：
  - 优点：对比设计干净，能直接体现通信模块的增量贡献。
  - 不足：缺少以下维度的系统分析——(a) 对居民数量的鲁棒性；(b) 对不同通信内容质量/噪声的敏感性；(c) 不同交互频次下的性能曲线；(d) 与纯视觉搜索（无通信但更长时间探索）的公平对比；(e) 真实场景或更复杂仿真的泛化性验证。因此，实验**初步验证了有效性，但全面性与深度仍有不足**。

### 6. 主要结论

- 主动的人-机器人通信（在简洁结构化信息下）能**显著提升**多人环境中定位特定个体的效率（定位成功率 +7%，终局成功率 +8%）。
- 使用**通信前置任务预训练**可有效应对稀疏交互信号问题，使机器人更稳定地学习和利用通信机会。
- 该结果为“社交导航”提供了新范式：从**被动避让**迈向**主动协作感知**。

### 7. 优点

- **问题新颖性强**：首次系统性地将“主动询问”纳入社交导航任务定义，填补了该领域缺乏主动信息获取机制的空白。
- **环境贡献突出**：扩展 Habitat 3.0 为 Habitat 3.0c，为后续研究者提供了可直接使用的通信式多智能体导航仿真平台，具备清晰的 benchmark 价值。
- **方法简洁有效**：将通信作为辅助模块叠加到现有 SOTA 模型上，带来 7–8% 的稳定提升，说明主动通信是一种低成本、高收益的增强项。
- **预训练设计有洞见**：针对真实场景中“交互信号稀疏”的痛点提出预训练策略，体现出对实际部署问题的考量。

### 8. 不足与局限

- **实验覆盖有限**：仅报告了单一基准上的两组关键实验结果，缺乏多场景、多任务参数（居民密度、环境规模、通信噪声等）的全面消融与敏感性分析。
- **信息协议过于理想化**：“结构化信息交换”假设居民总能给出准确、有用的回答，未考虑语言歧义、错误指引、恶意误导或居民不愿回答的真实世界情况。
- **缺乏真实世界验证**：全部实验在仿真中完成，未涉及真实机器人或真实人群，结果能否迁移到嘈杂、动态、非协作的真实环境中尚属未知。
- **对比基准单一**：只与一个 SOTA 模型对比，未与如纯视觉主动搜索（active visual search）、其他多模态目标定位方法进行对比，增量的 7–8% 是否代表最优权衡存在不确定性。
- **算力信息缺失**：未报告训练成本，不利于复现性与资源评估。
- **通信代价未被充分建模**：未考虑提问打断他人造成的社交成本、时间成本或对居民行为的干扰，这些在实际应用中可能是决定性因素。

（完）
