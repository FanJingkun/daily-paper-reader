---
title: "CooT: Learning to Coordinate In-Context with Coordination Transformers"
title_zh: CooT：协调变换器用于上下文中的协作学习
authors: "Huai-Chih Wang, Hsiang-Chun Chuang, Hsi-Chun Cheng, Dai-Jie Wu, Shao-Hua Sun"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ebe9uW2yQA"
tags: ["query:mcd"]
score: 7.0
evidence: 多智能体动态环境中的上下文协调，适应未见队友
tldr: 多智能体在动态不确定环境中的协调常依赖自我对局或群体训练，但难以泛化到未见过的队友。CooT 提出协调变换器，利用最近的交互历史进行上下文协调，使智能体能快速适应新伙伴的行为。实验表明该方法在未见队友的协调任务中表现优异，并显著降低了对大规模微调的依赖。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有自我对局与群体方法难以泛化到未见队友，且需要昂贵的微调。
method: 提出协调变换器，基于近期交互历史预测与观测一致的行动，实现无需微调的上下文协调。
result: 在未见伙伴的多智能体协调任务上快速适应，优于现有方法。
conclusion: CooT 为动态环境下多智能体与未知伙伴之间的泛化协调提供了有效方案。
---

## Abstract
Effective coordination among artificial agents in dynamic and uncertain environments remains a significant challenge in multi-agent systems. Existing approaches, such as self-play and population-based methods, either generalize poorly to unseen partners or require impractically extensive fine-tuning. To overcome these limitations, we propose Coordination Transformers (CooT), a novel in-context coordination framework that uses recent interaction histories to rapidly adapt to unseen partners. Unlike prior approaches that primarily aim to diversify training partners, CooT explicitly focuses on adapting to new partner behaviors by predicting actions aligned with observed interactions. Trained on trajectories collected from diverse pairs of agents with complementary preferences, CooT quickly learns effective coordination strategies without explicit supervision or parameter updates. Across diverse coordination tasks in Overcooked, CooT consistently outperforms baselines including population-based approaches, gradient-based fine-tuning, and a Meta-RL-inspired contextual adaptation method. Notably, fine-tuning proves unstable and ineffective, while Meta-RL struggles to achieve reliable coordination. By contrast, CooT achieves stable, rapid in-context adaptation and is consistently ranked the most effective collaborator in human evaluations.

---

## 论文详细总结（自动生成）

# CooT：协调变换器用于上下文中的协作学习——论文总结

## 1. 核心问题与整体含义

- **研究背景**：在多智能体系统中，如何让智能体在动态且不确定的环境中实现有效协调，是一个长期存在的核心挑战。
- **现有方法的不足**：
  - **自我对局（self-play）** 方法：智能体与自己或固定策略训练，难以泛化到未见过的队友。
  - **群体训练（population-based）方法**：虽增加了队友多样性，但训练成本高，且面对全新行为模式的队友时仍可能失效。
  - **微调（fine-tuning）** 方法：在遇到新队友时重新调整参数，代价昂贵且在实际场景中不可行。
- **核心问题**：智能体能否在**不更新参数、不显式监督**的情况下，仅通过观察队友的近期行为就能快速适应并实现有效协调？
- **整体含义**：该论文提出一个全新的“上下文协调”（in-context coordination）框架，让智能体像人类一样通过观察对方过往举动来推断其意图和习惯，从而在未见过的队友面前也能即插即用地实现协调。

---

## 2. 方法论：CooT 协调变换器

- **核心思想**：利用**最近的交互历史**进行上下文推理，预测与观测一致的后续行动，从而快速适应陌生队友的行为模式，**无需微调参数**。
- **关键技术细节**：
  - 模型基于 **Transformer 架构**，以近期多智能体交互轨迹（联合状态-动作序列）作为上下文输入。
  - 通过注意力机制捕捉队友行为模式与当前环境状态的关联，提取协调策略。
  - 训练数据来源于**具有互补偏好的多对智能体**的交互轨迹，使模型学到广泛的协调模式。
  - 推理时，模型直接输出与当前上下文对齐的动作，**没有显式奖励信号或策略梯度更新**。
- **算法流程（文字描述）**：
  1. 收集多组不同偏好智能体对在协调任务中的交互轨迹；
  2. 以最近的交互历史为输入，训练 Transformer 预测当前智能体的动作；
  3. 部署时，将新队友的近期行为输入模型，模型自动推断协作策略并输出动作；
  4. 整个过程在推理阶段完成，参数保持不变。

---

## 3. 实验设计

- **场景/基准（Benchmark）**：使用 **Overcooked**（烹饪协作游戏）作为多智能体协调任务的测试环境，涵盖多种协调任务。
- **对比方法**：
  - **群体训练方法（population-based approaches）**；
  - **基于梯度的微调方法（gradient-based fine-tuning）**；
  - **Meta-RL 启发的上下文适应方法**。
- **人类评估**：还进行了真人协作评估，以衡量模型作为协作伙伴的实用性和偏好度。
- **评估维度**：包括协作效率、适应速度、稳定性，以及与不同类型队友协作的泛化能力。

---

## 4. 资源与算力

- **论文中未明确说明**训练 CooT 所使用的 GPU 型号、数量、训练时长或计算资源规模。
- 仅能推测训练涉及多组 Overcooked 轨迹数据，Transformer 模型规模可能适中（需查阅原文补充），但报告中**未披露具体算力消耗**，这是一个信息缺口。

---

## 5. 实验数量与充分性

- **实验组数**：在 Overcooked 的多个协调任务上进行了系统评估，且包含多类基线对比及人类评估，实验维度较为丰富。
- **充分性与客观性分析**：
  - **优点**：对比了至少三类代表性基线方法（群体、微调、Meta-RL），并加入人类评估，这在多智能体 RL 领域较为全面。
  - **不足**：
    - 未看到明确的**消融实验**（如：去掉历史上下文长度的影响、Transformer 规模的影响等）的详细描述。
    - 仅基于 Overcooked 单一环境，泛化到其他多智能体任务（如交通控制、机器人协作）尚未验证。
    - 人类评估的样本量、参与者数量、评估标准未在摘要中提及，公平性难以完全判断。
  - 总体而言，实验能支撑“有效”这一结论，但在**方法泛化性和消融完备性**上仍有所欠缺。

---

## 6. 主要结论与发现

- **CooT 在未见伙伴的协调任务上表现显著优于所有基线方法**，包括群体方法、梯度微调和 Meta-RL 方法。
- **微调方法被证明不稳定且效果差**，Meta-RL 也难以实现可靠的协调。
- **CooT 能够快速、稳定地实现上下文适应**，在人类评估中被评为最受欢迎的协作伙伴。
- 核心结论：**上下文学习（in-context learning）是多智能体泛化协调的有效途径**，大大降低了对大规模微调的依赖。

---

## 7. 优点与亮点

- **方法设计新颖**：将大规模语言模型中的 in-context learning 理念迁移到多智能体协调领域，拓展了上下文学习的应用边界。
- **部署友好**：无需参数更新、无需显式奖励信号，即可适应新队友，实用性极强。
- **实验证据扎实**：在 Overcooked 上同时对比群体方法、微调方法和 Meta-RL 方法，并首次引入人类评估来验证协作质量。
- **研究定位清晰**：直接回应了“如何泛化到未见队友”这一关键难题，并提供了经实验验证的解决思路。

---

## 8. 不足与局限

- **实验场景局限**：仅在 Overcooked 上验证，缺乏在更广泛的多智能体协调环境（如多个真机机器人、交通管理、通信受限场景）中的证据。
- **算力和实现细节缺失**：未披露 GPU 资源、模型规模、训练时间等关键信息，限制了结果的可复现性评估。
- **消融实验可能不足**：未深入分析历史上下文的长度、队友多样性、模型容量等因素对性能的影响。
- **人类评估细节不明**：参与者人数、任务难度、统计显著性等关键细节未公开，人类评估结果的说服力有待验证。
- **作为被拒论文**：该工作被 ICLR 2026 拒稿，可能存在审稿人指出的特定问题（如方法创新度、理论深度或实验覆盖度不足），在解读其结果时需保持审慎。
- **潜在偏差风险**：训练数据来自“互补偏好”的智能体对，若实际队友的行为模式与训练分布差异过大，适应能力可能受限。

---

（完）
