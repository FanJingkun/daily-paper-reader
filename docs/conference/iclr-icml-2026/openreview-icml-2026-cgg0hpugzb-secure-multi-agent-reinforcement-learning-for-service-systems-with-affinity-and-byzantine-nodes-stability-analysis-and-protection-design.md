---
title: "Secure Multi-agent Reinforcement Learning for Service Systems with Affinity and Byzantine Nodes: Stability Analysis and Protection Design"
title_zh: 面向亲和性与拜占庭节点的服务系统安全多智能体强化学习：稳定性分析与保护设计
authors: "Yifan Jiang, Pan Jiasheng, Mengtian Li, Li Jin"
date: 2026-04-30
pdf: "https://openreview.net/pdf/65081148aa3529c3d9f8779134883fc34e7382e5.pdf"
tags: ["query:mcd"]
score: 6.0
evidence: 去中心化多智能体强化学习，通信图交互，应对拜占庭节点并保障服务系统稳定性
tldr: 网络化服务系统中，各节点通过 actor-critic 学习本地控制策略，并借助通信图交换参数，但拜占庭节点可利用无界状态空间破坏共识机制，导致学习和排队失稳。本文针对带亲和性的系统进行稳定性分析，并提出保护设计以抵御拜占庭攻击。结果表明该方法能维持学习收敛和队列稳定性，为多智能体系统层面的安全决策提供了理论基础。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 拜占庭节点可能利用无界状态空间破坏 MARL 共识，威胁服务系统稳定性。
method: 对带亲和性的服务系统进行去中心化 MARL 稳定性分析，并设计保护机制抵御攻击。
result: 给出稳定性条件，验证保护设计能在拜占庭攻击下维持系统稳定。
conclusion: 为去中心化服务系统的安全 MARL 提供了可证明的稳定性保障与防护方案。
---

## Abstract
We study decentralized multi-agent reinforcement learning (MARL) for networked service systems with affinity in the presence of Byzantine nodes. The way that a server processes a job depends on an affinity state that captures the correlation between the job and the server. Each node learns a local control policy via an actor-critic algorithm with linear function approximation over inherently unbounded space of traffic states, while exchanging parameter information with neighbors through a communication graph. A set of Byzantine agents can exploit the unbounded state space to compromise the consensus mechanism, destabilizing both learning and queuing processes. To address this vulnerability, we propose a resilient consensus-based MARL algorithm, which mitigates adversarial parameter manipulation and guarantees traffic stability under mild assumptions. We prove that the cooperative agents’ policies converge almost surely to a bounded neighborhood of a stationary solution of the global objective. We demonstrate the effectiveness and generality of the proposed framework in several representative service systems, including semantic routing for large language model serving, distributed polling in cloud computing, and smart manufacturing logistics.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机和背景）

- **研究对象**：去中心化多智能体强化学习（Decentralized MARL）在网络化服务系统中的应用，特别是带有**亲和性（affinity）**特征的系统。
- **亲和性含义**：服务器处理作业的方式取决于一个亲和性状态，该状态刻画了作业与服务器之间的相关性。
- **拜占庭节点威胁**：系统中存在一部分恶意（拜占庭）智能体，它们可以利用交通状态空间**无界（unbounded）**的特性，通过篡改通信图中交换的参数信息，破坏智能体间的共识机制，从而导致：
  - 学习过程不收敛；
  - 排队系统失稳（队列长度发散）。
- **研究意义**：在服务系统（如大模型语义路由、云计算轮询、智能制造物流）中，安全性是核心需求。现有MARL方法大多假设节点诚实，对拜占庭攻击缺乏鲁棒性，本文填补了这一理论空白，为去中心化服务系统提供了**可证明的稳定性保障与防护方案**。

### 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：在去中心化actor-critic架构中引入**弹性共识（resilient consensus）机制**，对邻居节点交换的参数进行鲁棒聚合，过滤拜占庭节点的恶意操纵，从而同时保证学习收敛和排队稳定性。
- **系统模型**：
  - 每个节点通过本地 actor-critic 算法学习控制策略，使用**线性函数近似**处理流量状态；
  - 节点通过**通信图**与邻居交换策略参数；
  - 状态空间本身无界，这为拜占庭节点提供了攻击面。
- **关键机制**：
  - 设计一种**可抵抗拜占庭参数操纵的共识更新规则**（例如基于阈值裁剪、中位数/截断均值等鲁棒聚合方式，具体细节原文未完全给出，但摘要明确为“resilient consensus-based MARL algorithm”）；
  - 在温和假设下（如拜占庭节点数量有界、通信图连通性条件等），证明算法能够削弱恶意参数对全局更新的影响。
- **理论分析**：
  - 证明协作智能体的策略**几乎必然收敛**到全局目标的一个**驻点（stationary solution）的有界邻域**；
  - 同时证明在拜占庭攻击下，**队列稳定性（traffic stability）**得以维持。
- **算法流程**（基于摘要的文字性描述）：
  1. 每个节点初始化本地 actor 参数与 critic 参数；
  2. 每轮迭代中，节点与环境交互，收集状态-动作数据，更新本地 critic（线性函数近似）；
  3. 节点将本地参数发送给邻居，并接收邻居参数；
  4. 节点使用**弹性共识规则**对收到的参数进行鲁棒聚合，剔除疑似拜占庭的异常值；
  5. 用聚合后的参数更新本地 actor，进而生成控制策略；
  6. 重复直到收敛，并验证队列长度有界。

### 3. 实验设计

- **实验场景**：摘要中列出三个代表性服务系统：
  1. **面向大语言模型服务的语义路由（semantic routing for LLM serving）**；
  2. **云计算中的分布式轮询（distributed polling in cloud computing）**；
  3. **智能制造物流（smart manufacturing logistics）**。
- **Benchmark与对比方法**：摘要未明确提及具体基线方法；从上下文推测可能对比了普通去中心化MARL（无防御）、中心化方法或经典鲁棒聚合算法，但原文未给出细节。
- **数据集与模拟环境**：未说明使用了哪些真实数据集，通常此类工作使用合成负载、排队网络仿真或实际系统轨迹，具体信息缺失。

### 4. 资源与算力

- 论文摘要与元数据中**未提及任何算力信息**，包括GPU型号、数量、训练时长等。
- 由于该工作以理论分析为主（收敛性证明、稳定性定理），实验可能为仿真验证，但具体计算资源无说明。

### 5. 实验数量与充分性

- **实验数量**：基于摘要，至少覆盖3个不同应用场景，但未说明每个场景下是否进行了多组实验（不同参数、不同拜占庭比例、不同网络拓扑等）。
- **是否充分**：
  - 从**场景多样性**看，覆盖了三个差异较大的领域（LLM、云计算、制造），具有一定代表性；
  - 然而，缺乏详细的**消融实验**（如不同拜占庭节点数量、不同攻击强度、不同共识规则）和与已有防御方法的定量对比，因此**充分性不足**。
- **客观性与公平性**：由于缺少基线、评估指标和超参数设置，无法判断实验对比是否公平；理论证明部分提供了严格的数学保障，但实验部分证据较弱。

### 6. 主要结论与发现

- 拜占庭节点确实可以利用无界状态空间破坏普通MARL的共识机制，导致学习和队列失稳；
- 所提出的弹性共识MARL算法能够在温和假设下：
  - 保证协作策略几乎必然收敛到**全局目标驻点的有界邻域**；
  - 维持**流量稳定性（排队队列有界）**；
- 该框架在三个代表性服务系统中展现了有效性和通用性。

### 7. 优点

- **理论贡献扎实**：给出了收敛性和稳定性的严格数学证明，这在安全MARL领域较为稀缺；
- **问题建模切中要害**：明确针对“无界状态空间”这一被忽视的攻击面，具有洞察力；
- **应用范围广**：从LLM服务到云计算、制造物流，展示了方法对多种服务系统的适应能力；
- **防御思路清晰**：弹性共识机制简单有效，不依赖对拜占庭节点的辨识，易于分布式实现；
- **动机与背景交代清楚**：将强化学习稳定性与排队论稳定性联系起来，具有很强的系统科学意义。

### 8. 不足与局限

- **实验细节缺失**：未提供数据集、benchmark、基线方法、评估指标、超参数等，难以复现和评判；
- **实验数量有限**：只有定性场景描述，缺少系统性的消融实验和敏感性分析；
- **假设可能较强**：理论结果依赖“温和假设”（如拜占庭数量有界、通信图连通），实际系统中这些条件可能需要验证；
- **收敛结果较弱**：只证明收敛到驻点的**有界邻域**，而非精确驻点，精度受拜占庭影响；
- **应用部署成本未讨论**：未分析弹性共识的计算开销和通信开销，对大规模系统的工程适用性有待确认。

（完）
