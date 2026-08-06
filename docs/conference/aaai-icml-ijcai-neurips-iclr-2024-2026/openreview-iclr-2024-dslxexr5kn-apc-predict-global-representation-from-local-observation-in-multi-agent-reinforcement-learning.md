---
title: "APC: Predict Global Representation From Local Observation In Multi-Agent Reinforcement Learning"
title_zh: APC：在多智能体强化学习中从局部观测预测全局表征
authors: "Xiaoyang Li, Guohua Yang, Dawei Zhang, Tianzhu Zhang, Jianhua Tao"
date: 2023-09-19
pdf: "https://openreview.net/pdf?id=DslxExr5Kn"
tags: ["query:mcd"]
score: 9.0
evidence: 演员网络学习预测全局表征，连接集中训练与分布式执行
tldr: 针对合作型多智能体强化学习在同步执行时只能使用局部观测、无法利用全局信息的问题，提出演员预测评论家（APC）算法。该算法基于演员-评论家架构，让演员学习从局部观测预测全局表征，从而在执行阶段也能获得全局信息以增强协作。实验结果表明该方法可有效改善合作任务的性能。
source: ICLR-2024-Rejected-Public
selection_source: conference_retrieval
motivation: 多智能体协作任务中，全局信息仅训练时可用，执行时各智能体只能使用局部观测，限制了协作能力。
method: 提出APC算法，在演员-评论家架构下让演员学习从局部观测预测全局表征，在执行时利用预测的全局信息。
result: 实验验证APC能有效提升合作任务的性能，改善了同步执行下的智能体协作效果。
conclusion: 通过在执行时预测全局表征，弥补了集中训练与分布执行之间信息利用的鸿沟。
---

## Abstract
Multi-agent reinforcement learning (MARL) algorithms with sequential decision-making strategies have achieved great success in cooperation tasks recently. To overcome the non-stationarity problem, these methods design a centralized controller that takes global observation as input and chooses actions for each agent in sequence. However, in most scenarios, global information is only available at training time, while agents act synchronously with their local observation at execution time, which prevents agents from leveraging more information in cooperation. In this paper, based on actor-critic architecture, we propose the actor-predicts-critic (APC) algorithm, in which the actor learns to predict the global representations of centralized critic from local observation. During the training, the actor not only receives the estimated state values, but also takes the critic's representations that are extracted from global information as the prediction targets. Since these global representations are closely related to agents' goals and rewards, agents can achieve better cooperation on MARL tasks utilizing the predicted representations. To prove the validity of APC, we evaluate the algorithm on StarCraft2, Google Research Football, and Multi-Agent Mujoco benchmarks. The results show that APC significantly outperforms the strong baselines in centralized training and decentralized execution (CTDE) framework, including MAT-Dec, MAPPO, and fine-tuned QMIX.

---

## 论文详细总结（自动生成）

# 中文总结：APC：在多智能体强化学习中从局部观测预测全局表征

## 1. 核心问题与研究动机
- 多智能体强化学习（MARL）在合作任务中通常采用“顺序决策”策略，并设计一个集中式控制器，以全局观测为输入、为每个智能体依次选择动作，从而缓解环境非平稳性问题。
- 在大多数实际场景中，全局信息只能在训练阶段获得；执行阶段智能体往往只能利用各自的局部观测**同步地**做出决策。
- 这种“集中训练、分布式执行”（CTDE）框架下的信息不对称，导致智能体在执行时无法利用全局信息，限制了合作效果。
- 论文要解决的核心问题：**如何让智能体在执行阶段也能获得与全局信息相关的表征，从而提升协作性能**。

## 2. 方法论：演员预测评论家（APC）
- 论文提出基于演员-评论家（Actor-Critic）架构的算法 **APC（Actor-Predicts-Critic）**。
- 核心思想：让 **演员（Actor）学会从局部观测中预测集中式评论家（Critic）所使用的全局表征**。
- 训练时，演员网络不仅接收估计的状态价值（state value）作为强化学习信号，还以评论家从全局信息中提取的表示（global representations）作为额外的预测目标。
- 由于这些全局表征与智能体的目标和奖励密切相关，智能体在执行阶段利用预测出的全局表征，可以更好地做出协调决策。
- 算法流程（文字描述）：
  1. 集中训练阶段：评论家使用全局观测提取表征并估计状态价值；
  2. 评论家的全局表征被复制为“预测目标”；
  3. 演员网络接收局部观测，通过辅助预测任务学习生成与全局表征尽量一致的表示；
  4. 同时，演员仍按照标准 actor-critic 方式接收策略梯度/状态价值信号进行策略更新；
  5. 分布式执行阶段：演员仅依赖局部观测，但利用已经学会的“全局表征预测能力”输出动作，从而近似获得全局信息带来的协作收益。
- 原文未给出具体公式或损失函数细节，仅概括了上述框架。

## 3. 实验设计
- 基准环境（Benchmark）：
  - **StarCraft II**（星际争霸 II，通常为 SMAC 微操场景）
  - **Google Research Football**（谷歌研究足球）
  - **Multi-Agent MuJoCo**（多智能体 MuJoCo 连续控制）
- 对比方法：
  - **MAT-Dec**（基于 Transformer 的顺序决策方法）
  - **MAPPO**（多智能体 PPO，强 baseline）
  - **fine-tuned QMIX**（经过调优的 QMIX）
- 实验目标：验证 APC 在 CTDE 框架下是否显著优于上述强基线。

## 4. 资源与算力
- 论文提供的文本中**未明确说明**使用的 GPU 型号、数量、训练时长、环境配置等资源信息。
- 因此无法从当前摘要文本中评估其训练成本或可复现性细节。

## 5. 实验数量与充分性
- 从摘要看，实验覆盖了**三类具有代表性的 MARL 基准**，对比了三个强基线方法，覆盖面较广。
- 但在其提供的内容中，**没有给出具体实验组数、随机种子数、标准差、消融实验、超参数敏感性分析、曲线图等**。
- 因此，仅凭摘要**无法充分判断**实验是否全面、客观、严格；需要查看论文正文或附录才能确认实验设计的完整性。
- 另外，来源信息标记为 **ICLR-2024-Rejected-Public**，提示该论文在评审中可能存在问题，但当前文本未包含详细评审意见。

## 6. 主要结论与发现
- 实验结果显示，APC 在 StarCraft II、Google Research Football 和 Multi-Agent MuJoCo 上，均**显著优于** MAT-Dec、MAPPO 和 fine-tuned QMIX 等强基线。
- 论文认为，通过让演员在执行阶段预测全局表征，能够有效弥补集中训练与分布式执行之间的信息鸿沟，从而提升合作任务的性能。

## 7. 优点与亮点
- **思路新颖**：将“评论家中的全局表征”作为演员的预测目标，而不是直接预测全局状态/动作，目标信息更紧密地与任务奖励和协作目标相关。
- **框架简单通用**：基于 actor-critic 架构，易于集成到主流 CTDE 算法中。
- **针对痛点明确**：直接解决同侪 MARL 在执行时无法使用全局信息的问题。
- **实验场景多样**：覆盖离散决策（SMAC）、团队对抗（足球）和连续控制（MAMuJoCo），有利于证明方法的泛化性。

## 8. 不足与局限
- **信息不完整**：当前文本缺少公式、伪代码、训练细节、超参数设置、奖励设计等关键内容。
- **实验细节缺失**：未说明种子数、重复次数、显著性检验、消融实验、可视化分析等，难以判断结果是否稳健。
- **算力成本未披露**：无法评估训练效率和部署成本。
- **通用性仍有限**：APC 依赖评论家的“全局表征”质量；在更复杂、部分可观测或大规模智能体场景中是否有效仍需进一步验证。
- **来源标注为 ICLR 2024 被拒论文**，可能意味着方法在评审中受到质疑，如与现有方法对比的公平性、增益是否来自“预测表征”本身或额外网络容量，都没有在摘要层面给出解释。

（完）
