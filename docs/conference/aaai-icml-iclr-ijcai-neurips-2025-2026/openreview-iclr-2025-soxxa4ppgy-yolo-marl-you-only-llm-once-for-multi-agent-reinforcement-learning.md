---
title: "YOLO-MARL: You Only LLM Once for Multi-agent Reinforcement Learning"
title_zh: YOLO-MARL：多智能体强化学习只需一次LLM调用
authors: "Yuan Zhuang, Yi Shen, Zhili Zhang, Yuxiao Chen, Fei Miao"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=SOXxa4pPGY"
tags: ["query:mcd"]
score: 5.0
evidence: 将LLM高层任务规划集成到多智能体强化学习策略学习，提升合作策略。
tldr: 本文针对深度多智能体强化学习在部分博弈中难以学到合作策略的问题，提出YOLO-MARL框架。该框架利用大语言模型的高层任务规划能力，在策略学习过程中仅调用一次LLM来提供任务分解指导，避免频繁推理的高昂代价。实验表明，在多个合作博弈环境中，该方法能显著提升多智能体的协调性能，为LLM与MARL结合提供了高效范式。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 深度多智能体强化学习在部分合作环境中难以学到有效策略，LLM推理成本高，难以频繁使用。
method: YOLO-MARL利用LLM进行一次性高层任务规划，将规划结果嵌入MARL策略学习过程，降低推理开销。
result: 在多个合作博弈环境中，性能优于基线，协调能力显著提升。
conclusion: LLM的高层规划能力可以有效引导多智能体策略学习，兼顾性能与效率。
---

## Abstract
Advancements in deep multi-agent reinforcement learning (MARL) have positioned it as a promising approach for decision-making in cooperative games. However, it still remains challenging for MARL agents to learn cooperative strategies for some game environments. Recently, large language models (LLMs) have demonstrated emergent reasoning capabilities, making them promising candidates for enhancing coordination among the agents. However, due to the model size of LLMs, it can be expensive to frequently infer LLMs for actions that agents can take. In this work, we propose You Only LLM Once for MARL (YOLO-MARL), a novel framework that leverages the high-level task planning capabilities of LLMs to improve the policy learning process of multi-agents in cooperative games. Notably, for each game environment, YOLO-MARL only requires one time interaction with LLMs in the proposed strategy generation, state interpretation and planning function generation modules,  before the MARL policy training process. This avoids the ongoing costs and computational time associated with frequent LLMs API calls during training. Moreover, the trained decentralized normal-sized neural network-based policies operate independently of the LLM. We evaluate our method across three different environments and demonstrate that YOLO-MARL outperforms traditional MARL algorithms.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：深度多智能体强化学习（MARL）在合作决策任务中表现出巨大潜力，但在某些合作博弈环境中，智能体仍难以自发学习到有效的协同策略。与此同时，大语言模型（LLM）展现出强大的涌现式推理与任务规划能力，理论上可以辅助多智能体实现更高层级的协调。
- **核心矛盾**：直接利用LLM指导或控制智能体动作时，由于LLM模型体积巨大、API推理昂贵，频繁调用 LLM 来决策会产生高昂的计算和时间成本，难以应用于大规模训练场景。
- **核心问题**：如何在利用LLM高层推理能力提升MARL合作策略学习效果的同时，规避训练过程中频繁调用LLM的高额开销？
- **整体含义**：该工作提出一种“只用一次LLM”的框架 YOLO-MARL，将LLM的高层任务规划能力注入策略学习流程，使训练得到的常规规模神经网络策略既能获得LLM的规划先验，又不依赖LLM在线推理，从而兼顾性能与效率。

## 2. 提出的方法论

- **核心思想**：将LLM的角色从“在线决策者”转变为“离线规划先验提供者”。在每个游戏环境中，仅在 MARL 策略训练开始前调用一次 LLM，生成高层任务规划信息，后续所有智能体策略完全由普通神经网络训练得到，训练和推理阶段不再与 LLM 交互。
- **关键技术细节**：论文提出三个利用 LLM 的模块：
  - 策略生成（strategy generation）：借助LLM生成合作层次的整体策略思路或子任务分解，为智能体的行为提供结构化指导。
  - 状态解释（state interpretation）：利用LLM对复杂或高维状态空间进行语义解释，帮助智能体理解环境状态的关键特征与任务含义。
  - 规划函数生成（planning function generation）：基于任务分解和状态语义，生成可用于塑造奖励函数或辅助策略学习的规划函数。
- **训练流程**（文字描述）：
  1. 在训练前的初始化阶段，对环境状态、任务描述等信息进行一次LLM调用，产出各模块的规划与解释结果；
  2. 将LLM生成的规划信息嵌入到 MARL 策略学习过程中（例如以辅助奖励、额外特征或课程引导的形式）；
  3. 使用标准的强化学习或 MARL 算法对常规规模神经网络策略进行训练；
  4. 训练完成后，得到去中心化的、可独立运行的策略网络，运行时完全脱离LLM。
- **现有材料未明确提供**：具体算法流程图、数学公式、嵌入方式的形式化定义未在摘要中给出，属于论文正文细节。

## 3. 实验设计

- **使用场景**：在三个不同的合作博弈环境上进行评估（具体环境名称如粒子环境、星际争霸微操等未在摘要中列出）。
- **对比方法**：与传统 MARL 算法基线进行比较（具体基线算法名称未在摘要中列出）。
- **评价指标**：以多智能体合作任务的成功率或累计奖励为核心衡量指标。
- **总体结果表明**：YOLO-MARL 在三类环境中均优于传统 MARL 算法。

## 4. 资源与算力

- 论文提供的摘要文本中**未明确说明**使用的 GPU 型号、GPU 数量、训练时长、API 调用成本等具体算力信息。
- 唯一与该维度相关的设计亮点为：本方法在整个训练流程中只需要一次 LLM API 调用，因此可以推断其相比

## 5. 实验数量与充分性

- **实验数量**：报告了三个不同环境上的对比实验，属于基础性的多环境验证，对于方法有效性的初步证明而言覆盖范围尚可。
- **未在摘要中体现的内容**：未提及消融实验（如是否每个模块都必要）、不同 LLM 型号的敏感性分析、不同 MARL 基线的数量、种子数、统计显著性检验、训练曲线或方差表示。
- **公平性与客观性分析**：由于缺少关于基线是否强化的细节，以及环境、奖励、超参设置的统一策略说明，无法完全判定实验对比的公平性；但该方法能在多个环境中稳定超过传统算法，仍然有一定的说服力。

## 6. 主要结论与发现

- LLM 的高层任务规划能力能够有效引导强化学习智能体学习到更好的合作策略。
- 仅需在每环境中一次性调用 LLM，即可显著提升多智能体协调性能，避免了在线LLM推理的开销。
- 训练完成后得到的策略是常规大小的去中心化神经网络，推理时可完全离线运行，不依赖LLM，具备实际部署价值。
- 该方法“一次调用、长期受益”的范式，为LLM与MARL结合提供了一种低成本的可行方向。

## 7. 优点

- **成本效率突出**：相比每步/每回合调用LLM的方式，本方法只需一次调用即可完成先验注入，工程可行性高。
- **训练/推理解耦**：LLM只在训练前使用，训练后的策略独立于LLM，保证了部署时的低延迟和高可用性。
- **模块化设计**：策略生成、状态解释和规划函数生成三个模块分工清晰，便于未来替换或扩展不同能力的LLM。
- **拓展了LLM在MARL中的角色**：将LLM从“在线动作生成器”转变为“离线任务规划器”，视角新颖且更贴近实际应用约束。
- **实验初步显示通用性**：在三个不同合作环境中均超过传统 MARL 基线，体现一定泛化能力。

## 8. 不足与局限

- **实验覆盖相对有限**：仅三个合作环境，且未在摘要中明确指出环境和基线名称，缺乏大规模复杂场景（如更多智能体数量、混合协作-竞争场景、真实世界任务）的验证。
- **缺乏消融与分析**：未说明去掉任一模块后性能下降多少，也无法确认三个模块各自的贡献度，难以判断哪些机制真正有效。
- **LLM先验可能存在偏差**：LLM生成"策划"或"状态解释"可能带有模型固有的语义偏见或幻觉错误，在部分环境中可能误导策略学习；论文未讨论如何校验或兜底规划函数的质量。
- **理论层面尚浅**：如何保证LLM给出的规划先验与强化学习优化目标的一致性，缺乏理论分析，只能依赖实验经验。
- **可复现性与超参刻画不足**：LLM提示词设计、API型号选择、规划函数如何转化为奖励等细节在公开摘要中未展示，直接影响该研究的可复现性。

（完）
