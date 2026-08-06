---
title: LLM Collaboration with Multi-Agent Reinforcement Learning
title_zh: 大语言模型与多智能体强化学习的协作
authors: "Shuo Liu, Zeyu Liang, Xueguang Lyu, Christopher Amato"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40487/44448"
tags: ["query:mcd"]
score: 8.0
evidence: 将大语言模型协作建模为合作式多智能体强化学习问题
tldr: 现有大语言模型各自预训练，不专门针对协作优化，且常用个体奖励需精心设计。本文将LLM协作建模为合作式多智能体强化学习问题，并提出多智能体、多轮算法MAGRPO，基于组奖励进行策略优化。在写作和编码协作任务上，该算法有效提升了多LLM协作效果，展示了用MARL微调语言模型的潜力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1696, \"height\": 764}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1533, \"height\": 435}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1532, \"height\": 850}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40487/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1677, \"height\": 529}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40487/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1838, \"height\": 674}]"
motivation: LLM预训练未针对协作优化，个体奖励设计复杂且难以鼓励协作。
method: 将LLM协作建模为合作式MARL，提出多智能体组相对策略优化算法MAGRPO。
result: 在写作与编码协作实验中，微调后的LLM协作性能显著提升。
conclusion: MARL为提升大语言模型多智能体协作提供了有效训练框架。
---

## Abstract
A large amount of work has been done in Multi-Agent Systems (MAS) for modeling and solving problems with multiple interacting agents. However, most LLMs are pretrained independently and not specifically optimized for coordination. Existing LLM fine-tuning frameworks rely on individual rewards, which require complex reward designs for each agent to encourage collaboration. To address these challenges, we model LLM collaboration as a cooperative Multi-Agent Reinforcement Learning (MARL) problem. We develop a multi-agent, multi-turn algorithm, Multi-Agent Group Relative Policy Optimization (MAGRPO), to solve it, building on current RL approaches for LLMs as well as MARL techniques. Our experiments on LLM writing and coding collaboration demonstrate that fine-tuning MAS with MAGRPO enables agents to generate high-quality responses efficiently through effective cooperation. Our approach opens the door to using MARL methods for LLM collaboration and highlights the associated challenges.

---

## 论文详细总结（自动生成）

# 《LLM Collaboration with Multi-Agent Reinforcement Learning》论文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：多智能体系统（MAS）已有大量工作用于建模和求解多智能体交互问题，但大多数 LLM 是独立预训练的，并未针对协作进行专门优化。现有 LLM 微调框架通常依赖**个体奖励**，需要为每个智能体设计复杂的奖励函数才能鼓励协作，这不仅繁琐且难以扩展。
- **核心问题**：如何用统一的协作式学习框架，让多个 LLM 通过协同训练产生高质量、低开销的联合响应，而不依赖复杂的逐智能体奖励设计。
- **整体含义**：论文将 LLM 协作建模为**合作式多智能体强化学习（Cooperative MARL）** 问题，并形式化为 Dec-POMDP，提出了多智能体、多轮训练算法 **MAGRPO**，实验证明该方法能显著提升 LLM 在写作和编程任务中的协作效率与输出质量，为 MARL 方法在 LLM 协作训练中开辟了新方向。

## 2. 论文提出的方法论

- **问题形式化**：将 LLM 协作建模为 Dec-POMDP，定义为一个元组 $\langle I, S, \{O_i\}, \{A_i\}, R, T, H \rangle$：
  - $I$：$n$ 个 LLM 智能体；
  - $S$：全局状态，包含可访问状态 $s^{acc}_t$ 和不可访问的用户状态 $s^{usr}_t$；
  - $O_i$：局部观测（自然语言提示）；
  - $A_i$：局部动作（自然语言响应）；
  - $R$：基于可访问状态和联合动作的联合奖励函数；
  - $T$：状态转移函数；
  - $H$：对话轮次上限。
- **核心算法 MAGRPO（Multi-Agent Group Relative Policy Optimization）**：
  - 借鉴单智能体 GRPO 和 MAPPO 的思想，在**集中式训练、分散式执行**（CTDE）框架下，用**组相对优势**实现联合优化。
  - 训练时每个智能体对同一历史生成 $G$ 条响应，构成联合动作组，由环境给出联合奖励。
  - **优势估计**（组内相对优势）：
    \[
    \hat{A}_t^{(g)} = R_t^{(g)} - \frac{1}{G}\sum_{g=1}^G R_t^{(g)}
    \]
    其中 $R_t^{(g)} = \sum_{\tau=t}^{H-1} r_\tau^{(g)}$ 为回报。
  - **策略梯度**：
    \[
    J(\theta_i) = \mathbb{E}\left[ \frac{1}{G} \sum_{g=1}^G \hat{A}_t^{(g)} \log \pi_{\theta_i}(a_{i,t}^{(g)} | h_{i,t}^G) \right]
    \]
  - 简化设计：不使用重要性采样和 epsilon clipping，KL 散度系数设为 0，允许策略更大幅度偏离基座模型。
  - **算法流程**：采样任务 → 初始化观测与历史 → 每轮生成一组响应 → 获取联合奖励 → 接收新观测并更新历史 → 回合结束后反向逐轮计算回报、估计优势、更新策略。

## 3. 实验设计

- **写作协作任务**：
  - **TLDR 摘要**：2 个 Qwen3-1.7B 智能体分别担任"核心要点生成器"和"详细摘要器"，对 Reddit 帖文生成摘要。
  - **arXiv 扩展**：2 个智能体分别撰写研究背景与动机、方法与实验，协作生成论文引言。
  - **奖励指标**：结构（长度与比例）、风格一致性（归一化 Jaccard 相似度）、逻辑连贯性（过渡词计数）。
- **编程协作任务**：
  - **HumanEval（HE）**：164 道手写编程题，含自然语言描述、函数签名和单元测试。2 个 Qwen2.5-Coder-3B 智能体分别生成辅助函数和主函数。
  - **CoopHumanEval（CHE）**：自构建的协作编程数据集，筛选/新增具有可分解协作结构的问题，规避 HE 中不适合协作的"原子操作"噪声。
  - **奖励模型**：分级奖励——结构完整性 → 语法正确性 → 测试通过率 → 协作质量奖励（主函数正确调用辅助函数时给予奖励）。
- **对比基线**：
  - 单模型（Qwen3-4B / Qwen2.5-Coder-7B）、微调后的单模型；
  - 多智能体零样本基线：**朴素并行生成**、**顺序流水线**、**单轮讨论**。
  - 所有基线均保持任务描述固定，仅添加最少的协调指令，以公平对比。

## 4. 资源与算力

- 论文**未明确给出** GPU 数量、训练总时长、步数对应的具体算力开销。
- 提及的信息：
  - 推理速度（tokens/s）和响应时间在 **GeForce RTX 5090s** 上测量；
  - 训练使用了 **Delta 和 DeltaAI 计算资源**（美国国家超级计算应用中心 NCSA），由 NSF 基金资助（#2044993 和 #2409351）；
  - 训练步数在图表中显示约为 1.5K~2K 步；
  - 使用的模型规模较小（Qwen3-1.7B、Qwen2.5-Coder-3B 等），训练开销相对可控。

## 5. 实验数量与充分性

- **实验组数**：
  - 写作任务：TLDR 和 arXiv 两个数据集，每组 10 次运行取平均；
  - 编程任务：HE 和 CHE 两个数据集，包含**单轮**和**多轮**两种训练设置，每组 10 次运行；
  - 额外结果（附录）：包括 pass@k、外部工具消融（自演化与专家引导）等。
- **充分性评价**：
  - 覆盖了**写作**和**编码**两个典型领域，任务类型多样（摘要、扩展、函数生成）；
  - 对比了单智能体和多智能体、固定模型和微调模型、并行和顺序通信等多种基线，对比对象具有代表性；
  - 实验说明了不同协作模式（回退、装饰、协调器、策略过滤器）的涌现，具有一定深度。
  - 不足：仅使用 2 个智能体的简单场景（单轮或两轮交互），未对更大智能体数量、更长对话轮次、更大模型规模进行扩展验证。

## 6. 论文的主要结论与发现

- MAGRPO 能使多 LLM 系统在写作协作中生成结构良好、风格一致、逻辑连贯的内容，在总收益上显著优于所有基线。
- MAGRPO 在编程协作中大幅提升协作质量（协作奖励和总收益），在 CHE 上效果优于 HE，说明**数据集的协作结构质量**对训练稳定性影响显著。
- 多轮训练中智能体初期难以消化外部反馈，但经训练后能学会利用错误信号改进输出质量。
- 与参数规模相当的单模型（如 Qwen3-4B、Qwen2.5-Coder-7B）相比，MAGRPO 微调的双小模型系统在**推理速度上快约 3 倍**，同时获得更高响应质量。
- 在简单联合奖励下，训练中涌现出多种合作模式：辅助函数承担核心逻辑、主函数做备份/装饰、主函数作为协调者分解任务、辅助函数充当策略过滤器等。

## 7. 优点

- **问题建模新颖**：首次系统地将 LLM 协作规范化为合作式 MARL / Dec-POMDP 问题，用联合奖励替代复杂的逐智能体奖励设计。
- **算法设计高效**：MAGRPO 结合了 GRPO 的无价值模型优点和 MAPPO 的集中式训练优势，通过组相对优势实现协作信号共享，同时保持分散式执行。
- **实验设计较严谨**：在写作和编程两个领域验证，基线设置考虑了提示公平性（最小指令差异），并在附录提供奖励细节与额外实验。
- **具有实际价值**：展示了对小模型进行多智能体微调即可获得比大模型单智能体更高效、更高质量的协作结果。

## 8. 不足与局限

- **规模局限**：实验仅涉及 2 个智能体和 1~2 轮交互，未验证大规模智能体（如 5~10 个）或长视界多轮任务下的表现。
- **奖励模型简单**：写作任务使用启发式指标（长度比、Jaccard 相似度、过渡词计数）作为奖励，与真实人类偏好存在偏差；编程任务的分级奖励也较为粗糙。
- **HE 上测试通过率提升有限**：HumanEval 的噪声条目导致反馈不可靠，说明方法对数据集的协作结构较为敏感。
- **训练方差与稳定性**：基于组蒙特卡洛回报的优势估计方差较高，多轮训练初期收益显著下降，收敛稳定性有待进一步改善。
- **基线范围有限**：未与其他多智能体微调方法（如基于个体奖励的微调、角色条件奖励微调）进行直接对比，说服力略有不足。
- **缺少算力明细**：论文未披露训练阶段精确的 GPU 数量、时长和能耗，难以评估方法的经济成本与复现开销。
- **理论保证缺失**：论文对方法的收敛性和优势估计的偏差/方差特性未给出理论分析，仅凭实验验证。

（完）
