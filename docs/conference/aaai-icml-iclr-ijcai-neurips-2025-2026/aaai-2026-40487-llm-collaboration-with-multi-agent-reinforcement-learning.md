---
title: LLM Collaboration with Multi-Agent Reinforcement Learning
title_zh: 基于多智能体强化学习的大语言模型协作
authors: "Shuo Liu, Zeyu Liang, Xueguang Lyu, Christopher Amato"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40487/44448"
tags: ["query:mcd"]
score: 7.0
evidence: 将大模型协同建模为合作式多智能体强化学习，提出分组相对策略优化算法
tldr: 现有大语言模型各自预训练且缺乏协同优化，个体奖励设计复杂难以促进协作。本文将大模型协作任务建模为合作式多智能体强化学习问题，提出多智能体多轮算法MAGRPO，融合大模型强化学习与多智能体强化学习技术。实验表明，在写作与代码协作任务上微调后能有效提升多智能体协作表现，为多智能体奖励设计与长期协同优化提供了新框架。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1696, \"height\": 764, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1533, \"height\": 435, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40487/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1532, \"height\": 850, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40487/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1677, \"height\": 529, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40487/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1838, \"height\": 674, \"label\": \"Table\"}]"
motivation: 大语言模型预训练时未针对协同进行优化，个体奖励设计复杂且难以促进协作。
method: 将大模型协作建模为合作式多智能体强化学习，提出分组相对策略优化算法MAGRPO。
result: 在写作与代码协作任务上验证了该微调方法能够提升多智能体协作效果。
conclusion: MAGRPO为以大模型为智能体的协同决策与奖励设计提供了可行的MARL框架。
---

## Abstract
A large amount of work has been done in Multi-Agent Systems (MAS) for modeling and solving problems with multiple interacting agents. However, most LLMs are pretrained independently and not specifically optimized for coordination. Existing LLM fine-tuning frameworks rely on individual rewards, which require complex reward designs for each agent to encourage collaboration. To address these challenges, we model LLM collaboration as a cooperative Multi-Agent Reinforcement Learning (MARL) problem. We develop a multi-agent, multi-turn algorithm, Multi-Agent Group Relative Policy Optimization (MAGRPO), to solve it, building on current RL approaches for LLMs as well as MARL techniques. Our experiments on LLM writing and coding collaboration demonstrate that fine-tuning MAS with MAGRPO enables agents to generate high-quality responses efficiently through effective cooperation. Our approach opens the door to using MARL methods for LLM collaboration and highlights the associated challenges.

---

## 论文详细总结（自动生成）

## 1. 论文核心问题与整体含义（研究动机与背景）

- **研究动机**：当前主流大语言模型（LLM）通常独立预训练，没有针对多智能体间的协调与协作进行优化。已有的 LLM 微调方法多依赖个体奖励，需要为每个智能体单独设计复杂奖励函数，难以有效促进协作。
- **核心问题**：如何将 LLM 之间的协作建模为可学习的、可优化的联合决策问题，使多个 LLM 能通过训练在写作、编程等任务中高效协同，而非仅在推理时靠 prompt 交互。
- **整体贡献**：本文将 LLM 协作建模为合作式多智能体强化学习（cooperative MARL）问题，并将其形式化为去中心化部分可观测马尔可夫决策过程（Dec-POMDP）；在此基础上提出多智能体、多轮算法 **MAGRPO**，为 LLM 协作提供了一种新的训练范式。

## 2. 提出的方法论：核心思想、关键技术细节与算法流程

- **核心思想**：借鉴单智能体 GRPO（Group Relative Policy Optimization）和多智能体 MAPPO 的思想，在集中式训练与去中心化执行（CTDE）框架下，利用**联合奖励**和**组相对优势**实现多 LLM 的协同优化，避免为每个智能体单独设计奖励。
- **问题形式化**：
  - 将 LLM 协作定义为一个 Dec-POMDP：`⟨I, S, {O_i}, {A_i}, R, T, H⟩`；
  - 每个 LLM 是智能体，观察为自然语言指令（prompt），动作为生成的自然语言回复，奖励为基于可观测状态与联合动作的联合奖励。
- **MAGRPO 算法流程（文字说明）**：
  1. 从数据集中采样任务，初始化各智能体的观测和历史；
  2. 每一轮（turn），每个智能体基于自身历史各自生成一组候选回复（group），构成一组联合动作；
  3. 环境根据联合动作给出联合奖励，状态转移，智能体获得新的观测并更新历史；
  4. 回合结束后，计算每个 group 的折扣回报；
  5. 利用组内平均回报作为基线，计算每个联合动作的优势：  
     `\hat{A}^{(g)}_t = R^{(g)}_t − (1/G) Σ_g R^{(g)}_t`；
  6. 用策略梯度更新每个智能体策略，目标为：  
     `J(θ_i) = E[(1/G) Σ_g \hat{A}^{(g)}_t log π_{θ_i}(a^{(g)}_{i,t}|h^G_{i,t})]`；
  7. 算法不采用重要性采样和 epsilon clipping，KL 散度系数设为 0，允许更大策略偏离。
- **技术特点**：无需大型价值模型，通过蒙特卡洛抽样组内回报估计优势，实现集中式优势估计但保持去中心化执行，兼顾可扩展性与训练稳定性。

## 3. 实验设计：数据集、场景、基准与对比方法

- **写作协作任务**：
  - **TLDR 摘要（TLDR summarization）**：基于 Reddit 帖子数据，两个 Qwen3-1.7B 智能体分别生成核心总结（TLDR）和详细摘要；
  - **arXiv 扩写（arXiv expansion）**：两个智能体基于论文摘要协作生成引言，一个写背景与动机，另一个写方法与实验；
  - **奖励模型**：由结构（长度与比例）、风格一致性（Jaccard 相似度）、逻辑连贯性（过渡词数量）加权组合。
- **代码协作任务**：
  - **HumanEval（HE）**：164 个人工编写的编程问题，含描述、函数签名和单元测试；
  - **CoopHumanEval（CHE）**：作者构造的更具协作潜力的数据集，包含可分解的原始 HE 问题与额外问题；
  - **奖励模型**：分级奖励，依次检查结构完整性、语法正确性、测试通过率、协作质量奖励。
- **对比方法（baselines）**：
  - 写作任务：单模型（Qwen3-4B）、并行生成（naive concatenation）、顺序流水线（sequential pipeline）、单轮讨论（one-round discussion）；
  - 代码任务：固定单模型与微调单模型（Qwen2.5-Coder-7B）、并行生成、顺序流水线、单轮讨论；
  - 此外还有单轮与多轮 MAGRPO 的对比。

## 4. 资源与算力

- 文中未明确给出 GPU 数量、总训练时长或整体算力开销。
- 文中提到在 GeForce RTX 5090 上记录推理速度与响应时间；致谢部分提到使用了 NCSA Delta 和 DeltaAI 计算资源（分配号 CIS250443、CIS250554）。
- 整体算力细节（如 GPU 时数、训练迭代总耗时）未披露，这是本文在可复现性信息上的一个欠缺。

## 5. 实验数量与充分性

- **实验组数**：
  - 写作：2 个任务（TLDR、arXiv），对比 4 类基线；
  - 代码：2 个数据集（HE、CHE），对比 5 类基线，并做了单轮 vs 多轮 MAGRPO 对比；
  - 附录中还有外部工具消融（self-evolving、expert guidance）和 pass@k 等额外结果。
- **充分性评价**：
  - 覆盖了自然语言写作和程序生成两个典型协作场景，且包含多轮交互设置，实验设计比较全面；
  - 奖励函数多为较为简单的代理指标（Jaccard、过渡词计数、结构规则），与真实复杂任务存在差距；
  - 基线方法均无微调，仅靠 prompt 交互，与有训练的 MAGRPO 对比难免存在不公平的一面（虽然作者强调尽量固定 prompt 并添加最小指令）；
  - CHE 数据集为作者自行构造，可能引入偏置；HE 中非协作任务易造成噪声，作者对此进行了讨论。

## 6. 主要结论与发现

- **效率与质量提升**：MAGRPO 能使多 LLM 快速学出有效协作方式，显著优于 prompt 层面的多智能体交互方法；写作任务中速度约为参数量相当的单 Qwen3-4B 的 3 倍，同时文章结构、风格一致性和连贯性更好。
- **协作模式涌现**：在代码协作中，MAGRPO 能自发学到多种协作方案，如辅助函数承担核心逻辑、主函数做装饰/兜底，或主函数作为协调者分解任务等。
- **多轮反馈有效**：多轮训练下智能体能够利用外部模型的错误反馈逐步改进，在 CHE 上性能优于单轮训练，说明在具有明确协作结构的数据上，MAGRPO 可充分利用多轮信息。
- **框架意义**：以联合奖励训练多个 LLM 是可行且有效的，为 MARL 在 LLM 协作中的应用打开了新方向。

## 7. 优点

- **问题建模新颖**：将 LLM 协作严格建模为 Dec-POMDP，为多 LLM 协作提供了理论支撑。
- **算法设计合理**：MAGRPO 无需大型价值模型，用组相对优势实现集中式估计，同时保持去中心化执行，兼顾性能与可扩展性。
- **避免个体奖励工程**：采用统一联合奖励即可促进角色分工和协作，显著简化训练设计。
- **实验场景丰富**：同时覆盖写作与编程两类任务，并考虑单轮与多轮设定，能展示方法在不同协作难度下的表现。
- **诚实讨论局限**：对 HE 数据噪声、自然语言表示、训练范式开放性等问题进行了清晰的说明。

## 8. 不足与局限

- **算力信息缺失**：没有报告具体 GPU 数量、训练步数对应的显存/时间消耗，难以评估成本与可复现性。
- **奖励模型过于简化**：写作任务使用 Jaccard 相似度、过渡词数量等代理指标，并不能完全反映语义质量和真实用户偏好；代码任务使用分级规则奖励，覆盖有限。
- **实验公平性存疑**：MAGRPO 经过训练，而多智能体基线均未微调，比较不够对等；最好的比较应是有训练的基线。
- **模型与任务规模有限**：实验仅涉及 1.7B/3B/7B 级别模型、两智能体的短 horizon 任务，未测试更大模型、更多智能体或更长多轮协作。
- **泛化性不足**：在 HumanEval 上测试通过率改善不显著，说明协作训练在噪声数据和复杂逻辑任务上的收益有限；多轮训练初期智能体难以有效利用外部反馈。
- **算法设计仍有简化**：放弃重要性采样和 clipping、KL 系数设为 0，可能带来训练不稳定或策略退化风险。

（完）
