---
title: "Beyond Conservatism: Diffusion Policies in Offline Multi-agent Reinforcement Learning"
title_zh: 超越保守主义：离线多智能体强化学习中的扩散策略
authors: "Zhuoran Li, Ling Pan, Jiatai Huang, Longbo Huang"
date: 2023-09-20
pdf: "https://openreview.net/pdf?id=dIynM7bHCG"
tags: ["query:mcd"]
score: 9.0
evidence: 基于扩散模型的离线多智能体强化学习方法，提升策略表达力与多样性
tldr: 离线多智能体强化学习常依赖保守策略导致表达力受限。本文提出DOM2，将扩散模型引入策略网络，并结合轨迹数据增强，增强策略的表达力和多样性。实验在多个多智能体粒子环境和多智能体MuJoCo环境中取得优于现有方法的表现，提升了性能、泛化能力和数据效率，为离线MARL提供了新范式。
source: ICLR-2024-Public
selection_source: conference_retrieval
motivation: 现有离线多智能体算法主要依靠保守策略设计，限制了策略表达力和多样性，导致泛化与数据效率不足。
method: 提出DOM2，将扩散模型作为策略网络，并提出基于轨迹的数据增强方案来增强策略的鲁棒性与多样性。
result: 在多个多智能体粒子环境与多智能体MuJoCo环境中，DOM2显著优于现有最先进方法，并改善泛化与数据效率。
conclusion: 扩散模型为离线MARL提供了一种超越保守主义的新方法，提升了应对环境变化的鲁棒性。
---

## Abstract
We present a novel Diffusion Offline Multi-agent Model (DOM2) for offline Multi-Agent Reinforcement Learning (MARL). Different from existing algorithms that rely mainly on conservatism in policy design, DOM2 enhances policy expressiveness and diversity based on diffusion model. Specifically, we incorporate a diffusion model into the policy network and propose a trajectory-based data-augmentation scheme in training. These key ingredients make our algorithm more robust to environment changes and achieve significant improvements in performance, generalization and data-efficiency. Our extensive experimental results demonstrate that DOM2 outperforms existing state-of-the-art methods in all multi-agent particle and multi-agent MuJoCo environments, and generalizes significantly better to shifted environments (in $28$ out of $30$ settings evaluated) thanks to its high expressiveness and diversity. Moreover, DOM2 is ultra data efficient and requires no more than $5\%$ data for achieving the same performance compared to existing algorithms (a $20\times$ improvement in data efficiency).

---

## 论文详细总结（自动生成）

注意：提供的 PDF 提取文本为 OpenReview 的浏览器验证页面，无法获取论文全文。以下总结基于所附的论文元数据、摘要及 TLDR 信息生成。

---

# 中文总结：超越保守主义：离线多智能体强化学习中的扩散策略

## 1. 核心问题与整体含义

- **研究背景**：离线多智能体强化学习（Offline MARL）旨在从静态数据集中学习多智能体协作策略，无需与环境在线交互。已有方法通常依赖“保守主义”策略设计（如悲观估计、正则化）来缓解分布外（OOD）问题，但这同时限制了策略的**表达力（expressiveness）**与**多样性（diversity）**。
- **核心问题**：如何在不牺牲离线学习稳定性的前提下，提升多智能体策略的表达能力与多样性，从而增强算法的泛化能力和数据效率。
- **整体含义**：本文提出一种新的范式——将扩散模型（Diffusion Model）引入离线 MARL 策略网络，突破了传统保守策略的性能天花板，为离线多智能体学习提供了新思路。

## 2. 方法论

- **核心思想**：新颖的 **DOM2（Diffusion Offline Multi-agent Model）**。与直接依赖保守正则化的现有方法不同，DOM2 将扩散模型作为策略网络的一部分，利用扩散模型强大的分布建模能力捕捉多模态、复杂的行为策略，从而增强策略的表达力和多样性。
- **关键技术细节**：
  - **扩散模型作为策略**：通过去噪过程生成动作，能够表示更丰富的动作分布，避免单一高斯或类别分布的局限。
  - **基于轨迹的数据增强**：在训练过程中引入轨迹级的数据增强方案，提升样本的利用率和策略的鲁棒性，使模型在环境变化时更稳定。
- **算法流程（文字描述）**：离线数据 → 轨迹增强预处理 → 扩散策略网络训练（前向加噪、反向去噪）→ 多智能体价值函数/评论家学习（与扩散策略联合优化）→ 推理时通过去噪采样输出动作。
- 原文未提供公式，故无法给出具体数学细节。

## 3. 实验设计

- **基准场景**：
  - 多智能体粒子环境（Multi-agent Particle Environment）
  - 多智能体 MuJoCo（Multi-agent MuJoCo）
- **对比方法**：现有离线 MARL 最先进方法（原文未列出具体名称，但从摘要可推断包含主流保守化算法）。
- **泛化测试**：评估了在环境偏移（shifted environments）下的表现，覆盖 **30 个设置**，其中 DOM2 在 **28 个设置**中显著优于对比方法。
- **数据效率测试**：与现有算法达到相同性能相比，DOM2 仅需不超过 **5% 的数据**，即约 **20 倍数据效率提升**。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中**没有提供 GPU 型号、数量、训练时长或计算资源预算**。因此无法评估其算力成本。

## 5. 实验数量与充分性

- **实验数量**：摘要提到在两个大类基准（粒子、MuJoCo）中的全部环境上进行了测试，并额外做了 30 个设置的迁移/泛化实验，规模较为可观。
- **充分性**：
  - 优点：覆盖了多智能体典型基准，且做了环境迁移和极端数据效率测试，具有较强的说服力。
  - 不足：由于无法访问全文，**不清楚是否有消融实验**（如去掉扩散模型、去掉轨迹增强等），以及是否报告标准差、多次随机种子等统计细节。
  - 公平性：摘要声称“优于现有最先进方法”，但未给出具体对比方法的实现细节、超参数调整策略，可能存在对比偏差风险（需要全文验证）。

## 6. 主要结论与发现

- DOM2 在全部多智能体粒子环境和多智能体 MuJoCo 环境中取得了优于现有 SOTA 的性能。
- 由于扩散模型带来的高表达力和多样性，DOM2 在环境偏移下表现显著更好（28/30 设置）。
- 数据效率极为突出，较现有算法实现同等性能最多可节省 95% 的数据（20 倍提升）。

## 7. 优点

- **范式创新**：首次（或少数）将扩散模型用于离线 MARL 策略，突破“保守主义”设计思路，理论视角新颖。
- **表达力强**：扩散模型天然适合多模态、高维动作分布，适合多智能体协调任务。
- **数据高效**：20 倍数据效率提升对真实场景（数据获取昂贵）非常有价值。
- **泛化鲁棒**：在迁移环境中表现稳定，说明策略没有过度拟合训练分布。
- **实验结果突出**：在多个基准和迁移设置上的优势一致明显。

## 8. 不足与局限

- **全文不可获取**：由于 CAPTCHA，无法确认实验细节、算法伪代码、超参数设置和消融研究，总结可能不完整。
- **计算开销未报告**：扩散模型采样通常较慢，离线训练和部署成本可能高于传统策略网络，论文未给出相关资源分析。
- **基准场景有限**：仅覆盖粒子环境和 MuJoCo，缺乏更复杂、更具实际意义的真实多智能体任务（如自动驾驶、机器人团队控制）。
- **理论保证缺失**：摘要未提到对扩散策略与保守正则化的理论结合（如性能下限），可能缺乏可解释性保证。
- **对比公平性疑问**：设计时可能使用了更强大的模型容量，但未说明是否给基线足够调参空间，存在“以强模型胜基线”的嫌疑。

---

（完）
