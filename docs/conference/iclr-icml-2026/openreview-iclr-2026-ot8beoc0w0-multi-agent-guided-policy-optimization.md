---
title: Multi-Agent Guided Policy Optimization
title_zh: 多智能体引导策略优化
authors: "Yueheng Li, Guangming Xie, Zongqing Lu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=OT8beoc0W0"
tags: ["query:mcd"]
score: 9.0
evidence: 协作多智能体强化学习，集中训练分散执行与集中引导
tldr: 针对现有CTDE方法未充分利用集中训练且缺乏理论保证的问题，提出多智能体引导策略优化（MAGPO）框架。该方法以自回归联合策略进行可扩展的协调探索，并显式与去中心化策略对齐，确保部分可观测下可部署。理论证明了策略的单调改进，实证结果展示了在协作任务上的性能提升。该工作为集中指导与去中心化执行结合提供了新范式。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有CTDE方法在部分可观测和通信受限条件下未充分利用集中训练，且缺乏理论保证。
method: 提出MAGPO框架，将自回归联合策略的集中引导与去中心化执行策略显式对齐，实现可扩展的协调探索。
result: 提供单调策略改进的理论保证，并在实证中验证了方法有效性。
conclusion: 通过集中引导与去中心化执行的结合，提升了协作多智能体强化学习的性能与可部署性。
---

## Abstract
Due to practical constraints such as partial observability and limited communication, Centralized Training with Decentralized Execution (CTDE) has become the dominant paradigm in cooperative Multi-Agent Reinforcement Learning (MARL). 
However, existing CTDE methods often underutilize centralized training or lack theoretical guarantees. 
We propose Multi-Agent Guided Policy Optimization (MAGPO), a novel framework that better leverages centralized training by integrating centralized guidance with decentralized execution. 
MAGPO uses an autoregressive joint policy for scalable, coordinated exploration and explicitly aligns it with decentralized policies to ensure deployability under partial observability. 
We provide theoretical guarantees of monotonic policy improvement and empirically evaluate MAGPO on 43 tasks across 6 diverse environments. 
Results show that MAGPO consistently outperforms strong CTDE baselines and matches or surpasses fully centralized approaches, offering a principled and practical solution for decentralized multi-agent learning.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究背景**：在协作多智能体强化学习（MARL）中，由于部分可观测性（partial observability）和通信受限等现实约束，集中训练与分散执行（CTDE）成为主流范式。CTDE 允许在训练时利用全局信息，而在执行时仅依赖局部观测，兼顾性能与可部署性。
- **现有不足**：现有 CTDE 方法虽已广泛使用，但普遍存在两个问题：
  - **对集中训练的利用不充分**：许多方法只将集中信息用于训练价值函数或辅助模块，未能充分发挥集中式全局协调的潜力；
  - **缺乏理论保证**：大多数方法在策略优化过程中没有提供单调改进（monotonic improvement）之类的强理论保障，难以确保学习过程的稳定性和收敛性。
- **论文定位**：针对上述痛点，提出一个新的 CTDE 框架 **MAGPO（Multi-Agent Guided Policy Optimization，多智能体引导策略优化）**，旨在把集中式引导与分散式执行更有效地结合起来，同时提供理论支撑，从而提升协作 MARL 的性能、稳定性和可部署性。

## 2. 方法论：核心思想与技术细节

- **核心思想**：通过“集中引导（centralized guidance）+ 分散执行（decentralized execution）”的显式结合，让集中式策略在训练阶段生成具有全局协调性的指导信号，而分散策略则学习如何在局部信息下逼近这一指导，保证训练后的策略可以在部分可观测环境中实际部署。
- **关键模块与技术**：
  - **自回归联合策略（Autoregressive Joint Policy）**：用于集中的、可扩展的协调探索。该策略将联合动作按顺序逐维度/逐智能体地生成，从而在指数级动作空间中实现高效的全局协调探索，避免维度爆炸问题。
  - **显式对齐（Explicit Alignment）**：MAGPO 显式地让分散化策略与自回归联合策略的输出分布进行对齐，使得集中探索得到的协调行为能够被分散策略“学走”，从而在部分可观测条件下依然保持联合策略的协调效果。
- **公式或算法流程（文字描述）**：
  1. 在集中训练阶段，维护一个自回归联合策略，利用全局状态/信息进行协调式探索，生成高质量的联合动作样本；
  2. 同时训练各智能体的分散策略，使其在只依赖局部观测的条件下，尽可能逼近自回归联合策略给出的动作分布（通过 KL 散度或类似损失进行对齐）；
  3. 在策略优化过程中，利用集中信息计算优势函数或价值函数，并基于对齐后的分散策略进行更新；
  4. 重复上述过程直至收敛。算法最终输出既具备集中协调知识、又满足局部可观测约束的分散策略。
- **理论保证**：论文提供了**策略单调改进（monotonic policy improvement）**的理论保证，即每一步更新都能保证策略性能不会下降，这是现有许多 CTDE 方法所缺失的。

## 3. 实验设计

- **Benchmark 与场景**：论文在 **6 个不同环境**、共 **43 个任务**上进行了评估。环境覆盖常见的协作多智能体测试场景（如粒子任务、星际争霸微操、多机器人控制等，具体环境名称在元数据中未逐一列出）。
- **对比方法**：
  - **强 CTDE 基线**：如 MAPPO、QMIX、VDN 等（具体名称未在元数据中列出，但属于该领域代表性方法）；
  - **完全集中式方法**：用于对照“性能上界”的方法，评估 MAGPO 在分散执行限制下能否接近集中式方法的表现。
- **评估指标**：主要采用各任务的平均回报/成功率（具体指标未详述）。

## 4. 资源与算力

- 论文元数据中**未明确提及**使用的 GPU 型号、数量或训练时长等算力信息。
- 由于仅提供了摘要和元数据，无法得知具体的计算资源配置。若需完整了解，需查阅论文正文中的实验设置部分。

## 5. 实验数量与充分性

- **实验数量**：
  - 覆盖 **6 个环境**、**43 个任务**，实验规模在 MARL 领域中属于较为充分的范围；
  - 未提及单独的消融实验（如移除自回归联合策略、移除对齐项等），但元数据中未明确说明，不能断定没有。
- **充分性评估**：
  - **优点**：多环境、多任务测试增强了结论的普适性；同时对比了 CTDE 基线与完全集中式方法，可以从“分散-集中”两个维度定位 MAGPO 的性能区间。
  - **潜在不足**：由于缺乏消融实验和超参数敏感性分析等信息，目前无法全面评估每个组件对最终性能的贡献；另外，仅在仿真环境验证，未涉及真实机器人或大规模场景。

## 6. 主要结论与发现

- **性能优势**：MAGPO 在 43 个任务上**持续优于强 CTDE 基线**，表明其集中引导与分散对齐机制确实能有效提升分散策略的协作能力。
- **逼近集中式方法**：MAGPO 的表现能够**匹配甚至超越完全集中式方法**，这说明分散部署并不必然以牺牲性能为代价，合理的集中引导设计可以显著减少 CTDE 方法与传统集中式方法之间的性能鸿沟。
- **理论贡献**：提供了单调改进的理论保证，为算法在实际应用中的稳定性和收敛性提供了可信依据。
- **通用价值**：该框架为“集中指导与分散执行”结合提供了新范式，有望推广到更多部分可观测、通信受限的协作任务中。

## 7. 优点

- **理论严谨性**：给出了单调策略改进的证明，这在多智能体 CTDE 方法中较为难得，增强了方法的可信度。
- **方法创新性**：利用自回归联合策略实现可扩展的协调探索，并显式与分散策略对齐，解决了“集中训练信息无法有效迁移到分散执行”的关键问题。
- **实验充分**：在 6 个环境、43 个任务上验证，覆盖面广；同时对比强基线和集中式上界，评价体系合理。
- **实用性强**：设计的算法可直接用于部分可观测环境下的分散部署，契合实际应用中的通信约束。

## 8. 不足与局限

- **算力信息缺失**：论文摘要和元数据未报告训练所需的 GPU 资源、时间成本，读者难以评估其计算开销和可复现性。
- **缺乏详细消融分析**：未明确展示每个核心组件（如自回归联合策略、显式对齐、单调改进机制）的独立贡献，无法精确判断哪些设计最为关键。
- **实验环境覆盖有限**：虽然包含 6 个环境，但可能仍局限于模拟器和常见 benchmark，未讨论真实世界中的传感器噪声、通信时延、硬件限制等开放性问题。
- **理论假设的适用范围**：单调改进的证明可能依赖于特定假设（如集中信息完全可观测、智能体间协作结构假设等），这些假设在更复杂或非平稳环境中的适用性有待验证。
- **集中式方法的对比偏保守**：若集中式基线未做充分调优，则“匹配或超越集中式”的结论可能带有一定偏差；需要更多细节确认对比的公平性。
- **扩展性风险**：自回归联合策略虽然缓解了维度爆炸，但在智能体数量非常大（如数百个）时，其训练复杂度和对齐难度仍可能成为瓶颈。

（完）
