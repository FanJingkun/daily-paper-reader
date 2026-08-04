---
title: "Benchmarking LLMs' Swarm intelligence"
title_zh: 基准测试大语言模型的群体智能
authors: "Kai Ruan, Mowen Huang, Ji-Rong Wen, Hao Sun"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GAVA5zqtVB"
tags: ["query:maspd"]
score: 8.0
evidence: 在网格环境中用LLM测试追捕、觅食、集群、运输等群体协调任务
tldr: 现有基准忽略了去中心化协调中不完整时空信息的挑战。本文提出SwarmBench，在二维网格中设置追捕、同步、觅食、集群和运输五类群体协调任务，让LLM智能体仅依靠局部感知与局部通信进行决策。零样本评估揭示了群体动态的涌现现象，为研究LLM的群体智能提供系统化基准。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 缺少对LLM在严格群体约束下涌现协调能力的系统性评测。
method: 构建SwarmBench基准，包含五个网格群体任务，用局部视野和通信评测LLM去中心化协调。
result: 零样本评测显示LLM在简单协调中有效但复杂任务上有局限，并提出协调有效性指标。
conclusion: SwarmBench可系统评估LLM群体智能，为多智能体空间协作研究提供基准。
---

## Abstract
Large Language Models (LLMs) show reasoning potential, but their capacity for emergent coordination in Multi-Agent Systems (MAS) under strict swarm-like constraints (e.g., limited local perception and communication) remains unexplored. Existing benchmarks often overlook the challenges of decentralized coordination with incomplete spatio-temporal information. We introduce SwarmBench, a benchmark to systematically evaluate the swarm intelligence of LLMs as decentralized agents. SwarmBench features five MAS coordination tasks (Pursuit, Synchronization, Foraging, Flocking, Transport) in a 2D grid where agents rely on local sensory input ($k\times k$ view) and local communication. We propose metrics for coordination effectiveness and analyze emergent group dynamics. Zero-shot evaluations of leading LLMs (e.g., deepseek-v3, o4-mini) reveal task-dependent performance variations. While showing rudimentary coordination, current LLMs struggle with long-range planning and adaptive strategy formation under decentralized uncertainty. Assessing LLMs under such constraints is crucial for their application in future decentralized systems. We release SwarmBench as an open, extensible toolkit with environments, prompts, evaluation scripts, and comprehensive datasets. It aims to foster research into LLM-based MAS coordination under severe informational decentralization.

---

## 论文详细总结（自动生成）

## 论文总结：SwarmBench——基准测试大语言模型的群体智能

### 1. 核心问题与整体含义
- **研究背景**：大语言模型（LLMs）展现出强大的推理能力，但在多智能体系统（MAS）中，它们在严格群体约束条件下的涌现协调能力（如有限的局部感知与通信）尚未被系统性探索。
- **核心问题**：现有基准多侧重于集中式协调或全局信息假设，忽略了**去中心化协调中不完整时空信息**这一关键挑战。
- **整体含义**：该论文旨在回答“LLM在仅有局部信息与局部通信的分散化条件下，能否形成有效的群体智能行为？”这一问题，为未来去中心化系统的实际应用提供评估基础。

### 2. 方法论
- **核心思想**：构建一个系统化基准 **SwarmBench**，在二维网格环境中让LLM智能体作为去中心化代理执行群体任务，仅依赖局部感知（k×k视野）与局部通信进行决策。
- **任务设计**：包含五类MAS协调任务：
  - 追捕（Pursuit）
  - 同步（Synchronization）
  - 觅食（Foraging）
  - 集群（Flocking）
  - 运输（Transport）
- **关键技术细节**：
  - 智能体只能看到自身周围k×k范围内的环境信息；
  - 通信仅限于局部范围，模拟严格的群体约束；
  - 采用**零样本评估**方式，测试LLM的即时协调能力。
- **评估指标**：提出**协调有效性指标（coordination effectiveness metrics）**，用于量化群体协作效果的优劣。
- 注：原文提供的材料仅含摘要，未提供具体的算法公式或伪代码，此处只能以文字描述整体流程框架。

### 3. 实验设计
- **Benchmark**：SwarmBench（开放、可扩展的工具包，包含环境、提示词、评估脚本与数据集）。
- **场景/数据集**：在二维网格中设置上述五类群体协调任务，每个任务均为多智能体空间协作场景。
- **评估方法**：零样本（zero-shot）评估，不进行任务特定微调。
- **对比模型**：主要评测了多个前沿LLM，包括 **deepseek-v3** 与 **o4-mini**。
- **对比方式**：由于是零样本基准测试，主要比较不同LLM在同类任务上的表现差异，而非与特定强化学习或经典MAS算法对比（摘要中未提及与其他非LLM方法的具体对比）。

### 4. 资源与算力
- 论文提供的元数据与摘要中**未明确说明**所使用的GPU型号、数量、训练时长或推理算力配置。
- 作为零样本评估类工作，推断其主要开销在于大规模推理（LLM前向推理），而非训练，但具体算力需求无法从现有信息中确认。

### 5. 实验数量与充分性
- **可确认的实验规模**：在五个任务上对多个LLM进行了零样本评估；摘要中明确提到的模型有 deepseek-v3 与 o4-mini。
- **充分性评估**：
  - 由于公开信息仅限摘要，无法确认每个任务的具体回合数、重复实验次数、随机种子设置或详细消融实验（如不同k值视野大小、通信范围的影响、不同提示词设计的影响）。
  - 从已有描述看，实验覆盖了任务维度与模型维度的横向比较，但**缺乏系统性消融**和**统计显著性分析**的描述，亦未提及与非LLM基线（如经典群体算法）的对比。
  - 总体而言，可作为初步基准验证，但充分的客观性与公平性需依赖全文实验细节进一步佐证。

### 6. 主要结论与发现
- LLM在简单协调任务中能表现出**初步的协调能力**（rudimentary coordination）。
- 在复杂任务中，LLM在**长程规划（long-range planning）** 与**自适应策略形成（adaptive strategy formation）** 方面存在明显不足，尤其在去中心化不确定性条件下表现受限。
- 不同LLM在不同任务上表现具有**任务依赖性（task-dependent performance variations）**，不存在单一模型在所有任务上全面领先。
- 零样本评估揭示了群体动态中的**涌现现象（emergent group dynamics）**，说明LLM具备一定的自发协调倾向，但尚未达到强鲁棒性水平。

### 7. 优点
- **填补空白**：首次系统性地将LLM置于严格群体约束场景下评估，弥补了现有基准对不完整时空信息关注的不足。
- **任务多样性**：五类群体任务涵盖捕猎、同步、觅食、集群、运输等典型去中心化协调场景，覆盖面广。
- **评估指标专门化**：提出协调有效性指标，针对群体协作特性而非单个智能体能力进行度量。
- **开放性与可扩展性**：公开了环境、提示词、评估脚本与数据集，便于后续研究者复现与扩展。
- **零样本设计**：避免了微调带来的任务泄露风险，更能反映LLM本身的内禀群体推理能力。

### 8. 不足与局限
- **信息不透明**：由于可获得的材料仅元数据与摘要，无法验证实验细节（如回合数、重复次数、消融设计、统计显著性），降低了结果可信度的可评估性。
- **模型覆盖有限**：仅提到了 deepseek-v3 和 o4-mini 两个模型，缺少对更大规模LLM（如GPT-4、Claude系列）或开源小模型的广泛覆盖。
- **缺乏非LLM基线**：未提及与经典MAS算法（如蚁群、粒子群、强化学习群体方法）的直接对比，因此“LLM群体智能水平”缺少一个客观的基线参照系。
- **环境抽象度限制**：二维网格、局部视野与局部通信是高度简化的抽象，难以完全代表真实场景中的感知噪声、通信延迟、物理约束等复杂性。
- **仅零样本设定**：未考察少样本学习、在线适应或指令微调后再评估的情形，无法回答“LLM能否通过学习提升群体协调能力”这一扩展问题。
- **潜在偏差**：任务设计、提示词模板、评估指标本身可能对某些模型不够公平，而摘要中未提及对提示词鲁棒性的测试。

（完）
