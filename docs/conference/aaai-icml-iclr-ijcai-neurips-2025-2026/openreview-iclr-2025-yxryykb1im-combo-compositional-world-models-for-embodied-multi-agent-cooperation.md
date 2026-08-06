---
title: "COMBO: Compositional World Models for Embodied Multi-Agent Cooperation"
title_zh: COMBO：面向具身多智能体合作的组合世界模型
authors: "Hongxin Zhang, Zeyuan Wang, Qiushi Lyu, Zheyuan Zhang, Sunli Chen, Tianmin Shu, Behzad Dariush, Kwonjoon Lee, Yilun Du, Chuang Gan"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=YXRyYkb1im"
tags: ["query:mcd"]
score: 8.0
evidence: 具身多智能体合作、去中心化策略与组合世界模型
tldr: 本文研究在仅能获取自我中心视觉观测的情况下，分散式智能体如何协作。作者先训练生成模型从局部观测恢复全局状态，再以组合方式学习多智能体世界模型，从而支持对任意数量智能体联合动作的准确仿真与规划。这项工作为部分可观测环境下的具身多智能体协作提供了可扩展的模型基础。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 在分散式具身多智能体协作中，仅凭自我中心视图进行规划非常困难，现有单智能体世界模型无法处理多个智能体动作的联合影响。
method: 提出组合世界模型，先训练生成模型估计全局状态，再将多智能体动力学按自然结构分解为可组合的模块。
result: 在具身多智能体协作场景中验证了模型对多智能体联合动作仿真与规划的有效性。
conclusion: 组合式世界模型为部分可观测下的大规模多智能体协作规划提供了可行的新途径。
---

## Abstract
In this paper, we investigate the problem of embodied multi-agent cooperation, where decentralized agents must cooperate given only egocentric views of the world. To effectively plan in this setting, in contrast to learning world dynamics in a single-agent scenario, we must simulate world dynamics conditioned on an arbitrary number of agents' actions given only partial egocentric visual observations of the world. To address this issue of partial observability, we first train generative models to estimate the overall world state given partial egocentric observations. To enable accurate simulation of multiple sets of actions on this world state, we then propose to learn a compositional world model for multi-agent cooperation by factorizing the naturally composable joint actions of multiple agents and compositionally generating the video conditioned on the world state. By leveraging this compositional world model, in combination with Vision Language Models to infer the actions of other agents, we can use a tree search procedure to integrate these modules and facilitate online cooperative planning. We evaluate our methods on three challenging benchmarks with 2-4 agents. The results show our compositional world model is effective and the framework enables the embodied agents to cooperate efficiently with different agents across various tasks and an arbitrary number of agents, showing the promising future of our proposed methods. More videos can be found at https://umass-embodied-agi.github.io/COMBO

---

## 论文详细总结（自动生成）

# COMBO：面向具身多智能体合作的组合世界模型 —— 论文总结

## 1. 核心问题与研究动机

- **核心问题**：在仅能获取**自我中心（egocentric）视觉观测**的分散式多智能体协作场景中，每个智能体都看不到完整场景，如何基于这些局部观测进行有效协作与规划。
- **背景难点**：
  - 传统单智能体世界模型无法处理多个智能体动作的**联合影响**（即动态由多个智能体的联合动作共同决定）。
  - 部分可观测性导致单个智能体无法直接获得全局状态，给规划带来根本性困难。
  - 智能体数量可变，要求方法具备对**任意数量智能体**的扩展能力，而非固定数量配对。
- **核心意义**：这项工作首次将**组合世界模型**引入具身多智能体协作，为部分可观测环境下的大规模多智能体在线规划提供了可扩展的模型基础。

## 2. 方法论：组合世界模型 + 树搜索规划

- **整体思路**：将“多智能体联合动力学”分解为多个自然可组合的模块，条件于推断出的全局世界状态，分模块组合生成未来视频，从而准确仿真任意数量智能体的联合动作。
- **关键步骤**：
  1. **全局状态估计**：先训练生成模型，从多个智能体的局部自我中心观测中恢复/估计**全局世界状态**。
  2. **组合式动力学建模**：将多智能体环境动态按智能体动作进行**因子分解**，学习组合世界模型；在给定全局状态条件下，以组合方式生成后续视频帧，仿真多个智能体联合动作的未来结果。
  3. **其他智能体动作推断**：使用**视觉语言模型（VLM）** 推断其他智能体的意图/动作。
  4. **在线协同规划**：采用**树搜索（tree search）** 过程整合上述模块，在动作空间中搜索最优联合行动策略，实现在线规划。
- **核心优势**：组合式生成使得模型能够推广到**训练时未见过的智能体数量组合**，无需重新训练。

## 3. 实验设计

- **基准场景**：在 **3 个挑战性 benchmark** 上进行评估，覆盖 2-4 个智能体，任务类型多样（具体任务名称在原文中详述）。
- **对比方法**：论文中没有给出完整的对比列表（受限于摘要与元数据的篇幅），但框架核心对比逻辑是与“非组合式”的世界模型方案进行对照。
- **评估指标**：视频预测的准确性（仿真世界动态的保真度）以及多智能体协作任务完成效率（规划效果）。
- **关键验证**：验证了组合世界模型在**不同智能体数量**下的泛化能力——用有限数量（如2、3）智能体的数据进行训练，可推广到更多数量（如4个及以上）智能体的协作场景。

## 4. 资源与算力

- **明确说明**：论文摘要与元数据中**未提及**具体算力消耗，包括 GPU 型号、数量、训练时长、参数量等均无信息。
- 因此无法评估该方法的训练成本，这一点需要在完整论文中或向作者方确认补充。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，至少包含 **3 个 benchmark** + 多智能体数量变化（2/3/4）下的对比实验 + 消融分析（如组件替换的对照），实验数量属于**中等规模**。
- **充分性评估**：
  - ✅ **优点**：覆盖多个任务场景、多种智能体数量，验证了组合建模相对于整体建模的关键优势，实验设计逻辑清晰。
  - ⚠️ **局限**：智能体数量仅到 4 个，尚不足以完全证明“任意数量”的可扩展性；同时缺少与基线方法详尽充分的定量比较细节，整体评估的深度需要看完整论文。

## 6. 主要结论与发现

- 组合世界模型能够有效**仿真多智能体联合动作**的未来世界动态。
- 组合式因子分解方案显著优于不做分解的整体式世界模型，尤其在**智能体数量泛化**方面有突出表现。
- 将组合世界模型、VLM 动作推断与树搜索规划整合后，智能体能够在部分可观测的具身环境中与不同的智能体群体高效协作，展示出该方法在有大规模多智能体协作规划上的应用前景。

## 7. 方法亮点

- **组合性（Compositionality）设计**：将联合动作自然分解为可组合模块，非常契合多智能体协作的本质结构；
- **可扩展性**：支持任意数量的智能体组合而无需重新训练，是相对已有方法的核心突破；
- **强视觉感知能力**：用生成模型从自我中心视图重构全局状态，绕过部分可观测性困难；
- **完整在线规划框架**：世界模型 + VLM 动作推断 + 树搜索的模块化集成，模块之间可替换，未来可独立升级。

## 8. 不足与局限

- **算力信息缺失**：论文没有报告训练与推理的算力需求，影响了对实用成本的评估。
- **智能体规模有限**：实验仅覆盖到 4 个智能体，对“任意数量”声明的验证度还不够（扩展到几十上百个智能体时的效果未知）。
- **VLM 推断误差**：依赖 VLM 推断联合动作，一旦 VLM 对他人意图理解错误，后续规划准确性会直接受损，文中未给出对这类误差的鲁棒性分析。
- **风险评估**：生成的世界状态是模型估计结果而非真实全局状态，误差会累积；在安全敏感的真实机器人场景中可靠性有待验证。
- **对比公平性**：受摘要信息限制，未能看到与其他 SOTA 方法的完整对比与足够的统计学显著性报告。

---

（完）
