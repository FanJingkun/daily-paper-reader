---
title: Offline Multi-Agent Reinforcement Learning via Sequential Score Decomposition
title_zh: 基于序列得分分解的离线多智能体强化学习
authors: "Dan Qiao, Wenhao Li, Shanchao Yang, Hongyuan Zha, Baoxiang Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/42646707bb9bbc1bf3382011364ece1fae959b04.pdf"
tags: ["query:mcd"]
score: 7.0
evidence: 离线合作多智能体强化学习，多模态联合行为，序列分解用于信用分配
tldr: 离线合作MARL中，数据集常包含多种合作行为模式，导致独立正则化失效。本文提出OMSD，将联合行为策略序列分解为每个智能体的条件分布，并利用扩散生成模型进行模态协调的正则化。实验表明该方法能有效缓解分布偏移，提升离线多智能体策略的协调性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 离线合作MARL数据分布多模态，独立策略正则化易导致分布偏移。
method: 提出OMSD，顺序分解联合策略为条件分布，并利用扩散模型进行协调正则化。
result: 实验证明OMSD有效缓解分布偏移，提升离线多智能体策略的协调性。
conclusion: 序列得分分解是一种处理离线MARL多模态行为的有效范式。
---

## Abstract
Offline cooperative multi-agent reinforcement learning (MARL) faces unique challenges due to the distribution shift between online and offline data collection. While online MARL typically converges to a single coordinated joint policy, offline datasets are often mixtures of diverse cooperative behaviors, resulting in highly multimodal joint behavior distributions. In such settings, independent policy regularization often misaligns joint policy constraints and leads to severe distribution shift. To address this, we propose OMSD, which sequentially decomposes the joint behavior policy into individual conditional distributions and leverages diffusion-based generative models to provide modality-coordinated regularization for each agent. Combined with centralized critic guidance, OMSD achieves coordinated exploration within high-value, in-distribution regions, and avoids out-of-distribution joint actions. Experiments across multiple datasets on various continuous control tasks demonstrate that OMSD consistently achieves state-of-the-art performance, especially in challenging multimodal scenarios. Our results highlight the necessity of modality-aware coordination for robust offline MARL.

---

## 论文详细总结（自动生成）

# 论文总结：基于序列得分分解的离线多智能体强化学习

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：离线合作多智能体强化学习（offline cooperative MARL）旨在仅从静态数据集学习多智能体协同策略，其核心挑战在于在线与离线数据收集之间的**分布偏移（distribution shift）**。
- **关键难点**：在线 MARL 通常收敛到单一协同联合策略，而离线数据集往往是**多种不同合作行为模式的混合**，导致联合行为分布呈现高度多模态（multimodal）。
- **现有方法的不足**：在该设定下，传统的**独立策略正则化**（即对各智能体策略分别施加约束）无法与真实的联合策略约束对齐，容易导致严重的分布偏移，使学到策略越界到数据分布之外的区域。
- **整体意义**：本文指出离线 MARL 的鲁棒性高度依赖于对多模态联合行为的**模态感知协调**，并为此提供了新的求解范式。

## 2. 方法论：OMSD

- **核心思想**：提出 **OMSD（Offline Multi-agent reinforcement learning via Sequential score Decomposition，序列得分分解）**，将联合行为策略按顺序**分解为每个智能体在给定前序智能体行为条件下的条件分布**，从而在保留联合分布多模态结构的同时，对每个智能体进行正则化。
- **关键技术细节**：
  - **序列分解**：利用概率链式法则，将高维联合行为分布分解为一系列低维条件分布，避免对高度多模态联合分布直接建模的困难。
  - **扩散生成模型**：使用基于扩散的生成模型来表达这些条件分布，并通过得分（score）匹配提供**模态协调的正则化**，使各智能体的策略更新被约束在数据支撑的区域内。
  - **集中式评论家引导**：在生成过程中引入集中训练的价值函数（critic）进行引导，使策略在**高价值且在分布内**的区域进行协调探索，避免越界联合动作。
- **算法流程**（文字说明）：对每个智能体逐一采样条件动作，每个条件分布由扩散模型近似，并利用集中评论家的梯度信息对采样过程加以引导；通过联合优化各条件分布，最终得到协调的整体策略。

## 3. 实验设计

- **实验场景与基准**：论文在**多种连续控制任务**的**多个数据集**上进行评估，但摘要与元数据中未明确列出具体环境名称（如 MuJoCo 或 D4RL 类基准的具体任务名与数据集划分）。
- **对比方法**：与多个现有离线 MARL 方法进行对比，摘要表述为“state-of-the-art performance”，即与当前最先进方法比较，但具体 baseline 方法名称未在此处列出。
- **重点场景**：特别关注具有挑战性的**多模态数据场景**，在这些场景下考察方法处理分布偏移的能力。

## 4. 资源与算力

- 论文的摘要与元数据中**没有公开任何算力信息**。
- 包括 GPU 型号、数量、训练时长、总计算量等资源信息均未说明，因此无法评估实验的计算成本或可复现的硬件要求。

## 5. 实验数量与充分性

- **实验数量**：摘要表明实验覆盖多个数据集、多种连续控制任务，并做了与最先进方法的对比；但未直接提及消融实验、超参数敏感性分析或各模块的逐项贡献验证。
- **充分性判断**：
  - 从现有信息看，多数据集与多任务的覆盖有助于验证方法的泛化性，但**缺少对消融实验的描述**，难以判断序列分解与扩散模型各自的独立贡献。
  - 未提及具体数据集与任务列表，也无法核对是否包含了标准的公开离线 MARL benchmark，以及是否覆盖了低维与高维、简单与复杂多模态等不同难度谱系。
  - 实验的客观性与公平性依赖具体实现细节（如 baseline 超参调优、种子数、评估协议等），这些信息在当前给定内容中不可见，需依赖完整论文进一步确认。

## 6. 主要结论与发现

- OMSD 在多个连续控制数据集上**一致地取得了最先进（state-of-the-art）性能**，尤其在具有挑战性的多模态场景中优势明显。
- 实验证据表明，**序列得分分解能够有效缓解分布偏移**，提升离线多智能体策略的协调性。
- 结论环节进一步指出：**序列得分分解是一种处理离线 MARL 多模态行为分布的有效范式**，强调了模态感知协调对鲁棒离线 MARL 的必要性。

## 7. 优点

- **问题定位准确**：清楚地识别出离线 MARL 中“联合行为多模态”这一以往被忽视的独特难点，而非简单沿用单智能体的独立正则化思路。
- **方法设计有理论支撑**：以概率链式法则为基础，将联合分布分解为条件分布的乘积，兼顾了多模态保持与复杂度可控。
- **生成模型选型合理**：扩散模型天然适合表达复杂多模态分布，用于条件分布建模与模态协调正则化符合问题结构。
- **价值引导机制完整**：与集中式评论家结合，实现了高价值且在分布内的协调探索，兼顾策略优化与安全约束（避免越界动作）。
- **结果具说服力**：在多样化的连续控制任务与多数据集上全面超越现有方法，验证了方法的有效性和普适性。

## 8. 不足与局限

- **实验细节披露不足**：提供的摘要与元数据中没有具体数据集名称、任务列表、baseline 方法名称、评价指标及超参数设置，难以完整复现和横向比较。
- **缺乏消融分析信息**：未见到针对序列分解、扩散模型、集中评论家引导等各模块的消融实验，方法的因果贡献尚不够清晰。
- **算力资源未说明**：未报告 GPU 型号、数量与训练时长，增加了复现与部署的成本不确定性。
- **应用范围有待验证**：实验仅覆盖连续控制任务，对离散动作空间、大规模智能体数量（如数百个智能体）、部分可观测设定（POMG）以及非平稳对手等更复杂场景的适用性未得到验证。
- **潜在偏差风险**：多模态离线数据中的模态不均衡可能导致条件扩散模型偏向数据量大的模态；此外，序列分解的顺序选择（智能体排序）可能影响结果，论文未讨论该敏感性。

（完）
