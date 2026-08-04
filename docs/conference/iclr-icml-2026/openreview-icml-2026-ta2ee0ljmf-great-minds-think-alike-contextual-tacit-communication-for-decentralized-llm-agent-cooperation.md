---
title: "Great Minds Think Alike: Contextual Tacit Communication for Decentralized LLM-Agent Cooperation"
title_zh: 英雄所见略同：面向分散式LLM智能体协作的上下文默契通信
authors: "Yue Pei, Hongming Zhang, Jiarui Guan, Jusheng Zhang, Liang Lin, Haogang Zhu, Ziliang Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/925d9b0a94ca124020f77273b98dfa6a4ff58ba2.pdf"
tags: ["query:mcd"]
score: 8.0
evidence: 面向分散式LLM智能体协作的默契通信机制
tldr: 大型语言模型作为协作具身智能体的规划器时，显式通信代价高或不具备，导致分散式决策容易失协调。本文提出上下文默契通信协议，通过联合LLM价值分数对齐分散决策，利用残差分带定位失协调动作，无需显式消息动作。该方法在部分可观测的多智能体环境中提升了协作表现，为无通信协作提供了新机制。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体LLM在部分可观测环境下显式通信成本高，移除通信后独立规划易失协调。
method: 使用联合LLM价值分数和残差分带的上下文默契通信协议，对齐分散决策且无需显式消息。
result: 在部分可观测多智能体任务中缓解失协调，提升协作性能。
conclusion: 默契通信为分散式LLM智能体提供了无显式通信的高效协作协议。
---

## Abstract
Large language models (LLMs) are increasingly used as planners for cooperative embodied agents, but multi-agent settings amplify inconsistency under partial observability and make explicit communication costly or even unavailable. Many existing approaches rely on online message passing; when communication is removed, agents often fall back to independent local planning that suffers from miscoordination. We introduce Contextual Tacit Communication, a gradient-free protocol that aligns decentralized decisions with a joint LLM value score without explicit message actions. Our method measures context-conditioned value rectifications via residual banding to pinpoint miscoordination actions and amortizes the resulting coordination signals into a retrieval-augmented Tacit Rule Memory that provides prompt-level cooperation rules at execution time. Experiments on VIKI, C-WAH, and TDW-MAT show that our approach improves cooperation performance over baselines while reducing runtime overhead compared with communication-based methods.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义

- **研究动机**：大型语言模型（LLM）正越来越多地被用作协作型具身智能体的规划器。然而，在多智能体场景中，**部分可观测性（partial observability）** 会放大智能体之间决策的不一致性，同时**显式通信（explicit communication）** 的成本高昂，甚至在许多实际环境中不可用。
- **现有方法的不足**：已有的大多数方法依赖在线的消息传递机制；当通信被移除时，智能体往往退化为独立的局部规划，导致严重的**失协调（miscoordination）** 问题。
- **核心意义**：本文探讨了一个根本性问题——“当显式通信不可行时，分散式LLM智能体如何实现高效协作？”该研究为无通信条件下的多智能体协作提供了新的思路，打破了对显式消息传递的依赖。

### 2. 方法论

- **核心思想**：提出**上下文默契通信（Contextual Tacit Communication）**——一种无需梯度训练（gradient-free）的协议，通过联合LLM价值分数对齐分散式决策，且**不需要显式的消息动作**。
- **关键技术细节**：
  1. **联合LLM价值分数**：利用一个覆盖全局上下文的LLM价值函数来评估和校准各智能体的独立决策。
  2. **残差分带（Residual Banding）**：通过对上下文条件下的价值修正进行分带处理，**精准定位导致失协调的动作**，识别出哪些决策需要调整。
  3. **默契规则记忆（Tacit Rule Memory）**：将协调信号经检索增强（retrieval-augmented）方式存储，运行阶段以提示级（prompt-level）的合作规则形式提供给各智能体。
- **算法流程（文字说明）**：智能体在局部计划时不断生成候选动作 → 计算与联合LLM价值分数之间的残差 → 通过残差分带判断是否存在失协调动作 → 将识别出的协调信号编码进 Tacit Rule Memory → 在后续执行中通过检索记忆引导各智能体制约/修正自身决策，从而达成上下文层面的“默契”。

### 3. 实验设计

- **数据集 / 场景**：使用了三个具身/多智能体合作基准场景：
  - **VIKI**
  - **C-WAH**
  - **TDW-MAT**
- **Benchmark 特点**：这些场景均包含**部分可观测**的多智能体协作任务，是评估LLM规划器协作能力的常用测试环境。
- **对比方法**：与基于通信的LLM多智能体协作方法，以及无通信的独立局部规划基线进行了对比。
- **评估指标**：主要关注协作任务完成表现，同时比较了**运行时开销（runtime overhead）**。

### 4. 资源与算力

- 原文提供的材料中**未明确说明**训练/推理所使用的 GPU 型号、数量或总时长等信息。值得一提的是，该方法本身是 gradient-free 协议，意味着不需要进行模型微调，主要算力开销在于运行时推理和检索，但具体算力细节在现有信息中缺失。

### 5. 实验数量与充分性

- **实验数量**：在三个不同基准（VIKI、C-WAH、TDW-MAT）上进行了实验，并对比了多类基线方法，同时报告了性能与开销的双重指标。
- **充分性评估**：从现有摘要来看，实验覆盖了多个异构环境，能说明方法具有一定**泛化性**。但由于未看到具体消融实验的详细描述（如去掉残差分带、移除记忆模块等），对各组件的独立贡献验证情况尚不明确，因此**实验充分性的完整判断需要全文数据才能确认**。

### 6. 主要结论与发现

- 上下文默契通信方法在**VIKI、C-WAH、TDW-MAT**三个基准上均优于基线方法，显著缓解了部分可观测环境下的失协调问题。
- 相比传统的基于显式通信的方法，该方法在提升协作表现的同时，**显著降低了运行时开销**。
- 验证了“无显式消息也能达成有效协作”这一假设的可行性，为LLM智能体在通信受限环境下的协作提供了新范式。

### 7. 优点

- **创新性强**：首次将“默契/隐性通信”概念引入LLM多智能体协作，摆脱了对显式消息的依赖。
- **高效性**：无需梯度训练，且比通信方法运行时开销更低，实用性强。
- **机制设计合理**：残差分带能够精准识别失协调动作，结合检索增强的Tacit Rule Memory，使协调信号能跨时间步复用和推广。
- **实验覆盖面广**：在多个具有代表性的具身多智能体基准上验证了有效性。

### 8. 不足与局限

- **信息透明性不足**：提供的文本中缺少关于算力资源、具体实验配置、消融实验细节等信息，限制了对此方法可复现性和效率的全面评估。
- **适用范围可能受限**：方法依赖于联合LLM价值分数作为校准信号，在现实世界中如何获取可靠联合评估函数仍是一个挑战；同时，部分可观测的假设使得在完全不可观测或高动态环境下应用存在不确定性。
- **与通信方法的比较可能不完全公平**：虽然运行时开销降低，但未说明在任务完成质量上是否以牺牲复杂长程任务为代价（现有摘要不足以判别）。
- **未讨论安全性和偏差**：LLM价值函数可能继承预训练模型中的偏见，在多智能体协作中可能造成系统性偏差，而现有材料中未涉及该风险讨论。

（完）
