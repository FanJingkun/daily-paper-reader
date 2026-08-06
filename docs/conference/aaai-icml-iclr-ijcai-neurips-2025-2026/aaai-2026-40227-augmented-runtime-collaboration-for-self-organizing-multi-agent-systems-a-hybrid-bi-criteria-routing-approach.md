---
title: "Augmented Runtime Collaboration for Self-Organizing Multi-Agent Systems: A Hybrid Bi-Criteria Routing Approach"
title_zh: 自组织多智能体系统的增强运行时协作：混合双准则路由方法
authors: "Qingwen Yang, Feiyu Qu, Tiezheng Guo, Yanyi Liu, Yingyou Wen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40227/44188"
tags: ["query:mcd"]
score: 4.0
evidence: 面向自组织多智能体系统的运行时协作与任务路由，与多智能体协调交互相关。
tldr: 本文针对LLM多智能体系统协作策略受限的问题，提出BiRouter双标准路由方法，使每个智能体在运行时自主执行下一跳任务路由，仅依赖局部信息进行协作规划。该方法克服了静态拓扑和集中式全局规划的局限，提升了开放分布式网络的扩展性和适应性。实验验证了自组织多智能体系统协作效率的提升，为分布式多智能体协作提供了新路径。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1824, \"height\": 921, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 875, \"height\": 790, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 881, \"height\": 509, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 882, \"height\": 779, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 880, \"height\": 427, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 887, \"height\": 618, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40227/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 876, \"height\": 963, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40227/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 885, \"height\": 302, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40227/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1784, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40227/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 390, \"label\": \"Table\"}]"
motivation: 现有LLM多智能体系统依赖静态拓扑和集中式规划，限制开放网络中的可扩展性和适应性。
method: 提出BiRouter双标准路由方法，使各智能体利用局部信息在运行时自主进行任务路由，实现自组织协作。
result: 在开放分布式网络设置中，BiRouter提升了任务性能和协作效率，优于集中式基线。
conclusion: 局部信息驱动的动态路由能有效提升自组织多智能体系统的协作能力与可扩展性。
---

## Abstract
LLM-based multi-agent systems have demonstrated significant capabilities across diverse domains. However, the task performance and efficiency are fundamentally constrained by their collaboration strategies. Prevailing approaches rely on static topologies and centralized global planning, a paradigm that limits their scalability and adaptability in open, decentralized networks. Effective collaboration planning in distributed systems using only local information thus remains a formidable challenge. To address this, we propose BiRouter, a novel dual-criteria routing method for Self-Organizing Multi-Agent Systems (SO-MAS). This method enables each agent to autonomously execute "next-hop" task routing at runtime, relying solely on local information.  Its core decision-making mechanism is predicated on balancing two metrics: (1) the ImpScore, which evaluates a candidate agent's long-term importance to the overall goal, and (2) the GapScore, which assesses its contextual continuity for the current task state. Furthermore, we introduce a dynamically updated reputation mechanism to bolster system robustness in untrustworthy environments and have developed a large-scale, cross-domain dataset, comprising thousands of annotated task-routing paths, to enhance the model's generalization. Extensive experiments demonstrate that BiRouter achieves superior performance and token efficiency over existing baselines, while maintaining strong robustness and effectiveness in information-limited, decentralized, and untrustworthy settings.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

**研究动机与背景：**
- 基于 LLM 的多智能体系统（MAS）虽已在多个领域展现出色能力，但任务性能与效率受制于其协作策略。
- 现有主流方法依赖**静态拓扑**和**集中式全局规划**（如 GPTSwarm、G-Designer），该范式在开放、去中心化网络中面临可扩展性与适应性瓶颈——没有单一智能体拥有全局视野。
- 虽然 A2A、ANP 等协议解决了智能体发现与互联的基础设施问题，但缺乏**面向任务的动态协作策略**，导致资源浪费、级联失效等问题。
- 核心研究空白是：**仅依靠局部信息，智能体如何动态做出“下一跳”路由决策，从而涌现出全局高效的解决方案？**

**整体含义：**
论文将研究置于自组织多智能体系统（SO-MAS）框架下，主张协作路径不应由中心调度器预先计算，而应由智能体在运行时通过局部决策逐步涌现形成。

---

## 2. 论文提出的方法论

**核心思想：**
提出 **BiRouter**——一种双准则路由方法，受 A* 搜索算法启发，将智能体的后继选择建模拟为启发式路径搜索问题。

**关键技术细节：**

| 组件 | 功能 | 类比 |
|------|------|------|
| **ImpScore（重要度分数）** | 评估候选智能体对整体任务的长期能力相关性 | A* 的启发式代价 h(n) |
| **GapScore（连贯性分数）** | 评估候选智能体与当前任务状态之间的上下文衔接合理性 | A* 的路径代价 g(n) |
| **信誉机制（Scrd）** | 动态更新的信誉分数，对结合后的任务相关分数进行乘法门控 | 开放环境中抵御不可信代理 |

**算法流程（文字说明）：**
1. 每个智能体维护一个局部候选集合 Cxi。
2. 当前智能体 xi 的 BiRouter 模块通过共享编码器加两个分支（重要性分支 RImp 与连贯性分支 RGap）分别计算候选的 SImp 和 SGap。
3. 两个分数按超参数 α 加权融合，再与信誉分数做逐元素乘积，经 Softmax 生成后继选择的概率分布（式 6）。
4. 选定后继后，当前智能体生成**后继感知消息** mi，定制化传递给下一个智能体（式 7）。
5. 引入专门的 **Finisher 智能体**，使任务可自适应终止，避免固定长度导致的 token 浪费。
6. 整条智能体链的生成概率为每一步转移概率的乘积（式 8）。

**训练数据生成（MARS 数据集）：**
- 由 LLM 生成 115 个代表性领域的关键词，再生成单领域或跨领域查询。
- 使用 RBF 核的语义密度函数 ρ(x) 识别嵌入空间的稀疏区域，定向补充查询以丰富语义覆盖（式 9）。
- ImpScore 基于多轮 LLM 对智能体关键性的排序，辅以长路径惩罚因子 γ，最后经缩放 Sigmoid 映射到 [l, u] 区间（式 10）。
- GapScore 根据智能体在完整路径中的位置计算（式 11）。

---

## 3. 实验设计

**Benchmark 数据集（6 个）：**
- MMLU（通用推理）、GSM8K（数学）、MultiArith（数学）、SVAMP（数学）、HumanEval（代码）、MBPP（代码）。

**对比基线（12 个）：**
1. **单智能体方法**：Single-agent LLM、CoT、ComplexCoT、CoT-SC。
2. **静态协调 MAS**：MacNet、LLM-Blender、LLM-Debate、GPTSwarm、G-Designer。
3. **动态协调 MAS**：AgentVerse、DyLAN、MaAS。

**实验场景：**
- **集中式 MAS**：所有智能体在每个阶段均可见，对比 12 个基线（表 1）。
- **SO-MAS 分布式场景**：每个智能体仅能与 3 个随机邻居通信，与 DyLAN、MaAS 对比（表 2）。
- **鲁棒性测试**：向网络注入等量不可靠智能体（用 gpt-4.1 重写提示词使其故意出错），观察性能下降与信誉分数动态（图 5-6）。
- **案例研究**：GSM8K 上的路由路径可视化（图 7）。
- **消融研究**：三个变体——去掉信誉机制（w/o Scrd）、去掉后继感知消息（w/o Succ）、去掉 Finisher 智能体（w/o Fin）（表 3）。

---

## 4. 资源与算力

- 论文**未明确报告 GPU 型号、数量或训练时长**。
- 文中仅提到：使用 `qwen3-embedding-0.6b` 作为冻结编码器，仅训练 Cross-Attention 和评分头；所有智能体通过 OpenAI API 调用 `gpt-4o-mini`；使用 `gpt-4.1` 生成智能体配置。
- 需要注意的是，训练数据生成和推理均依赖 API 调用，未披露具体实验成本。

---

## 5. 实验数量与充分性

**实验数量：**
- 集中式场景：6 个 benchmark × 13 种方法（含 BiRouter）。
- SO-MAS 场景：2 个 benchmark × 3 种方法。
- 鲁棒性实验：2 个 benchmark，攻击前后对比。
- 超参数调优：在 GSM8K 和 MBPP 上分别取 100/200 条查询。
- 案例研究 1 个、消融实验 4 个配置 × 2 个 benchmark。

**充分性评估：**
- **较充分**：覆盖了推理、数学、代码三类任务，对比了单智能体、静态 MAS、动态 MAS 三个类别，且兼顾集中式和分布式两种架构，实验设计较为全面。
- **客观性有一定保障**：BiRouter 在表 1 中有 4/6 个 benchmark 达到最优。
- **公平性存在瑕疵**：
  - 表 1 中 G-Designer 在 MBPP、MaAS 在 MMLU 和 MultiArith 上的结果为“–”（缺失），导致平均值的比较口径不完全一致。
  - 所有实验仅运行 **2 次取平均**，方差信息完全缺失，统计显著性未知。
  - 未说明基线方法的超参数是否经过同等调优。

---

## 6. 论文的主要结论与发现

1. **集中式环境中性能领先**：BiRouter 平均准确率 91.73%，超越所有基线，相对单智能体方法提升 5.86%–7.06%、静态协调方法提升 4.65%–7.58%、动态协调方法提升 4.52%–5.23%。
2. **Token 效率显著**：在 SVAMP 和 GSM8K 上，BiRouter 在取得最高性能的同时 token 消耗最低；消融实验显示去掉 Finisher 会使 token 开销增加约 69%（GSM8K）和 50%（HumanEval）。
3. **分布式场景韧性最强**：在 SO-MAS 局部信息受限条件下，BiRouter 性能下降最小（GSM8K 91.99、HumanEval 89.63），优于依赖全局信息的 DyLAN 和 MaAS。
4. **信誉机制有效**：BiRouter 能快速识别不可靠智能体并降低其权重，在攻击下性能保持最高。
5. **三个组件缺一不可**：信誉机制（提升不可信环境鲁棒性）、后继感知消息（提升协作效率）和 Finisher（动态终止+节流）均有实质贡献。

---

## 7. 优点

- **方法新颖性**：将多智能体协作建模为分布式路由问题，借用 A* 双准则思想，在概念上有理论支撑（g(n) 与 h(n) 的类比清晰合理）。
- **架构适配性**：同一方法无缝适配集中式和分布式两种范式，拓补了静态规划与去中心化执行之间的隔阂。
- **训练数据贡献**：构建并公开 MARS 数据集——一个跨 115 个领域的、包含数千条标注路由路径的数据集，对社区有实际价值。
- **可靠性与效率兼顾**：信誉机制为开放环境中不可信代理问题提供了务实解决方案；后继感知消息和 Finisher 设计精细地降低了 token 浪费。
- **实验场景丰富**：从集中式到分布式、从正常到攻击环境，实验设计覆盖面广。

---

## 8. 不足与局限

- **训练数据为纯合成数据**：MARS 全部由 LLM 生成，缺少真实多智能体系统中的人类行为或真实交互数据，存在领域偏移风险，真实世界的泛化性有待验证。
- **基线与评估的公平性存疑**：部分基线的数据缺失（G-Designer 在 MBPP、MaAS 在 MMLU/MultiArith），比较口径不完全统一；实验仅运行 2 次，缺乏误差区间和显著性检验。
- **学理深度受限**：虽然称为“学习”，实际仅训练了轻量评分头（Cross-Attention + MLP）且编码器冻结，没有明确说明如何保证合成训练数据的标注质量。
- **信誉机制的局限**：信誉更新依赖 LLM 事后评估，引入了额外推理开销，且评估本身的偏差未被讨论。
- **协同模式简单**：仅支持单向线性链式的“下一跳”路由，不涉及并行分支、汇聚、协商等更复杂的协作拓扑，扩展性仍有瓶颈。
- **安全性分析不足**：对恶意智能体的假设较简单（只是输出错误），未考虑更为复杂的安全威胁（如刻意误导路由方向的对抗性攻击）。
- **资源细节缺失**：没有报告训练时长、GPU 配置、API 调用成本等信息，复现成本难以评估。

---

（完）
