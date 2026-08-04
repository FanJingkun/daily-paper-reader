---
title: Offline Multi-Agent Reinforcement Learning via Sequential Score Decomposition
title_zh: 基于序列分数分解的离线多智能体强化学习
authors: "Dan Qiao, Wenhao Li, Shanchao Yang, Hongyuan Zha, Baoxiang Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/42646707bb9bbc1bf3382011364ece1fae959b04.pdf"
tags: ["query:mcd"]
score: 8.0
evidence: 离线合作多智能体强化学习，序列分数分解与扩散正则化
tldr: 离线合作多智能体强化学习面临数据分布偏移与多模态联合行为分布的双重挑战。OMSD将联合行为策略顺序分解为各智能体条件分布，并利用扩散生成模型提供模态协调的正则化，使独立策略约束与联合策略约束对齐。实验证明其能有效缓解离线数据的分布偏移问题，提升多智能体离线决策的性能与鲁棒性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 离线数据中多智能体联合行为多模态，独立正则化易导致分布偏移。
method: 将联合行为策略顺序分解为条件分布，结合扩散模型进行模态协调正则。
result: 缓解离线多智能体强化学习中的分布偏移，性能提升。
conclusion: 序列分解与生成式正则化提升了离线多智能体学习鲁棒性。
---

## Abstract
Offline cooperative multi-agent reinforcement learning (MARL) faces unique challenges due to the distribution shift between online and offline data collection. While online MARL typically converges to a single coordinated joint policy, offline datasets are often mixtures of diverse cooperative behaviors, resulting in highly multimodal joint behavior distributions. In such settings, independent policy regularization often misaligns joint policy constraints and leads to severe distribution shift. To address this, we propose OMSD, which sequentially decomposes the joint behavior policy into individual conditional distributions and leverages diffusion-based generative models to provide modality-coordinated regularization for each agent. Combined with centralized critic guidance, OMSD achieves coordinated exploration within high-value, in-distribution regions, and avoids out-of-distribution joint actions. Experiments across multiple datasets on various continuous control tasks demonstrate that OMSD consistently achieves state-of-the-art performance, especially in challenging multimodal scenarios. Our results highlight the necessity of modality-aware coordination for robust offline MARL.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景与动机**：离线合作多智能体强化学习（MARL）中，训练数据为预先收集的离线数据集，与在线交互数据之间存在**分布偏移**问题。在线 MARL 通常收敛到一个**单一协调的联合策略**，而离线数据集往往由**多种多样的协作行为混合而成**，导致联合行为分布具有高度**多模态性**。
- **核心挑战**：在离线多模态数据下，若对各智能体进行**独立策略正则化**，容易造成联合策略约束之间的**错位**，进而引发严重的分布偏移，使智能体产生超出分布（OOD）的联合动作。
- **研究意义**：该工作旨在解决离线 MARL 在多模态行为分布下的策略约束对齐问题，提升离线决策的性能与鲁棒性，是多智能体离线强化学习的重要进展。

## 2. 论文提出的方法论

- **方法名称**：**OMSD**（Offline Multi-agent Reinforcement Learning via Sequential Score Decomposition，基于序列分数分解的离线多智能体强化学习）。
- **核心思想**：将联合行为策略进行**顺序分解**，转化为各智能体的**条件分布**，从而保留数据中多模态协作模式的结构信息。
- **关键技术**：
  - 利用**扩散生成模型**为每个智能体提供**模态协调的正则化**，使各智能体的独立策略约束与联合策略约束保持一致。
  - 结合**集中式评论家（centralized critic）引导**，使智能体能在**高价值、处于分布内**的区域进行协调探索，有效避免产生 OOD 联合动作。
- **算法流程（文字描述）**：首先从离线数据中学习联合行为策略的序列分数分解；然后针对每个智能体构造条件生成模型（扩散模型）作为正则化器；训练各智能体策略时，使用集中式 critic 提供价值引导，同时用扩散模型约束策略行为，使得整体策略既靠近数据分布又具有高价值，最终实现多智能体协同决策。

## 3. 实验设计

- **任务类型**：连续控制任务（continuous control tasks）。
- **数据集**：在多个数据集上进行评测，重点涵盖了**多模态挑战场景**，即离线数据包含多种不同协作行为的混合。
- **基准（Benchmark）**：未在摘要中明确说明具体的测试环境名称（如 Multi-Agent MuJoCo、SMAC 等），但使用了标准连续控制多智能体离线数据集。
- **对比方法**：摘要未列出具体基线算法，但指出 OMSD 在对比中**一致地达到当前最优（state-of-the-art）性能**，可推测与常见离线 MARL 方法（如独立正则化、BCQ、CQL 类扩展方法等）进行了比较。

## 4. 资源与算力

- **摘要中未提供**任何关于 GPU 型号、数量、训练时长、显存消耗等算力相关信息。
- **说明**：由于仅有摘要内容，无法得知实验的具体硬件与时间成本，这是信息缺失项。

## 5. 实验数量与充分性

- **实验数量**：摘要提及在**多个数据集**上进行了实验，并重点报告了几类代表性结果，但未给出具体的数据集个数、每种场景的重复次数或消融实验详情。
- **充分性评价**：
  - **优点**：实验覆盖了多种数据分布，特别包含多模态难点，能初步验证方法的核心优势。
  - **不足**：缺乏对方法各组件（如顺序分解、扩散正则化、集中式 critic 引导）的消融分析展示；未说明独立重复次数或统计显著性检验；对比方法不明确，难以全面评估公平性。总体而言，摘要层面信息不足，完整论文中可能包含更充分实验。

## 6. 论文的主要结论与发现

- OMSD 通过在**序列分解的联合行为策略**上施加**扩散模型正则化**，并配合**集中式 critic 引导**，有效缓解了离线多智能体强化学习中的分布偏移问题。
- 实验表明 OMSD 在多个连续控制任务的离线数据集上均取得**最先进性能**，尤其是处理**多模态协作行为**时优势明显。
- 结论强调：**模态感知的协调机制**对于鲁棒的离线多智能体强化学习必不可少，仅做独立策略正则化不足以应对多模态数据。

## 7. 优点

- **方法创新性强**：将联合行为策略的“顺序分解”与“扩散生成模型”结合，提供了一种对多模态协作行为结构进行显式建模的思路。
- **理论扎实**：从分布偏移与策略约束错位的根本症结入手，设计具有针对性的正则化方案，逻辑清晰。
- **实践效果好**：在多种连续控制任务和数据集上实现 SOTA，验证了方法有效性与普适性。
- **有明确的应用价值**：为离线多智能体场景（如机器人集群、自动驾驶协同）提供更可靠的学习范式，避免 OOD 灾难性失败。

## 8. 不足与局限

- **实验细节透明度不足**：摘要中未给出具体基准环境、基线方法、超参数设置和消融实验，限制了对结果客观性和公平性的全面判断。
- **算力资源未披露**：缺少 GPU 型号/数量、训练时间等能耗信息，不利于复现和横向对比效率。
- **可能的应用限制**：方法依赖扩散模型的采样与训练，推理成本较高；序列分解顺序的选择可能影响性能，论文未讨论其敏感性；实验仅在连续控制任务上验证，对离散动作或更复杂多智能体场景的适用性未知。
- **潜在偏差风险**：数据集若来自单一模拟器或特定混合方式，可能无法泛化到真实世界中的异构多模态数据。

（完）
