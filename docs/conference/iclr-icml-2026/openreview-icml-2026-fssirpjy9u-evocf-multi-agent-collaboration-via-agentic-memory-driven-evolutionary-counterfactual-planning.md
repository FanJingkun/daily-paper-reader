---
title: "EvoCF: Multi-Agent Collaboration via Agentic Memory-Driven Evolutionary Counterfactual Planning"
title_zh: EvoCF：基于智能体记忆的演化反事实多智能体协作规划
authors: "Haotian Chi, Zeyu Feng, Xingrui Yu, Linbo Luo, Yew-Soon Ong, Ivor Tsang, Hechang Chen, Yi Chang, Haiyan Yin"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9fad72d7b7bbe2d670d065bff44b04133136bbb2.pdf"
tags: ["query:maspd"]
score: 7.0
evidence: 面向多智能体具身协作的演化反事实规划，涉及物理与协调约束
tldr: 多智能体具身系统协作规划常忽略现实物理与协调约束，导致LLM规划器难以生成可行策略。EvoCF提出基于智能体记忆的演化反事实规划框架，从失败中归纳符号化约束形成演化规则库，并通过规则条件变异系统地探索语义一致的策略变体，从而发现更优的多智能体协作方案。该方法为具身多智能体协调规划提供了新的生成与评估范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM规划器难以捕捉多智能体具身环境中物理与协调约束，导致协作策略不可行。
method: 提出符号约束归纳器从失败中抽取可复用约束，并利用演化反事实规划生成器进行规则条件化变异探索。
result: 在具身多智能体协作任务中生成更优协作策略，改善物理约束下的协调表现。
conclusion: 演化反事实规划结合经验记忆可有效提升多智能体具身协作规划能力。
---

## Abstract
Planning collaboration strategies for multi-agent embodied systems remains a core challenge for LLM-based planners, which often fail to capture the physical and coordination constraints of realworld environments. To address this, we present EvoCF, an agentic memory-driven evolutionary counterfactual planning framework for discovering improved multi-agent collaboration strategies through counterfactual plan generation and evaluation. First, we propose a symbolic constraint inductor that induces reusable symbolic constraints from failures, forming an evolving rule library. Then, we propose an evolutionary counterfactual plan generator that systematically explores semantically consistent plan variants through rule-conditioned mutations, enabling robust collaboration strategies beyond short-sighted one-shot LLM plans. Finally, we design an agentic memory-grounded evaluator that ranks candidate plans using retrieval-augmented evidence, producing interpretable, constraint-aware selections. Across multi-agent embodied simulation benchmarks, EvoCF consistently discovers more robust and executable plans compared to baseline approaches. Our results demonstrate that grounding multi-agent planning in agentic memory and counterfactual reasoning significantly enhances both effectiveness and robustness.

---

## 论文详细总结（自动生成）

## 中文总结

### 1. 核心问题与整体含义
- **研究背景**：多智能体具身系统（如机器人协作）的协作规划是 LLM 赋能的规划器所面临的核心挑战。
- **问题所在**：现有 LLM 规划器往往忽视真实世界中的物理约束（如运动学、动力学）与协调约束（如资源竞争、时序配合），导致生成的协作策略不可行或不鲁棒。
- **论文意义**：提出一种名为 **EvoCF**（演化反事实规划）的框架，通过“智能体记忆 + 反事实推理 + 演化搜索”来发现更优的多智能体协作策略，强调从失败中学习和系统性变异探索，而非一次性短视生成。

---

### 2. 方法论
- **核心思想**：将多智能体协作规划建模为一个“反事实生成—评估—演化”的迭代过程，利用经验记忆中的符号化约束指导变异，从而在语义一致的前提下探索更好的计划变体。
- **关键技术模块**：
  1. **符号约束归纳器（Symbolic Constraint Inductor）**
     - 从规划失败的案例中自动归纳出可复用的符号化约束（如顺序约束、空间约束、资源限制等）。
     - 这些约束形成“演化规则库”，并随时间不断更新。
  2. **演化反事实计划生成器（Evolutionary Counterfactual Plan Generator）**
     - 基于规则库中的约束，对现有计划进行“规则条件变异”（rule-conditioned mutation）。
     - 系统性地探索语义一致的计划变体，避免随机或无意义的改动。
     - 通过演化迭代逐步逼近更优的协作方案。
  3. **智能体记忆接地评估器（Agentic Memory-Grounded Evaluator）**
     - 使用检索增强（retrieval-augmented）的证据对候选计划进行排序。
     - 选择过程具有可解释性，且能感知约束满足情况。
- **算法流程（文字说明）**：
  1. 生成初始计划（如通过 LLM 一次性生成）。
  2. 在环境中执行/仿真，收集失败经验。
  3. 从失败经验中归纳新的符号化约束，更新规则库。
  4. 基于规则库对当前计划进行条件变异，产生多个候选计划。
  5. 使用记忆接地评估器对候选计划进行打分与排序。
  6. 选择最优计划并进入下一轮演化，直至满足终止条件。

---

### 3. 实验设计
- **数据集/场景**：论文摘要仅提及使用了“多智能体具身仿真基准”（multi-agent embodied simulation benchmarks），未给出具体的仿真环境名称（如 Habitat、Gibson、AI2-THOR 等）。
- **Benchmark**：未明确说明。推测为涉及多智能体协作任务的具身模拟环境，需要同时满足物理可行性与协调性。
- **对比方法**：摘要仅称“对比基线方法”（baseline approaches），未具体列出名称。通常可能包括：
  - 直接 LLM 规划器（一次性生成）
  - 简单的迭代反馈/重规划方法
  - 不引入记忆或符号约束的规划方法
- **评价指标**：侧重“鲁棒性”和“可执行性”，具体指标未在摘要中给出。

> 注：由于仅提供摘要，实验设计的具体细节无法确认，需参考全文。

---

### 4. 资源与算力
- **原文未明确说明**：摘要和元数据中没有关于 GPU 型号、数量、训练/推理时长、显存消耗等任何算力信息。
- 需查看论文正文中的实验设置部分才能获取相关细节，但就当前提供内容而言，无法总结。

---

### 5. 实验数量与充分性
- **实验数量**：未知。摘要仅给出总体结论（“在多个基准上优于基线”），没有提供实验数量、任务种类或配置。
- **充分性/公平性**：
  - 由于缺少消融实验、参数敏感性分析、不同环境下的对比结果等描述，无法评估实验的全面性。
  - 也无法判断对比基线是否进行超参数调优、是否采用相同计算预算等公平性问题。
  - 因此，**就摘要信息而言，实验充分性无法确认**，需依赖完整论文验证。

---

### 6. 主要结论与发现
- EvoCF 在多智能体具身仿真基准上持续发现了比基线更鲁棒、更可执行的协作计划。
- 将多智能体规划“接地”于智能体记忆（agentic memory）和反事实推理（counterfactual reasoning）能够显著提升规划的有效性与鲁棒性。
- 经验记忆中的符号化约束可以反复利用，避免重复犯相同类型的错误，从而提升整体协作质量。

---

### 7. 优点
- **问题切入角度好**：明确指出 LLM 规划器在具身多智能体场景中的物理与协调约束缺失问题，具有实际意义。
- **方法设计新颖**：将反事实思维（counterfactual thinking）与演化计算结合，用于规划策略探索，区别于传统一次性生成或简单重试。
- **记忆可积累**：通过符号约束归纳将失败经验转化为显式规则，支持持续学习和知识复用，具备可解释性。
- **评估机制有依据**：使用检索增强证据进行计划排序，而非单纯依赖 LLM 打分，增强评估的可靠性和透明度。
- **模块化架构**：约束归纳、变异生成、评估三个模块相对独立，易于扩展和改进。

---

### 8. 不足与局限
- **实验细节缺失**：当前摘要未提供具体的仿真环境、任务规模、基线对比数量、消融实验等关键信息，无法全面判断方法的有效性和泛化能力。
- **可扩展性问题**：符号约束归纳的质量依赖于失败样本的覆盖面，若初始策略空间过窄，可能导致规则库不完整。
- **计算开销**：演化迭代和检索增强评估可能带来较高的计算成本，摘要中未给出效率分析。
- **语义一致性保障**：虽然声称变异是“语义一致”的，但没有说明如何度量和保证语义一致性，可能存在离群变异。
- **应用范围**：方法主要面向具身多智能体仿真，真实世界的噪声、部分可观测性和通信延迟等挑战尚未讨论。

---

（完）
