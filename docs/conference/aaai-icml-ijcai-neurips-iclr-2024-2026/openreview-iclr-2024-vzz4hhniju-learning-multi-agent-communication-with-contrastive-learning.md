---
title: Learning Multi-Agent Communication with Contrastive Learning
title_zh: 基于对比学习的多智能体通信学习
authors: "Yat Long Lo, Biswa Sengupta, Jakob Nicolaus Foerster, Michael Noukhovitch"
date: 2024-01-16
pdf: "https://openreview.net/pdf?id=vZZ4hhniJU"
tags: ["query:mcd"]
score: 9.0
evidence: 用对比学习学习多智能体通信
tldr: 通信是MARL中协调的有力工具，但学习有效通用语言尤其困难。论文将智能体间传递的消息视为环境状态的不同不完整视图，通过对比学习最大化轨迹内收发消息之间的互信息来学习通信。在需要通信的环境中，该方法在性能和收敛速度上均超越先前工作，并通过定量指标与表示探针显示其诱导出更对称的通信协议。
source: ICLR-2024-Accepted
selection_source: conference_retrieval
motivation: 在分散式通信中难以学习有效且通用的通信语言。
method: 将消息看作环境状态的不完整视图，用对比学习最大化收发消息的互信息。
result: 在通信必要环境中性能与学习速度均优于先前方法，通信协议更对称。
conclusion: 为多智能体通信学习提供了一种对比视角的新范式。
---

## Abstract
Communication is a powerful tool for coordination in multi-agent RL. But inducing an effective, common language is a difficult challenge, particularly in the decentralized setting. In this work, we introduce an alternative perspective where communicative messages sent between agents are considered as different incomplete views of the environment state. By examining the relationship between messages sent and received, we propose to learn to communicate using contrastive learning to maximize the mutual information between messages of a given trajectory. In communication-essential environments, our method outperforms previous work in both performance and learning speed. Using qualitative metrics and representation probing, we show that our method induces more symmetric communication and captures global state information from the environment. Overall, we show the power of contrastive learning and the importance of leveraging messages as encodings for effective communication.

---

## 论文详细总结（自动生成）

# 基于对比学习的多智能体通信学习——论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- 多智能体强化学习（MARL）中，智能体之间的通信是实现协调的重要工具。
- 然而，学习一种有效且通用的通信语言非常困难，尤其在**分散式（decentralized）**设置下，智能体没有集中的监督信号来指导消息语义的形成。
- 现有方法往往难以保证通信的有效性、对称性以及对全局状态信息的编码能力。
- 本文提出一种新视角：将智能体之间发送和接收的消息视为**环境状态的不同不完整视图（incomplete views of the environment state）**，通过**对比学习**来最大化同一轨迹内收发消息之间的互信息，从而学习通信。
- 整体意义在于为多智能体通信学习提供了一种不依赖手工奖励或集中式语义的对比学习范式。

## 2. 方法论：核心思想与关键技术

- **核心思想**：
  - 在同一轨迹中，智能体发送的消息与其接收到的消息之间存在对应关系，这种关系可以类比为同一环境状态的不同视角。
  - 利用对比学习，使得来自同一轨迹的“发送-接收”消息对在表示空间中更接近，而不同轨迹的消息对更远离，从而增强消息的语义一致性和信息承载能力。

- **关键技术细节**：
  - 通过对比损失（contrastive loss）最大化轨迹内收发消息之间的**互信息（mutual information）**。
  - 消息被视为环境状态的编码，对比学习推动这些编码捕获全局状态信息。
  - 方法不依赖集中式训练或额外奖励，适用于分散式执行。

- **算法流程（文字说明）**：
  1. 每个智能体根据自身观测生成消息（发送）。
  2. 其他智能体接收消息，并结合自身观测生成新消息或采取动作。
  3. 对于每条轨迹，将发送的消息与接收的消息组成正样本对，将不同轨迹或批次中的消息组成负样本对。
  4. 使用对比学习目标（如 InfoNCE 风格）训练消息编码器，使正样本对互信息最大化。
  5. 整体强化学习目标（如策略梯度）与对比学习目标联合优化。

## 3. 实验设计：场景、基准与对比方法

- **场景/基准**：
  - 论文强调“需要通信的环境（communication-essential environments）”，但未在提供的摘要中列出具体环境名称（如 SMAC、Traffic Junction、Multi-Robot 等未提及）。
  - 基准测试应遵循 MARL 领域常用通信任务，但具体名称不明。

- **对比方法**：
  - 与“先前工作（previous work）”进行比较，通常包括：
    - 基线方法：无通信方法（如独立 PPO）。
    - 已有通信学习方法：如 DIAL、CommNet、IC3Net、MADDPG 等（摘要中未具体点名，仅笼统提及“previous work”）。
  - 评估指标：性能（任务回报）、学习速度（收敛步数）、通信协议对称性（定性/定量指标）、表示探针（representation probing）以检测全局状态信息编码。

## 4. 资源与算力

- 论文提供的文本中**未明确说明**使用的 GPU 型号、数量、训练时长或总计算量。
- 仅能推断为一般 MARL 实验规模，但无具体算力信息可供总结。

## 5. 实验数量与充分性

- 根据摘要，实验主要覆盖：
  - 在通信必要环境下对比性能与学习速度。
  - 使用定性指标（如通信协议可视化）和表示探针分析协议对称性与信息编码。
- 未提及具体的消融实验数量、不同随机种子数或多种环境变体。
- 受限于提供的文本，**无法判断实验的完整充分性**。但作者声称方法在性能和速度上均优于先前工作，并提供了协议性质分析，说明实验设计至少包含主实验+分析，初步满足实证要求。
- 公平性：未展示细节，但对比学习方法的增益在需要通信的环境中较为直接，仍可能存在对特定环境调优的偏差风险。

## 6. 主要结论与发现

- 所提对比学习方法在**需要通信的 MARL 环境**中，相比先前工作在**性能**和**学习速度**上均有显著提升。
- 该方法诱导出**更对称的通信协议**，即智能体对消息语义的理解更一致、可复用。
- 表示探针显示，学习到的消息编码能够**捕获环境全局状态信息**，而不仅是局部观测。
- 验证了“消息作为环境状态的不完整视图，通过对比学习增强互信息”这一范式的有效性。

## 7. 优点

- **新颖视角**：将消息视为环境状态的多视图表示，与自监督学习中的对比学习思想自然衔接。
- **通用性强**：不依赖集中式训练或专门的通信奖励，可直接在分散式策略上训练。
- **理论支撑明确**：以互信息最大化为目标，具有良好的信息论解释。
- **分析全面**：除了性能指标，还通过定性和定量方法分析通信协议性质，证明了所学通信语言的对称性与全局信息编码能力。
- **强调学习速度**：不仅关注最终性能，还关注采样效率，实际意义较强。

## 8. 不足与局限

- **实验细节缺乏**：提供的文本未给出具体环境、超参数、基准实现的详细信息，无法复现验证。
- **算力信息缺失**：未报告训练资源，难以估计方法的大规模应用成本。
- **消融实验不明确**：未说明对不同对比损失变体、消息长度、智能体数量等的敏感性分析。
- **可能存在的偏差风险**：性能提升可能在特定“通信必要”的环境下显著，但在通信冗余或可忽略的环境中是否依然有效不明确。
- **通信协议对称性分析**：虽然提及更对称，但对称性是否等同于语言的可组合性或泛化能力仍需进一步探讨。
- **扩展性**：未讨论通信消息为连续/离散、数量大于两个智能体等复杂场景下的表现。

（完）
