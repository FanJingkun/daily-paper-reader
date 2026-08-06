---
title: Robust and Diverse Multi-Agent Learning via Rational Policy Gradient
title_zh: 基于理性策略梯度的鲁棒多样多智能体学习
authors: "Niklas Lauffer, Ameesh Shah, Micah Carroll, Sanjit A. Seshia, Stuart Russell, Michael D Dennis"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ipEqdzO2IT"
tags: ["query:hetero-marl"]
score: 6.0
evidence: 通过保持理性避免自我破坏，将对抗优化扩展到合作MARL，提升鲁棒多样性
tldr: 对抗优化在零和博弈中可有效训练鲁棒多样策略，但在合作博弈中直接应用会导致智能体自我破坏而停止学习。本文提出保持理性的策略优化(RPO)，通过约束每个智能体的策略相对于某些搭档为最优，避免自我破坏。该方法使得对抗式搜索可安全用于合作MARL，在保持理性的同时产生鲁棒且多样化的联合策略。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 对抗优化在合作场景会诱导智能体自我破坏，阻碍任务完成和持续学习。
method: 提出RPO形式化框架，约束策略相对某些搭档最优以保持理性。
result: 在保持理性的同时获得鲁棒且多样化的合作策略。
conclusion: 将对抗搜索安全引入合作MARL，扩展了鲁棒多样性训练的新途径。
---

## Abstract
Adversarial optimization algorithms that explicitly search for flaws in agents' policies have been successfully applied to finding robust and diverse policies in the context of multi-agent learning. However, the success of adversarial optimization has been largely limited to zero-sum settings because its naive application in cooperative settings leads to a critical failure mode: agents are irrationally incentivized to *self-sabotage*, blocking the completion of tasks and halting further learning. To address this, we introduce *Rationality-preserving Policy Optimization (RPO)*, a formalism for adversarial optimization that avoids self-sabotage by ensuring agents remain *rational*—that is, their policies are optimal with respect to some possible partner policy. To solve RPO, we develop *Rational Policy Gradient (RPG)*, which trains agents to maximize their own reward in a modified version of the original game in which we use *opponent shaping* techniques to optimize the adversarial objective. RPG enables us to extend a variety of existing adversarial optimization algorithms that, no longer subject to the limitations of self-sabotage, can find adversarial examples, improve robustness and adaptability, and learn diverse policies. We empirically validate that our approach achieves strong performance in several popular cooperative and general-sum environments. Our project page can be found at https://rational-policy-gradient.github.io.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：在多智能体学习（Multi-Agent Learning）中，对抗优化算法（adversarial optimization）通过主动搜索智能体策略的缺陷来生成鲁棒且多样的策略，已在零和博弈（zero-sum）场景中取得成功。然而，将这些算法直接应用于合作场景时会引发严重缺陷：智能体被非理性地激励去“自我破坏”（self-sabotage），即故意阻碍任务完成，从而停止进一步学习，导致对抗优化失效。
- **核心问题**：如何将对抗优化的优势（鲁棒性、多样性）安全地扩展到合作博弈和一般和博弈（general-sum）场景，同时避免自我破坏问题？
- **整体含义**：本文提出一种新的形式化框架与算法，使得对抗式搜索能够在不牺牲理性（rationality）的前提下用于合作 MARL，从而拓展鲁棒多样性策略训练的新途径。这项工作填补了对抗优化在合作场景应用的理论与算法空白。

## 2. 论文提出的方法论

- **核心思想**：保持智能体的理性——即每个智能体的策略相对于“某些可能的搭档策略”而言是最优的。通过限制策略空间，避免对抗优化诱导智能体采用自我破坏的行为。
- **提出框架**：**Rationality-preserving Policy Optimization (RPO)**  
  - 将对抗优化形式化为一个约束问题：目标函数是最大化自身奖励，但策略必须满足“理性”约束（策略相对于某些搭档策略为最优）。
  - 该约束确保训练过程中的策略不会走上自我破坏的路径，从而保证持续学习和任务完成能力。
- **具体算法**：**Rational Policy Gradient (RPG)**  
  - 在原始博弈的修改版本中训练智能体最大化自身奖励。
  - 使用**对手塑形（opponent shaping）**技术来优化对抗目标，即调整对手的更新或策略以帮助实现鲁棒性和多样性。
  - RPG 可以扩展多种已有对抗优化算法，使其不再受自我破坏限制，从而能够寻找对抗样本、提升鲁棒性与适应性、学习多样化策略。
- **理论贡献**：给出了 RPO 的形式化定义，并证明满足理性约束的前提下，对抗优化可以安全地应用于合作与一般和博弈。

## 3. 实验设计

- **实验环境**：论文声称在多个流行的合作与一般和环境中进行实证验证，但摘要中未列出具体环境名称（例如 MPE、SMAC、Overcooked 等），也未提供具体 benchmark 细节。
- **对比方法**：摘要未明确列出基线方法，但暗示与“已有对抗优化算法”（如 PSRO 类方法）进行了对比，并比较了有无 RPG 约束的变体。
- **实验目标**：验证 RPO/RPG 能够：
  - 避免自我破坏；
  - 找到对抗样本；
  - 提升鲁棒性和适应性；
  - 学习多样化策略。

## 4. 资源与算力

- 论文摘要与提供的元数据中**未说明**使用的 GPU 型号、数量、训练时长或任何具体算力资源。
- 由于只有部分文本，可能完整论文中有相关内容，但当前信息无法给出具体资源消耗。

## 5. 实验数量与充分性

- 摘要仅概述了实验结论（“在几个流行的合作和一般和环境中取得了强性能”），没有给出实验数量、具体数据集或消融研究细节。
- 从可获取的内容看，实验描述的充分性不足，无法判断是否进行了多组对比实验、消融实验或重复实验。
- 由于缺乏具体实验细节（如环境、基线、指标、随机种子数等），无法客观评估实验的公平性和广泛性。需要阅读全文才能判断。

## 6. 论文的主要结论与发现

- 对抗优化在合作场景中直接应用会导致自我破坏，阻碍学习。
- 通过确保策略的理性（最优性约束），RPO 可以避免自我破坏，使对抗搜索在合作场景中安全有效。
- 提出的 RPG 算法能够成功扩展多种对抗优化算法，使智能体在保持理性的同时获得鲁棒且多样化的联合策略。
- 实证结果支持该方法在多个合作与一般和环境中具有强性能。

## 7. 优点

- **理论创新**：首次将“理性保持”概念引入对抗优化，解决了一个关键的失败模式——自我破坏，具有较强的理论意义。
- **方法通用性**：RPG 是一个通用框架，可兼容多种现有对抗优化算法，具有较好的扩展性。
- **应用价值**：将对抗优化的鲁棒多样性优势引入合作 MARL，扩展了实际应用范围（如机器人协作、多智能体协同控制等）。
- **问题界定清晰**：论文明确指出合作场景下对抗优化的根本障碍，并给出了数学形式化的解决方案。

## 8. 不足与局限

- **实验描述不完整**：摘要中缺乏具体的 benchmark、环境名称、对比基线和实验数量，无法评估实验的充分性和客观性。
- **算力信息缺失**：未说明训练资源与耗时，难以衡量方法实际成本。
- **理论假设限制**：理性约束要求策略相对于“某些搭档策略”最优，但如何选择搭档策略集合、在复杂环境中的计算可行性等可能成为实际应用的限制。
- **一般和博弈的验证尚需深化**：虽然声明适用于合作与一般和博弈，但摘要中的验证细节有限，具体表现和局限性还需阅读全文确认。
- **可能的偏差风险**：未提供消融实验或失败案例分析，无法判断方法在极端情况下的稳定性。

（完）
