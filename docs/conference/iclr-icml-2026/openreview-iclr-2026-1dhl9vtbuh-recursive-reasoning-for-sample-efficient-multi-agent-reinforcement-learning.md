---
title: Recursive Reasoning for Sample-Efficient Multi-Agent Reinforcement Learning
title_zh: 用于样本高效多智能体强化学习的递归推理
authors: "Aryaman Reddi, Gabriele Tiboni, Jan Peters, Carlo D'Eramo"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=1dhL9VtBuH"
tags: ["query:mcd"]
score: 9.0
evidence: 递归推理更新策略梯度以提升多智能体协调与样本效率
tldr: 针对深层多智能体强化学习中策略梯度更新未考虑同一步内其他智能体更新而导致的协调不足与样本效率低下，本文提出递归推理机制：每个智能体根据其他智能体更新后的策略递归细化自身梯度。该方法在竞争性和合作性场景的on/off-policy算法上均有实现，实验表明能加速发现有效协调策略并显著提升样本效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 传统MARL策略梯度未能预测队友同一更新步中的策略变化，导致失协调和低样本效率。
method: 提出递归推理的梯度更新方法，使每个智能体针对其他智能体更新后的策略细化更新，适用于on/off-policy。
result: 在多个MARL基准上加速有效协调策略发现并大幅提升样本效率。
conclusion: 递归推理为样本高效的多智能体协作训练提供了新的优化视角。
---

## Abstract
Policy gradient algorithms for deep multi-agent reinforcement learning (MARL) typically employ an update that responds to the current strategies of other agents. While being straightforward, this approach does not account for the updates of other agents within the same update step, resulting in miscoordination and reduced sample efficiency. In this paper, we introduce methods that recursively refine the policy gradient by updating each agent against the updated policies of other agents within the same update step, speeding up the discovery of effective coordinated policies. We provide principled implementations of recursive reasoning in MARL by applying it to competitive multi-agent algorithms in both on and off-policy regimes. Empirically, we demonstrate superior performance and sample efficiency over existing deep MARL algorithms in StarCraft II and multi-agent MuJoCo. We
theoretically prove that higher recursive reasoning in gradient-based methods with finite iterates achieves monotonic convergence to a local Nash equilibrium under certain conditions.

---

## 论文详细总结（自动生成）

# 论文总结：用于样本高效多智能体强化学习的递归推理

## 1. 核心问题与整体含义（研究动机和背景）

- 深度多智能体强化学习（MARL）中的策略梯度算法通常采用“针对其他智能体当前策略”进行更新的方式，即更新时假设队友策略固定不变。
- 这种方式忽略了在同一更新步内其他智能体也会同步更新策略这一事实，导致智能体之间的“协调不足”（miscoordination），进而降低样本效率。
- 论文的核心动机是：在梯度更新时显式地考虑其他智能体的同步更新，从而加速有效协调策略的发现，并提升样本效率。
- 整体含义：该研究为深层 MARL 中的策略优化提供了一种新的“递归推理”视角，强调智能体之间的互动更新对学习动态的影响。

## 2. 论文提出的方法论

- 核心思想：**递归推理（Recursive Reasoning）**——每个智能体在计算自身策略梯度时，不仅考虑其他智能体当前的策略，还考虑其他智能体在同一更新步中可能采取的“更新后策略”，并以此为基础递归地细化自身梯度。
- 关键技术细节：
  - 在同一更新步内，将其他智能体的策略更新视为可预测的步骤，并为每个智能体构建“如果对方已更新，我应如何更新”的梯度计算链。
  - 方法可应用于 on-policy 和 off-policy 两种学习范式的多智能体算法。
  - 提供了理论分析：在有限迭代（finite iterates）条件下，更高阶的递归推理在特定条件下能实现单调收敛到局部纳什均衡（local Nash equilibrium）。
- 公式或算法流程（文字说明）：
  - 论文未在提供的文本中给出具体公式推导，但可以推断其更新过程类似“迭代式最佳响应”：智能体 i 的梯度更新依赖于其他智能体更新后的策略，而其他智能体的更新又依赖于智能体 i 自身的更新，从而形成递归依赖关系。实际操作中可能通过有限阶展开或迭代计算来近似实现这一递归过程。

## 3. 实验设计

- 使用的 benchmark / 环境：
  - **StarCraft II**（通常指 SMAC 多智能体对战环境）
  - **Multi-Agent MuJoCo**（连续控制协作任务）
- 对比方法：提到了与“existing deep MARL algorithms”对比，但具体对比了哪些算法（如 MAPPO、QMIX、VDN 等）在提供的文本中未明确列出。
- 实验性质：摘要声称在竞争性和合作性场景中均有实现（元数据 tldr 提到“竞争性和合作性场景的 on/off-policy 算法上均有实现”），但摘要正文只明确提到“competitive multi-agent algorithms”，因此具体场景覆盖需查阅原文确认。

## 4. 资源与算力

- 提供的论文内容中**没有明确说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 这也可能是论文正文中有但未出现在摘要和元数据中，但从现有信息无法确认。需要注意：如果读者关心可复现性，算力信息缺失是一个不足。

## 5. 实验数量与充分性

- 已知实验覆盖了至少两类主流 MARL 基准环境（SMAC 和 MAMuJoCo），并提到了 on/off-policy 两种设置。
- 但关于具体实验组数、任务难度（如 SMAC 的不同地图、MAMuJoCo 的不同关节控制任务）、消融实验（如不同递归阶数的对比、不同更新步数的影响）等，在提供的文本中**未透露**。
- 因此无法判断实验的充分性和详尽程度。若论文只进行少量场景验证，则结论的普适性可能有限；需依赖完整论文进行审阅。

## 6. 论文的主要结论与发现

- 递归推理方法能够**加速有效协调策略的发现**，并在多个 MARL 基准上显著提升样本效率。
- 理论上证明了在特定条件下，有限迭代的递归梯度更新方法可以**单调收敛到局部纳什均衡**，为方法的稳定性提供了部分保证。
- 整体结论是：递归推理是一种值得采用的样本高效 MARL 优化策略。

## 7. 优点

- **创新性强**：将“其他智能体同步更新”这一被忽视的因素引入策略梯度计算，直击 MARL 中协调失败的关键痛点。
- **适用性广**：同时支持 on-policy 和 off-policy 算法，且可应用于竞争/合作等多种场景。
- **理论保障**：给出了收敛到局部纳什均衡的证明，增强了方法的学术可信度。
- **实验基准主流**：选择 SMAC 和 MAMuJoCo 两个公认的 MARL 基准，结果有代表性。

## 8. 不足与局限

- **实验细节缺失**：提供的信息中未列出具体的对比算法、任务配置、超参数设置、重复实验次数，难以判断方法的普适性和稳健性。
- **计算复杂性**：递归推理需要估计其他智能体更新后的策略，可能引入额外计算开销，尤其是递归阶数增加时，但文本未讨论效率与性能的权衡。
- **理论条件不明确**：仅提到“under certain conditions”收敛，具体条件（如步长限制、奖励结构、智能体同质性等）未在摘要中给出，实用性有待考察。
- **场景覆盖疑义**：摘要中提到方法应用于“competitive multi-agent algorithms”，但元数据 tldr 又提到合作性场景，二者描述不一致，需要原文澄清。
- **可扩展性问题**：递归推理在智能体数量众多或动作空间较大时，计算复杂度可能急剧上升，文本未提供针对大规模任务的实验验证。

（完）
