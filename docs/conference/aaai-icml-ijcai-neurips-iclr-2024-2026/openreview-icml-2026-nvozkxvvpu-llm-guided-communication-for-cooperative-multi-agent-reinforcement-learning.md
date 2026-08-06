---
title: LLM-Guided Communication for Cooperative Multi-Agent Reinforcement Learning
title_zh: 基于大语言模型引导的协作多智能体强化学习通信
authors: "Sangjun Bae, Yisak Park, Sanghyeon Lee, Seungyul Han"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c378370b92f4bab9a0d9b5b19aec7bfbf164c3db.pdf"
tags: ["query:mcd"]
score: 10.0
evidence: 利用大语言模型为协作MARL设计通信协议
tldr: 针对多智能体部分可观测环境中通信效率低、状态信息传递不足的问题，本文提出LLM驱动的通信方法LMAC。LMAC借助大语言模型推理能力设计通信协议，使各智能体尽可能准确且一致地重建底层状态，并通过显式状态感知准则迭代优化协议。在多个MARL基准任务上，LMAC显著改善状态重建质量和协作性能。该方法展示了将LLM先验知识融入MARL通信的潜力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多智能体通信方法信息交换低效或无法充分传递状态信息，难以为决策提供足够依据。
method: 利用大语言模型推理能力生成通信协议，并以状态重建准确度作为显式准则迭代优化协议，促进智能体间知识一致。
result: 在多种MARL基准上改善了跨智能体的状态重建，并提升协作性能。
conclusion: 为MARL通信设计提供了一种利用LLM先验的新范式，可扩展至更复杂的部分可观测协作任务。
---

## Abstract
Communication is a key component in multi-agent reinforcement learning (MARL) for mitigating partial observability, yet prior approaches often rely on inefficient information exchange or fail to transmit sufficient state information. To address this, we propose LLM-driven Multi-Agent Communication (LMAC), which leverages an LLM's reasoning capability to design a communication protocol that enables all agents to reconstruct the underlying state as accurately and uniformly as possible. LMAC iteratively refines the protocol using an explicit state-awareness criterion, improving state recovery while narrowing differences in agents' knowledge. Experiments on diverse MARL benchmarks show that LMAC improves state reconstruction across agents and yields substantial performance gains over prior communication baselines.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **背景**：在多智能体强化学习（MARL）中，部分可观测性是一个关键挑战，智能体需要依靠通信来获取全局信息。
- **现有问题**：已有的通信方法通常存在信息交换效率低下，或者无法传递足够、有效的状态信息，导致决策依据不充分。
- **核心问题**：如何设计高效且有效的通信协议，使所有智能体能够尽可能准确且一致地重建底层全局状态，从而提升协作性能。
- **整体含义**：该论文提出一种新的范式，将大语言模型（LLM）的推理能力引入MARL通信协议设计，为通信协议学习提供先验知识，具有重要的研究价值。

## 2. 论文提出的方法论

- **核心思想**：利用LLM的推理能力自动生成并迭代优化通信协议，使各智能体重建底层状态的准确性和一致性最大化。
- **方法名称**：LMAC（LLM-driven Multi-Agent Communication）。
- **关键技术细节**：
  - 使用LLM生成本质上的通信协议（包括消息格式、内容选择等）。
  - 引入显式的“状态感知准则”（state-awareness criterion），用于评估通信协议对状态重建的帮助程度。
  - 根据该准则迭代细化协议，同时优化状态恢复质量，并缩小智能体间知识差异。
- **算法流程（据摘要推断）**：
  1. 初始通信协议由LLM生成；
  2. 智能体依据当前协议进行通信和决策；
  3. 用状态重建准确度作为显式准则评估协议质量；
  4. 迭代更新协议，直到收敛。
- **说明**：由于仅提供摘要，具体公式和实现细节未呈现，以上为逻辑层面的概括。

## 3. 实验设计

- **基准场景**：在多个MARL基准任务上进行了评估（摘要称“diverse MARL benchmarks”），但未具体列出如SMAC、MPE等标准环境名称。
- **对比方法**：与先前多个通信基线方法进行比较，但具体基线名称未在摘要中给出。
- **评估指标**：跨智能体的状态重建质量，以及协作性能（如累积奖励）指标。

## 4. 资源与算力

- **未提及**：提供的摘要和元数据中没有说明使用的GPU型号、数量、训练时长等算力信息。
- **结论**：无法评估方法计算成本或可复现性。

## 5. 实验数量与充分性

- 摘要声称在“多个基准”上取得效果，暗示实验涵盖多种环境或任务，但**没有具体实验数量、消融研究、统计显著性检验**等信息。
- 由于缺少全文，无法判断实验是否足够全面、是否包含消融分析，以及对比是否公平。
- 从摘要的正面语气看，结果可能有一定说服力，但**客观充分性需阅读全文后才能确认**。

## 6. 论文的主要结论与发现

- LMAC在多个MARL基准上显著改善了跨智能体的状态重建。
- 相比现有通信基线，协作性能提升明显。
- 证明了LLM的先验知识可以用于指导MARL通信协议设计，为后续研究提供了新方向。

## 7. 优点

- **创新性**：首次将LLM推理能力应用于MARL通信协议设计，突破了传统学习方法的局限。
- **准则明确**：以显式状态重建准确度作为优化目标，学习过程更具可解释性和针对性。
- **通用潜力**：方法框架可能扩展到更复杂的部分可观测协作任务。
- **解决问题直接**：同时兼顾通信效率与信息充分性，直击现有方法缺陷。

## 8. 不足与局限

- **信息不足**：基于提供的摘要内容，无法获得具体实验细节、参数设置和算法伪代码，限制了深入评估。
- **计算成本**：LLM参与通信协议生成可能带来较高的推理开销和延迟，对实时MARL应用可能构成挑战（推断）。
- **依赖先验**：通信协议质量严重依赖LLM的先验知识，在特定领域可能失效或需要额外调优。
- **可扩展性未知**：智能体数量增加或状态空间变大时，LLM参与协议生成的可扩展性未在摘要中讨论。
- **实验结果不透明**：缺少消融实验、失败案例分析以及与基线方法的显著性检验，可能存在报告偏差风险。

（完）
